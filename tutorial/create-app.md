---
ms.openlocfilehash: e731c1caff6986556a1bfec6b669ddcb35a3af4c
ms.sourcegitcommit: 02054b307013cce781be2a3512ec1e54f1a322eb
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/12/2019
ms.locfileid: "35634757"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="e64c9-101">在要创建项目的目录中打开命令行界面 (CLI)。</span><span class="sxs-lookup"><span data-stu-id="e64c9-101">Open your command-line interface (CLI) in a directory where you want to create the project.</span></span> <span data-ttu-id="e64c9-102">运行以下命令以创建新的 Maven 项目。</span><span class="sxs-lookup"><span data-stu-id="e64c9-102">Run the following command to create a new Maven project.</span></span>

```Shell
mvn archetype:generate -DarchetypeArtifactId=maven-archetype-quickstart -DarchetypeGroupId=org.apache.maven.archetypes -DgroupId=com.contoso -DartifactId=graphtutorial -Dversion=1.0-SNAPSHOT
```

> [!IMPORTANT]
> <span data-ttu-id="e64c9-103">您可以输入与上面指定的值不同的`DgroupId`组 id (参数) 和`DartifactId`项目 ID (参数) 的不同值。</span><span class="sxs-lookup"><span data-stu-id="e64c9-103">You can enter different values for the group ID (`DgroupId` parameter) and artifact ID (`DartifactId` parameter) than the values specified above.</span></span> <span data-ttu-id="e64c9-104">本教程中的示例代码假定使用了组 ID `com.contoso` 。</span><span class="sxs-lookup"><span data-stu-id="e64c9-104">The sample code in this tutorial assumes that the group ID `com.contoso` was used.</span></span> <span data-ttu-id="e64c9-105">如果使用不同的值, 请务必将任何示例`com.contoso`代码中的替换为您的组 ID。</span><span class="sxs-lookup"><span data-stu-id="e64c9-105">If you use a different value, be sure to replace `com.contoso` in any sample code with your group ID.</span></span>

<span data-ttu-id="e64c9-106">出现提示时, 请确认配置, 然后等待创建项目。</span><span class="sxs-lookup"><span data-stu-id="e64c9-106">When prompted, confirm the configuration, then wait for the project to be created.</span></span> <span data-ttu-id="e64c9-107">创建项目后, 通过运行以下命令来打包并在 CLI 中运行应用程序, 以验证它是否正常工作。</span><span class="sxs-lookup"><span data-stu-id="e64c9-107">Once the project is created, verify that it works by running the following commands to package and run the app in your CLI.</span></span>

```Shell
mvn package
java -cp target/graphtutorial-1.0-SNAPSHOT.jar com.contoso.App
```

<span data-ttu-id="e64c9-108">如果它正常运行, 则应用应`Hello World!`输出。</span><span class="sxs-lookup"><span data-stu-id="e64c9-108">If it works, the app should output `Hello World!`.</span></span> <span data-ttu-id="e64c9-109">在继续之前, 请添加一些以后将使用的其他依赖项。</span><span class="sxs-lookup"><span data-stu-id="e64c9-109">Before moving on, add some additional dependencies that you will use later.</span></span>

- <span data-ttu-id="e64c9-110">[适用于 Java 的 Microsoft 身份验证库 (MSAL)](https://github.com/AzureAD/microsoft-authentication-library-for-java) , 用于对用户进行身份验证并获取访问令牌。</span><span class="sxs-lookup"><span data-stu-id="e64c9-110">[Microsoft Authentication Library (MSAL) for Java](https://github.com/AzureAD/microsoft-authentication-library-for-java) to authenticate the user and acquire access tokens.</span></span>
- <span data-ttu-id="e64c9-111">[Microsoft GRAPH SDK For Java](https://github.com/microsoftgraph/msgraph-sdk-java) , 用于调用 microsoft graph。</span><span class="sxs-lookup"><span data-stu-id="e64c9-111">[Microsoft Graph SDK for Java](https://github.com/microsoftgraph/msgraph-sdk-java) to make calls to the Microsoft Graph.</span></span>
- <span data-ttu-id="e64c9-112">[SLF4J NOP 绑定](https://mvnrepository.com/artifact/org.slf4j/slf4j-nop)到禁止显示来自 MSAL 的日志记录。</span><span class="sxs-lookup"><span data-stu-id="e64c9-112">[SLF4J NOP Binding](https://mvnrepository.com/artifact/org.slf4j/slf4j-nop) to suppress logging from MSAL.</span></span>

<span data-ttu-id="e64c9-113">打开 **./graphtutorial/pom.xml**。</span><span class="sxs-lookup"><span data-stu-id="e64c9-113">Open **./graphtutorial/pom.xml**.</span></span> <span data-ttu-id="e64c9-114">在`<dependencies>`元素内添加以下项。</span><span class="sxs-lookup"><span data-stu-id="e64c9-114">Add the following inside the `<dependencies>` element.</span></span>

```xml
<dependency>
  <groupId>org.slf4j</groupId>
  <artifactId>slf4j-nop</artifactId>
  <version>1.8.0-beta4</version>
</dependency>

<dependency>
  <groupId>com.microsoft.graph</groupId>
  <artifactId>microsoft-graph</artifactId>
  <version>1.4.0</version>
</dependency>

<dependency>
  <groupId>com.microsoft.azure</groupId>
  <artifactId>msal4j</artifactId>
  <version>0.4.0-preview</version>
</dependency>
```

<span data-ttu-id="e64c9-115">下次生成项目时, Maven 将下载这些依赖项。</span><span class="sxs-lookup"><span data-stu-id="e64c9-115">The next time you build the project, Maven will download those dependencies.</span></span>

## <a name="design-the-app"></a><span data-ttu-id="e64c9-116">设计应用程序</span><span class="sxs-lookup"><span data-stu-id="e64c9-116">Design the app</span></span>

<span data-ttu-id="e64c9-117">打开 " **./graphtutorial/src/main/java/com/contoso/App.java** " 文件, 并将其内容替换为以下内容。</span><span class="sxs-lookup"><span data-stu-id="e64c9-117">Open the **./graphtutorial/src/main/java/com/contoso/App.java** file and replace its contents with the following.</span></span>

```java
package com.contoso;

import java.util.InputMismatchException;
import java.util.Scanner;

/**
 * Graph Tutorial
 *
 */
public class App {
    public static void main(String[] args) {
        System.out.println("Java Graph Tutorial");
        System.out.println();

        Scanner input = new Scanner(System.in);

        int choice = -1;

        while (choice != 0) {
            System.out.println("Please choose one of the following options:");
            System.out.println("0. Exit");
            System.out.println("1. Display access token");
            System.out.println("2. List calendar events");

            try {
                choice = input.nextInt();
            } catch (InputMismatchException ex) {
                // Skip over non-integer input
                input.nextLine();
            }

            // Process user choice
            switch(choice) {
                case 0:
                    // Exit the program
                    System.out.println("Goodbye...");
                    break;
                case 1:
                    // Display access token
                case 2:
                    // List the calendar
                    break;
                default:
                    System.out.println("Invalid choice");
            }
        }

        input.close();
    }
}
```

<span data-ttu-id="e64c9-118">这将实现一个基本菜单, 并从命令行读取用户的选择。</span><span class="sxs-lookup"><span data-stu-id="e64c9-118">This implements a basic menu and reads the user's choice from the command line.</span></span>
