---
ms.openlocfilehash: 3eec5a727d21e481525e2a2892e33a0a30a9bb25
ms.sourcegitcommit: 2af94da662c454e765b32edeb9406812e3732406
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/13/2019
ms.locfileid: "40018792"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="04d23-101">本教程向您介绍如何构建使用 Microsoft Graph API 检索用户的日历信息的 Java 控制台应用程序。</span><span class="sxs-lookup"><span data-stu-id="04d23-101">This tutorial teaches you how to build a Java console app that uses the Microsoft Graph API to retrieve calendar information for a user.</span></span>

> [!TIP]
> <span data-ttu-id="04d23-102">如果您只想下载已完成的教程，可以下载或克隆[GitHub 存储库](https://github.com/microsoftgraph/msgraph-training-java)。</span><span class="sxs-lookup"><span data-stu-id="04d23-102">If you prefer to just download the completed tutorial, you can download or clone the [GitHub repository](https://github.com/microsoftgraph/msgraph-training-java).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="04d23-103">先决条件</span><span class="sxs-lookup"><span data-stu-id="04d23-103">Prerequisites</span></span>

<span data-ttu-id="04d23-104">在开始本教程之前，您应在开发计算机上安装[JAVA SE 开发工具包（JDK）](https://java.com/en/download/faq/develop.xml)和[Maven](https://maven.apache.org/) 。</span><span class="sxs-lookup"><span data-stu-id="04d23-104">Before you start this tutorial, you should have the [Java SE Development Kit (JDK)](https://java.com/en/download/faq/develop.xml) and [Maven](https://maven.apache.org/) installed on your development machine.</span></span> <span data-ttu-id="04d23-105">如果您没有 JDK 或 Maven，请访问以前的链接获取下载选项。</span><span class="sxs-lookup"><span data-stu-id="04d23-105">If you do not have the JDK or Maven, visit the previous links for download options.</span></span>

> [!NOTE]
> <span data-ttu-id="04d23-106">本教程是使用 OpenJDK 版本12.0.1 和 Maven 3.6.1 编写的。</span><span class="sxs-lookup"><span data-stu-id="04d23-106">This tutorial was written with OpenJDK version 12.0.1 and Maven 3.6.1.</span></span> <span data-ttu-id="04d23-107">本指南中的步骤可能适用于其他版本，但尚未经过测试。</span><span class="sxs-lookup"><span data-stu-id="04d23-107">The steps in this guide may work with other versions, but that has not been tested.</span></span>

## <a name="feedback"></a><span data-ttu-id="04d23-108">反馈</span><span class="sxs-lookup"><span data-stu-id="04d23-108">Feedback</span></span>

<span data-ttu-id="04d23-109">请在[GitHub 存储库](https://github.com/microsoftgraph/msgraph-training-java)中提供有关本教程的任何反馈。</span><span class="sxs-lookup"><span data-stu-id="04d23-109">Please provide any feedback on this tutorial in the [GitHub repository](https://github.com/microsoftgraph/msgraph-training-java).</span></span>
