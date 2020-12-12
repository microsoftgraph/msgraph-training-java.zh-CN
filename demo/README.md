---
ms.openlocfilehash: 50206b77504979c1cf67b4d0daa0b6f6576bde32
ms.sourcegitcommit: eb935a250f8531b04a42710356072b80d46ee3a4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/11/2020
ms.locfileid: "49661051"
---
# <a name="how-to-run-the-completed-project"></a><span data-ttu-id="5aed8-101">如何运行已完成的项目</span><span class="sxs-lookup"><span data-stu-id="5aed8-101">How to run the completed project</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5aed8-102">先决条件</span><span class="sxs-lookup"><span data-stu-id="5aed8-102">Prerequisites</span></span>

<span data-ttu-id="5aed8-103">若要在此文件夹中运行已完成的项目，您需要以下各项：</span><span class="sxs-lookup"><span data-stu-id="5aed8-103">To run the completed project in this folder, you need the following:</span></span>

- <span data-ttu-id="5aed8-104">[Java安装在开发 (JDK) ](https://java.com/en/download/faq/develop.xml)[和 Gradle](https://gradle.org/)的 SE 开发工具包。</span><span class="sxs-lookup"><span data-stu-id="5aed8-104">[Java SE Development Kit (JDK)](https://java.com/en/download/faq/develop.xml) and [Gradle](https://gradle.org/) installed on your development machine.</span></span> <span data-ttu-id="5aed8-105">如果你没有 JDK 或 Gradle，请访问前面的链接，查看下载选项。</span><span class="sxs-lookup"><span data-stu-id="5aed8-105">If you do not have the JDK or Gradle, visit the previous links for download options.</span></span> <span data-ttu-id="5aed8-106"> (**注意：** 本教程是使用 OpenJDK 版本 14.0.0.36 和 Gradle 6.7.1 编写的。</span><span class="sxs-lookup"><span data-stu-id="5aed8-106">(**Note:** This tutorial was written with OpenJDK version 14.0.0.36 and Gradle 6.7.1.</span></span> <span data-ttu-id="5aed8-107">本指南中的步骤可能与其他版本一样，但尚未测试。) </span><span class="sxs-lookup"><span data-stu-id="5aed8-107">The steps in this guide may work with other versions, but that has not been tested.)</span></span>
- <span data-ttu-id="5aed8-108">Microsoft 工作或学校帐户。</span><span class="sxs-lookup"><span data-stu-id="5aed8-108">A Microsoft work or school account.</span></span>

<span data-ttu-id="5aed8-109">如果你没有 Microsoft 帐户，可以注册 [Office 365 开发人员](https://developer.microsoft.com/office/dev-program) 计划，获取免费的 Office 365 订阅。</span><span class="sxs-lookup"><span data-stu-id="5aed8-109">If you don't have a Microsoft account, you can [sign up for the Office 365 Developer Program](https://developer.microsoft.com/office/dev-program) to get a free Office 365 subscription.</span></span>

## <a name="register-a-web-application-with-the-azure-active-directory-admin-center"></a><span data-ttu-id="5aed8-110">向 Azure Active Directory 管理中心注册 Web 应用程序</span><span class="sxs-lookup"><span data-stu-id="5aed8-110">Register a web application with the Azure Active Directory admin center</span></span>

1. <span data-ttu-id="5aed8-111">打开浏览器，并转到 [Azure Active Directory 管理中心](https://aad.portal.azure.com)。</span><span class="sxs-lookup"><span data-stu-id="5aed8-111">Open a browser and navigate to the [Azure Active Directory admin center](https://aad.portal.azure.com).</span></span> <span data-ttu-id="5aed8-112">使用 **个人帐户**（亦称为“Microsoft 帐户”）或 **工作或学校帐户** 登录。</span><span class="sxs-lookup"><span data-stu-id="5aed8-112">Login using a **personal account** (aka: Microsoft Account) or **Work or School Account**.</span></span>

1. <span data-ttu-id="5aed8-113">选择左侧导航栏中的“**Azure Active Directory**”，再选择“**管理**”下的“**应用注册**”。</span><span class="sxs-lookup"><span data-stu-id="5aed8-113">Select **Azure Active Directory** in the left-hand navigation, then select **App registrations** under **Manage**.</span></span>

    ![<span data-ttu-id="5aed8-114">应用注册的屏幕截图</span><span class="sxs-lookup"><span data-stu-id="5aed8-114">A screenshot of the App registrations</span></span> ](/tutorial/images/aad-portal-app-registrations.png)

1. <span data-ttu-id="5aed8-115">选择“新注册”。</span><span class="sxs-lookup"><span data-stu-id="5aed8-115">Select **New registration**.</span></span> <span data-ttu-id="5aed8-116">在“注册应用”页上，按如下方式设置值。</span><span class="sxs-lookup"><span data-stu-id="5aed8-116">On the **Register an application** page, set the values as follows.</span></span>

    - <span data-ttu-id="5aed8-117">将“名称”设置为“`Java Graph Tutorial`”。</span><span class="sxs-lookup"><span data-stu-id="5aed8-117">Set **Name** to `Java Graph Tutorial`.</span></span>
    - <span data-ttu-id="5aed8-118">将 **支持的帐户类型设置为\*\*\*\*任何组织目录中的帐户**。</span><span class="sxs-lookup"><span data-stu-id="5aed8-118">Set **Supported account types** to **Accounts in any organizational directory**.</span></span>
    - <span data-ttu-id="5aed8-119">保留“重定向 URI”为空。</span><span class="sxs-lookup"><span data-stu-id="5aed8-119">Leave **Redirect URI** empty.</span></span>

    !["注册应用程序"页的屏幕截图](/tutorial/images/aad-register-an-app.png)

1. <span data-ttu-id="5aed8-121">选择 **“注册”**。</span><span class="sxs-lookup"><span data-stu-id="5aed8-121">Choose **Register**.</span></span> <span data-ttu-id="5aed8-122">在 **"Java Graph** 教程"页上，复制 **Application (客户端**) ID 并保存它，下一步中将需要该值。</span><span class="sxs-lookup"><span data-stu-id="5aed8-122">On the **Java Graph Tutorial** page, copy the value of the **Application (client) ID** and save it, you will need it in the next step.</span></span>

    ![新应用注册的应用程序 ID 屏幕截图](/tutorial/images/aad-application-id.png)

1. <span data-ttu-id="5aed8-124">选择 **"添加重定向 URI"** 链接。</span><span class="sxs-lookup"><span data-stu-id="5aed8-124">Select the **Add a Redirect URI** link.</span></span> <span data-ttu-id="5aed8-125">在" **重定向 URI"** 页上，找到"建议用于移动 **、桌面 (客户端** 的重定向 URI) 部分。</span><span class="sxs-lookup"><span data-stu-id="5aed8-125">On the **Redirect URIs** page, locate the **Suggested Redirect URIs for public clients (mobile, desktop)** section.</span></span> <span data-ttu-id="5aed8-126">选择 `https://login.microsoftonline.com/common/oauth2/nativeclient` URI。</span><span class="sxs-lookup"><span data-stu-id="5aed8-126">Select the `https://login.microsoftonline.com/common/oauth2/nativeclient` URI.</span></span>

    ![重定向 URI 页面的屏幕截图](/tutorial/images/aad-redirect-uris.png)

1. <span data-ttu-id="5aed8-128">找到 **"默认客户端类型**"部分，将 **"将应用程序视为公共客户端**"开关更改为 **"是**"，然后选择"**保存"。**</span><span class="sxs-lookup"><span data-stu-id="5aed8-128">Locate the **Default client type** section and change the **Treat application as a public client** toggle to **Yes**, then choose **Save**.</span></span>

    !["默认客户端类型"部分屏幕截图](/tutorial/images/aad-default-client-type.png)

## <a name="configure-the-sample"></a><span data-ttu-id="5aed8-130">配置示例</span><span class="sxs-lookup"><span data-stu-id="5aed8-130">Configure the sample</span></span>

1. <span data-ttu-id="5aed8-131">将文件 `oAuth.properties.example` 重命名为 `oAuth.properties` 。</span><span class="sxs-lookup"><span data-stu-id="5aed8-131">Rename the `oAuth.properties.example` file to `oAuth.properties`.</span></span>
1. <span data-ttu-id="5aed8-132">编辑 `oAuth.properties` 文件并做出以下更改。</span><span class="sxs-lookup"><span data-stu-id="5aed8-132">Edit the `oAuth.properties` file and make the following changes.</span></span>
    1. <span data-ttu-id="5aed8-133">替换为 `YOUR_APP_ID_HERE` 从 **应用** 注册门户获得的应用程序 ID。</span><span class="sxs-lookup"><span data-stu-id="5aed8-133">Replace `YOUR_APP_ID_HERE` with the **Application Id** you got from the App Registration Portal.</span></span>

## <a name="build-and-run-the-sample"></a><span data-ttu-id="5aed8-134">生成和运行示例</span><span class="sxs-lookup"><span data-stu-id="5aed8-134">Build and run the sample</span></span>

<span data-ttu-id="5aed8-135">在命令行界面 (CLI) ，导航到项目目录并运行以下命令。</span><span class="sxs-lookup"><span data-stu-id="5aed8-135">In your command-line interface (CLI), navigate to the project directory and run the following commands.</span></span>

```Shell
./gradlew --console plain run
```
