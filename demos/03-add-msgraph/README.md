---
ms.openlocfilehash: a890c296f4269b674356118463578c9b79001b0e
ms.sourcegitcommit: 02054b307013cce781be2a3512ec1e54f1a322eb
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/12/2019
ms.locfileid: "35634715"
---
# <a name="how-to-run-the-completed-project"></a><span data-ttu-id="500db-101">如何运行已完成的项目</span><span class="sxs-lookup"><span data-stu-id="500db-101">How to run the completed project</span></span>

## <a name="prerequisites"></a><span data-ttu-id="500db-102">先决条件</span><span class="sxs-lookup"><span data-stu-id="500db-102">Prerequisites</span></span>

<span data-ttu-id="500db-103">若要在此文件夹中运行已完成的项目, 您需要以下各项:</span><span class="sxs-lookup"><span data-stu-id="500db-103">To run the completed project in this folder, you need the following:</span></span>

- <span data-ttu-id="500db-104">在开发计算机上安装的[JAVA SE 开发工具包 (JDK)](https://java.com/en/download/faq/develop.xml)和[Maven](https://maven.apache.org/) 。</span><span class="sxs-lookup"><span data-stu-id="500db-104">[Java SE Development Kit (JDK)](https://java.com/en/download/faq/develop.xml) and [Maven](https://maven.apache.org/) installed on your development machine.</span></span> <span data-ttu-id="500db-105">如果您没有 JDK 或 Maven, 请访问以前的链接获取下载选项。</span><span class="sxs-lookup"><span data-stu-id="500db-105">If you do not have the JDK or Maven, visit the previous links for download options.</span></span> <span data-ttu-id="500db-106">(**注意:** 本教程是使用 OpenJDK 版本12.0.1 和 Maven 3.6.1 编写的。</span><span class="sxs-lookup"><span data-stu-id="500db-106">(**Note:** This tutorial was written with OpenJDK version 12.0.1 and Maven 3.6.1.</span></span> <span data-ttu-id="500db-107">本指南中的步骤可能适用于其他版本, 但尚未经过测试。</span><span class="sxs-lookup"><span data-stu-id="500db-107">The steps in this guide may work with other versions, but that has not been tested.)</span></span>
- <span data-ttu-id="500db-108">Microsoft 工作或学校帐户。</span><span class="sxs-lookup"><span data-stu-id="500db-108">A Microsoft work or school account.</span></span>

<span data-ttu-id="500db-109">如果你没有 Microsoft 帐户, 可以[注册 office 365 开发人员计划](https://developer.microsoft.com/office/dev-program)以获取免费的 office 365 订阅。</span><span class="sxs-lookup"><span data-stu-id="500db-109">If you don't have a Microsoft account, you can [sign up for the Office 365 Developer Program](https://developer.microsoft.com/office/dev-program) to get a free Office 365 subscription.</span></span>

## <a name="register-a-web-application-with-the-azure-active-directory-admin-center"></a><span data-ttu-id="500db-110">向 Azure Active Directory 管理中心注册 web 应用程序</span><span class="sxs-lookup"><span data-stu-id="500db-110">Register a web application with the Azure Active Directory admin center</span></span>

1. <span data-ttu-id="500db-111">打开浏览器，并转到 [Azure Active Directory 管理中心](https://aad.portal.azure.com)。</span><span class="sxs-lookup"><span data-stu-id="500db-111">Open a browser and navigate to the [Azure Active Directory admin center](https://aad.portal.azure.com).</span></span> <span data-ttu-id="500db-112">使用**个人帐户**（亦称为“Microsoft 帐户”）或**工作或学校帐户**登录。</span><span class="sxs-lookup"><span data-stu-id="500db-112">Login using a **personal account** (aka: Microsoft Account) or **Work or School Account**.</span></span>

1. <span data-ttu-id="500db-113">在左侧导航栏中选择 " **Azure Active Directory** ", 然后选择 "**管理**" 下的 "**应用程序注册**"。</span><span class="sxs-lookup"><span data-stu-id="500db-113">Select **Azure Active Directory** in the left-hand navigation, then select **App registrations** under **Manage**.</span></span>

    ![<span data-ttu-id="500db-114">应用注册的屏幕截图</span><span class="sxs-lookup"><span data-stu-id="500db-114">A screenshot of the App registrations</span></span> ](/tutorial/images/aad-portal-app-registrations.png)

1. <span data-ttu-id="500db-115">选择“新注册”\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="500db-115">Select **New registration**.</span></span> <span data-ttu-id="500db-116">在“注册应用”\*\*\*\* 页上，按如下方式设置值。</span><span class="sxs-lookup"><span data-stu-id="500db-116">On the **Register an application** page, set the values as follows.</span></span>

    - <span data-ttu-id="500db-117">将“名称”\*\*\*\* 设置为“`Java Graph Tutorial`”。</span><span class="sxs-lookup"><span data-stu-id="500db-117">Set **Name** to `Java Graph Tutorial`.</span></span>
    - <span data-ttu-id="500db-118">将**支持的帐户类型**设置为**任何组织目录中的帐户**。</span><span class="sxs-lookup"><span data-stu-id="500db-118">Set **Supported account types** to **Accounts in any organizational directory**.</span></span>
    - <span data-ttu-id="500db-119">保留“重定向 URI”\*\*\*\* 为空。</span><span class="sxs-lookup"><span data-stu-id="500db-119">Leave **Redirect URI** empty.</span></span>

    !["注册应用程序" 页的屏幕截图](/tutorial/images/aad-register-an-app.png)

1. <span data-ttu-id="500db-121">选择“注册”\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="500db-121">Choose **Register**.</span></span> <span data-ttu-id="500db-122">在 " **Java Graph 教程**" 页上, 复制**应用程序 (客户端) ID**的值并保存它, 下一步将需要它。</span><span class="sxs-lookup"><span data-stu-id="500db-122">On the **Java Graph Tutorial** page, copy the value of the **Application (client) ID** and save it, you will need it in the next step.</span></span>

    ![新应用注册的应用程序 ID 的屏幕截图](/tutorial/images/aad-application-id.png)

1. <span data-ttu-id="500db-124">选择 "**添加重定向 URI** " 链接。</span><span class="sxs-lookup"><span data-stu-id="500db-124">Select the **Add a Redirect URI** link.</span></span> <span data-ttu-id="500db-125">在 "**重定向 uri** " 页上, 找到 "**公共客户端 (移动、桌面)** " 部分的 "建议的重定向 uri" 部分。</span><span class="sxs-lookup"><span data-stu-id="500db-125">On the **Redirect URIs** page, locate the **Suggested Redirect URIs for public clients (mobile, desktop)** section.</span></span> <span data-ttu-id="500db-126">选择`https://login.microsoftonline.com/common/oauth2/nativeclient` URI。</span><span class="sxs-lookup"><span data-stu-id="500db-126">Select the `https://login.microsoftonline.com/common/oauth2/nativeclient` URI.</span></span>

    !["重定向 Uri" 页的屏幕截图](/tutorial/images/aad-redirect-uris.png)

1. <span data-ttu-id="500db-128">找到 "**默认客户端类型**" 部分, 将 "将**应用程序视为公用客户端**" 切换为 **"是"**, 然后选择 "**保存**"。</span><span class="sxs-lookup"><span data-stu-id="500db-128">Locate the **Default client type** section and change the **Treat application as a public client** toggle to **Yes**, then choose **Save**.</span></span>

    ![默认 "客户端类型" 部分的屏幕截图](/tutorial/images/aad-default-client-type.png)

## <a name="configure-the-sample"></a><span data-ttu-id="500db-130">配置示例</span><span class="sxs-lookup"><span data-stu-id="500db-130">Configure the sample</span></span>

1. <span data-ttu-id="500db-131">将`oAuth.properties.example`文件重命名`oAuth.properties`为。</span><span class="sxs-lookup"><span data-stu-id="500db-131">Rename the `oAuth.properties.example` file to `oAuth.properties`.</span></span>
1. <span data-ttu-id="500db-132">编辑`oAuth.properties`文件并进行以下更改。</span><span class="sxs-lookup"><span data-stu-id="500db-132">Edit the `oAuth.properties` file and make the following changes.</span></span>
    1. <span data-ttu-id="500db-133">将`YOUR_APP_ID_HERE`替换为你从应用注册门户获取的**应用程序 Id** 。</span><span class="sxs-lookup"><span data-stu-id="500db-133">Replace `YOUR_APP_ID_HERE` with the **Application Id** you got from the App Registration Portal.</span></span>

## <a name="build-and-run-the-sample"></a><span data-ttu-id="500db-134">生成和运行示例</span><span class="sxs-lookup"><span data-stu-id="500db-134">Build and run the sample</span></span>

<span data-ttu-id="500db-135">在命令行界面 (CLI) 中, 导航到项目目录并运行以下命令。</span><span class="sxs-lookup"><span data-stu-id="500db-135">In your command-line interface (CLI), navigate to the project directory and run the following commands.</span></span>

```Shell
mvn package
java -cp target/graphtutorial-1.0-SNAPSHOT.jar com.contoso.App
```
