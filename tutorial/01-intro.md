---
ms.openlocfilehash: f13ee9baa26bb487d5b50bd966e40cee9ba64378
ms.sourcegitcommit: 22ec2c0e4803b82c9d360f3609f8b15ba5d017ca
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/29/2020
ms.locfileid: "43940474"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="fb327-101">本教程向您介绍如何构建使用 Microsoft Graph API 检索用户的日历信息的 Java 控制台应用程序。</span><span class="sxs-lookup"><span data-stu-id="fb327-101">This tutorial teaches you how to build a Java console app that uses the Microsoft Graph API to retrieve calendar information for a user.</span></span>

> [!TIP]
> <span data-ttu-id="fb327-102">如果您只想下载已完成的教程，可以下载或克隆[GitHub 存储库](https://github.com/microsoftgraph/msgraph-training-java)。</span><span class="sxs-lookup"><span data-stu-id="fb327-102">If you prefer to just download the completed tutorial, you can download or clone the [GitHub repository](https://github.com/microsoftgraph/msgraph-training-java).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="fb327-103">先决条件</span><span class="sxs-lookup"><span data-stu-id="fb327-103">Prerequisites</span></span>

<span data-ttu-id="fb327-104">在开始本教程之前，您应在开发计算机上安装[JAVA SE 开发工具包（JDK）](https://java.com/en/download/faq/develop.xml)和[Gradle](https://gradle.org/) 。</span><span class="sxs-lookup"><span data-stu-id="fb327-104">Before you start this tutorial, you should have the [Java SE Development Kit (JDK)](https://java.com/en/download/faq/develop.xml) and [Gradle](https://gradle.org/) installed on your development machine.</span></span> <span data-ttu-id="fb327-105">如果您没有 JDK 或 Gradle，请访问以前的链接获取下载选项。</span><span class="sxs-lookup"><span data-stu-id="fb327-105">If you do not have the JDK or Gradle, visit the previous links for download options.</span></span>

<span data-ttu-id="fb327-106">您还应具有一个个人的 Microsoft 帐户，其中包含 Outlook.com 上的邮箱，或 Microsoft 工作或学校帐户。</span><span class="sxs-lookup"><span data-stu-id="fb327-106">You should also have either a personal Microsoft account with a mailbox on Outlook.com, or a Microsoft work or school account.</span></span> <span data-ttu-id="fb327-107">如果你没有 Microsoft 帐户，可以使用以下几种方法获取免费帐户：</span><span class="sxs-lookup"><span data-stu-id="fb327-107">If you don't have a Microsoft account, there are a couple of options to get a free account:</span></span>

- <span data-ttu-id="fb327-108">你可以[注册新的个人 Microsoft 帐户](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1)。</span><span class="sxs-lookup"><span data-stu-id="fb327-108">You can [sign up for a new personal Microsoft account](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).</span></span>
- <span data-ttu-id="fb327-109">你可以[注册 office 365 开发人员计划](https://developer.microsoft.com/office/dev-program)以获取免费的 office 365 订阅。</span><span class="sxs-lookup"><span data-stu-id="fb327-109">You can [sign up for the Office 365 Developer Program](https://developer.microsoft.com/office/dev-program) to get a free Office 365 subscription.</span></span>

> [!NOTE]
> <span data-ttu-id="fb327-110">本教程是使用 OpenJDK 版本14.0.0.36 和 Gradle 6.3 编写的。</span><span class="sxs-lookup"><span data-stu-id="fb327-110">This tutorial was written with OpenJDK version 14.0.0.36 and Gradle 6.3.</span></span> <span data-ttu-id="fb327-111">本指南中的步骤可能适用于其他版本，但尚未经过测试。</span><span class="sxs-lookup"><span data-stu-id="fb327-111">The steps in this guide may work with other versions, but that has not been tested.</span></span>

## <a name="feedback"></a><span data-ttu-id="fb327-112">反馈</span><span class="sxs-lookup"><span data-stu-id="fb327-112">Feedback</span></span>

<span data-ttu-id="fb327-113">请在[GitHub 存储库](https://github.com/microsoftgraph/msgraph-training-java)中提供有关本教程的任何反馈。</span><span class="sxs-lookup"><span data-stu-id="fb327-113">Please provide any feedback on this tutorial in the [GitHub repository](https://github.com/microsoftgraph/msgraph-training-java).</span></span>
