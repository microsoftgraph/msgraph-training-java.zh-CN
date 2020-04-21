---
ms.openlocfilehash: 93688a97872ad640c12c7137f4cc09ede4a98416
ms.sourcegitcommit: 189f87d879c57b11992e7bc75580b4c69e014122
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/21/2020
ms.locfileid: "43612066"
---
<!-- markdownlint-disable MD002 MD041 -->

在本练习中，将把 Microsoft Graph 合并到应用程序中。 对于此应用程序，您将使用[适用于 Java 的 Microsoft GRAPH SDK](https://github.com/microsoftgraph/msgraph-sdk-java)调用 microsoft graph。

## <a name="implement-an-authentication-provider"></a>实现身份验证提供程序

Microsoft Graph SDK for Java 需要`IAuthenticationProvider`接口的实现来实例化其`GraphServiceClient`对象。

1. 在名为**SimpleAuthProvider**的 **/graphtutorial/src/main/java/graphtutorial**目录中创建一个新文件，并添加以下代码。

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/SimpleAuthProvider.java" id="AuthProviderSnippet":::

## <a name="get-user-details"></a>获取用户详细信息

1. 在 **/graphtutorial/src/main/java/graphtutorial**目录中创建一个名为**Graph**的新文件，并添加以下代码。

    ```java
    package graphtutorial;

    import java.util.LinkedList;
    import java.util.List;

    import com.microsoft.graph.logger.DefaultLogger;
    import com.microsoft.graph.logger.LoggerLevel;
    import com.microsoft.graph.models.extensions.Event;
    import com.microsoft.graph.models.extensions.IGraphServiceClient;
    import com.microsoft.graph.models.extensions.User;
    import com.microsoft.graph.options.Option;
    import com.microsoft.graph.options.QueryOption;
    import com.microsoft.graph.requests.extensions.GraphServiceClient;
    import com.microsoft.graph.requests.extensions.IEventCollectionPage;

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
                .get();

            return me;
        }
    }
    ```

1. 在`import` **应用程序**的顶部添加以下语句。

    ```java
    import com.microsoft.graph.models.extensions.User;
    ```

1. 在**应用程序**中，将以下代码添加到`Scanner input = new Scanner(System.in);`行之前，以获取用户并输出用户的显示名称。

    ```java
    // Greet the user
    User user = Graph.getUser(accessToken);
    System.out.println("Welcome " + user.displayName);
    System.out.println();
    ```

1. 运行应用程序。 登录后，应用程序会欢迎你使用名称。

## <a name="get-calendar-events-from-outlook"></a>从 Outlook 获取日历事件

1. 将以下函数添加到`Graph` **Graph**中的类，以从用户的日历中获取事件。

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/Graph.java" id="GetEventsSnippet":::

考虑此代码执行的操作。

- 将调用的 URL 为`/me/events`。
- `select`函数将为每个事件返回的字段限制为仅应用程序实际使用的字段。
- `QueryOption`用于按创建日期和时间对结果进行排序，最新项目最先开始。

## <a name="display-the-results"></a>显示结果

1. 在 .java 中`import`添加以下**App.java**语句。

    ```java
    import java.time.LocalDateTime;
    import java.time.format.DateTimeFormatter;
    import java.time.format.FormatStyle;
    import java.util.List;
    import com.microsoft.graph.models.extensions.DateTimeTimeZone;
    import com.microsoft.graph.models.extensions.Event;
    ```

1. 将以下函数添加到`App`类，以将 Microsoft Graph 中的[datetimetimezone type](/graph/api/resources/datetimetimezone?view=graph-rest-1.0)属性设置为用户友好的格式。

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/App.java" id="FormatDateSnippet":::

1. 将以下函数添加到`App`类，以获取用户的事件并将其输出到控制台。

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/App.java" id="ListEventsSnippet":::

1. 在`// List the calendar` `main`函数中的注释后面添加以下项。

    ```java
    listCalendarEvents(accessToken);
    ```

1. 保存所有更改，生成应用程序，然后运行它。 选择 "**列出日历事件**" 选项以查看用户事件的列表。

    ```Shell
    Welcome Adele Vance

    Please choose one of the following options:
    0. Exit
    1. Display access token
    2. List calendar events
    2
    Events:
    Subject: Team meeting
      Organizer: Adele Vance
      Start: 5/22/19, 3:00 PM (UTC)
      End: 5/22/19, 4:00 PM (UTC)
    Subject: Team Lunch
      Organizer: Adele Vance
      Start: 5/24/19, 6:30 PM (UTC)
      End: 5/24/19, 8:00 PM (UTC)
    Subject: Flight to Redmond
      Organizer: Adele Vance
      Start: 5/26/19, 4:30 PM (UTC)
      End: 5/26/19, 7:00 PM (UTC)
    Subject: Let's meet to discuss strategy
      Organizer: Patti Fernandez
      Start: 5/27/19, 10:00 PM (UTC)
      End: 5/27/19, 10:30 PM (UTC)
    Subject: All-hands meeting
      Organizer: Adele Vance
      Start: 5/28/19, 3:30 PM (UTC)
      End: 5/28/19, 5:00 PM (UTC)
    ```
