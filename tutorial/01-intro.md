---
ms.openlocfilehash: 3b1e366ded1e9d59f048eec8da23a686a121dedf
ms.sourcegitcommit: eb935a250f8531b04a42710356072b80d46ee3a4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/11/2020
ms.locfileid: "49661072"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="28731-101">本教程指导你生成一个Java控制台应用，该应用使用 Microsoft Graph API 检索用户的日历信息。</span><span class="sxs-lookup"><span data-stu-id="28731-101">This tutorial teaches you how to build a Java console app that uses the Microsoft Graph API to retrieve calendar information for a user.</span></span>

> [!TIP]
> <span data-ttu-id="28731-102">如果你更喜欢仅下载已完成的教程，可以下载或克隆 [GitHub 存储库](https://github.com/microsoftgraph/msgraph-training-java)。</span><span class="sxs-lookup"><span data-stu-id="28731-102">If you prefer to just download the completed tutorial, you can download or clone the [GitHub repository](https://github.com/microsoftgraph/msgraph-training-java).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="28731-103">先决条件</span><span class="sxs-lookup"><span data-stu-id="28731-103">Prerequisites</span></span>

<span data-ttu-id="28731-104">在开始本教程之前，应在开发计算机上安装 Java SE 开发工具包[ (JDK) ](https://java.com/en/download/faq/develop.xml)和[Gradle。](https://gradle.org/)</span><span class="sxs-lookup"><span data-stu-id="28731-104">Before you start this tutorial, you should have the [Java SE Development Kit (JDK)](https://java.com/en/download/faq/develop.xml) and [Gradle](https://gradle.org/) installed on your development machine.</span></span> <span data-ttu-id="28731-105">如果你没有 JDK 或 Gradle，请访问前面的链接，查看下载选项。</span><span class="sxs-lookup"><span data-stu-id="28731-105">If you do not have the JDK or Gradle, visit the previous links for download options.</span></span>

<span data-ttu-id="28731-106">你还应该拥有具有邮箱的个人 Microsoft 帐户Outlook.com或 Microsoft 工作或学校帐户。</span><span class="sxs-lookup"><span data-stu-id="28731-106">You should also have either a personal Microsoft account with a mailbox on Outlook.com, or a Microsoft work or school account.</span></span> <span data-ttu-id="28731-107">如果你没有 Microsoft 帐户，则有几个选项可以获取免费帐户：</span><span class="sxs-lookup"><span data-stu-id="28731-107">If you don't have a Microsoft account, there are a couple of options to get a free account:</span></span>

- <span data-ttu-id="28731-108">你可以 [注册新的个人 Microsoft 帐户](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1)。</span><span class="sxs-lookup"><span data-stu-id="28731-108">You can [sign up for a new personal Microsoft account](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).</span></span>
- <span data-ttu-id="28731-109">你可以 [注册 Office 365 开发人员计划](https://developer.microsoft.com/office/dev-program) ，获取免费的 Office 365 订阅。</span><span class="sxs-lookup"><span data-stu-id="28731-109">You can [sign up for the Office 365 Developer Program](https://developer.microsoft.com/office/dev-program) to get a free Office 365 subscription.</span></span>

> [!NOTE]
> <span data-ttu-id="28731-110">本教程使用 OpenJDK 版本 14.0.0.36 和 Gradle 6.7.1 编写。</span><span class="sxs-lookup"><span data-stu-id="28731-110">This tutorial was written with OpenJDK version 14.0.0.36 and Gradle 6.7.1.</span></span> <span data-ttu-id="28731-111">本指南中的步骤可能与其他版本一起运行，但尚未经过测试。</span><span class="sxs-lookup"><span data-stu-id="28731-111">The steps in this guide may work with other versions, but that has not been tested.</span></span>

## <a name="feedback"></a><span data-ttu-id="28731-112">反馈</span><span class="sxs-lookup"><span data-stu-id="28731-112">Feedback</span></span>

<span data-ttu-id="28731-113">请在 GitHub 存储库中提供有关本教程 [的任何反馈](https://github.com/microsoftgraph/msgraph-training-java)。</span><span class="sxs-lookup"><span data-stu-id="28731-113">Please provide any feedback on this tutorial in the [GitHub repository](https://github.com/microsoftgraph/msgraph-training-java).</span></span>
