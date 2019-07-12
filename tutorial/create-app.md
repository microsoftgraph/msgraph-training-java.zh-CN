---
ms.openlocfilehash: e731c1caff6986556a1bfec6b669ddcb35a3af4c
ms.sourcegitcommit: 02054b307013cce781be2a3512ec1e54f1a322eb
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/12/2019
ms.locfileid: "35634757"
---
<!-- markdownlint-disable MD002 MD041 -->

在要创建项目的目录中打开命令行界面 (CLI)。 运行以下命令以创建新的 Maven 项目。

```Shell
mvn archetype:generate -DarchetypeArtifactId=maven-archetype-quickstart -DarchetypeGroupId=org.apache.maven.archetypes -DgroupId=com.contoso -DartifactId=graphtutorial -Dversion=1.0-SNAPSHOT
```

> [!IMPORTANT]
> 您可以输入与上面指定的值不同的`DgroupId`组 id (参数) 和`DartifactId`项目 ID (参数) 的不同值。 本教程中的示例代码假定使用了组 ID `com.contoso` 。 如果使用不同的值, 请务必将任何示例`com.contoso`代码中的替换为您的组 ID。

出现提示时, 请确认配置, 然后等待创建项目。 创建项目后, 通过运行以下命令来打包并在 CLI 中运行应用程序, 以验证它是否正常工作。

```Shell
mvn package
java -cp target/graphtutorial-1.0-SNAPSHOT.jar com.contoso.App
```

如果它正常运行, 则应用应`Hello World!`输出。 在继续之前, 请添加一些以后将使用的其他依赖项。

- [适用于 Java 的 Microsoft 身份验证库 (MSAL)](https://github.com/AzureAD/microsoft-authentication-library-for-java) , 用于对用户进行身份验证并获取访问令牌。
- [Microsoft GRAPH SDK For Java](https://github.com/microsoftgraph/msgraph-sdk-java) , 用于调用 microsoft graph。
- [SLF4J NOP 绑定](https://mvnrepository.com/artifact/org.slf4j/slf4j-nop)到禁止显示来自 MSAL 的日志记录。

打开 **./graphtutorial/pom.xml**。 在`<dependencies>`元素内添加以下项。

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

下次生成项目时, Maven 将下载这些依赖项。

## <a name="design-the-app"></a>设计应用程序

打开 " **./graphtutorial/src/main/java/com/contoso/App.java** " 文件, 并将其内容替换为以下内容。

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

这将实现一个基本菜单, 并从命令行读取用户的选择。
