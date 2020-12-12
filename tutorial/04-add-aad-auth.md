---
ms.openlocfilehash: 4c04317462240ff0696ac1381fae886481db7847
ms.sourcegitcommit: eb935a250f8531b04a42710356072b80d46ee3a4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/11/2020
ms.locfileid: "49661065"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="4b59d-101">在此练习中，你将扩展上一练习中的应用程序，以支持使用 Azure AD 进行身份验证。</span><span class="sxs-lookup"><span data-stu-id="4b59d-101">In this exercise you will extend the application from the previous exercise to support authentication with Azure AD.</span></span> <span data-ttu-id="4b59d-102">这是获取调用 Microsoft Graph 所需的 OAuth 访问令牌所必需的。</span><span class="sxs-lookup"><span data-stu-id="4b59d-102">This is required to obtain the necessary OAuth access token to call the Microsoft Graph.</span></span> <span data-ttu-id="4b59d-103">在此步骤中，将 [Microsoft 身份验证库 (MSAL) ，Java](https://github.com/AzureAD/microsoft-authentication-library-for-java) 应用程序。</span><span class="sxs-lookup"><span data-stu-id="4b59d-103">In this step you will integrate the [Microsoft Authentication Library (MSAL) for Java](https://github.com/AzureAD/microsoft-authentication-library-for-java) into the application.</span></span>

1. <span data-ttu-id="4b59d-104">在 **./src/main/resources** 目录中新建一个名为 **graphtu一l** 的目录。</span><span class="sxs-lookup"><span data-stu-id="4b59d-104">Create a new directory named **graphtutorial** in the **./src/main/resources** directory.</span></span>

1. <span data-ttu-id="4b59d-105">在 **./src/main/resources/graphtu一l** 目录中创建一个名为 **oAuth.properties** 的新文件，并添加该文件中的以下文本。</span><span class="sxs-lookup"><span data-stu-id="4b59d-105">Create a new file in the **./src/main/resources/graphtutorial** directory named **oAuth.properties**, and add the following text in that file.</span></span> <span data-ttu-id="4b59d-106">替换为 `YOUR_APP_ID_HERE` 在 Azure 门户中创建的应用程序 ID。</span><span class="sxs-lookup"><span data-stu-id="4b59d-106">Replace `YOUR_APP_ID_HERE` with the application ID you created in the Azure portal.</span></span>

    :::code language="ini" source="../demo/graphtutorial/src/main/resources/graphtutorial/oAuth.properties.example":::

    <span data-ttu-id="4b59d-107">值 `app.scopes` 包含应用程序所需的权限范围。</span><span class="sxs-lookup"><span data-stu-id="4b59d-107">The value of `app.scopes` contains the permission scopes the application requires.</span></span>

    - <span data-ttu-id="4b59d-108">**User.Read** 允许应用访问用户配置文件。</span><span class="sxs-lookup"><span data-stu-id="4b59d-108">**User.Read** allows the app to access the user's profile.</span></span>
    - <span data-ttu-id="4b59d-109">**MailboxSettings.Read** 允许应用访问用户邮箱中的设置，包括用户配置的时区。</span><span class="sxs-lookup"><span data-stu-id="4b59d-109">**MailboxSettings.Read** allows the app to access settings from the user's mailbox, including the user's configured time zone.</span></span>
    - <span data-ttu-id="4b59d-110">**Calendars.ReadWrite** 允许应用列出用户的日历，并添加新事件到日历。</span><span class="sxs-lookup"><span data-stu-id="4b59d-110">**Calendars.ReadWrite** allows the app to list the user's calendar and add new events to the calendar.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="4b59d-111">如果你使用的是源代码管理（如 git），那么现在应该从源代码管理中排除 **oAuth.properties** 文件，以避免意外泄露应用 ID。</span><span class="sxs-lookup"><span data-stu-id="4b59d-111">If you're using source control such as git, now would be a good time to exclude the **oAuth.properties** file from source control to avoid inadvertently leaking your app ID.</span></span>

1. <span data-ttu-id="4b59d-112">打开 **App.java** 并添加以下 `import` 语句。</span><span class="sxs-lookup"><span data-stu-id="4b59d-112">Open **App.java** and add the following `import` statements.</span></span>

    ```java
    import java.io.IOException;
    import java.util.Properties;
    ```

1. <span data-ttu-id="4b59d-113">在行之前添加以下代码 `Scanner input = new Scanner(System.in);` 以加载 **oAuth.properties** 文件。</span><span class="sxs-lookup"><span data-stu-id="4b59d-113">Add the following code just before the `Scanner input = new Scanner(System.in);` line to load the **oAuth.properties** file.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/App.java" id="LoadSettingsSnippet":::

## <a name="implement-sign-in"></a><span data-ttu-id="4b59d-114">实现登录</span><span class="sxs-lookup"><span data-stu-id="4b59d-114">Implement sign-in</span></span>

1. <span data-ttu-id="4b59d-115">在名为 **Authentication.java** 的 **./graphtu一l/src/main/java/graphtu上l** 目录中创建新文件，并添加以下代码。</span><span class="sxs-lookup"><span data-stu-id="4b59d-115">Create a new file in the **./graphtutorial/src/main/java/graphtutorial** directory named **Authentication.java** and add the following code.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/Authentication.java" id="AuthenticationSnippet":::

1. <span data-ttu-id="4b59d-116">在 **App.java** 中，将以下代码添加到该行的正前 `Scanner input = new Scanner(System.in);` ，以获取访问令牌。</span><span class="sxs-lookup"><span data-stu-id="4b59d-116">In **App.java**, add the following code just before the `Scanner input = new Scanner(System.in);` line to get an access token.</span></span>

    ```java
    // Get an access token
    Authentication.initialize(appId);
    final String accessToken = Authentication.getUserAccessToken(appScopes);
    ```

1. <span data-ttu-id="4b59d-117">在注释后添加以下 `// Display access token` 行。</span><span class="sxs-lookup"><span data-stu-id="4b59d-117">Add the following line after the `// Display access token` comment.</span></span>

    ```java
    System.out.println("Access token: " + accessToken);
    ```

1. <span data-ttu-id="4b59d-118">运行应用程序。</span><span class="sxs-lookup"><span data-stu-id="4b59d-118">Run the app.</span></span> <span data-ttu-id="4b59d-119">应用程序显示 URL 和设备代码。</span><span class="sxs-lookup"><span data-stu-id="4b59d-119">The application displays a URL and device code.</span></span>

    ```Shell
    Java Graph Tutorial

    To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code F7CG945YZ to authenticate.
    ```

1. <span data-ttu-id="4b59d-120">打开浏览器并浏览到显示的 URL。</span><span class="sxs-lookup"><span data-stu-id="4b59d-120">Open a browser and browse to the URL displayed.</span></span> <span data-ttu-id="4b59d-121">输入提供的代码并登录。</span><span class="sxs-lookup"><span data-stu-id="4b59d-121">Enter the provided code and sign in.</span></span> <span data-ttu-id="4b59d-122">完成后，返回到应用程序并选择 **1。显示访问令牌** 选项以显示访问令牌。</span><span class="sxs-lookup"><span data-stu-id="4b59d-122">Once completed, return to the application and choose the **1. Display access token** option to display the access token.</span></span>

> [!TIP]
> <span data-ttu-id="4b59d-123">可以分析 Microsoft 工作或学校帐户的访问令牌，以便进行疑难解答 [https://jwt.ms](https://jwt.ms) 。</span><span class="sxs-lookup"><span data-stu-id="4b59d-123">Access tokens for Microsoft work or school accounts can be parsed for troubleshooting purposes at [https://jwt.ms](https://jwt.ms).</span></span> <span data-ttu-id="4b59d-124">个人 Microsoft 帐户的访问令牌使用专有格式，无法进行分析。</span><span class="sxs-lookup"><span data-stu-id="4b59d-124">Access tokens for personal Microsoft accounts use a proprietary format and cannot be parsed.</span></span>
