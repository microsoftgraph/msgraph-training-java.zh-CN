---
ms.openlocfilehash: 3fb56d613dbaa56474c9af58ebdd0b157c53bf1a
ms.sourcegitcommit: 2af94da662c454e765b32edeb9406812e3732406
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/13/2019
ms.locfileid: "40018793"
---
<!-- markdownlint-disable MD002 MD041 -->

在本练习中，将把 Microsoft Graph 合并到应用程序中。 对于此应用程序，您将使用[适用于 Java 的 Microsoft GRAPH SDK](https://github.com/microsoftgraph/msgraph-sdk-java)调用 microsoft graph。

## <a name="implement-an-authentication-provider"></a>实现身份验证提供程序

Microsoft Graph SDK for Java 需要`IAuthenticationProvider`接口的实现来实例化其`GraphServiceClient`对象。 首先创建一个简单的类，将访问令牌添加到传出请求。 在名为**SimpleAuthProvider**的 **/graphtutorial/src/main/java/com/contoso**目录中创建一个新文件，并添加以下代码。

```java
package com.contoso;

import com.microsoft.graph.authentication.IAuthenticationProvider;
import com.microsoft.graph.http.IHttpRequest;

/**
 * SimpleAuthProvider
 */
public class SimpleAuthProvider implements IAuthenticationProvider {

    private String accessToken = null;

    public SimpleAuthProvider(String accessToken) {
        this.accessToken = accessToken;
    }

    @Override
    public void authenticateRequest(IHttpRequest request) {
        // Add the access token in the Authorization header
        request.addHeader("Authorization", "Bearer " + accessToken);
    }
}
```

## <a name="get-user-details"></a>获取用户详细信息

首先，添加一个新类以包含所有图形功能。 在 **/graphtutorial/src/main/java/com/contoso**目录中创建一个名为**Graph**的新文件，并添加以下代码。

```java
package com.contoso;

import com.microsoft.graph.logger.DefaultLogger;
import com.microsoft.graph.logger.LoggerLevel;
import com.microsoft.graph.models.extensions.IGraphServiceClient;
import com.microsoft.graph.models.extensions.User;
import com.microsoft.graph.requests.extensions.GraphServiceClient;
import java.util.LinkedList;
import java.util.List;import com.microsoft.graph.models.extensions.Event;import com.microsoft.graph.options.Option;
import com.microsoft.graph.options.QueryOption;
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

在**应用程序**中，将以下代码添加到`Scanner input = new Scanner(System.in);`行之前，以获取用户并输出用户的显示名称。

```java
// Greet the user
User user = Graph.getUser(accessToken);
System.out.println("Welcome " + user.displayName);
System.out.println();
```

如果现在运行应用程序，则在登录后，应用程序会欢迎你使用名称。

## <a name="get-calendar-events-from-outlook"></a>从 Outlook 获取日历事件

将以下`import`语句添加到**Graph**中。

```java
import java.util.LinkedList;
import java.util.List;
import com.microsoft.graph.models.extensions.Event;
import com.microsoft.graph.options.Option;
import com.microsoft.graph.options.QueryOption;
import com.microsoft.graph.requests.extensions.IEventCollectionPage;
```

将以下函数添加到`Graph` **Graph**中的类，以从用户的日历中获取事件。

```java
public static List<Event> getEvents(String accessToken) {
    ensureGraphClient(accessToken);

    // Use QueryOption to specify the $orderby query parameter
    final List<Option> options = new LinkedList<Option>();
    // Sort results by createdDateTime, get newest first
    options.add(new QueryOption("orderby", "createdDateTime DESC"));

    // GET /me/events
    IEventCollectionPage eventPage = graphClient
        .me()
        .events()
        .buildRequest(options)
        .select("subject,organizer,start,end")
        .get();

    return eventPage.getCurrentPage();
}
```

考虑此代码执行的操作。

- 将调用的 URL 为`/me/events`。
- `select`函数将为每个事件返回的字段限制为仅应用程序实际使用的字段。
- `QueryOption`用于按创建日期和时间对结果进行排序，最新项目最先开始。

## <a name="display-the-results"></a>显示结果

首先在 .java 中添加`import`以下语句****。

```java
import java.time.LocalDateTime;
import java.time.format.DateTimeFormatter;
import java.time.format.FormatStyle;
import java.util.List;
```

然后，将以下函数添加到`App`类中，将 Microsoft Graph 中的[datetimetimezone type](/graph/api/resources/datetimetimezone?view=graph-rest-1.0)属性设置为用户友好的格式。

```java
private static String formatDateTimeTimeZone(DateTimeTimeZone date) {
    LocalDateTime dateTime = LocalDateTime.parse(date.dateTime);

    return dateTime.format(DateTimeFormatter.ofLocalizedDateTime(FormatStyle.SHORT)) + " (" + date.timeZone + ")";
}
```

接下来，将以下函数添加到`App`类，以获取用户的事件并将其输出到控制台。

```java
private static void listCalendarEvents(String accessToken) {
    // Get the user's events
    List<Event> events = Graph.getEvents(accessToken);

    System.out.println("Events:");

    for (Event event : events) {
        System.out.println("Subject: " + event.subject);
        System.out.println("  Organizer: " + event.organizer.emailAddress.name);
        System.out.println("  Start: " + formatDateTimeTimeZone(event.start));
        System.out.println("  End: " + formatDateTimeTimeZone(event.end));
    }

    System.out.println();
}
```

最后，在`// List the calendar` `main`函数中的注释后面添加以下项。

```java
listCalendarEvents(accessToken);
```

保存所有更改并运行应用程序。 选择 "**列出日历事件**" 选项以查看用户事件的列表。

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
