---
ms.openlocfilehash: bbb71d370fea90a3f7d2eae2f27d04f7e19d3d94
ms.sourcegitcommit: 5c09eff01b265ddfcca9090c14dca80a95320edd
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/13/2021
ms.locfileid: "51695784"
---
<!-- markdownlint-disable MD002 MD041 -->

在此部分中，您将添加在用户日历上创建事件的能力。

1. 打开 **./graphtu一l/src/main/java/graphtu一l/Graph.java，** 将以下函数添加到 **Graph** 类。

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/Graph.java" id="CreateEventSnippet":::

1. 打开 **./graphtu一l/src/main/java/graphtu一l/App.java，** 将以下函数添加到 **App** 类。

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/App.java" id="CreateEventSnippet":::

    此函数会提示用户输入主题、与会者、开始、结束和正文，然后使用这些值调用 `Graph.createEvent` 。

1. 在 函数中的注释 `// Create a new event` 后添加 `Main` 以下内容。

    ```java
    createEvent(user.mailboxSettings.timeZone, input);
    ```

1. 保存所有更改并运行应用。 选择" **添加事件"** 选项。 响应提示以在用户日历上创建新事件。
