---
ms.openlocfilehash: 72936993d940cdfb86c864a6ffc543ed466127d1
ms.sourcegitcommit: eb935a250f8531b04a42710356072b80d46ee3a4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/11/2020
ms.locfileid: "49661079"
---
<!-- markdownlint-disable MD002 MD041 -->

在此部分中，你将创建一个基本的Java控制台应用。

1. 在要创建项目的目录中 (CLI) 打开命令行界面。 运行以下命令以创建新的 Gradle 项目。

    ```Shell
    gradle init --dsl groovy --test-framework junit --type java-application --project-name graphtutorial --package graphtutorial
    ```

1. 创建项目后，通过运行以下命令以在 CLI 中运行应用来验证项目是否正常工作。

    ```Shell
    ./gradlew --console plain run
    ```

    如果运行正常，应用应输出 `Hello World.` 。

## <a name="install-dependencies"></a>安装依赖项

在继续之前，添加一些稍后将使用的其他依赖项。

- [Microsoft 身份验证库 (MSAL) Java](https://github.com/AzureAD/microsoft-authentication-library-for-java) 验证用户身份并获取访问令牌。
- [用于调用Java](https://github.com/microsoftgraph/msgraph-sdk-java) Microsoft Graph 的 Microsoft Graph SDK。
- [SLF4J NOP 绑定](https://mvnrepository.com/artifact/org.slf4j/slf4j-nop) ，以禁止从 MSAL 进行日志记录。

1. 打开 **./build.gradle**。 更新 `dependencies` 节以添加这些依赖项。

    :::code language="gradle" source="../demo/graphtutorial/build.gradle" id="DependenciesSnippet" highlight="7-9":::

1. 将以下内容添加到 **./build.gradle 的末尾**。

    :::code language="gradle" source="../demo/graphtutorial/build.gradle" id="StandardInputSnippet":::

下次生成项目时，Gradle 将下载这些依赖项。

## <a name="design-the-app"></a>设计应用

1. 打开 **./src/main/java/graphtu一l/App.java** 文件，并将其内容替换为以下内容。

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

    这将实现基本菜单，并读取命令行中的用户选择。
