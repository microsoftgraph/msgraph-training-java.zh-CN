---
ms.openlocfilehash: 7118c8300b17adb66893324dd2f2269d9fb8d00b
ms.sourcegitcommit: 02054b307013cce781be2a3512ec1e54f1a322eb
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/12/2019
ms.locfileid: "35634701"
---
# <a name="completed-module-add-azure-ad-authentication"></a><span data-ttu-id="a3022-101">已完成模块: 添加 Azure AD 身份验证</span><span class="sxs-lookup"><span data-stu-id="a3022-101">Completed module: Add Azure AD authentication</span></span>

<span data-ttu-id="a3022-102">此目录中的项目版本反映了通过[添加 AZURE AD 身份验证](https://docs.microsoft.com/graph/tutorials/java?tutorial-step=3)完成教程。</span><span class="sxs-lookup"><span data-stu-id="a3022-102">The version of the project in this directory reflects completing the tutorial up through [Add Azure AD authentication](https://docs.microsoft.com/graph/tutorials/java?tutorial-step=3).</span></span> <span data-ttu-id="a3022-103">如果您使用此版本的项目, 您需要完成教程的其余部分 (从[获取日历数据](https://docs.microsoft.com/graph/tutorials/java?tutorial-step=4)开始)。</span><span class="sxs-lookup"><span data-stu-id="a3022-103">If you use this version of the project, you need to complete the rest of the tutorial starting at [Get calendar data](https://docs.microsoft.com/graph/tutorials/java?tutorial-step=4).</span></span>

> <span data-ttu-id="a3022-104">**注意:** 假定您已在应用注册门户中注册了应用程序, 如在[门户中注册应用](https://docs.microsoft.com/graph/tutorials/java?tutorial-step=2)程序中所指定的那样。</span><span class="sxs-lookup"><span data-stu-id="a3022-104">**Note:** It is assumed that you have already registered an application in the app registration portal as specified in [Register the app in the portal](https://docs.microsoft.com/graph/tutorials/java?tutorial-step=2).</span></span> <span data-ttu-id="a3022-105">您需要配置此示例版本, 如下所示:</span><span class="sxs-lookup"><span data-stu-id="a3022-105">You need to configure this version of the sample as follows:</span></span>
>
> 1. <span data-ttu-id="a3022-106">将`oAuth.properties.example`文件重命名`oAuth.properties`为。</span><span class="sxs-lookup"><span data-stu-id="a3022-106">Rename the `oAuth.properties.example` file to `oAuth.properties`.</span></span>
> 1. <span data-ttu-id="a3022-107">编辑`oAuth.properties`文件并进行以下更改。</span><span class="sxs-lookup"><span data-stu-id="a3022-107">Edit the `oAuth.properties` file and make the following changes.</span></span>
>     1. <span data-ttu-id="a3022-108">将`YOUR_APP_ID_HERE`替换为你从应用注册门户获取的**应用程序 Id** 。</span><span class="sxs-lookup"><span data-stu-id="a3022-108">Replace `YOUR_APP_ID_HERE` with the **Application Id** you got from the App Registration Portal.</span></span>
