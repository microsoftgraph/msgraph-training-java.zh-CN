---
ms.openlocfilehash: 3e4d7f11c89947da29873c85ab2808279c94265a
ms.sourcegitcommit: 189f87d879c57b11992e7bc75580b4c69e014122
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/21/2020
ms.locfileid: "43612059"
---
<!-- markdownlint-disable MD002 MD041 -->

在本练习中，你将扩展上一练习中的应用程序，以支持 Azure AD 的身份验证。 若要获取所需的 OAuth 访问令牌以调用 Microsoft Graph，这是必需的。 在此步骤中，您将把[Java 的 Microsoft 身份验证库（MSAL）](https://github.com/AzureAD/microsoft-authentication-library-for-java)集成到应用程序中。

1. 在 **./src/main/resources**目录中创建一个名为**graphtutorial**的新目录。

1. 在 **/src/main/resources/graphtutorial**目录中创建一个名为 " **oAuth. properties**" 的新文件，并将以下文本添加到该文件中。

    :::code language="ini" source="../demo/graphtutorial/src/main/resources/graphtutorial/oAuth.properties.example":::

    将`YOUR_APP_ID_HERE`替换为你在 Azure 门户中创建的应用程序 ID。

    > [!IMPORTANT]
    > 如果您使用的是源代码管理（如 git），现在是从源代码控制中排除**oAuth. properties**文件，以避免无意中泄漏您的应用程序 ID，这是一种很合适的时机。

1. 打开**应用程序 .java**并添加以下`import`语句。

    ```java
    import java.io.IOException;
    import java.util.Properties;
    ```

1. 将以下代码添加到`Scanner input = new Scanner(System.in);`行前面，以加载**oAuth. properties**文件。

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/App.java" id="LoadSettingsSnippet":::

## <a name="implement-sign-in"></a>实施登录

1. 在 **/graphtutorial/src/main/java/graphtutorial**目录中创建一个名为 " **Authentication** " 的新文件，并添加以下代码。

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/Authentication.java" id="AuthenticationSnippet":::

1. 在 **.java**中，将以下代码添加到`Scanner input = new Scanner(System.in);`行前面，以获取访问令牌。

    ```java
    // Get an access token
    Authentication.initialize(appId);
    final String accessToken = Authentication.getUserAccessToken(appScopes);
    ```

1. 在`// Display access token`注释后面添加以下行。

    ```java
    System.out.println("Access token: " + accessToken);
    ```

1. 运行应用程序。 应用程序将显示 URL 和设备代码。

    ```Shell
    Java Graph Tutorial

    To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code F7CG945YZ to authenticate.
    ```

1. 打开浏览器并浏览到显示的 URL。 输入提供的代码并登录。 完成后，返回到应用程序并选择 " **1"。显示**访问令牌选项以显示访问令牌。
