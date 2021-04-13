---
ms.openlocfilehash: bbb71d370fea90a3f7d2eae2f27d04f7e19d3d94
ms.sourcegitcommit: 5c09eff01b265ddfcca9090c14dca80a95320edd
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/13/2021
ms.locfileid: "51695784"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="fab61-101">在此部分中，您将添加在用户日历上创建事件的能力。</span><span class="sxs-lookup"><span data-stu-id="fab61-101">In this section you will add the ability to create events on the user's calendar.</span></span>

1. <span data-ttu-id="fab61-102">打开 **./graphtu一l/src/main/java/graphtu一l/Graph.java，** 将以下函数添加到 **Graph** 类。</span><span class="sxs-lookup"><span data-stu-id="fab61-102">Open **./graphtutorial/src/main/java/graphtutorial/Graph.java** and add the following function to the **Graph** class.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/Graph.java" id="CreateEventSnippet":::

1. <span data-ttu-id="fab61-103">打开 **./graphtu一l/src/main/java/graphtu一l/App.java，** 将以下函数添加到 **App** 类。</span><span class="sxs-lookup"><span data-stu-id="fab61-103">Open **./graphtutorial/src/main/java/graphtutorial/App.java** and add the following function to the **App** class.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/App.java" id="CreateEventSnippet":::

    <span data-ttu-id="fab61-104">此函数会提示用户输入主题、与会者、开始、结束和正文，然后使用这些值调用 `Graph.createEvent` 。</span><span class="sxs-lookup"><span data-stu-id="fab61-104">This function prompts the user for subject, attendees, start, end, and body, then uses those values to call `Graph.createEvent`.</span></span>

1. <span data-ttu-id="fab61-105">在 函数中的注释 `// Create a new event` 后添加 `Main` 以下内容。</span><span class="sxs-lookup"><span data-stu-id="fab61-105">Add the following just after the `// Create a new event` comment in the `Main` function.</span></span>

    ```java
    createEvent(user.mailboxSettings.timeZone, input);
    ```

1. <span data-ttu-id="fab61-106">保存所有更改并运行应用。</span><span class="sxs-lookup"><span data-stu-id="fab61-106">Save all of your changes and run the app.</span></span> <span data-ttu-id="fab61-107">选择" **添加事件"** 选项。</span><span class="sxs-lookup"><span data-stu-id="fab61-107">Choose the **Add an event** option.</span></span> <span data-ttu-id="fab61-108">响应提示以在用户日历上创建新事件。</span><span class="sxs-lookup"><span data-stu-id="fab61-108">Respond to the prompts to create a new event on the user's calendar.</span></span>
