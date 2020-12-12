---
ms.openlocfilehash: 4c04317462240ff0696ac1381fae886481db7847
ms.sourcegitcommit: eb935a250f8531b04a42710356072b80d46ee3a4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/11/2020
ms.locfileid: "49661065"
---
<!-- markdownlint-disable MD002 MD041 -->

在此练习中，你将扩展上一练习中的应用程序，以支持使用 Azure AD 进行身份验证。 这是获取调用 Microsoft Graph 所需的 OAuth 访问令牌所必需的。 在此步骤中，将 [Microsoft 身份验证库 (MSAL) ，Java](https://github.com/AzureAD/microsoft-authentication-library-for-java) 应用程序。

1. 在 **./src/main/resources** 目录中新建一个名为 **graphtu一l** 的目录。

1. 在 **./src/main/resources/graphtu一l** 目录中创建一个名为 **oAuth.properties** 的新文件，并添加该文件中的以下文本。 替换为 `YOUR_APP_ID_HERE` 在 Azure 门户中创建的应用程序 ID。

    :::code language="ini" source="../demo/graphtutorial/src/main/resources/graphtutorial/oAuth.properties.example":::

    值 `app.scopes` 包含应用程序所需的权限范围。

    - **User.Read** 允许应用访问用户配置文件。
    - **MailboxSettings.Read** 允许应用访问用户邮箱中的设置，包括用户配置的时区。
    - **Calendars.ReadWrite** 允许应用列出用户的日历，并添加新事件到日历。

    > [!IMPORTANT]
    > 如果你使用的是源代码管理（如 git），那么现在应该从源代码管理中排除 **oAuth.properties** 文件，以避免意外泄露应用 ID。

1. 打开 **App.java** 并添加以下 `import` 语句。

    ```java
    import java.io.IOException;
    import java.util.Properties;
    ```

1. 在行之前添加以下代码 `Scanner input = new Scanner(System.in);` 以加载 **oAuth.properties** 文件。

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/App.java" id="LoadSettingsSnippet":::

## <a name="implement-sign-in"></a>实现登录

1. 在名为 **Authentication.java** 的 **./graphtu一l/src/main/java/graphtu上l** 目录中创建新文件，并添加以下代码。

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/Authentication.java" id="AuthenticationSnippet":::

1. 在 **App.java** 中，将以下代码添加到该行的正前 `Scanner input = new Scanner(System.in);` ，以获取访问令牌。

    ```java
    // Get an access token
    Authentication.initialize(appId);
    final String accessToken = Authentication.getUserAccessToken(appScopes);
    ```

1. 在注释后添加以下 `// Display access token` 行。

    ```java
    System.out.println("Access token: " + accessToken);
    ```

1. 运行应用程序。 应用程序显示 URL 和设备代码。

    ```Shell
    Java Graph Tutorial

    To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code F7CG945YZ to authenticate.
    ```

1. 打开浏览器并浏览到显示的 URL。 输入提供的代码并登录。 完成后，返回到应用程序并选择 **1。显示访问令牌** 选项以显示访问令牌。

> [!TIP]
> 可以分析 Microsoft 工作或学校帐户的访问令牌，以便进行疑难解答 [https://jwt.ms](https://jwt.ms) 。 个人 Microsoft 帐户的访问令牌使用专有格式，无法进行分析。
