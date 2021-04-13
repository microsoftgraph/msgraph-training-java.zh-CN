---
ms.openlocfilehash: 397d564fc3389f341e06977bd4cba861dd1c9e5b
ms.sourcegitcommit: 5c09eff01b265ddfcca9090c14dca80a95320edd
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/13/2021
ms.locfileid: "51695805"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="5924d-101">在此练习中，你将 Microsoft Graph 合并到应用程序中。</span><span class="sxs-lookup"><span data-stu-id="5924d-101">In this exercise you will incorporate the Microsoft Graph into the application.</span></span> <span data-ttu-id="5924d-102">对于此应用程序，你将使用 Microsoft [Graph SDK for Java](https://github.com/microsoftgraph/msgraph-sdk-java) 调用 Microsoft Graph。</span><span class="sxs-lookup"><span data-stu-id="5924d-102">For this application, you will use the [Microsoft Graph SDK for Java](https://github.com/microsoftgraph/msgraph-sdk-java) to make calls to Microsoft Graph.</span></span>

## <a name="get-user-details"></a><span data-ttu-id="5924d-103">获取用户详细信息</span><span class="sxs-lookup"><span data-stu-id="5924d-103">Get user details</span></span>

1. <span data-ttu-id="5924d-104">在名为 **Graph.java** 的 **./graphtutorial/src/main/java/graphtutorial** 目录中创建新文件并添加以下代码。</span><span class="sxs-lookup"><span data-stu-id="5924d-104">Create a new file in the **./graphtutorial/src/main/java/graphtutorial** directory named **Graph.java** and add the following code.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/Graph.java" id="GetUserSnippet":::

1. <span data-ttu-id="5924d-105">在 `import` **App.java** 的顶部添加以下语句。</span><span class="sxs-lookup"><span data-stu-id="5924d-105">Add the following `import` statement at the top of **App.java**.</span></span>

    ```java
    import com.microsoft.graph.models.User;
    ```

1. <span data-ttu-id="5924d-106">在 **App.java 的** 行前添加以下代码，获取用户并输出 `Scanner input = new Scanner(System.in);` 用户显示名称。</span><span class="sxs-lookup"><span data-stu-id="5924d-106">Add the following code in **App.java** just before the `Scanner input = new Scanner(System.in);` line to get the user and output the user's display name.</span></span>

    ```java
    // Greet the user
    User user = Graph.getUser();
    System.out.println("Welcome " + user.displayName);
    System.out.println("Time zone: " + user.mailboxSettings.timeZone);
    System.out.println();
    ```

1. <span data-ttu-id="5924d-107">运行应用程序。</span><span class="sxs-lookup"><span data-stu-id="5924d-107">Run the app.</span></span> <span data-ttu-id="5924d-108">登录应用后，欢迎你按名称登录。</span><span class="sxs-lookup"><span data-stu-id="5924d-108">After you log in the app welcomes you by name.</span></span>

## <a name="get-calendar-events-from-outlook"></a><span data-ttu-id="5924d-109">从 Outlook 获取日历事件</span><span class="sxs-lookup"><span data-stu-id="5924d-109">Get calendar events from Outlook</span></span>

1. <span data-ttu-id="5924d-110">将以下函数添加到 `Graph` **Graph.java** 中的 类，获取用户日历中的事件。</span><span class="sxs-lookup"><span data-stu-id="5924d-110">Add the following function to the `Graph` class in **Graph.java** to get events from the user's calendar.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/Graph.java" id="GetEventsSnippet":::

<span data-ttu-id="5924d-111">考虑此代码将执行什么工作。</span><span class="sxs-lookup"><span data-stu-id="5924d-111">Consider what this code is doing.</span></span>

- <span data-ttu-id="5924d-112">将调用的 URL 为 `/me/calendarview`。</span><span class="sxs-lookup"><span data-stu-id="5924d-112">The URL that will be called is `/me/calendarview`.</span></span>
  - <span data-ttu-id="5924d-113">`QueryOption` 对象用于添加 和 `startDateTime` `endDateTime` 参数，并设置日历视图的起始和结束。</span><span class="sxs-lookup"><span data-stu-id="5924d-113">`QueryOption` objects are used to add the `startDateTime` and `endDateTime` parameters, setting the start and end of the calendar view.</span></span>
  - <span data-ttu-id="5924d-114">`QueryOption`对象用于添加参数 `$orderby` ，按开始时间对结果进行排序。</span><span class="sxs-lookup"><span data-stu-id="5924d-114">A `QueryOption` object is used to add the `$orderby` parameter, sorting the results by start time.</span></span>
  - <span data-ttu-id="5924d-115">对象用于添加标头，从而导致根据用户的时区调整开始时间和 `HeaderOption` `Prefer: outlook.timezone` 结束时间。</span><span class="sxs-lookup"><span data-stu-id="5924d-115">A `HeaderOption` object is used to add the `Prefer: outlook.timezone` header, causing the start and end times to be adjusted to the user's time zone.</span></span>
  - <span data-ttu-id="5924d-116">`select`函数将每个事件返回的字段限定为应用将实际使用的字段。</span><span class="sxs-lookup"><span data-stu-id="5924d-116">The `select` function limits the fields returned for each event to just those the app will actually use.</span></span>
  - <span data-ttu-id="5924d-117">`top`函数将响应中的事件数最多限制为 25 个。</span><span class="sxs-lookup"><span data-stu-id="5924d-117">The `top` function limits the number of events in the response to a maximum of 25.</span></span>
- <span data-ttu-id="5924d-118">如果本周的事件超过 25 个，则函数用于请求其他 `getNextPage` 结果页面。</span><span class="sxs-lookup"><span data-stu-id="5924d-118">The `getNextPage` function is used to request additional pages of results if there are more than 25 events in the current week.</span></span>

1. <span data-ttu-id="5924d-119">在名为 **GraphToIana.java** 的 **./graphtutorial/src/main/java/graphtutorial** 目录中创建新文件并添加以下代码。</span><span class="sxs-lookup"><span data-stu-id="5924d-119">Create a new file in the **./graphtutorial/src/main/java/graphtutorial** directory named **GraphToIana.java** and add the following code.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/GraphToIana.java" id="zoneMappingsSnippet":::

    <span data-ttu-id="5924d-120">此类实现一个简单的查找以将 Windows 时区名称转换为 IANA 标识符，并基于 Windows 时区名称生成 **ZoneId。**</span><span class="sxs-lookup"><span data-stu-id="5924d-120">This class implements a simple lookup to convert Windows time zone names to IANA identifiers, and to generate a **ZoneId** based on a Windows time zone name.</span></span>

## <a name="display-the-results"></a><span data-ttu-id="5924d-121">显示结果</span><span class="sxs-lookup"><span data-stu-id="5924d-121">Display the results</span></span>

1. <span data-ttu-id="5924d-122">在 `import` **App.java** 中添加以下语句。</span><span class="sxs-lookup"><span data-stu-id="5924d-122">Add the following `import` statements in **App.java**.</span></span>

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
    import com.microsoft.graph.models.DateTimeTimeZone;
    import com.microsoft.graph.models.Event;
    ```

1. <span data-ttu-id="5924d-123">将以下函数添加到 类，以 `App` 将 Microsoft Graph 中的 [dateTimeTimeZone](/graph/api/resources/datetimetimezone?view=graph-rest-1.0) 属性格式化为用户友好格式。</span><span class="sxs-lookup"><span data-stu-id="5924d-123">Add the following function to the `App` class to format the [dateTimeTimeZone](/graph/api/resources/datetimetimezone?view=graph-rest-1.0) properties from Microsoft Graph into a user-friendly format.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/App.java" id="FormatDateSnippet":::

1. <span data-ttu-id="5924d-124">将以下函数 `App` 添加到 类，获取用户的事件，并输出到控制台。</span><span class="sxs-lookup"><span data-stu-id="5924d-124">Add the following function to the `App` class to get the user's events and output them to the console.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/App.java" id="ListEventsSnippet":::

1. <span data-ttu-id="5924d-125">在 函数中的注释 `// List the calendar` 后添加 `main` 以下内容。</span><span class="sxs-lookup"><span data-stu-id="5924d-125">Add the following just after the `// List the calendar` comment in the `main` function.</span></span>

    ```java
    listCalendarEvents(user.mailboxSettings.timeZone);
    ```

1. <span data-ttu-id="5924d-126">保存所有更改，生成应用，然后运行它。</span><span class="sxs-lookup"><span data-stu-id="5924d-126">Save all of your changes, build the app, then run it.</span></span> <span data-ttu-id="5924d-127">选择 **"列表日历事件** "选项以查看用户事件的列表。</span><span class="sxs-lookup"><span data-stu-id="5924d-127">Choose the **List calendar events** option to see a list of the user's events.</span></span>

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
