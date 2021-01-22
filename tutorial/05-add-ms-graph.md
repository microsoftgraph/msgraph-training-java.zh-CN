---
ms.openlocfilehash: 16e96edc78ed2f6955bc14654edba1cb26323648
ms.sourcegitcommit: 2c0e0d2d6de994022dfa0faa10131582fb10e9b1
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/21/2021
ms.locfileid: "49919527"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="fdd03-101">在此练习中，你将 Microsoft Graph 合并到应用程序中。</span><span class="sxs-lookup"><span data-stu-id="fdd03-101">In this exercise you will incorporate the Microsoft Graph into the application.</span></span> <span data-ttu-id="fdd03-102">对于此应用程序，你将使用 Microsoft [Graph SDK for Java](https://github.com/microsoftgraph/msgraph-sdk-java) 调用 Microsoft Graph。</span><span class="sxs-lookup"><span data-stu-id="fdd03-102">For this application, you will use the [Microsoft Graph SDK for Java](https://github.com/microsoftgraph/msgraph-sdk-java) to make calls to Microsoft Graph.</span></span>

## <a name="implement-an-authentication-provider"></a><span data-ttu-id="fdd03-103">实现身份验证提供程序</span><span class="sxs-lookup"><span data-stu-id="fdd03-103">Implement an authentication provider</span></span>

<span data-ttu-id="fdd03-104">Microsoft Graph SDK for Java需要实现 `IAuthenticationProvider` 接口来实例化其 `GraphServiceClient` 对象。</span><span class="sxs-lookup"><span data-stu-id="fdd03-104">The Microsoft Graph SDK for Java requires an implementation of the `IAuthenticationProvider` interface to instantiate its `GraphServiceClient` object.</span></span>

1. <span data-ttu-id="fdd03-105">在名为 **SimpleAuthProvider.java** 的 **./graphtu一l/src/main/java/graphtu上l** 目录中创建新文件，并添加以下代码。</span><span class="sxs-lookup"><span data-stu-id="fdd03-105">Create a new file in the **./graphtutorial/src/main/java/graphtutorial** directory named **SimpleAuthProvider.java** and add the following code.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/SimpleAuthProvider.java" id="AuthProviderSnippet":::

## <a name="get-user-details"></a><span data-ttu-id="fdd03-106">获取用户详细信息</span><span class="sxs-lookup"><span data-stu-id="fdd03-106">Get user details</span></span>

1. <span data-ttu-id="fdd03-107">在名为 **Graph.java** 的 **./graphtu一l/src/main/java/graphtu加载项目录中** 创建新文件，并添加以下代码。</span><span class="sxs-lookup"><span data-stu-id="fdd03-107">Create a new file in the **./graphtutorial/src/main/java/graphtutorial** directory named **Graph.java** and add the following code.</span></span>

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

1. <span data-ttu-id="fdd03-108">在 `import` **App.java** 顶部添加以下语句。</span><span class="sxs-lookup"><span data-stu-id="fdd03-108">Add the following `import` statement at the top of **App.java**.</span></span>

    ```java
    import com.microsoft.graph.models.extensions.User;
    ```

1. <span data-ttu-id="fdd03-109">在 **App.java** 中，将以下代码添加到该行的正前，以获取用户并输出 `Scanner input = new Scanner(System.in);` 用户显示名称。</span><span class="sxs-lookup"><span data-stu-id="fdd03-109">Add the following code in **App.java** just before the `Scanner input = new Scanner(System.in);` line to get the user and output the user's display name.</span></span>

    ```java
    // Greet the user
    User user = Graph.getUser(accessToken);
    System.out.println("Welcome " + user.displayName);
    System.out.println("Time zone: " + user.mailboxSettings.timeZone);
    System.out.println();
    ```

1. <span data-ttu-id="fdd03-110">运行应用程序。</span><span class="sxs-lookup"><span data-stu-id="fdd03-110">Run the app.</span></span> <span data-ttu-id="fdd03-111">登录应用后，按名称欢迎使用。</span><span class="sxs-lookup"><span data-stu-id="fdd03-111">After you log in the app welcomes you by name.</span></span>

## <a name="get-calendar-events-from-outlook"></a><span data-ttu-id="fdd03-112">从 Outlook 获取日历事件</span><span class="sxs-lookup"><span data-stu-id="fdd03-112">Get calendar events from Outlook</span></span>

1. <span data-ttu-id="fdd03-113">将以下函数添加到 `Graph` **Graph.java** 中的类，以从用户日历获取事件。</span><span class="sxs-lookup"><span data-stu-id="fdd03-113">Add the following function to the `Graph` class in **Graph.java** to get events from the user's calendar.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/Graph.java" id="GetEventsSnippet":::

<span data-ttu-id="fdd03-114">考虑此代码正在做什么。</span><span class="sxs-lookup"><span data-stu-id="fdd03-114">Consider what this code is doing.</span></span>

- <span data-ttu-id="fdd03-115">将调用的 URL 为 `/me/calendarview` 。</span><span class="sxs-lookup"><span data-stu-id="fdd03-115">The URL that will be called is `/me/calendarview`.</span></span>
  - <span data-ttu-id="fdd03-116">`QueryOption` 对象用于添加 `startDateTime` 和 `endDateTime` 参数，并设置日历视图的开始和结束。</span><span class="sxs-lookup"><span data-stu-id="fdd03-116">`QueryOption` objects are used to add the `startDateTime` and `endDateTime` parameters, setting the start and end of the calendar view.</span></span>
  - <span data-ttu-id="fdd03-117">`QueryOption`对象用于添加参数 `$orderby` ，按开始时间对结果进行排序。</span><span class="sxs-lookup"><span data-stu-id="fdd03-117">A `QueryOption` object is used to add the `$orderby` parameter, sorting the results by start time.</span></span>
  - <span data-ttu-id="fdd03-118">对象用于添加标头，从而导致根据用户的时区调整开始时间和 `HeaderOption` `Prefer: outlook.timezone` 结束时间。</span><span class="sxs-lookup"><span data-stu-id="fdd03-118">A `HeaderOption` object is used to add the `Prefer: outlook.timezone` header, causing the start and end times to be adjusted to the user's time zone.</span></span>
  - <span data-ttu-id="fdd03-119">`select`该函数将每个事件返回的字段限制为仅应用将实际使用的字段。</span><span class="sxs-lookup"><span data-stu-id="fdd03-119">The `select` function limits the fields returned for each event to just those the app will actually use.</span></span>
  - <span data-ttu-id="fdd03-120">`top`该函数将响应中的事件数限制为最多 25 个。</span><span class="sxs-lookup"><span data-stu-id="fdd03-120">The `top` function limits the number of events in the response to a maximum of 25.</span></span>
- <span data-ttu-id="fdd03-121">如果当前一周的事件超过 25 个，则此函数用于请求其他 `getNextPage` 结果页。</span><span class="sxs-lookup"><span data-stu-id="fdd03-121">The `getNextPage` function is used to request additional pages of results if there are more than 25 events in the current week.</span></span>

1. <span data-ttu-id="fdd03-122">在名为 **GraphToIana.java** 的 **./graphtu一l/src/main/java/graphtu加载项目录中** 创建新文件，并添加以下代码。</span><span class="sxs-lookup"><span data-stu-id="fdd03-122">Create a new file in the **./graphtutorial/src/main/java/graphtutorial** directory named **GraphToIana.java** and add the following code.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/GraphToIana.java" id="zoneMappingsSnippet":::

    <span data-ttu-id="fdd03-123">此类实现一个简单的查找以将 Windows 时区名称转换为 IANA 标识符，并基于 Windows 时区名称生成 **ZoneId。**</span><span class="sxs-lookup"><span data-stu-id="fdd03-123">This class implements a simple lookup to convert Windows time zone names to IANA identifiers, and to generate a **ZoneId** based on a Windows time zone name.</span></span>

## <a name="display-the-results"></a><span data-ttu-id="fdd03-124">显示结果</span><span class="sxs-lookup"><span data-stu-id="fdd03-124">Display the results</span></span>

1. <span data-ttu-id="fdd03-125">在 `import` **App.java** 中添加以下语句。</span><span class="sxs-lookup"><span data-stu-id="fdd03-125">Add the following `import` statements in **App.java**.</span></span>

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

1. <span data-ttu-id="fdd03-126">将以下函数添加到类中，将 `App` Microsoft Graph [中的 dateTimeTimeZone](/graph/api/resources/datetimetimezone?view=graph-rest-1.0) 属性格式化为用户友好格式。</span><span class="sxs-lookup"><span data-stu-id="fdd03-126">Add the following function to the `App` class to format the [dateTimeTimeZone](/graph/api/resources/datetimetimezone?view=graph-rest-1.0) properties from Microsoft Graph into a user-friendly format.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/App.java" id="FormatDateSnippet":::

1. <span data-ttu-id="fdd03-127">将以下函数 `App` 添加到类，获取用户的事件，并输出到控制台。</span><span class="sxs-lookup"><span data-stu-id="fdd03-127">Add the following function to the `App` class to get the user's events and output them to the console.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/App.java" id="ListEventsSnippet":::

1. <span data-ttu-id="fdd03-128">在函数中的注释 `// List the calendar` 后添加 `main` 以下内容。</span><span class="sxs-lookup"><span data-stu-id="fdd03-128">Add the following just after the `// List the calendar` comment in the `main` function.</span></span>

    ```java
    listCalendarEvents(accessToken, user.mailboxSettings.timeZone);
    ```

1. <span data-ttu-id="fdd03-129">保存所有更改，生成应用，然后运行它。</span><span class="sxs-lookup"><span data-stu-id="fdd03-129">Save all of your changes, build the app, then run it.</span></span> <span data-ttu-id="fdd03-130">选择 **"列表日历事件** "选项以查看用户事件的列表。</span><span class="sxs-lookup"><span data-stu-id="fdd03-130">Choose the **List calendar events** option to see a list of the user's events.</span></span>

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
