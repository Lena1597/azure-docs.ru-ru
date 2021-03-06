---
title: Модель ресурсов приложения Service Fabric Azure
description: В этой статье содержатся общие сведения об управлении приложением Service Fabric Azure с помощью Azure Resource Manager
ms.topic: conceptual
ms.date: 10/21/2019
ms.custom: sfrev
ms.openlocfilehash: 44073967730d95e803f57d504aa9d8c529250a8d
ms.sourcegitcommit: 380e3c893dfeed631b4d8f5983c02f978f3188bf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/08/2020
ms.locfileid: "75751197"
---
# <a name="service-fabric-application-resource-model"></a>Модель ресурсов приложения Service Fabric

Рекомендуется развертывать Service Fabric приложения на кластере Service Fabric с помощью Azure Resource Manager. Этот метод позволяет описать приложения и службы в JSON и развернуть их в том же шаблоне диспетчер ресурсов, что и кластер. В отличие от развертывания приложений и управления ими с помощью PowerShell или Azure CLI, нет необходимости ждать готовности кластера. Регистрацию, подготовку и развертывание приложения можно выполнить за один шаг. Это рекомендуемая методика управления жизненным циклом приложения в кластере. Дополнительные сведения см. в разделе рекомендации [: инфраструктура как код](https://docs.microsoft.com/azure/service-fabric/service-fabric-best-practices-infrastructure-as-code#azure-service-fabric-resources).

Если это возможно, следует управлять приложениями как ресурсами Resource Manager, чтобы улучшить:

* Журнал аудита: Resource Manager выполняется аудит каждой операции и хранит подробный *журнал действий*, позволяющий отслеживать все изменения, внесенные в эти приложения и кластер.
* Управление доступом на основе ролей. Управление доступом к кластерам, а также приложениям, развернутым в кластере, можно выполнить с помощью одного и того же шаблона диспетчер ресурсов.
* Azure Resource Manager (через портал Azure) станет одним-остановкой для управления кластером и критическими развертываниями приложений.

## <a name="service-fabric-application-life-cycle-with-azure-resource-manager"></a>Service Fabric жизненного цикла приложения с Azure Resource Manager

В этом документе вы узнаете, как выполнять следующие задачи:

> [!div class="checklist"]
>
> * Развертывание ресурсов приложения с помощью Azure Resource Manager
> * Обновление ресурсов приложения с помощью Azure Resource Manager
> * Удаление ресурсов приложения

## <a name="deploy-application-resources-using-azure-resource-manager"></a>Развертывание ресурсов приложения с помощью Azure Resource Manager

Чтобы развернуть приложение и его службы с помощью модели ресурсов Azure Resource Manager приложения, необходимо упаковать код приложения, передать пакет, а затем указать ссылку на расположение пакета в шаблоне Azure Resource Manager в качестве приложения. ресурсов. Дополнительные сведения см. в [пакете приложения](https://docs.microsoft.com/azure/service-fabric/service-fabric-package-apps#create-an-sfpkg).

Затем создайте шаблон Azure Resource Manager, обновите файл параметров, указав сведения о приложении, и разверните его в кластере Service Fabric. Ознакомьтесь с примерами [здесь](https://github.com/Azure-Samples/service-fabric-dotnet-quickstart/tree/master/ARM).

### <a name="create-a-storage-account"></a>Создание учетной записи хранения

Для развертывания приложения из шаблона диспетчер ресурсов требуется учетная запись хранения для размещения образа приложения. Вы можете повторно использовать существующую учетную запись хранения или создать новую учетную запись хранения для размещения приложений. Если вы хотите использовать существующую учетную запись хранения, этот шаг можно пропустить. 

![Создание учетной записи хранилища][CreateStorageAccount]

### <a name="configure-storage-account"></a>Настройка учетной записи хранения

После создания учетной записи хранения необходимо создать контейнер больших двоичных объектов, в котором можно разместить приложения. В портал Azure перейдите к учетной записи хранения, в которой вы хотите хранить приложения. Выберите колонку **BLOB-объектов** и нажмите кнопку **добавить контейнер** . Ресурсы в кластере можно защитить, задав для общего уровня доступа значение частный. Доступ можно предоставить несколькими способами.

* [Авторизация доступа к BLOB-объектам и очередям с помощью Azure Active Directory](../storage/common/storage-auth-aad-app.md)
* [Предоставление доступа к BLOB-объектам Azure и создание очереди данных с использованием RBAC на портале Azure](../storage/common/storage-auth-aad-rbac-portal.md)
* [Делегирование доступа с помощью подписанного URL-доступа (SAS)](https://docs.microsoft.com/rest/api/storageservices/delegate-access-with-shared-access-signature
)

 В этом примере мы будем использовать анонимный доступ на чтение для больших двоичных объектов.

![Создать BLOB-объект][CreateBlob]

### <a name="stage-application-in-a-storage-account"></a>Промежуточное приложение в учетной записи хранения

Прежде чем можно будет развернуть приложение, оно должно быть помещено в хранилище BLOB-объектов. В этом учебнике мы создадим пакет приложения вручную, однако этот шаг можно автоматизировать.  Дополнительные сведения см. в [пакете приложения](https://docs.microsoft.com/azure/service-fabric/service-fabric-package-apps#create-an-sfpkg). В следующих шагах будет использоваться [пример приложения для голосования](https://github.com/Azure-Samples/service-fabric-dotnet-quickstart) .

1. В Visual Studio щелкните правой кнопкой мыши проект голосования и выберите пакет.
][PackageApplication] приложения пакета ![  
2. Откройте только что созданный каталог **.\сервице-фабрик-дотнет-куиккстарт\вотинг\пкг\дебуг** и заархивировать содержимое в файл с именем **голосование. zip** , чтобы файл ApplicationManifest. XML находился в корне ZIP-файла.  
][ZipApplication] приложения ![ZIP  
3. Переименуйте расширение файла с ZIP на **sfpkg**.
4. В портал Azure в контейнере **приложения** вашей учетной записи хранения щелкните **Отправить** и отправить **голосование. sfpkg**.  
![отправить пакет приложения][UploadAppPkg]

Теперь приложение размещается на промежуточном уровне. Теперь все готово для создания шаблона Azure Resource Manager для развертывания приложения.

### <a name="create-the-azure-resource-manager-template"></a>Создание шаблона Azure Resource Manager

Пример приложения содержит [шаблоны Azure Resource Manager](https://github.com/Azure-Samples/service-fabric-dotnet-quickstart/tree/master/ARM) , которые можно использовать для развертывания приложения. Файлы шаблонов имеют имена **усерапп. JSON** и **усерапп. parameters. JSON**.

> [!NOTE]
> Файл **усерапп. parameters. JSON** должен быть обновлен именем кластера.
>
>

| Параметр              | Description                                 | Пример                                                      | Комментарии                                                     |
| ---------------------- | ------------------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| clusterName            | Имя кластера, в который выполняется развертывание | SF-cluster123                                                |                                                              |
| application            | Имя приложения                 | Голосовани                                                       |
| applicationTypeName    | Имя типа приложения.           | VotingType                                                   | Должно соответствовать ApplicationManifest. XML                 |
| applicationTypeVersion | Версия типа приложения         | 1.0.0                                                        | Должно соответствовать ApplicationManifest. XML                 |
| serviceName            | Имя службы,         | Голосование ~ VotingWeb                                             | Должен быть в формате ApplicationName ~ ServiceType            |
| serviceTypeName        | Имя типа службы                | VotingWeb                                                    | Должно соответствовать значению в файле ServiceManifest. XML                 |
| апппаккажеурл          | URL-адрес хранилища больших двоичных объектов приложения     | https://servicefabricapps.blob.core.windows.net/apps/Voting.sfpkg | URL-адрес пакета приложения в хранилище BLOB-объектов (процедура для установки описана ниже) |

```json
{
    "apiVersion": "2019-03-01",
    "type": "Microsoft.ServiceFabric/clusters/applications",
    "name": "[concat(parameters('clusterName'), '/', parameters('applicationName'))]",
    "location": "[variables('clusterLocation')]",
},
{
    "apiVersion": "2019-03-01",
    "type": "Microsoft.ServiceFabric/clusters/applicationTypes",
    "name": "[concat(parameters('clusterName'), '/', parameters('applicationTypeName'))]",
    "location": "[variables('clusterLocation')]",
},
{
    "apiVersion": "2019-03-01",
    "type": "Microsoft.ServiceFabric/clusters/applicationTypes/versions",
    "name": "[concat(parameters('clusterName'), '/', parameters('applicationTypeName'), '/', parameters('applicationTypeVersion'))]",
    "location": "[variables('clusterLocation')]",
},
{
    "apiVersion": "2019-03-01",
    "type": "Microsoft.ServiceFabric/clusters/applications/services",
    "name": "[concat(parameters('clusterName'), '/', parameters('applicationName'), '/', parameters('serviceName'))]",
    "location": "[variables('clusterLocation')]"
}
```

### <a name="deploy-the-application"></a>Развертывание приложения

Чтобы развернуть приложение, выполните команду New-Азресаурцеграупдеплоймент, чтобы выполнить развертывание в группу ресурсов, содержащую кластер.

```powershell
New-AzResourceGroupDeployment -ResourceGroupName "sf-cluster-rg" -TemplateParameterFile ".\UserApp.Parameters.json" -TemplateFile ".\UserApp.json" -Verbose
```

## <a name="upgrade-service-fabric-application-using-azure-resource-manager"></a>Обновление Service Fabric приложения с помощью Azure Resource Manager

Приложения, уже развернутые в кластере Service Fabric, будут обновлены по следующим причинам:

1. В приложение добавляется новая служба. Определение службы должно быть добавлено в файл сервице-манифест. XML и аппликатион-манифест. XML. Затем, чтобы отразить новую версию приложения, необходимо обновить версию типа приложения с 1.0.0 на 1.0.1 [усерапп. parameters. JSON](https://github.com/Azure-Samples/service-fabric-dotnet-quickstart/blob/master/ARM/UserApp.Parameters.json).

    ```json
    "applicationTypeVersion": {
        "value": "1.0.1"
    },
    "serviceName2": {
        "value": "Voting~VotingData"
    },
    "serviceTypeName2": {
        "value": "VotingDataType"
    }
    ```

2. В приложение добавляется новая версия существующей службы. Это включает изменение кода приложения и обновление версии и имени типа приложения.

    ```json
     "applicationTypeVersion": {
        "value": "1.0.1"
    },
    ```

## <a name="delete-application-resources"></a>Удаление ресурсов приложения

Приложения, развернутые с помощью модели ресурсов приложения в Azure Resource Manager можно удалить из кластера, выполнив приведенные ниже действия.

1) Получите идентификатор ресурса для приложения с помощью команды [Get-азресаурце](https://docs.microsoft.com/powershell/module/az.resources/get-azresource?view=azps-2.5.0):

    ```powershell
    Get-AzResource  -Name <String> | f1
    ```

2) Удалите ресурсы приложения с помощью команды [Remove-азресаурце](https://docs.microsoft.com/powershell/module/az.resources/remove-azresource?view=azps-2.5.0):

    ```powershell
    Remove-AzResource  -ResourceId <String> [-Force] [-ApiVersion <String>]
    ```

## <a name="next-steps"></a>Дальнейшие действия

Получение сведений о модели ресурсов приложения:

* [Моделирование приложения в Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-model)
* [Service Fabric манифесты приложения и службы](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-and-service-manifests)

## <a name="see-also"></a>См. также:

* [Рекомендации](https://docs.microsoft.com/azure/service-fabric/service-fabric-best-practices-infrastructure-as-code)
* [Управление приложениями и службами как ресурсами Azure](https://docs.microsoft.com/azure/service-fabric/service-fabric-best-practices-infrastructure-as-code)

<!--Image references-->
[CreateStorageAccount]: ./media/service-fabric-application-model/create-storage-account.png
[CreateBlob]: ./media/service-fabric-application-model/create-blob.png
[PackageApplication]: ./media/service-fabric-application-model/package-application.png
[ZipApplication]: ./media/service-fabric-application-model/zip-application.png
[UploadAppPkg]: ./media/service-fabric-application-model/upload-app-pkg.png
