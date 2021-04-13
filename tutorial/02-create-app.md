---
ms.openlocfilehash: b80de156a5ed1708ccafbaabf34b49b119f4099c
ms.sourcegitcommit: 5c09eff01b265ddfcca9090c14dca80a95320edd
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/13/2021
ms.locfileid: "51695798"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="a778f-101">在此部分中，你将创建一个基本Java控制台应用。</span><span class="sxs-lookup"><span data-stu-id="a778f-101">In this section you'll create a basic Java console app.</span></span>

1. <span data-ttu-id="a778f-102">在要创建项目的 (CLI) 打开命令行接口。</span><span class="sxs-lookup"><span data-stu-id="a778f-102">Open your command-line interface (CLI) in a directory where you want to create the project.</span></span> <span data-ttu-id="a778f-103">运行以下命令以创建新的 Gradle 项目。</span><span class="sxs-lookup"><span data-stu-id="a778f-103">Run the following command to create a new Gradle project.</span></span>

    ```Shell
    gradle init --dsl groovy --test-framework junit --type java-application --project-name graphtutorial --package graphtutorial
    ```

1. <span data-ttu-id="a778f-104">创建项目后，通过运行以下命令在 CLI 中运行应用来验证其是否正常工作。</span><span class="sxs-lookup"><span data-stu-id="a778f-104">Once the project is created, verify that it works by running the following command to run the app in your CLI.</span></span>

    ```Shell
    ./gradlew --console plain run
    ```

    <span data-ttu-id="a778f-105">如果运行正常，应用应输出 `Hello World.` 。</span><span class="sxs-lookup"><span data-stu-id="a778f-105">If it works, the app should output `Hello World.`.</span></span>

## <a name="install-dependencies"></a><span data-ttu-id="a778f-106">安装依赖项</span><span class="sxs-lookup"><span data-stu-id="a778f-106">Install dependencies</span></span>

<span data-ttu-id="a778f-107">在继续之前，添加一些你稍后将使用的附加依赖项。</span><span class="sxs-lookup"><span data-stu-id="a778f-107">Before moving on, add some additional dependencies that you will use later.</span></span>

- <span data-ttu-id="a778f-108">[用于验证用户Java](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/identity/azure-identity) 获取访问令牌的 Azure Identity 客户端库。</span><span class="sxs-lookup"><span data-stu-id="a778f-108">[Azure Identity client library for Java](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/identity/azure-identity) to authenticate the user and acquire access tokens.</span></span>
- <span data-ttu-id="a778f-109">[用于调用Java](https://github.com/microsoftgraph/msgraph-sdk-java) Microsoft Graph 的 Microsoft Graph SDK。</span><span class="sxs-lookup"><span data-stu-id="a778f-109">[Microsoft Graph SDK for Java](https://github.com/microsoftgraph/msgraph-sdk-java) to make calls to the Microsoft Graph.</span></span>

1. <span data-ttu-id="a778f-110">打开 **./build.gradle**。</span><span class="sxs-lookup"><span data-stu-id="a778f-110">Open **./build.gradle**.</span></span> <span data-ttu-id="a778f-111">更新 `dependencies` 部分以添加这些依赖项。</span><span class="sxs-lookup"><span data-stu-id="a778f-111">Update the `dependencies` section to add those dependencies.</span></span>

    :::code language="gradle" source="../demo/graphtutorial/build.gradle" id="DependenciesSnippet" highlight="7-8":::

1. <span data-ttu-id="a778f-112">将以下内容添加到 **./build.gradle 的末尾**。</span><span class="sxs-lookup"><span data-stu-id="a778f-112">Add the following to the end of **./build.gradle**.</span></span>

    :::code language="gradle" source="../demo/graphtutorial/build.gradle" id="StandardInputSnippet":::

<span data-ttu-id="a778f-113">下次生成项目时，Gradle 将下载这些依赖项。</span><span class="sxs-lookup"><span data-stu-id="a778f-113">The next time you build the project, Gradle will download those dependencies.</span></span>

## <a name="design-the-app"></a><span data-ttu-id="a778f-114">设计应用</span><span class="sxs-lookup"><span data-stu-id="a778f-114">Design the app</span></span>

1. <span data-ttu-id="a778f-115">打开 **./src/main/java/graphtu一一l/App.java** 文件，并将其内容替换为以下内容。</span><span class="sxs-lookup"><span data-stu-id="a778f-115">Open the **./src/main/java/graphtutorial/App.java** file and replace its contents with the following.</span></span>

    ```java
    package graphtutorial;

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
                System.out.println("2. View this week's calendar");
                System.out.println("3. Add an event");

                try {
                    choice = input.nextInt();
                } catch (InputMismatchException ex) {
                    // Skip over non-integer input
                }

                input.nextLine();

                // Process user choice
                switch(choice) {
                    case 0:
                        // Exit the program
                        System.out.println("Goodbye...");
                        break;
                    case 1:
                        // Display access token
                        break;
                    case 2:
                        // List the calendar
                        break;
                    case 3:
                        // Create a new event
                        break;
                    default:
                        System.out.println("Invalid choice");
                }
            }

            input.close();
        }
    }
    ```

    <span data-ttu-id="a778f-116">这将实现一个基本菜单，并读取命令行中的用户选择。</span><span class="sxs-lookup"><span data-stu-id="a778f-116">This implements a basic menu and reads the user's choice from the command line.</span></span>
