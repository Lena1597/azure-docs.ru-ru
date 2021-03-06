---
title: Руководство по копированию данных в Azure Data Box Heavy через NFS | Документация Майкрософт
description: Узнайте, как копировать данные в Azure Data Box Heavy через NFS.
services: databox
author: alkohli
ms.service: databox
ms.subservice: heavy
ms.topic: tutorial
ms.date: 07/03/2019
ms.author: alkohli
ms.openlocfilehash: 4361cee3d07408c3abb5031d2ab18c15c92c5e0a
ms.sourcegitcommit: ccb9a7b7da48473362266f20950af190ae88c09b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2019
ms.locfileid: "67595813"
---
# <a name="tutorial-copy-data-to-azure-data-box-heavy-via-nfs"></a>Руководство по Копирование данных в Azure Data Box Heavy через NFS

В этом руководстве объясняется, как подключиться и скопировать данные с главного компьютера на устройство Azure Data Box Heavy с помощью локального пользовательского веб-интерфейса.

Из этого руководства вы узнаете, как выполнять следующие задачи:

> [!div class="checklist"]
> * Предварительные требования
> * подключение к Azure Data Box Heavy;
> * скопировать данные в Data Box Heavy.

## <a name="prerequisites"></a>Предварительные требования

Перед тем как начать, убедитесь в следующем.

1. Вы завершили [руководство Настройка Azure Data Box Heavy](data-box-heavy-deploy-set-up.md).
2. Вы получили устройство Data Box Heavy (состояние заказа на портале — **Доставлено**).
3. У вас есть главный компьютер с данными, которые необходимо скопировать в Data Box Heavy. На главном компьютере должно быть следующее ПО:
    - [поддерживаемая операционная система](data-box-heavy-system-requirements.md);
    - Компьютер должен быть подключен к высокоскоростной сети. Чтобы ускорить копирование, можно параллельно использовать два подключения 40 GbE (по одному на каждый узел). Если у вас нет доступного подключения на 40 GbE, мы рекомендуем использовать хотя бы два подключения 10 GbE (по одному на каждый узел). 

## <a name="connect-to-data-box-heavy"></a>подключение к Azure Data Box Heavy;

В зависимости от выбранной учетной записи хранения Data Box Heavy создает перечисленные ниже ресурсы.
- До трех общих папок для каждой связанной учетной записи хранения (GPv1 и GPv2).
- Одна общая папка для хранилища класса Premium.
- Одна общая папка для учетной записи хранения BLOB-объектов.

Эти общие папки создаются на обоих узлах устройства.

В общих папках блочных и страничных BLOB-объектов:
- сущности первого уровня являются контейнерами;
- сущности второго уровня являются большими двоичными объектами.

В общих папках для Файлов Azure:
- сущности первого уровня являются общими папками;
- сущности второго уровня являются файлами.

В следующей таблице приведен UNC-путь к общим папкам в Azure Data Box Heavy и URL-адрес службы хранилища Azure, куда отправляются данные. Конечный URL-адрес службы хранилища Azure может быть производным от UNC-пути к общей папке.
 
|                   |                                                            |
|-------------------|--------------------------------------------------------------------------------|
| Блочные BLOB-объекты Azure | <li>UNC-путь к общим папкам: `//<DeviceIPAddress>/<StorageAccountName_BlockBlob>/<ContainerName>/files/a.txt`</li><li>URL-адрес службы хранилища Azure: `https://<StorageAccountName>.blob.core.windows.net/<ContainerName>/files/a.txt`</li> |  
| Страничные BLOB-объекты Azure  | <li>UNC-путь к общим папкам: `//<DeviceIPAddres>/<StorageAccountName_PageBlob>/<ContainerName>/files/a.txt`</li><li>URL-адрес службы хранилища Azure: `https://<StorageAccountName>.blob.core.windows.net/<ContainerName>/files/a.txt`</li>   |  
| Файлы Azure       |<li>UNC-путь к общим папкам: `//<DeviceIPAddres>/<StorageAccountName_AzFile>/<ShareName>/files/a.txt`</li><li>URL-адрес службы хранилища Azure: `https://<StorageAccountName>.file.core.windows.net/<ShareName>/files/a.txt`</li>        |

Если вы используете главный компьютер с ОС Linux, то выполните указанные ниже действия, чтобы настроить устройство для доступа к NFS-клиентам.

1. Укажите IP-адреса клиентов, которым разрешен доступ к общей папке. В локальном пользовательском веб-интерфейсе перейдите на страницу **Подключение и копирование**. В разделе **Параметры NFS** щелкните **Клиентский доступ NFS**. 

    ![Настройка клиентского доступа NFS 1](media/data-box-deploy-copy-data/nfs-client-access.png)

2. Укажите IP-адрес NFS-клиента и щелкните **Добавить**. Вы можете настроить доступ для нескольких NFS-клиентов, выполнив это действие для каждого клиента. Последовательно выберите **ОК**.

    ![Настройка клиентского доступа NFS 2](media/data-box-deploy-copy-data/nfs-client-access2.png)

2. Убедитесь, что на главном компьютере с ОС Linux установлен NFS-клиент [поддерживаемой версии](data-box-system-requirements.md). Используйте версию, подходящую для используемого вами дистрибутива ОС Linux. 

3. После установки NFS-клиента подключите общую папку NFS на устройстве Data Box, выполнив указанную ниже команду.

    `sudo mount <Data Box Heavy device IP>:/<NFS share on Data Box Heavy device> <Path to the folder on local Linux computer>`

    В примере ниже показано, как подключиться к общей папке Data Box Heavy через NFS. IP-адрес устройства Data Box Heavy — `10.161.23.130`, общая папка `Mystoracct_Blob` подключена в виртуальной машине ubuntuVM, а точка подключения называется `/home/databoxheavyubuntuhost/databoxheavy`.

    `sudo mount -t nfs 10.161.23.130:/Mystoracct_Blob /home/databoxheavyubuntuhost/databoxheavy`
    
    Для клиентов Mac необходимо добавить дополнительный параметр следующим образом: 
    
    `sudo mount -t nfs -o sec=sys,resvport 10.161.23.130:/Mystoracct_Blob /home/databoxheavyubuntuhost/databoxheavy`

    **Всегда создавайте отдельную папку для файлов, которые вы собираетесь скопировать в общую папку**. Папка, созданная в общих папках блочных и страничных BLOB-объектов, представляет собой контейнер, куда передаются данные в виде больших двоичных объектов. Вы не можете копировать файлы напрямую в *корневую* папку в учетной записи хранения.

## <a name="copy-data-to-data-box-heavy"></a>копирование данных в Data Box Heavy.

После подключения к общим папкам Data Box Heavy можно скопировать данные. Прежде чем начать копирование данных, ознакомьтесь со следующими моментами.

- Убедитесь, что вы собираетесь копировать данные в общие папки, предназначенные для используемого вами формата данных. Например, данные блочного BLOB-объекта необходимо копировать в общую папку для блочных BLOB-объектов. Скопируйте виртуальные жесткие диски в страничные BLOB-объекты. Если формат данных не соответствует общей папке, то на следующем этапе вам не удастся отправить данные в Azure.
-  При копировании данных убедитесь, что размер данных соответствует ограничениям на размер, указанным в статье об [ограничениях для службы хранилища Azure и Data Box Heavy](data-box-heavy-limits.md). 
- Если данные, отправляемые Data Box Heavy, одновременно отправляются другими приложениями за пределами устройства, это может привести к сбоям заданий отправки и повреждению данных.
- Рекомендуем не использовать протоколы SMB и NFS одновременно либо копировать одни и те же данные в одно и то же конечное расположение в Azure. В таких случаях невозможно предсказать окончательный результат.
- **Всегда создавайте отдельную папку для файлов, которые вы собираетесь скопировать в общую папку**. Папка, созданная в общих папках блочных и страничных BLOB-объектов, представляет собой контейнер, куда передаются данные в виде больших двоичных объектов. Вы не можете копировать файлы напрямую в *корневую* папку в учетной записи хранения.
- При приеме имен каталогов и файлов с учетом регистра из общей папки NFS в NFS на устройстве Data Box Heavy: 
    - регистр в имени сохраняется;
    - регистр в файлах не учитывается.
    
    Например, при копировании `SampleFile.txt` и `Samplefile.Txt` на устройство регистр в имени сохранится, но второй файл перезапишет первый, так как они считаются одним и тем же файлом.


Если у вас главный компьютер с ОС Linux, используйте программу копирования, аналогичную Robocopy. Вот некоторые программы, доступные в ОС Linux: [rsync](https://rsync.samba.org/), [FreeFileSync](https://www.freefilesync.org/), [Unison](https://www.cis.upenn.edu/~bcpierce/unison/), [Ultracopier](https://ultracopier.first-world.info/).  

Команда `cp` — один из лучших способов копировать каталоги. Дополнительные сведения об использовании команды см. на [страницах руководства команды cp](http://man7.org/linux/man-pages/man1/cp.1.html).

При копировании при помощи rsync с использованием нескольких потоков следуйте указанным ниже рекомендациям.

 - Установите пакет **CIFS Utils** или **NFS Utils** (в зависимости от того, какая файловая система используется в клиенте с ОС Linux).

    `sudo apt-get install cifs-utils`

    `sudo apt-get install nfs-utils`

 -  Установите программу **Rsync** и **Parallel** (в зависимости от версии используемого дистрибутива ОС Linux).

    `sudo apt-get install rsync`
   
    `sudo apt-get install parallel` 

 - Создайте точку подключения.

    `sudo mkdir /mnt/databoxheavy`

 - Подключите том.

    `sudo mount -t NFS4  //Databox-heavy-IP-Address/share_name /mnt/databoxheavy` 

 - Сделайте зеркальное отражение структуры каталогов.  

    `rsync -za --include='*/' --exclude='*' /local_path/ /mnt/databoxheavy`

 - Скопируйте файлы. 

    `cd /local_path/; find -L . -type f | parallel -j X rsync -za {} /mnt/databoxheavy/{}`

     здесь j — количество случаев распараллеливания, а X — количество параллельных копий

     Рекомендуем начать работу с 16 параллельных копий и увеличить количество потоков в зависимости от доступных ресурсов.

> [!IMPORTANT]
> Не поддерживаются следующие типы файлов Linux: символьные ссылки, символьные файлы, блочные файлы, сокеты и каналы. Эти типы файлов приведут к сбоям на этапе **Подготовка к отправке**.

Откройте папку назначения для просмотра и проверки скопированных файлов. Если в процессе копирования возникли ошибки, скачайте файл с ошибками для устранения неполадок. См. подробнее о [просмотре журналов ошибок во время копирования данных в Data Box Heavy](data-box-logs.md#view-error-log-during-data-copy). См. подробный список ошибок во время копирования данных в руководстве по [устранению неполадок с Azure Data Box Heavy](data-box-troubleshoot.md).

Чтобы обеспечить целостность данных, при копировании данных система вычисляет их контрольные суммы. По завершении копирования проверьте использованное и свободное место на устройстве.
    
   ![Проверка свободного и использованного места на панели мониторинга](media/data-box-deploy-copy-data/verify-used-space-dashboard.png)


## <a name="next-steps"></a>Дополнительная информация

Из этого руководства вы узнали следующие сведения об Azure Data Box Heavy:

> [!div class="checklist"]
> * Предварительные требования
> * подключение к Azure Data Box Heavy;
> * копирование данных в Data Box Heavy.


Перейдите к следующему руководству, чтобы узнать, как отправить свой Data Box обратно в корпорацию Майкрософт.

> [!div class="nextstepaction"]
> [Отправка Azure Data Box Heavy в Майкрософт](./data-box-heavy-deploy-picked-up.md)

