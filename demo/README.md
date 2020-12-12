---
ms.openlocfilehash: 50206b77504979c1cf67b4d0daa0b6f6576bde32
ms.sourcegitcommit: eb935a250f8531b04a42710356072b80d46ee3a4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/11/2020
ms.locfileid: "49661051"
---
# <a name="how-to-run-the-completed-project"></a>如何运行已完成的项目

## <a name="prerequisites"></a>先决条件

若要在此文件夹中运行已完成的项目，您需要以下各项：

- [Java安装在开发 (JDK) ](https://java.com/en/download/faq/develop.xml)[和 Gradle](https://gradle.org/)的 SE 开发工具包。 如果你没有 JDK 或 Gradle，请访问前面的链接，查看下载选项。  (**注意：** 本教程是使用 OpenJDK 版本 14.0.0.36 和 Gradle 6.7.1 编写的。 本指南中的步骤可能与其他版本一样，但尚未测试。) 
- Microsoft 工作或学校帐户。

如果你没有 Microsoft 帐户，可以注册 [Office 365 开发人员](https://developer.microsoft.com/office/dev-program) 计划，获取免费的 Office 365 订阅。

## <a name="register-a-web-application-with-the-azure-active-directory-admin-center"></a>向 Azure Active Directory 管理中心注册 Web 应用程序

1. 打开浏览器，并转到 [Azure Active Directory 管理中心](https://aad.portal.azure.com)。 使用 **个人帐户**（亦称为“Microsoft 帐户”）或 **工作或学校帐户** 登录。

1. 选择左侧导航栏中的“**Azure Active Directory**”，再选择“**管理**”下的“**应用注册**”。

    ![应用注册的屏幕截图 ](/tutorial/images/aad-portal-app-registrations.png)

1. 选择“新注册”。 在“注册应用”页上，按如下方式设置值。

    - 将“名称”设置为“`Java Graph Tutorial`”。
    - 将 **支持的帐户类型设置为****任何组织目录中的帐户**。
    - 保留“重定向 URI”为空。

    !["注册应用程序"页的屏幕截图](/tutorial/images/aad-register-an-app.png)

1. 选择 **“注册”**。 在 **"Java Graph** 教程"页上，复制 **Application (客户端**) ID 并保存它，下一步中将需要该值。

    ![新应用注册的应用程序 ID 屏幕截图](/tutorial/images/aad-application-id.png)

1. 选择 **"添加重定向 URI"** 链接。 在" **重定向 URI"** 页上，找到"建议用于移动 **、桌面 (客户端** 的重定向 URI) 部分。 选择 `https://login.microsoftonline.com/common/oauth2/nativeclient` URI。

    ![重定向 URI 页面的屏幕截图](/tutorial/images/aad-redirect-uris.png)

1. 找到 **"默认客户端类型**"部分，将 **"将应用程序视为公共客户端**"开关更改为 **"是**"，然后选择"**保存"。**

    !["默认客户端类型"部分屏幕截图](/tutorial/images/aad-default-client-type.png)

## <a name="configure-the-sample"></a>配置示例

1. 将文件 `oAuth.properties.example` 重命名为 `oAuth.properties` 。
1. 编辑 `oAuth.properties` 文件并做出以下更改。
    1. 替换为 `YOUR_APP_ID_HERE` 从 **应用** 注册门户获得的应用程序 ID。

## <a name="build-and-run-the-sample"></a>生成和运行示例

在命令行界面 (CLI) ，导航到项目目录并运行以下命令。

```Shell
./gradlew --console plain run
```
