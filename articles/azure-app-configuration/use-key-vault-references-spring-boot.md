---
title: Учебник по использованию ссылок Key Vault службы "Конфигурация приложений Azure" в приложении Java Spring Boot | Документация Майкрософт
description: В этом учебнике описано, как использовать ссылки Key Vault службы "Конфигурация приложений Azure" из приложения Java Spring Boot.
services: azure-app-configuration
documentationcenter: ''
author: lisaguthrie
manager: maiye
editor: ''
ms.assetid: ''
ms.service: azure-app-configuration
ms.workload: tbd
ms.devlang: csharp
ms.topic: tutorial
ms.date: 12/16/2019
ms.author: lcozzens
ms.custom: mvc
ms.openlocfilehash: 17d86f25de6eecee535d6f812f4ef0b078a4b6db
ms.sourcegitcommit: 380e3c893dfeed631b4d8f5983c02f978f3188bf
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/08/2020
ms.locfileid: "75752500"
---
# <a name="tutorial-use-key-vault-references-in-a-java-spring-app"></a>Руководство. Использование ссылок Key Vault в приложении Java Spring

В этом учебнике вы узнаете, как использовать службу "Конфигурация приложений Azure" с Azure Key Vault. Конфигурация приложений Azure и Key Vault — это дополняющие службы, которые используются параллельно в большинстве развертываний приложений.

Служба "Конфигурация приложений Azure" упрощает их использование, позволяя создавать ключи, ссылающиеся на значения, хранящиеся в Key Vault. Когда служба "Конфигурация приложений Azure" создает такие ключи, она сохраняет URI значений Key Vault, а не сами значения.

С помощью поставщика клиента "Конфигурация приложений Azure" ваше приложение получает ссылки на Key Vault так же, как и любые другие ключи, хранящиеся в службе "Конфигурация приложений Azure". В этом случае значения, хранящиеся в службе "Конфигурация приложений Azure", являются URI, которые ссылаются на значения в Key Vault. Это не значения или учетные данные Key Vault. Так как поставщик клиента распознает ключи как ссылки на Key Vault, для получения их значений он использует Key Vault.

Приложение отвечает за правильную проверку подлинности как в службе "Конфигурация приложений", так и в Key Vault. Две службы не обмениваются данными напрямую.

В этом учебнике показано, как можно реализовать ссылки на Key Vault в коде. При этом в качестве основы используется код веб-приложения, представленный в кратких руководствах. Прежде чем продолжить, прочитайте краткое руководство [Создание приложения Java Spring с помощью службы "Конфигурация приложений Azure"](./quickstart-java-spring-app.md).

Вы можете выполнять шаги в этом учебнике с помощью любого редактора кода. Например, [Visual Studio Code](https://code.visualstudio.com/) — это кроссплатформенный редактор кода, доступный для операционных систем Windows, macOS и Linux.

В этом руководстве описано следующее.

> [!div class="checklist"]
> * создание ключа службы "Конфигурация приложений Azure", который ссылается на значение, хранящееся в Key Vault;
> * доступ к значению этого ключа из веб-приложения Java Spring.

## <a name="prerequisites"></a>предварительные требования

Перед работой с этим учебником установите [пакет SDK для .NET Core](https://dotnet.microsoft.com/download).

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-vault"></a>Создание хранилища

1. Выберите параметр **Создать ресурс** в верхнем левом углу окна портала Azure.

    ![Выходные данные после создания Key Vault](./media/quickstarts/search-services.png)
1. В поле поиска введите **Key Vault**.
1. В списке результатов выберите **Key Vault** слева.
1. В разделе **Хранилища ключей** выберите **Добавить**.
1. Справа в разделе **Создать Key Vault** введите приведенные ниже сведения.
    - Выберите **Подписка**, чтобы выбрать подписку.
    - В разделе **Группа ресурсов** выберите **Создать** и введите имя группы ресурсов.
    - В поле **Имя Key Vault** укажите уникальное имя. Для работы с этим учебником введите **Contoso-vault2**.
    - В раскрывающемся списке **Регион** выберите расположение.
1. Для других свойств раздела **создания Key Vault** оставьте значения по умолчанию.
1. Нажмите кнопку **Создать**.

На этом этапе доступ к этому новому хранилищу может получать только учетная запись Azure.

![Выходные данные после создания Key Vault](./media/quickstarts/vault-properties.png)

## <a name="add-a-secret-to-key-vault"></a>Добавление секрета в Key Vault

Чтобы добавить секрет в хранилище, вам просто нужно выполнить несколько дополнительных шагов. В этом случае мы добавим сообщение, которое можно использовать для проверки возвращения Key Vault. Сообщение называется **Message**, и в нем сохраняется значение "Hello from Key Vault".

1. На странице свойств Key Vault выберите **Секреты**.
1. Выберите **Создать или импортировать**.
1. В области **Создание секрета** выберите следующие значения:
    - **Параметры отправки.** Введите **Вручную**.
    - **Name**: Введите **Message**.
    - **Value** (Значение): Введите **Hello from Key Vault**.
1. Для других свойств раздела **Создание секрета** оставьте значения по умолчанию.
1. Нажмите кнопку **Создать**.

## <a name="add-a-key-vault-reference-to-app-configuration"></a>Добавление ссылки на Key Vault в службу "Конфигурация приложений Azure"

1. Войдите на [портал Azure](https://portal.azure.com). Щелкните **Все ресурсы** и выберите экземпляр хранилища "Конфигурация приложений Azure", который вы создали по инструкциям из краткого руководства.

1. Выберите **Обозреватель конфигураций**.

1. Выберите **+ Создать** > **Ссылка на хранилище ключей** и укажите следующие значения:
    - **Ключ**: Выберите **/application/config.keyvaultmessage**
    - **Метка** — оставьте это значение пустым.
    - **Подписка**, **Группа ресурсов** и **Хранилище ключей**: введите значения, соответствующие значениям хранилища ключей, созданного в предыдущем разделе.
    - **Секрет**: выберите секрет с именем **Message**, созданный в предыдущем разделе.

## <a name="connect-to-key-vault"></a>Подключение к Key Vault

1. В этом учебнике вы используете субъект-службу для проверки подлинности в KeyVault. Создайте субъект-службу с помощью команды [az ad sp create-for-rbac](/cli/azure/ad/sp?view=azure-cli-latest#az-ad-sp-create-for-rbac) Azure CLI:

    ```azurecli
    az ad sp create-for-rbac -n "http://mySP" --sdk-auth
    ```

    Эта операция возвращает ряд пар "ключ — значение".

    ```console
    {
    "clientId": "7da18cae-779c-41fc-992e-0527854c6583",
    "clientSecret": "b421b443-1669-4cd7-b5b1-394d5c945002",
    "subscriptionId": "443e30da-feca-47c4-b68f-1636b75e16b3",
    "tenantId": "35ad10f1-7799-4766-9acf-f2d946161b77",
    "activeDirectoryEndpointUrl": "https://login.microsoftonline.com",
    "resourceManagerEndpointUrl": "https://management.azure.com/",
    "activeDirectoryGraphResourceId": "https://graph.windows.net/",
    "sqlManagementEndpointUrl": "https://management.core.windows.net:8443/",
    "galleryEndpointUrl": "https://gallery.azure.com/",
    "managementEndpointUrl": "https://management.core.windows.net/"
    }
    ```

1. Выполните следующую команду, чтобы предоставить субъекту-службе доступ к хранилищу ключей:

    ```
    az keyvault set-policy -n <your-unique-keyvault-name> --spn <clientId-of-your-service-principal> --secret-permissions delete get list set --key-permissions create decrypt delete encrypt get list unwrapKey wrapKey
    ```

1. Создайте следующие переменные среды, используя значения для субъекта-службы, которые были показаны на предыдущем шаге.

    * **AZURE_CLIENT_ID**: *clientId*
    * **AZURE_CLIENT_SECRET**: *clientSecret*
    * **AZURE_TENANT_ID**: *tenantId*

> [!NOTE]
> Эти учетные данные Key Vault используются только в вашем приложении. Приложение выполняет проверку подлинности непосредственно в Key Vault с использованием этих учетных данных. Они никогда не передаются в службу настройки приложений.

## <a name="update-your-code-to-use-a-key-vault-reference"></a>Обновление кода для использования ссылки Key Vault

1. Откройте *MessageProperties.java*. Добавьте новую переменную с именем *keyVaultMessage*:

    ```java
    private String keyVaultMessage;

    public String getKeyVaultMessage() {
        return keyVaultMessage;
    }

    public void setKeyVaultMessage(String keyVaultMessage) {
        this.keyVaultMessage = keyVaultMessage;
    }
    ```

1. Откройте *HelloController.java*. Обновите метод *getMessage*, чтобы включить сообщение, полученное от Key Vault.

    ```java
    @GetMapping
    public String getMessage() {
        return "Message: " + properties.getMessage() + "\nKey Vault message: " + properties.getKeyVaultMessage();
    }
    ```

1. Создайте приложение Spring Boot с помощью Maven и запустите его, например, следующим образом:

    ```shell
    mvn clean package
    mvn spring-boot:run
    ```
1. После запуска приложение можно протестировать с помощью средства *curl*, например:

      ```shell
      curl -X GET http://localhost:8080/
      ```
    Вы увидите сообщение, которое указали в хранилище конфигураций приложений. Вы также увидите сообщение, которое ввели в Key Vault.

## <a name="clean-up-resources"></a>Очистка ресурсов

[!INCLUDE [azure-app-configuration-cleanup](../../includes/azure-app-configuration-cleanup.md)]

## <a name="next-steps"></a>Дальнейшие действия

В этом учебнике вы создали ключ службы "Конфигурация приложений Azure", который ссылается на значение, хранящееся в Key Vault. Чтобы узнать, как использовать флаги функций в приложении Java Spring, перейдите к следующему руководству.

> [!div class="nextstepaction"]
> [Руководство по интеграции с управляемыми удостоверениями Azure](./quickstart-feature-flag-spring-boot.md)
