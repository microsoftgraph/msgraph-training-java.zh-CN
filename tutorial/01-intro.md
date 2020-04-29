---
ms.openlocfilehash: f13ee9baa26bb487d5b50bd966e40cee9ba64378
ms.sourcegitcommit: 22ec2c0e4803b82c9d360f3609f8b15ba5d017ca
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/29/2020
ms.locfileid: "43940474"
---
<!-- markdownlint-disable MD002 MD041 -->

本教程向您介绍如何构建使用 Microsoft Graph API 检索用户的日历信息的 Java 控制台应用程序。

> [!TIP]
> 如果您只想下载已完成的教程，可以下载或克隆[GitHub 存储库](https://github.com/microsoftgraph/msgraph-training-java)。

## <a name="prerequisites"></a>先决条件

在开始本教程之前，您应在开发计算机上安装[JAVA SE 开发工具包（JDK）](https://java.com/en/download/faq/develop.xml)和[Gradle](https://gradle.org/) 。 如果您没有 JDK 或 Gradle，请访问以前的链接获取下载选项。

您还应具有一个个人的 Microsoft 帐户，其中包含 Outlook.com 上的邮箱，或 Microsoft 工作或学校帐户。 如果你没有 Microsoft 帐户，可以使用以下几种方法获取免费帐户：

- 你可以[注册新的个人 Microsoft 帐户](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1)。
- 你可以[注册 office 365 开发人员计划](https://developer.microsoft.com/office/dev-program)以获取免费的 office 365 订阅。

> [!NOTE]
> 本教程是使用 OpenJDK 版本14.0.0.36 和 Gradle 6.3 编写的。 本指南中的步骤可能适用于其他版本，但尚未经过测试。

## <a name="feedback"></a>反馈

请在[GitHub 存储库](https://github.com/microsoftgraph/msgraph-training-java)中提供有关本教程的任何反馈。
