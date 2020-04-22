---
ms.openlocfilehash: 3e4d7f11c89947da29873c85ab2808279c94265a
ms.sourcegitcommit: 189f87d879c57b11992e7bc75580b4c69e014122
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/21/2020
ms.locfileid: "43612059"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="fb5e7-101">在本练习中，你将扩展上一练习中的应用程序，以支持 Azure AD 的身份验证。</span><span class="sxs-lookup"><span data-stu-id="fb5e7-101">In this exercise you will extend the application from the previous exercise to support authentication with Azure AD.</span></span> <span data-ttu-id="fb5e7-102">若要获取所需的 OAuth 访问令牌以调用 Microsoft Graph，这是必需的。</span><span class="sxs-lookup"><span data-stu-id="fb5e7-102">This is required to obtain the necessary OAuth access token to call the Microsoft Graph.</span></span> <span data-ttu-id="fb5e7-103">在此步骤中，您将把[Java 的 Microsoft 身份验证库（MSAL）](https://github.com/AzureAD/microsoft-authentication-library-for-java)集成到应用程序中。</span><span class="sxs-lookup"><span data-stu-id="fb5e7-103">In this step you will integrate the [Microsoft Authentication Library (MSAL) for Java](https://github.com/AzureAD/microsoft-authentication-library-for-java) into the application.</span></span>

1. <span data-ttu-id="fb5e7-104">在 **./src/main/resources**目录中创建一个名为**graphtutorial**的新目录。</span><span class="sxs-lookup"><span data-stu-id="fb5e7-104">Create a new directory named **graphtutorial** in the **./src/main/resources** directory.</span></span>

1. <span data-ttu-id="fb5e7-105">在 **/src/main/resources/graphtutorial**目录中创建一个名为 " **oAuth. properties**" 的新文件，并将以下文本添加到该文件中。</span><span class="sxs-lookup"><span data-stu-id="fb5e7-105">Create a new file in the **./src/main/resources/graphtutorial** directory named **oAuth.properties**, and add the following text in that file.</span></span>

    :::code language="ini" source="../demo/graphtutorial/src/main/resources/graphtutorial/oAuth.properties.example":::

    <span data-ttu-id="fb5e7-106">将`YOUR_APP_ID_HERE`替换为你在 Azure 门户中创建的应用程序 ID。</span><span class="sxs-lookup"><span data-stu-id="fb5e7-106">Replace `YOUR_APP_ID_HERE` with the application ID you created in the Azure portal.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="fb5e7-107">如果您使用的是源代码管理（如 git），现在是从源代码控制中排除**oAuth. properties**文件，以避免无意中泄漏您的应用程序 ID，这是一种很合适的时机。</span><span class="sxs-lookup"><span data-stu-id="fb5e7-107">If you're using source control such as git, now would be a good time to exclude the **oAuth.properties** file from source control to avoid inadvertently leaking your app ID.</span></span>

1. <span data-ttu-id="fb5e7-108">打开**应用程序 .java**并添加以下`import`语句。</span><span class="sxs-lookup"><span data-stu-id="fb5e7-108">Open **App.java** and add the following `import` statements.</span></span>

    ```java
    import java.io.IOException;
    import java.util.Properties;
    ```

1. <span data-ttu-id="fb5e7-109">将以下代码添加到`Scanner input = new Scanner(System.in);`行前面，以加载**oAuth. properties**文件。</span><span class="sxs-lookup"><span data-stu-id="fb5e7-109">Add the following code just before the `Scanner input = new Scanner(System.in);` line to load the **oAuth.properties** file.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/App.java" id="LoadSettingsSnippet":::

## <a name="implement-sign-in"></a><span data-ttu-id="fb5e7-110">实施登录</span><span class="sxs-lookup"><span data-stu-id="fb5e7-110">Implement sign-in</span></span>

1. <span data-ttu-id="fb5e7-111">在 **/graphtutorial/src/main/java/graphtutorial**目录中创建一个名为 " **Authentication** " 的新文件，并添加以下代码。</span><span class="sxs-lookup"><span data-stu-id="fb5e7-111">Create a new file in the **./graphtutorial/src/main/java/graphtutorial** directory named **Authentication.java** and add the following code.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/Authentication.java" id="AuthenticationSnippet":::

1. <span data-ttu-id="fb5e7-112">在 **.java**中，将以下代码添加到`Scanner input = new Scanner(System.in);`行前面，以获取访问令牌。</span><span class="sxs-lookup"><span data-stu-id="fb5e7-112">In **App.java**, add the following code just before the `Scanner input = new Scanner(System.in);` line to get an access token.</span></span>

    ```java
    // Get an access token
    Authentication.initialize(appId);
    final String accessToken = Authentication.getUserAccessToken(appScopes);
    ```

1. <span data-ttu-id="fb5e7-113">在`// Display access token`注释后面添加以下行。</span><span class="sxs-lookup"><span data-stu-id="fb5e7-113">Add the following line after the `// Display access token` comment.</span></span>

    ```java
    System.out.println("Access token: " + accessToken);
    ```

1. <span data-ttu-id="fb5e7-114">运行应用程序。</span><span class="sxs-lookup"><span data-stu-id="fb5e7-114">Run the app.</span></span> <span data-ttu-id="fb5e7-115">应用程序将显示 URL 和设备代码。</span><span class="sxs-lookup"><span data-stu-id="fb5e7-115">The application displays a URL and device code.</span></span>

    ```Shell
    Java Graph Tutorial

    To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code F7CG945YZ to authenticate.
    ```

1. <span data-ttu-id="fb5e7-116">打开浏览器并浏览到显示的 URL。</span><span class="sxs-lookup"><span data-stu-id="fb5e7-116">Open a browser and browse to the URL displayed.</span></span> <span data-ttu-id="fb5e7-117">输入提供的代码并登录。</span><span class="sxs-lookup"><span data-stu-id="fb5e7-117">Enter the provided code and sign in.</span></span> <span data-ttu-id="fb5e7-118">完成后，返回到应用程序并选择 " **1"。显示**访问令牌选项以显示访问令牌。</span><span class="sxs-lookup"><span data-stu-id="fb5e7-118">Once completed, return to the application and choose the **1. Display access token** option to display the access token.</span></span>
