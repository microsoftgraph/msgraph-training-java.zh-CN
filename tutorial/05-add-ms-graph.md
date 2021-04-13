---
ms.openlocfilehash: 397d564fc3389f341e06977bd4cba861dd1c9e5b
ms.sourcegitcommit: 5c09eff01b265ddfcca9090c14dca80a95320edd
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/13/2021
ms.locfileid: "51695805"
---
<!-- markdownlint-disable MD002 MD041 -->

在此练习中，你将 Microsoft Graph 合并到应用程序中。 对于此应用程序，你将使用 Microsoft [Graph SDK for Java](https://github.com/microsoftgraph/msgraph-sdk-java) 调用 Microsoft Graph。

## <a name="get-user-details"></a>获取用户详细信息

1. 在名为 **Graph.java** 的 **./graphtutorial/src/main/java/graphtutorial** 目录中创建新文件并添加以下代码。

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/Graph.java" id="GetUserSnippet":::

1. 在 `import` **App.java** 的顶部添加以下语句。

    ```java
    import com.microsoft.graph.models.User;
    ```

1. 在 **App.java 的** 行前添加以下代码，获取用户并输出 `Scanner input = new Scanner(System.in);` 用户显示名称。

    ```java
    // Greet the user
    User user = Graph.getUser();
    System.out.println("Welcome " + user.displayName);
    System.out.println("Time zone: " + user.mailboxSettings.timeZone);
    System.out.println();
    ```

1. 运行应用程序。 登录应用后，欢迎你按名称登录。

## <a name="get-calendar-events-from-outlook"></a>从 Outlook 获取日历事件

1. 将以下函数添加到 `Graph` **Graph.java** 中的 类，获取用户日历中的事件。

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/Graph.java" id="GetEventsSnippet":::

考虑此代码将执行什么工作。

- 将调用的 URL 为 `/me/calendarview`。
  - `QueryOption` 对象用于添加 和 `startDateTime` `endDateTime` 参数，并设置日历视图的起始和结束。
  - `QueryOption`对象用于添加参数 `$orderby` ，按开始时间对结果进行排序。
  - 对象用于添加标头，从而导致根据用户的时区调整开始时间和 `HeaderOption` `Prefer: outlook.timezone` 结束时间。
  - `select`函数将每个事件返回的字段限定为应用将实际使用的字段。
  - `top`函数将响应中的事件数最多限制为 25 个。
- 如果本周的事件超过 25 个，则函数用于请求其他 `getNextPage` 结果页面。

1. 在名为 **GraphToIana.java** 的 **./graphtutorial/src/main/java/graphtutorial** 目录中创建新文件并添加以下代码。

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
    import com.microsoft.graph.models.DateTimeTimeZone;
    import com.microsoft.graph.models.Event;
    ```

1. 将以下函数添加到 类，以 `App` 将 Microsoft Graph 中的 [dateTimeTimeZone](/graph/api/resources/datetimetimezone?view=graph-rest-1.0) 属性格式化为用户友好格式。

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/App.java" id="FormatDateSnippet":::

1. 将以下函数 `App` 添加到 类，获取用户的事件，并输出到控制台。

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/App.java" id="ListEventsSnippet":::

1. 在 函数中的注释 `// List the calendar` 后添加 `main` 以下内容。

    ```java
    listCalendarEvents(user.mailboxSettings.timeZone);
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
