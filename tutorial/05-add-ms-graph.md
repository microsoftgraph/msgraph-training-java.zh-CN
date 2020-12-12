---
ms.openlocfilehash: c26e5b8ab0b7c5c62b926e3f5416b94e3f10b601
ms.sourcegitcommit: eb935a250f8531b04a42710356072b80d46ee3a4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/11/2020
ms.locfileid: "49661044"
---
<!-- markdownlint-disable MD002 MD041 -->

在此练习中，你将 Microsoft Graph 合并到应用程序中。 对于此应用程序，你将使用 Microsoft [Graph SDK for Java](https://github.com/microsoftgraph/msgraph-sdk-java) 调用 Microsoft Graph。

## <a name="implement-an-authentication-provider"></a>实现身份验证提供程序

Microsoft Graph SDK for Java需要实现 `IAuthenticationProvider` 接口来实例化其 `GraphServiceClient` 对象。

1. 在名为 **SimpleAuthProvider.java** 的 **./graphtu一l/src/main/java/graphtu上l** 目录中创建新文件，并添加以下代码。

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/SimpleAuthProvider.java" id="AuthProviderSnippet":::

## <a name="get-user-details"></a>获取用户详细信息

1. 在名为 **Graph.java** 的 **./graphtu一l/src/main/java/graphtu加载项目录中** 创建新文件，并添加以下代码。

    ```java
    package graphtutorial;

    import java.time.LocalDateTime;
    import java.time.ZonedDateTime;
    import java.time.format.DateTimeFormatter;
    import java.util.LinkedList;
    import java.util.List;
    import java.util.Set;

    import com.microsoft.graph.logger.DefaultLogger;
    import com.microsoft.graph.logger.LoggerLevel;
    import com.microsoft.graph.models.extensions.Attendee;
    import com.microsoft.graph.models.extensions.DateTimeTimeZone;
    import com.microsoft.graph.models.extensions.EmailAddress;
    import com.microsoft.graph.models.extensions.Event;
    import com.microsoft.graph.models.extensions.IGraphServiceClient;
    import com.microsoft.graph.models.extensions.ItemBody;
    import com.microsoft.graph.models.extensions.User;
    import com.microsoft.graph.models.generated.AttendeeType;
    import com.microsoft.graph.models.generated.BodyType;
    import com.microsoft.graph.options.HeaderOption;
    import com.microsoft.graph.options.Option;
    import com.microsoft.graph.options.QueryOption;
    import com.microsoft.graph.requests.extensions.GraphServiceClient;
    import com.microsoft.graph.requests.extensions.IEventCollectionPage;
    import com.microsoft.graph.requests.extensions.IEventCollectionRequestBuilder;

    /**
     * Graph
     */
    public class Graph {

        private static IGraphServiceClient graphClient = null;
        private static SimpleAuthProvider authProvider = null;

        private static void ensureGraphClient(String accessToken) {
            if (graphClient == null) {
                // Create the auth provider
                authProvider = new SimpleAuthProvider(accessToken);

                // Create default logger to only log errors
                DefaultLogger logger = new DefaultLogger();
                logger.setLoggingLevel(LoggerLevel.ERROR);

                // Build a Graph client
                graphClient = GraphServiceClient.builder()
                    .authenticationProvider(authProvider)
                    .logger(logger)
                    .buildClient();
            }
        }

        public static User getUser(String accessToken) {
            ensureGraphClient(accessToken);

            // GET /me to get authenticated user
            User me = graphClient
                .me()
                .buildRequest()
                .select("displayName,mailboxSettings")
                .get();

            return me;
        }
    }
    ```

1. 在 `import` **App.java** 的顶部添加以下语句。

    ```java
    import com.microsoft.graph.models.extensions.User;
    ```

1. 在 **App.java** 中，将以下代码添加到该行的正前，以获取用户并输出 `Scanner input = new Scanner(System.in);` 用户显示名称。

    ```java
    // Greet the user
    User user = Graph.getUser(accessToken);
    System.out.println("Welcome " + user.displayName);
    System.out.println("Time zone: " + user.mailboxSettings.timeZone);
    System.out.println();
    ```

1. 运行应用程序。 登录应用后，按名称欢迎使用。

## <a name="get-calendar-events-from-outlook"></a>从 Outlook 获取日历事件

1. 将以下函数添加到 `Graph` **Graph.java** 中的类，以从用户日历获取事件。

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/Graph.java" id="GetEventsSnippet":::

考虑此代码正在做什么。

- 将调用的 URL 为 `/me/calendarview` 。
  - `QueryOption` 对象用于添加 `startDateTime` 和 `endDateTime` 参数，并设置日历视图的开始和结束。
  - `QueryOption`对象用于添加参数 `$orderby` ，按开始时间对结果进行排序。
  - 对象用于添加标头，从而导致根据用户的时区调整开始时间和 `HeaderOption` `Prefer: outlook.timezone` 结束时间。
  - `select`该函数将每个事件返回的字段限制为仅应用将实际使用的字段。
  - `top`该函数将响应中的事件数限制为最多 25 个。
- 如果当前一周的事件超过 25 个，则此函数用于请求其他 `getNextPage` 结果页。

1. 在名为 **GraphToIana.java** 的 **./graphtu一l/src/main/java/graphtu在** 目录中创建新文件，并添加以下代码。

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/GraphToIana.java" id="zoneMappingsSnippet":::

    此类实现一个简单的查找以将 Windows 时区名称转换为 IANA 标识符，并基于 Windows 时区名称生成 **ZoneId。**

## <a name="display-the-results"></a>显示结果

1. 在 `import` **App.java** 中添加以下语句。

    ```java
    import java.time.DayOfWeek;
    import java.time.LocalDateTime;
    import java.time.ZoneId;
    import java.time.ZonedDateTime;
    import java.time.format.DateTimeFormatter;
    import java.time.format.DateTimeParseException;
    import java.time.format.FormatStyle;
    import java.time.temporal.ChronoUnit;
    import java.time.temporal.TemporalAdjusters;
    import java.util.HashSet;
    import java.util.List;
    import com.microsoft.graph.models.extensions.DateTimeTimeZone;
    import com.microsoft.graph.models.extensions.Event;
    ```

1. 将以下函数添加到类中，将 `App` Microsoft Graph [中的 dateTimeTimeZone](/graph/api/resources/datetimetimezone?view=graph-rest-1.0) 属性格式化为用户友好格式。

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/App.java" id="FormatDateSnippet":::

1. 将以下函数添加到 `App` 类，获取用户的事件，并输出到控制台。

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/App.java" id="ListEventsSnippet":::

1. 在函数中的注释 `// List the calendar` 后添加 `main` 以下内容。

    ```java
    listCalendarEvents(accessToken);
    ```

1. 保存所有更改，生成应用，然后运行它。 选择 **"列表日历事件** "选项以查看用户事件的列表。

    ```Shell
    Welcome Adele Vance

    Please choose one of the following options:
    0. Exit
    1. Display access token
    2. View this week's calendar
    3. Add an event
    2
    Events:
    Subject: Weekly meeting
      Organizer: Lynne Robbins
      Start: 12/7/20, 2:00 PM (Pacific Standard Time)
      End: 12/7/20, 3:00 PM (Pacific Standard Time)
    Subject: Carpool
      Organizer: Lynne Robbins
      Start: 12/7/20, 4:00 PM (Pacific Standard Time)
      End: 12/7/20, 5:30 PM (Pacific Standard Time)
    Subject: Tailspin Toys Proposal Review + Lunch
      Organizer: Lidia Holloway
      Start: 12/8/20, 12:00 PM (Pacific Standard Time)
      End: 12/8/20, 1:00 PM (Pacific Standard Time)
    Subject: Project Tailspin
      Organizer: Lidia Holloway
      Start: 12/8/20, 3:00 PM (Pacific Standard Time)
      End: 12/8/20, 4:30 PM (Pacific Standard Time)
    Subject: Company Meeting
      Organizer: Christie Cline
      Start: 12/9/20, 8:30 AM (Pacific Standard Time)
      End: 12/9/20, 11:00 AM (Pacific Standard Time)
    Subject: Carpool
      Organizer: Lynne Robbins
      Start: 12/9/20, 4:00 PM (Pacific Standard Time)
      End: 12/9/20, 5:30 PM (Pacific Standard Time)
    Subject: Project Team Meeting
      Organizer: Lidia Holloway
      Start: 12/10/20, 8:00 AM (Pacific Standard Time)
      End: 12/10/20, 9:30 AM (Pacific Standard Time)
    Subject: Weekly Marketing Lunch
      Organizer: Adele Vance
      Start: 12/10/20, 12:00 PM (Pacific Standard Time)
      End: 12/10/20, 1:00 PM (Pacific Standard Time)
    Subject: Project Tailspin
      Organizer: Lidia Holloway
      Start: 12/10/20, 3:00 PM (Pacific Standard Time)
      End: 12/10/20, 4:30 PM (Pacific Standard Time)
    Subject: Lunch?
      Organizer: Lynne Robbins
      Start: 12/11/20, 12:00 PM (Pacific Standard Time)
      End: 12/11/20, 1:00 PM (Pacific Standard Time)
    Subject: Friday Unwinder
      Organizer: Megan Bowen
      Start: 12/11/20, 4:00 PM (Pacific Standard Time)
      End: 12/11/20, 5:00 PM (Pacific Standard Time)
    ```
