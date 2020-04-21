---
ms.openlocfilehash: 381e4166f07e1dbc51c072645f17002e43f6cc16
ms.sourcegitcommit: 189f87d879c57b11992e7bc75580b4c69e014122
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/21/2020
ms.locfileid: "43612017"
---
<!-- markdownlint-disable MD002 MD041 -->

在本节中，您将创建一个基本的 Java 控制台应用程序。

1. 在要创建项目的目录中打开命令行界面（CLI）。 运行以下命令以创建新的 Gradle 项目。

    ```Shell
    gradle init --dsl groovy --test-framework junit --type java-application --project-name graphtutorial --package graphtutorial
    ```

1. 创建项目后，通过运行以下命令在 CLI 中运行应用程序来验证该项目是否正常工作。

    ```Shell
    ./gradlew --console plain run
    ```

    如果它正常运行，则应用应`Hello World.`输出。

## <a name="install-dependencies"></a>安装依赖项

在继续之前，请添加一些以后将使用的其他依赖项。

- [适用于 Java 的 Microsoft 身份验证库（MSAL）](https://github.com/AzureAD/microsoft-authentication-library-for-java) ，用于对用户进行身份验证并获取访问令牌。
- [Microsoft GRAPH SDK For Java](https://github.com/microsoftgraph/msgraph-sdk-java) ，用于调用 microsoft graph。
- [SLF4J NOP 绑定](https://mvnrepository.com/artifact/org.slf4j/slf4j-nop)到禁止显示来自 MSAL 的日志记录。

1. 打开 **./build.gradle**。 更新`dependencies`节以添加这些依赖项。

    :::code language="gradle" source="../demo/graphtutorial/build.gradle" id="DependenciesSnippet" highlight="7-9":::

1. 将以下项添加到 **/build.gradle**的末尾。

    :::code language="gradle" source="../demo/graphtutorial/build.gradle" id="StandardInputSnippet":::

下次生成项目时，Gradle 将下载这些依赖项。

## <a name="design-the-app"></a>设计应用程序

1. 打开 " **./src/main/java/graphtutorial/App.java** " 文件，并将其内容替换为以下内容。

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
                        break;
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

    这将实现一个基本菜单，并从命令行读取用户的选择。
