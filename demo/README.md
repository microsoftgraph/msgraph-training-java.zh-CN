---
ms.openlocfilehash: 725648d1b8518c17938c17c160c5799258dc1d92
ms.sourcegitcommit: 189f87d879c57b11992e7bc75580b4c69e014122
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/21/2020
ms.locfileid: "43612347"
---
# <a name="how-to-run-the-completed-project"></a>如何运行已完成的项目

## <a name="prerequisites"></a>先决条件

若要在此文件夹中运行已完成的项目，您需要以下各项：

- 在开发计算机上安装的[JAVA SE 开发工具包（JDK）](https://java.com/en/download/faq/develop.xml)和[Gradle](https://gradle.org/) 。 如果您没有 JDK 或 Gradle，请访问以前的链接获取下载选项。 （**注意：** 本教程是使用 OpenJDK 版本14.0.0.36 和 Gradle 6.3 编写的。 本指南中的步骤可能适用于其他版本，但尚未经过测试。
- Microsoft 工作或学校帐户。

如果你没有 Microsoft 帐户，可以[注册 office 365 开发人员计划](https://developer.microsoft.com/office/dev-program)以获取免费的 office 365 订阅。

## <a name="register-a-web-application-with-the-azure-active-directory-admin-center"></a>向 Azure Active Directory 管理中心注册 web 应用程序

1. 打开浏览器，并转到 [Azure Active Directory 管理中心](https://aad.portal.azure.com)。 使用**个人帐户**（亦称为“Microsoft 帐户”）或**工作或学校帐户**登录。

1. 选择左侧导航栏中的“**Azure Active Directory**”，再选择“**管理**”下的“**应用注册**”。

    ![应用注册的屏幕截图 ](/tutorial/images/aad-portal-app-registrations.png)

1. 选择“新注册”****。 在“注册应用”**** 页上，按如下方式设置值。

    - 将“名称”**** 设置为“`Java Graph Tutorial`”。
    - 将**支持的帐户类型**设置为**任何组织目录中的帐户**。
    - 保留“重定向 URI”**** 为空。

    !["注册应用程序" 页的屏幕截图](/tutorial/images/aad-register-an-app.png)

1. 选择“注册”****。 在 " **Java Graph 教程**" 页上，复制**应用程序（客户端） ID**的值并保存它，下一步将需要它。

    ![新应用注册的应用程序 ID 的屏幕截图](/tutorial/images/aad-application-id.png)

1. 选择 "**添加重定向 URI** " 链接。 在 "**重定向 uri** " 页上，找到 "**公共客户端（移动、桌面）** " 部分的 "建议的重定向 uri" 部分。 选择`https://login.microsoftonline.com/common/oauth2/nativeclient` URI。

    !["重定向 Uri" 页的屏幕截图](/tutorial/images/aad-redirect-uris.png)

1. 找到 "**默认客户端类型**" 部分，将 "将**应用程序视为公用客户端**" 切换为 **"是"**，然后选择 "**保存**"。

    ![默认 "客户端类型" 部分的屏幕截图](/tutorial/images/aad-default-client-type.png)

## <a name="configure-the-sample"></a>配置示例

1. 将`oAuth.properties.example`文件重命名`oAuth.properties`为。
1. 编辑`oAuth.properties`文件并进行以下更改。
    1. 将`YOUR_APP_ID_HERE`替换为你从应用注册门户获取的**应用程序 Id** 。

## <a name="build-and-run-the-sample"></a>生成和运行示例

在命令行界面（CLI）中，导航到项目目录并运行以下命令。

```Shell
./gradlew --console plain run
```
