---
ms.openlocfilehash: 4e62f08217ab00427218cd1815d66d80fd2802de
ms.sourcegitcommit: 2af94da662c454e765b32edeb9406812e3732406
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/13/2019
ms.locfileid: "40018797"
---
<!-- markdownlint-disable MD002 MD041 -->

在本练习中，将使用 Azure Active Directory 管理中心创建新的 Azure AD 应用程序。

1. 打开浏览器，并转到 [Azure Active Directory 管理中心](https://aad.portal.azure.com)。然后，使用**个人帐户**（亦称为“Microsoft 帐户”）或**工作或学校帐户**登录。

1. 选择左侧导航栏中的“**Azure Active Directory**”，再选择“**管理**”下的“**应用注册**”。

    ![应用注册的屏幕截图 ](./images/aad-portal-app-registrations.png)

1. 选择“新注册”****。 在“注册应用”**** 页上，按如下方式设置值。

    - 将“名称”**** 设置为“`Java Graph Tutorial`”。
    - 将“受支持的帐户类型”**** 设置为“任何组织目录中的帐户和个人 Microsoft 帐户”****。
    - 保留“重定向 URI”**** 为空。

    !["注册应用程序" 页的屏幕截图](./images/aad-register-an-app.png)

1. 选择“**注册**”。 在 " **Java Graph 教程**" 页上，复制**应用程序（客户端） ID**的值并保存它，下一步将需要它。

    ![新应用注册的应用程序 ID 的屏幕截图](./images/aad-application-id.png)

1. 选择 "**添加重定向 URI** " 链接。 在 "**重定向 uri** " 页上，找到 "**公共客户端（移动、桌面）** " 部分的 "建议的重定向 uri" 部分。 选择`https://login.microsoftonline.com/common/oauth2/nativeclient` URI。

    !["重定向 Uri" 页的屏幕截图](./images/aad-redirect-uris.png)

1. 找到 "**默认客户端类型**" 部分，将 "将**应用程序视为公用客户端**" 切换为 **"是"**，然后选择 "**保存**"。

    ![默认 "客户端类型" 部分的屏幕截图](./images/aad-default-client-type.png)
