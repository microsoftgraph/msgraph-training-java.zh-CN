---
ms.openlocfilehash: 72936993d940cdfb86c864a6ffc543ed466127d1
ms.sourcegitcommit: eb935a250f8531b04a42710356072b80d46ee3a4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/11/2020
ms.locfileid: "49661079"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="33b82-101">在此部分中，你将创建一个基本的Java控制台应用。</span><span class="sxs-lookup"><span data-stu-id="33b82-101">In this section you'll create a basic Java console app.</span></span>

1. <span data-ttu-id="33b82-102">在要创建项目的目录中 (CLI) 打开命令行界面。</span><span class="sxs-lookup"><span data-stu-id="33b82-102">Open your command-line interface (CLI) in a directory where you want to create the project.</span></span> <span data-ttu-id="33b82-103">运行以下命令以创建新的 Gradle 项目。</span><span class="sxs-lookup"><span data-stu-id="33b82-103">Run the following command to create a new Gradle project.</span></span>

    ```Shell
    gradle init --dsl groovy --test-framework junit --type java-application --project-name graphtutorial --package graphtutorial
    ```

1. <span data-ttu-id="33b82-104">创建项目后，通过运行以下命令以在 CLI 中运行应用来验证项目是否正常工作。</span><span class="sxs-lookup"><span data-stu-id="33b82-104">Once the project is created, verify that it works by running the following command to run the app in your CLI.</span></span>

    ```Shell
    ./gradlew --console plain run
    ```

    <span data-ttu-id="33b82-105">如果运行正常，应用应输出 `Hello World.` 。</span><span class="sxs-lookup"><span data-stu-id="33b82-105">If it works, the app should output `Hello World.`.</span></span>

## <a name="install-dependencies"></a><span data-ttu-id="33b82-106">安装依赖项</span><span class="sxs-lookup"><span data-stu-id="33b82-106">Install dependencies</span></span>

<span data-ttu-id="33b82-107">在继续之前，添加一些稍后将使用的其他依赖项。</span><span class="sxs-lookup"><span data-stu-id="33b82-107">Before moving on, add some additional dependencies that you will use later.</span></span>

- <span data-ttu-id="33b82-108">[Microsoft 身份验证库 (MSAL) Java](https://github.com/AzureAD/microsoft-authentication-library-for-java) 验证用户身份并获取访问令牌。</span><span class="sxs-lookup"><span data-stu-id="33b82-108">[Microsoft Authentication Library (MSAL) for Java](https://github.com/AzureAD/microsoft-authentication-library-for-java) to authenticate the user and acquire access tokens.</span></span>
- <span data-ttu-id="33b82-109">[用于调用Java](https://github.com/microsoftgraph/msgraph-sdk-java) Microsoft Graph 的 Microsoft Graph SDK。</span><span class="sxs-lookup"><span data-stu-id="33b82-109">[Microsoft Graph SDK for Java](https://github.com/microsoftgraph/msgraph-sdk-java) to make calls to the Microsoft Graph.</span></span>
- <span data-ttu-id="33b82-110">[SLF4J NOP 绑定](https://mvnrepository.com/artifact/org.slf4j/slf4j-nop) ，以禁止从 MSAL 进行日志记录。</span><span class="sxs-lookup"><span data-stu-id="33b82-110">[SLF4J NOP Binding](https://mvnrepository.com/artifact/org.slf4j/slf4j-nop) to suppress logging from MSAL.</span></span>

1. <span data-ttu-id="33b82-111">打开 **./build.gradle**。</span><span class="sxs-lookup"><span data-stu-id="33b82-111">Open **./build.gradle**.</span></span> <span data-ttu-id="33b82-112">更新 `dependencies` 节以添加这些依赖项。</span><span class="sxs-lookup"><span data-stu-id="33b82-112">Update the `dependencies` section to add those dependencies.</span></span>

    :::code language="gradle" source="../demo/graphtutorial/build.gradle" id="DependenciesSnippet" highlight="7-9":::

1. <span data-ttu-id="33b82-113">将以下内容添加到 **./build.gradle 的末尾**。</span><span class="sxs-lookup"><span data-stu-id="33b82-113">Add the following to the end of **./build.gradle**.</span></span>

    :::code language="gradle" source="../demo/graphtutorial/build.gradle" id="StandardInputSnippet":::

<span data-ttu-id="33b82-114">下次生成项目时，Gradle 将下载这些依赖项。</span><span class="sxs-lookup"><span data-stu-id="33b82-114">The next time you build the project, Gradle will download those dependencies.</span></span>

## <a name="design-the-app"></a><span data-ttu-id="33b82-115">设计应用</span><span class="sxs-lookup"><span data-stu-id="33b82-115">Design the app</span></span>

1. <span data-ttu-id="33b82-116">打开 **./src/main/java/graphtu一l/App.java** 文件，并将其内容替换为以下内容。</span><span class="sxs-lookup"><span data-stu-id="33b82-116">Open the **./src/main/java/graphtutorial/App.java** file and replace its contents with the following.</span></span>

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

    <span data-ttu-id="33b82-117">这将实现基本菜单，并读取命令行中的用户选择。</span><span class="sxs-lookup"><span data-stu-id="33b82-117">This implements a basic menu and reads the user's choice from the command line.</span></span>
