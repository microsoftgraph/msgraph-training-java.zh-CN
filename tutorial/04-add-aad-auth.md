---
ms.openlocfilehash: 5e5168a6ccaf1374bc720f38a71a72d80d9d2b32
ms.sourcegitcommit: 5c09eff01b265ddfcca9090c14dca80a95320edd
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/13/2021
ms.locfileid: "51695791"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="9bc85-101">在此练习中，你将从上一练习中扩展应用程序，以支持使用 Azure AD 进行身份验证。</span><span class="sxs-lookup"><span data-stu-id="9bc85-101">In this exercise you will extend the application from the previous exercise to support authentication with Azure AD.</span></span> <span data-ttu-id="9bc85-102">这是获取调用 Microsoft Graph 所需的 OAuth 访问令牌所必需的。</span><span class="sxs-lookup"><span data-stu-id="9bc85-102">This is required to obtain the necessary OAuth access token to call the Microsoft Graph.</span></span> <span data-ttu-id="9bc85-103">在此步骤中，将 [Microsoft 身份验证库 (MSAL) ，Java](https://github.com/AzureAD/microsoft-authentication-library-for-java) 应用程序。</span><span class="sxs-lookup"><span data-stu-id="9bc85-103">In this step you will integrate the [Microsoft Authentication Library (MSAL) for Java](https://github.com/AzureAD/microsoft-authentication-library-for-java) into the application.</span></span>

1. <span data-ttu-id="9bc85-104">在 **./src/main/resources** 目录中创建一个名为 **graphtu一l** 的新目录。</span><span class="sxs-lookup"><span data-stu-id="9bc85-104">Create a new directory named **graphtutorial** in the **./src/main/resources** directory.</span></span>

1. <span data-ttu-id="9bc85-105">在 **./src/main/resources/graphtu一l** 目录中创建一个名为 **oAuth.properties** 的新文件，并添加该文件中的以下文本。</span><span class="sxs-lookup"><span data-stu-id="9bc85-105">Create a new file in the **./src/main/resources/graphtutorial** directory named **oAuth.properties**, and add the following text in that file.</span></span> <span data-ttu-id="9bc85-106">将 `YOUR_APP_ID_HERE` 替换为你在 Azure 门户中创建的应用程序 ID。</span><span class="sxs-lookup"><span data-stu-id="9bc85-106">Replace `YOUR_APP_ID_HERE` with the application ID you created in the Azure portal.</span></span>

    :::code language="ini" source="../demo/graphtutorial/src/main/resources/graphtutorial/oAuth.properties.example":::

    <span data-ttu-id="9bc85-107">的值 `app.scopes` 包含应用程序所需的权限范围。</span><span class="sxs-lookup"><span data-stu-id="9bc85-107">The value of `app.scopes` contains the permission scopes the application requires.</span></span>

    - <span data-ttu-id="9bc85-108">**User.Read** 允许应用访问用户配置文件。</span><span class="sxs-lookup"><span data-stu-id="9bc85-108">**User.Read** allows the app to access the user's profile.</span></span>
    - <span data-ttu-id="9bc85-109">**MailboxSettings.Read** 允许应用访问用户邮箱中的设置，包括用户配置的时区。</span><span class="sxs-lookup"><span data-stu-id="9bc85-109">**MailboxSettings.Read** allows the app to access settings from the user's mailbox, including the user's configured time zone.</span></span>
    - <span data-ttu-id="9bc85-110">**Calendars.ReadWrite** 允许应用列出用户的日历，以及向日历添加新事件。</span><span class="sxs-lookup"><span data-stu-id="9bc85-110">**Calendars.ReadWrite** allows the app to list the user's calendar and add new events to the calendar.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="9bc85-111">如果你使用的是源代码管理（如 git），那么现在应该从源代码管理中排除 **oAuth.properties** 文件，以避免意外泄露应用 ID。</span><span class="sxs-lookup"><span data-stu-id="9bc85-111">If you're using source control such as git, now would be a good time to exclude the **oAuth.properties** file from source control to avoid inadvertently leaking your app ID.</span></span>

1. <span data-ttu-id="9bc85-112">打开 **App.java** 并添加以下 `import` 语句。</span><span class="sxs-lookup"><span data-stu-id="9bc85-112">Open **App.java** and add the following `import` statements.</span></span>

    ```java
    import java.io.IOException;
    import java.util.Properties;
    ```

1. <span data-ttu-id="9bc85-113">在行的正前添加以下 `Scanner input = new Scanner(System.in);` 代码以加载 **oAuth.properties** 文件。</span><span class="sxs-lookup"><span data-stu-id="9bc85-113">Add the following code just before the `Scanner input = new Scanner(System.in);` line to load the **oAuth.properties** file.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/App.java" id="LoadSettingsSnippet":::

## <a name="implement-sign-in"></a><span data-ttu-id="9bc85-114">实施登录</span><span class="sxs-lookup"><span data-stu-id="9bc85-114">Implement sign-in</span></span>

1. <span data-ttu-id="9bc85-115">在名为 **Graph.java** 的 **./graphtutorial/src/main/java/graphtutorial** 目录中创建新文件并添加以下代码。</span><span class="sxs-lookup"><span data-stu-id="9bc85-115">Create a new file in the **./graphtutorial/src/main/java/graphtutorial** directory named **Graph.java** and add the following code.</span></span>

    ```java
    package graphtutorial;

    import java.net.URL;
    import java.time.LocalDateTime;
    import java.time.ZonedDateTime;
    import java.time.format.DateTimeFormatter;
    import java.util.LinkedList;
    import java.util.List;
    import java.util.Set;

    import okhttp3.Request;

    import com.azure.identity.DeviceCodeCredential;
    import com.azure.identity.DeviceCodeCredentialBuilder;

    import com.microsoft.graph.authentication.TokenCredentialAuthProvider;
    import com.microsoft.graph.logger.DefaultLogger;
    import com.microsoft.graph.logger.LoggerLevel;
    import com.microsoft.graph.models.Attendee;
    import com.microsoft.graph.models.DateTimeTimeZone;
    import com.microsoft.graph.models.EmailAddress;
    import com.microsoft.graph.models.Event;
    import com.microsoft.graph.models.ItemBody;
    import com.microsoft.graph.models.User;
    import com.microsoft.graph.models.AttendeeType;
    import com.microsoft.graph.models.BodyType;
    import com.microsoft.graph.options.HeaderOption;
    import com.microsoft.graph.options.Option;
    import com.microsoft.graph.options.QueryOption;
    import com.microsoft.graph.requests.GraphServiceClient;
    import com.microsoft.graph.requests.EventCollectionPage;
    import com.microsoft.graph.requests.EventCollectionRequestBuilder;

    public class Graph {

        private static GraphServiceClient<Request> graphClient = null;
        private static TokenCredentialAuthProvider authProvider = null;

        public static void initializeGraphAuth(String applicationId, List<String> scopes) {
            // Create the auth provider
            final DeviceCodeCredential credential = new DeviceCodeCredentialBuilder()
                .clientId(applicationId)
                .challengeConsumer(challenge -> System.out.println(challenge.getMessage()))
                .build();

            authProvider = new TokenCredentialAuthProvider(scopes, credential);

            // Create default logger to only log errors
            DefaultLogger logger = new DefaultLogger();
            logger.setLoggingLevel(LoggerLevel.ERROR);

            // Build a Graph client
            graphClient = GraphServiceClient.builder()
                .authenticationProvider(authProvider)
                .logger(logger)
                .buildClient();
        }

        public static String getUserAccessToken()
        {
            try {
                URL meUrl = new URL("https://graph.microsoft.com/v1.0/me");
                return authProvider.getAuthorizationTokenAsync(meUrl).get();
            } catch(Exception ex) {
                return null;
            }
        }
    }
    ```

1. <span data-ttu-id="9bc85-116">在 **App.java** 中，将以下代码添加到 行的正前 `Scanner input = new Scanner(System.in);` ，以获取访问令牌。</span><span class="sxs-lookup"><span data-stu-id="9bc85-116">In **App.java**, add the following code just before the `Scanner input = new Scanner(System.in);` line to get an access token.</span></span>

    ```java
    // Initialize Graph with auth settings
    Graph.initializeGraphAuth(appId, appScopes);
    final String accessToken = Graph.getUserAccessToken();
    ```

1. <span data-ttu-id="9bc85-117">在注释后添加以下 `// Display access token` 行。</span><span class="sxs-lookup"><span data-stu-id="9bc85-117">Add the following line after the `// Display access token` comment.</span></span>

    ```java
    System.out.println("Access token: " + accessToken);
    ```

1. <span data-ttu-id="9bc85-118">运行应用程序。</span><span class="sxs-lookup"><span data-stu-id="9bc85-118">Run the app.</span></span> <span data-ttu-id="9bc85-119">应用程序显示 URL 和设备代码。</span><span class="sxs-lookup"><span data-stu-id="9bc85-119">The application displays a URL and device code.</span></span>

    ```Shell
    Java Graph Tutorial

    To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code F7CG945YZ to authenticate.
    ```

1. <span data-ttu-id="9bc85-120">打开浏览器并浏览到显示的 URL。</span><span class="sxs-lookup"><span data-stu-id="9bc85-120">Open a browser and browse to the URL displayed.</span></span> <span data-ttu-id="9bc85-121">输入提供的代码并登录。</span><span class="sxs-lookup"><span data-stu-id="9bc85-121">Enter the provided code and sign in.</span></span> <span data-ttu-id="9bc85-122">完成后，返回到应用程序并选择 **1。显示访问** 令牌选项以显示访问令牌。</span><span class="sxs-lookup"><span data-stu-id="9bc85-122">Once completed, return to the application and choose the **1. Display access token** option to display the access token.</span></span>

> [!TIP]
> <span data-ttu-id="9bc85-123">可以分析 Microsoft 工作或学校帐户的访问令牌，以便进行疑难解答 [https://jwt.ms](https://jwt.ms) 。</span><span class="sxs-lookup"><span data-stu-id="9bc85-123">Access tokens for Microsoft work or school accounts can be parsed for troubleshooting purposes at [https://jwt.ms](https://jwt.ms).</span></span> <span data-ttu-id="9bc85-124">个人 Microsoft 帐户的访问令牌使用专有格式，无法进行分析。</span><span class="sxs-lookup"><span data-stu-id="9bc85-124">Access tokens for personal Microsoft accounts use a proprietary format and cannot be parsed.</span></span>
