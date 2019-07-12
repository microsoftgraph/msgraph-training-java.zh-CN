---
ms.openlocfilehash: 44426ee29265e8730ea3bc19f21091aa0180fd6e
ms.sourcegitcommit: 02054b307013cce781be2a3512ec1e54f1a322eb
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/12/2019
ms.locfileid: "35634743"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="184bf-101">在本练习中, 你将扩展上一练习中的应用程序, 以支持 Azure AD 的身份验证。</span><span class="sxs-lookup"><span data-stu-id="184bf-101">In this exercise you will extend the application from the previous exercise to support authentication with Azure AD.</span></span> <span data-ttu-id="184bf-102">若要获取所需的 OAuth 访问令牌以调用 Microsoft Graph, 这是必需的。</span><span class="sxs-lookup"><span data-stu-id="184bf-102">This is required to obtain the necessary OAuth access token to call the Microsoft Graph.</span></span> <span data-ttu-id="184bf-103">在此步骤中, 您将把[Java 的 Microsoft 身份验证库 (MSAL)](https://github.com/AzureAD/microsoft-authentication-library-for-java)集成到应用程序中。</span><span class="sxs-lookup"><span data-stu-id="184bf-103">In this step you will integrate the [Microsoft Authentication Library (MSAL) for Java](https://github.com/AzureAD/microsoft-authentication-library-for-java) into the application.</span></span>

<span data-ttu-id="184bf-104">在 **/graphtutorial/src/main/java/com/contoso**目录中创建一个名为 " **oAuth. properties**" 的新文件。</span><span class="sxs-lookup"><span data-stu-id="184bf-104">Create a new file in the **./graphtutorial/src/main/java/com/contoso** directory named **oAuth.properties**.</span></span> <span data-ttu-id="184bf-105">将以下文本添加到该文件中。</span><span class="sxs-lookup"><span data-stu-id="184bf-105">Add the following text in that file.</span></span>

```INI
app.id=YOUR_APP_ID_HERE
app.scopes=User.Read,Calendars.Read
```

<span data-ttu-id="184bf-106">将`YOUR_APP_ID_HERE`替换为你在 Azure 门户中创建的应用程序 ID。</span><span class="sxs-lookup"><span data-stu-id="184bf-106">Replace `YOUR_APP_ID_HERE` with the application ID you created in the Azure portal.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="184bf-107">如果您使用的是源代码管理 (如 git), 现在是从源代码控制中排除**oAuth. properties**文件, 以避免无意中泄漏您的应用程序 ID, 这是一种很合适的时机。</span><span class="sxs-lookup"><span data-stu-id="184bf-107">If you're using source control such as git, now would be a good time to exclude the **oAuth.properties** file from source control to avoid inadvertently leaking your app ID.</span></span>

<span data-ttu-id="184bf-108">打开**应用程序 .java**并添加以下`import`语句。</span><span class="sxs-lookup"><span data-stu-id="184bf-108">Open **App.java** and add the following `import` statements.</span></span>

```java
import java.io.IOException;
import java.util.Properties;
```

<span data-ttu-id="184bf-109">然后, 将以下代码添加到`Scanner input = new Scanner(System.in);`行前面, 以加载**oAuth. properties**文件。</span><span class="sxs-lookup"><span data-stu-id="184bf-109">Then add the following code just before the `Scanner input = new Scanner(System.in);` line to load the **oAuth.properties** file.</span></span>

```java
// Load OAuth settings
final Properties oAuthProperties = new Properties();
try {
    oAuthProperties.load(App.class.getResourceAsStream("oAuth.properties"));
} catch (IOException e) {
    System.out.println("Unable to read OAuth configuration. Make sure you have a properly formatted oAuth.properties file. See README for details.");
    return;
}

final String appId = oAuthProperties.getProperty("app.id");
final String[] appScopes = oAuthProperties.getProperty("app.scopes").split(",");
```

## <a name="implement-sign-in"></a><span data-ttu-id="184bf-110">实施登录</span><span class="sxs-lookup"><span data-stu-id="184bf-110">Implement sign-in</span></span>

<span data-ttu-id="184bf-111">在 **/graphtutorial/src/main/java/com/contoso**目录中创建一个名为 " **Authentication** " 的新文件, 并添加以下代码。</span><span class="sxs-lookup"><span data-stu-id="184bf-111">Create a new file in the **./graphtutorial/src/main/java/com/contoso** directory named **Authentication.java** and add the following code.</span></span>

```java
package com.contoso;
import java.net.MalformedURLException;
import java.util.Set;
import java.util.function.Consumer;

import com.microsoft.aad.msal4j.DeviceCode;
import com.microsoft.aad.msal4j.DeviceCodeFlowParameters;
import com.microsoft.aad.msal4j.IAuthenticationResult;
import com.microsoft.aad.msal4j.PublicClientApplication;

/**
 * Authentication
 */
public class Authentication {

    private static String applicationId;
    // Set authority to allow only organizational accounts
    // Device code flow only supports organizational accounts
    private final static String authority = "https://login.microsoftonline.com/organizations/";

    public static void initialize(String applicationId) {
        Authentication.applicationId = applicationId;
    }

    public static String getUserAccessToken(String[] scopes) {
        if (applicationId == null) {
            System.out.println("You must initialize Authentication before calling getUserAccessToken");
            return null;
        }

        Set<String> scopeSet = Set.of(scopes);

        PublicClientApplication app;
        try {
            // Build the MSAL application object with
            // app ID and authority
            app = PublicClientApplication.builder(applicationId)
                .authority(authority)
                .build();
        } catch (MalformedURLException e) {
            return null;
        }

        // Create consumer to receive the DeviceCode object
        // This method gets executed during the flow and provides
        // the URL the user logs into and the device code to enter
        Consumer<DeviceCode> deviceCodeConsumer = (DeviceCode deviceCode) -> {
            // Print the login information to the console
            System.out.println(deviceCode.message());
        };

        // Request a token, passing the requested permission scopes
        IAuthenticationResult result = app.acquireToken(
            DeviceCodeFlowParameters
                .builder(scopeSet, deviceCodeConsumer)
                .build()
        ).exceptionally(ex -> {
            System.out.println("Unable to authenticate - " + ex.getMessage());
            return null;
        }).join();

        if (result != null) {
            return result.accessToken();
        }

        return null;
    }
}
```

<span data-ttu-id="184bf-112">在 **.java**中, 将以下代码添加到`Scanner input = new Scanner(System.in);`行前面, 以获取访问令牌。</span><span class="sxs-lookup"><span data-stu-id="184bf-112">In **App.java**, add the following code just before the `Scanner input = new Scanner(System.in);` line to get an access token.</span></span>

```java
// Get an access token
Authentication.initialize(appId);
final String accessToken = Authentication.getUserAccessToken(appScopes);
```

<span data-ttu-id="184bf-113">然后在`// Display access token`注释后面添加以下行。</span><span class="sxs-lookup"><span data-stu-id="184bf-113">Then add the following line after the `// Display access token` comment.</span></span>

```java
System.out.println("Access token: " + accessToken);
```

<span data-ttu-id="184bf-114">生成并运行应用程序。</span><span class="sxs-lookup"><span data-stu-id="184bf-114">Build and run the app.</span></span> <span data-ttu-id="184bf-115">应用程序将显示 URL 和设备代码。</span><span class="sxs-lookup"><span data-stu-id="184bf-115">The application displays a URL and device code.</span></span>

```Shell
Java Graph Tutorial

To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code F7CG945YZ to authenticate.
```

<span data-ttu-id="184bf-116">打开浏览器并浏览到显示的 URL。</span><span class="sxs-lookup"><span data-stu-id="184bf-116">Open a browser and browse to the URL displayed.</span></span> <span data-ttu-id="184bf-117">输入提供的代码并登录。</span><span class="sxs-lookup"><span data-stu-id="184bf-117">Enter the provided code and sign in.</span></span> <span data-ttu-id="184bf-118">完成后, 返回到应用程序并选择 " **1"。显示**访问令牌选项以显示访问令牌。</span><span class="sxs-lookup"><span data-stu-id="184bf-118">Once completed, return to the application and choose the **1. Display access token** option to display the access token.</span></span>
