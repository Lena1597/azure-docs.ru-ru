---
title: Безопасность сети в Azure Data Lake Storage 1-го поколения | Документация Майкрософт
description: Общие сведения об интеграции с виртуальной сети с Azure Data Lake Storage 1-го поколения
services: data-lake-store
documentationcenter: ''
author: twooley
manager: mtillman
editor: cgronlun
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 10/09/2018
ms.author: elsung
ms.openlocfilehash: 7d6c826df2a509ffb378809e3682073bd5ab1301
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "60612555"
---
# <a name="virtual-network-integration-for-azure-data-lake-storage-gen1"></a>Интеграция с виртуальной сетью для Azure Data Lake Storage 1-го поколения

В статье описана интеграция с виртуальной сетью для Azure Data Lake Storage 1-го поколения. При интеграции с виртуальной сетью можно настроить учетные записи для приема трафика только из определенных виртуальных сетей и подсетей. 

Эта функция помогает защитить учетную запись Data Lake Storage от внешних угроз.

Функция интеграции с виртуальной сетью для ADLS 1-го поколения использует безопасность на уровне конечной точки служб для виртуальной сети между виртуальной сетью и службой Azure Active Directory (Azure AD) с целью создания дополнительных утверждений безопасности в маркере доступа. Эти утверждения используются для проверки подлинности виртуальной сети в учетной записи ADLS 1-го поколения и для предоставления доступа.

> [!NOTE]
> За использование этих возможностей не взимается дополнительная плата. Учетной записи будет выставлен счет по стандартному тарифу за Data Lake Storage 1-го поколения. Дополнительные сведения см. на странице [цен](https://azure.microsoft.com/pricing/details/data-lake-store/?cdn=disable). Дополнительные сведения о всех используемых службах Azure см. на странице [цен](https://azure.microsoft.com/pricing/#product-picker).

## <a name="scenarios-for-virtual-network-integration-for-data-lake-storage-gen1"></a>Сценарии для интеграции с виртуальной сетью для ADLS 1-го поколения

Функция интеграции с виртуальной сетью службы ADLS 1-го поколения позволяет предоставить доступ к учетной записи ADLS 1-го поколения только из указанных виртуальных сетей и подсетей. Другие виртуальные сети и виртуальные машины не смогут подключиться к вашей учетной записи, после того как она будет привязана к указанной подсети виртуальной сети. С функциональной точки зрения интеграция с виртуальной сетью службы ADLS 1-го поколения реализует тот же сценарий, что и [конечные точки служб для виртуальной сети](https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoints-overview). Несколько ключевых отличий подробно описаны в следующих разделах. 

![Диаграмма сценария для интеграции с виртуальной сетью для ADLS 1-го поколения](media/data-lake-store-network-security/scenario-diagram.png)

> [!NOTE]
> В дополнение к правилам виртуальной сети могут использоваться существующие правила брандмауэра для IP-адресов, чтобы также разрешать доступ из локальных сетей. 

## <a name="optimal-routing-with-data-lake-storage-gen1-virtual-network-integration"></a>Оптимальная маршрутизация для интеграции с виртуальной сетью для ADLS 1-го поколения

Ключевым преимуществом конечных точек служб для виртуальной сети является [оптимальная маршрутизация](https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoints-overview#key-benefits) из виртуальной сети. Вы можете достичь той же оптимизации маршрутизации для учетных записей ADLS 1-го поколения. Для этого используйте следующие [определяемые пользователем маршруты](https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview#user-defined) из виртуальной сети к учетным записям ADLS 1-го поколения.

**Общедоступный IP-адрес ADLS**. Используйте общедоступный IP-адрес для целевых учетных записей ADLS 1-го поколения. IP-адреса для учетных записей Azure Data Lake Storage 1-го поколения можно определить, [разрешая DNS-имена](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-connectivity-from-vnets#enabling-connectivity-to-azure-data-lake-storage-gen1-from-vms-with-restricted-connectivity) учетных записей. Создайте отдельную запись для каждого адреса.

    ```azurecli
    # Create a route table for your resource group.
    az network route-table create --resource-group $RgName --name $RouteTableName
    
    # Create route table rules for Data Lake Storage public IP addresses.
    # There's one rule per Data Lake Storage public IP address. 
    az network route-table route create --name toADLSregion1 --resource-group $RgName --route-table-name $RouteTableName --address-prefix <ADLS Public IP Address> --next-hop-type Internet
    
    # Update the virtual network, and apply the newly created route table to it.
    az network vnet subnet update --vnet-name $VnetName --name $SubnetName --resource-group $RgName --route-table $RouteTableName
    ```

## <a name="data-exfiltration-from-the-customer-virtual-network"></a>Утечка данных из виртуальной сети клиента

Помимо защиты доступа к учетным записям ADLS из виртуальной сети вы также можете убедиться в отсутствии утечки данных в несанкционированную учетную запись.

Рекомендуется использовать в виртуальной сети брандмауэр, чтобы фильтровать исходящий трафик на основе URL-адреса учетной записи назначения. Разрешать доступ следует только авторизованным учетным записям ADLS 1-го поколения.

Ниже приведены некоторые доступные варианты.
- [Брандмауэр Azure](https://docs.microsoft.com/azure/firewall/overview). [Разверните и настройте Брандмауэр Azure](https://docs.microsoft.com/azure/firewall/tutorial-firewall-deploy-portal) для виртуальной сети. Чтобы защитить исходящий трафик ADLS и ограничить его URL-адресом известной авторизованной учетной записи.
- Брандмауэр [сетевого виртуального модуля](https://azure.microsoft.com/solutions/network-appliances/). Администратор может разрешить использовать только определенных поставщиков коммерческих брандмауэров. Решение брандмауэра сетевого виртуального модуля, доступное в Azure Marketplace, можно использовать для выполнения той же функции.

> [!NOTE]
> При использовании брандмауэров в пути к данным появляется дополнительный прыжок. Это может влиять на производительность сети в комплексных сценариях обмена данными. В том числе на доступную пропускную способность и задержку при подключении. 

## <a name="limitations"></a>Ограничения

- Кластеры HDInsight, созданные до включения поддержки интеграции с виртуальной сетью для Data Lake Storage 1-го поколения, необходимо создать повторно, чтобы они поддерживали новую функцию.
 
- При создании кластера HDInsight выбор учетной записи ADLS 1-го поколения со включенной интеграцией с виртуальной сетью вызовет сбой процесса. Сначала необходимо отключить правило виртуальной сети. Кроме того, можно в колонке **Брандмауэр и виртуальные сети** учетной записи Data Lake Storage выбрать **Allow access from all networks and services** (Разрешить доступ из всех сетей и служб). Затем создайте кластер HDInsight, прежде чем повторно включать правило виртуальной сети или отменять выбор параметра **Allow access from all networks and services** (Разрешить доступ из всех сетей и служб). Дополнительные сведения см. в разделе [Исключения](#exceptions).

- Интеграция Data Lake Storage 1-го поколения с виртуальной сетью не поддерживает [управляемые удостоверения ресурсов Azure](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview).
  
- Данные файлов и папок в учетной записи ADLS 1-го поколения с поддержкой интеграции с виртуальной сетью недоступны на портале. Это включает доступ из виртуальной машины, которая находится в пределах виртуальной сети, и такие действия, как использование обозревателя данных. Действия по управлению учетными записями будут работать. Данные файлов и папок в учетной записи ADLS с интеграцией с виртуальной сетью доступны через все ресурсы вне портала. Это пакет SDK для доступа, скрипты PowerShell, другие службы Azure (когда поступают не с портала) и т. д. 

## <a name="configuration"></a>Параметр Configuration

### <a name="step-1-configure-your-virtual-network-to-use-an-azure-ad-service-endpoint"></a>Шаг 1. Настройка виртуальной сети для использования конечной точки службы AAD

1.  Перейдите на портал Azure и войдите, используя свою учетную запись Azure.
 
2.  [Создайте виртуальную сеть](https://docs.microsoft.com/azure/virtual-network/quick-create-portal) в подписке. Вы можете использовать имеющуюся. Виртуальная сеть должна находиться в том же регионе, что и учетная запись ADLS 1-го поколения.
 
3.  В колонке **Виртуальная сеть** выберите **Конечные точки службы**.
 
4.  Нажмите кнопку **Добавить**, чтобы добавить новую конечную точку службы.

    ![Добавление конечной точки службы для виртуальной сети](media/data-lake-store-network-security/config-vnet-1.png)

5.  Выберите **Microsoft.AzureActiveDirectory** как службу для конечной точки.

     ![Выбор конечной точки службы Microsoft.AzureActiveDirectory](media/data-lake-store-network-security/config-vnet-2.png)

6.  Выберите подсети, для которых вы собираетесь разрешить подключения. Выберите **Добавить**.

    ![Выбор подсети](media/data-lake-store-network-security/config-vnet-3.png)

7.  Добавление конечной точки службы может занять до 15 минут. После добавления она появляется в списке. Убедитесь, что конечная точка отображается и все сведения соответствуют настройкам.
 
    ![Успешное добавление конечной точки службы](media/data-lake-store-network-security/config-vnet-4.png)

### <a name="step-2-set-up-the-allowed-virtual-network-or-subnet-for-your-data-lake-storage-gen1-account"></a>Шаг 2. Настройка разрешенной виртуальной сети или подсети для учетной записи Data Lake Storage 1-го поколения

1.  После настройки виртуальной [сети создайте учетную запись Azure Data Lake Storage 1-го поколения](data-lake-store-get-started-portal.md#create-a-data-lake-storage-gen1-account) в подписке. Вы также можете перейти к имеющейся учетной записи ADLS 1-го поколения. Виртуальная сеть должна находиться в том же регионе, что и учетная запись ADLS 1-го поколения.
 
2.  Выберите пункт **Брандмауэр и виртуальные сети**.

    > [!NOTE]
    > Если вы не видите пункт **Брандмауэр и виртуальные сети** в параметрах, тогда следует выйти из портала. Закройте браузер и очистите его кэш. Перезапустите компьютер и повторите попытку.

       ![Добавление правила виртуальной сети в учетную запись Data Lake Storage](media/data-lake-store-network-security/config-adls-1.png)

3.  Выберите **Выбранные сети**.
 
4.  Выберите **Добавить существующую виртуальную сеть**.

    ![Добавление имеющейся виртуальной сети](media/data-lake-store-network-security/config-adls-2.png)

5.  Выберите виртуальные сети и подсети, чтобы разрешить подключение. Выберите **Добавить**.

    ![Выбор виртуальной сети и подсетей.](media/data-lake-store-network-security/config-adls-3.png)

6.  Убедитесь, что виртуальные сети и подсети отображаются в списке корректно. Щелкните **Сохранить**.

    ![Сохранение нового правила](media/data-lake-store-network-security/config-adls-4.png)

    > [!NOTE]
    > Вступление изменений в силу после сохранения может занять до 5 минут.

7.  [Необязательно.] На странице **Брандмауэр и виртуальные сети** в разделе **Брандмауэр** можно разрешить подключение с определенных IP-адресов. 

## <a name="exceptions"></a>Исключения
Вы можете включить подключение из служб Azure и виртуальных машин за пределами выбранных виртуальных сетей. В колонке **Брандмауэр и виртуальные сети** в области **Исключения** выберите один из двух вариантов:
 
- **Разрешить всем службам Azure доступ к этой учетной записи Data Lake Storage 1-го поколения.** Этот параметр позволяет службам Azure, таким как Фабрика данных Azure, Центры событий Azure и все виртуальные машины Azure, взаимодействовать с учетной записью Data Lake Storage.

- **Разрешить Azure Data Lake Analytics доступ к этой учетной записи Data Lake Storage 1-го поколения.** Этот параметр разрешает подключение Data Lake Analytics к этой учетной записи Data Lake Storage. 

  ![Исключения брандмауэра и виртуальной сети](media/data-lake-store-network-security/firewall-exceptions.png)

Рекомендуется оставить эти исключения отключенными. Включите их, только если вам требуется подключение из других служб за пределами вашей виртуальной сети.
