---
title: Копирование данных из Amazon S3 в службу хранилища Azure с помощью AzCopy | Документация Майкрософт
description: Перенос данных с помощью AzCopy и Amazon S3 контейнеров
services: storage
author: normesta
ms.service: storage
ms.topic: conceptual
ms.date: 01/13/2020
ms.author: normesta
ms.subservice: common
ms.openlocfilehash: a3180593eaf8c01c772fd761d88b5f5b9f7657ee
ms.sourcegitcommit: b5106424cd7531c7084a4ac6657c4d67a05f7068
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/14/2020
ms.locfileid: "75941503"
---
# <a name="copy-data-from-amazon-s3-to-azure-storage-by-using-azcopy"></a>Копирование данных из Amazon S3 в службу хранилища Azure с помощью AzCopy

AzCopy — это служебная программа командной строки, которую можно использовать для копирования больших двоичных объектов или файлов в учетную запись хранения или из нее. Эта статья поможет вам скопировать объекты, каталоги и контейнеры из Amazon Web Services (AWS) S3 в хранилище BLOB-объектов Azure с помощью AzCopy.

## <a name="choose-how-youll-provide-authorization-credentials"></a>Выбор порядка предоставления учетных данных для авторизации

* Для авторизации в службе хранилища Azure используйте Azure Active Directory (AD) или маркер подписанного URL-адрес (SAS).

* Для авторизации с помощью AWS S3 используйте ключ доступа AWS и секретный ключ доступа.

### <a name="authorize-with-azure-storage"></a>Авторизация в службе хранилища Azure

Ознакомьтесь с статьей начало [работы с AzCopy](storage-use-azcopy-v10.md) , чтобы скачать AzCopy, и выберите способ предоставления учетных данных авторизации для службы хранилища.

> [!NOTE]
> В примерах, приведенных в этой статье, предполагается, что вы прошли проверку подлинности с помощью команды `AzCopy login`. Затем AzCopy использует учетную запись Azure AD для авторизации доступа к данным в хранилище BLOB-объектов.
>
> Если вы предпочитаете использовать маркер SAS для авторизации доступа к данным большого двоичного объекта, можно добавить этот маркер к URL-адресу ресурса в каждой команде AzCopy.
>
> Например: `https://mystorageaccount.blob.core.windows.net/mycontainer?<SAS-token>`.

### <a name="authorize-with-aws-s3"></a>Авторизация с помощью AWS S3

Соберите ключ доступа AWS и секретный ключ доступа, а затем задайте следующие переменные среды:

| Операционная система | Get-Help  |
|--------|-----------|
| **Windows** | `set AWS_ACCESS_KEY_ID=<access-key>`<br>`set AWS_SECRET_ACCESS_KEY=<secret-access-key>` |
| **Linux** | `export AWS_ACCESS_KEY_ID=<access-key>`<br>`export AWS_SECRET_ACCESS_KEY=<secret-access-key>` |
| **MacOS** | `export AWS_ACCESS_KEY_ID=<access-key>`<br>`export AWS_SECRET_ACCESS_KEY=<secret-access-key>`|

## <a name="copy-objects-directories-and-buckets"></a>Копирование объектов, каталогов и контейнеров

AzCopy использует [блок размещения из API URL-адреса](https://docs.microsoft.com/rest/api/storageservices/put-block-from-url) , поэтому данные копируются непосредственно между AWS S3 и серверами хранилища. Эти операции копирования не используют пропускную способность сети компьютера.

> [!IMPORTANT]
> Эта функция в настоящее время находится на стадии предварительной версии. Если вы решили удалить данные из контейнеров S3 после операции копирования, убедитесь, что данные были правильно скопированы в учетную запись хранения перед удалением данных.

> [!TIP]
> В примерах этого раздела аргументы пути заключаются в одинарные кавычки (' '). Используйте одинарные кавычки во всех командных оболочках, кроме командной оболочки Windows (cmd. exe). Если вы используете командную оболочку Windows (cmd. exe), заключите аргументы пути в двойные кавычки ("") вместо одинарных кавычек ("").

 Эти примеры также работают с учетными записями, имеющими иерархическое пространство имен. [Многопротокольный доступ на Data Lake Storage](../blobs/data-lake-storage-multi-protocol-access.md) позволяет использовать один и тот же синтаксис URL-адреса (`blob.core.windows.net`) для этих учетных записей. 

### <a name="copy-an-object"></a>Копирование объекта

Используйте тот же синтаксис URL-адреса (`blob.core.windows.net`) для учетных записей, имеющих иерархическое пространство имен.

|    |     |
|--------|-----------|
| **Синтаксис** | `azcopy copy 'https://s3.amazonaws.com/<bucket-name>/<object-name>' 'https://<storage-account-name>.blob.core.windows.net/<container-name>/<blob-name>'` |
| **Пример** | `azcopy copy 'https://s3.amazonaws.com/mybucket/myobject' 'https://mystorageaccount.blob.core.windows.net/mycontainer/myblob'` |
| **Пример** (иерархическое пространство имен) | `azcopy copy 'https://s3.amazonaws.com/mybucket/myobject' 'https://mystorageaccount.blob.core.windows.net/mycontainer/myblob'` |

> [!NOTE]
> В примерах в этой статье используются URL-адреса в формате Path для контейнеров AWS S3 (например: `http://s3.amazonaws.com/<bucket-name>`). 
>
> Кроме того, можно использовать виртуальные URL-адреса в стиле (например: `http://bucket.s3.amazonaws.com`). 
>
> Дополнительные сведения о виртуальном размещении сегментов см. в разделе [виртуальное размещение сегментов]] (https://docs.aws.amazon.com/AmazonS3/latest/dev/VirtualHosting.html).

### <a name="copy-a-directory"></a>Копирование каталога

Используйте тот же синтаксис URL-адреса (`blob.core.windows.net`) для учетных записей, имеющих иерархическое пространство имен.

|    |     |
|--------|-----------|
| **Синтаксис** | `azcopy copy 'https://s3.amazonaws.com/<bucket-name>/<directory-name>' 'https://<storage-account-name>.blob.core.windows.net/<container-name>/<directory-name>' --recursive=true` |
| **Пример** | `azcopy copy 'https://s3.amazonaws.com/mybucket/mydirectory' 'https://mystorageaccount.blob.core.windows.net/mycontainer/mydirectory' --recursive=true` |
| **Пример** (иерархическое пространство имен)| `azcopy copy 'https://s3.amazonaws.com/mybucket/mydirectory' 'https://mystorageaccount.blob.core.windows.net/mycontainer/mydirectory' --recursive=true` |

### <a name="copy-a-bucket"></a>Копирование контейнера

Используйте тот же синтаксис URL-адреса (`blob.core.windows.net`) для учетных записей, имеющих иерархическое пространство имен.

|    |     |
|--------|-----------|
| **Синтаксис** | `azcopy copy 'https://s3.amazonaws.com/<bucket-name>' 'https://<storage-account-name>.blob.core.windows.net/<container-name>' --recursive=true` |
| **Пример** | `azcopy copy 'https://s3.amazonaws.com/mybucket' 'https://mystorageaccount.blob.core.windows.net/mycontainer' --recursive=true` |
| **Пример** (иерархическое пространство имен)| `azcopy copy 'https://s3.amazonaws.com/mybucket/mydirectory' 'https://mystorageaccount.blob.core.windows.net/mycontainer/mydirectory' --recursive=true` |

### <a name="copy-all-buckets-in-all-regions"></a>Копировать все контейнеры во всех регионах

Используйте тот же синтаксис URL-адреса (`blob.core.windows.net`) для учетных записей, имеющих иерархическое пространство имен.

|    |     |
|--------|-----------|
| **Синтаксис** | `azcopy copy 'https://s3.amazonaws.com/' 'https://<storage-account-name>.blob.core.windows.net' --recursive=true` |
| **Пример** | `azcopy copy 'https://s3.amazonaws.com' 'https://mystorageaccount.blob.core.windows.net' --recursive=true` |
| **Пример** (иерархическое пространство имен)| `azcopy copy 'https://s3.amazonaws.com/mybucket/mydirectory' 'https://mystorageaccount.blob.core.windows.net/mycontainer/mydirectory' --recursive=true` |

### <a name="copy-all-buckets-in-a-specific-s3-region"></a>Копирование всех контейнеров в заданном регионе S3

Используйте тот же синтаксис URL-адреса (`blob.core.windows.net`) для учетных записей, имеющих иерархическое пространство имен.

|    |     |
|--------|-----------|
| **Синтаксис** | `azcopy copy 'https://s3-<region-name>.amazonaws.com/' 'https://<storage-account-name>.blob.core.windows.net' --recursive=true` |
| **Пример** | `azcopy copy 'https://s3-rds.eu-north-1.amazonaws.com' 'https://mystorageaccount.blob.core.windows.net' --recursive=true` |
| **Пример** (иерархическое пространство имен)| `azcopy copy 'https://s3.amazonaws.com/mybucket/mydirectory' 'https://mystorageaccount.blob.core.windows.net/mycontainer/mydirectory' --recursive=true` |

## <a name="handle-differences-in-object-naming-rules"></a>Обработку различий в правилах именования объектов

AWS S3 имеет разный набор соглашений об именовании для имен контейнеров по сравнению с контейнерами больших двоичных объектов Azure. Сведения о них можно прочитать [здесь](https://docs.aws.amazon.com/AmazonS3/latest/dev/BucketRestrictions.html#bucketnamingrules). Если вы решили скопировать группу контейнеров в учетную запись хранения Azure, операция копирования может завершиться ошибкой из-за различий в именах.

AzCopy обрабатывает две наиболее распространенные проблемы, которые могут возникнуть. контейнеры, содержащие периоды и сегменты, которые содержат последовательные дефисы. Имена контейнеров S3 AWS могут содержать точки и последовательные дефисы, но контейнер в Azure не может. AzCopy заменяет точки дефисами и последовательными дефисами числом, представляющим количество последовательных дефисов (например, контейнер с именем `my----bucket` преобразуется в `my-4-bucket`. 

Кроме того, так как AzCopy выполняет копирование файлов, он проверяет наличие конфликтов именования и пытается разрешить их. Например, если есть контейнеры с именем `bucket-name` и `bucket.name`, то AzCopy разрешает контейнер с именем `bucket.name` сначала `bucket-name`, а затем в `bucket-name-2`.

## <a name="handle-differences-in-object-metadata"></a>Обработку различий в метаданных объекта

AWS S3 и Azure позволяют использовать разные наборы символов в именах объектных ключей. Вы можете ознакомиться с символами, которые использует AWS S3 [здесь](https://docs.aws.amazon.com/AmazonS3/latest/dev/UsingMetadata.html#object-keys). На стороне Azure ключи объектов большого двоичного объекта соответствуют правилам именования [ C# идентификаторов](https://docs.microsoft.com/dotnet/csharp/language-reference/).

В рамках команды AzCopy `copy` можно указать значение для необязательного флага `s2s-invalid-metadata-handle`, который определяет, как следует выполнять обработку файлов, в которых метаданные файла содержат несовместимые имена ключей. В следующей таблице описывается каждое значение флага.

| Значение флага | Description  |
|--------|-----------|
| **ексклудеифинвалид** | (Параметр по умолчанию) Метаданные не включаются в передаваемый объект. AzCopy регистрирует предупреждение. |
| **фаилифинвалид** | Объекты не копируются. AzCopy регистрирует ошибку и включает эту ошибку в число сбоев, которое отображается в сводке по перемещению.  |
| **ренамеифинвалид**  | AzCopy разрешает недопустимый ключ метаданных и копирует объект в Azure с помощью разрешенной пары "ключ — значение метаданных". Чтобы узнать, какие действия AzCopy для переименования объектных ключей, ознакомьтесь с разделом [как AzCopy переименовывает ключи объектов](#rename-logic) ниже. Если AzCopy не удается переименовать ключ, объект не будет скопирован. |

<a id="rename-logic" />

### <a name="how-azcopy-renames-object-keys"></a>Как AzCopy переименовывает объектные ключи

AzCopy выполняет следующие действия:

1. Заменяет недопустимые символы символом "_".

2. Добавляет строку `rename_` в начало нового допустимого ключа.

   Этот ключ будет использоваться для сохранения исходного **значения**метаданных.

3. Добавляет строку `rename_key_` в начало нового допустимого ключа.
   Этот ключ будет использоваться для сохранения исходных метаданных недопустимого **ключа**.
   Этот ключ можно использовать для попыток восстановления метаданных на стороне Azure, так как ключ метаданных сохраняется как значение в службе хранилища BLOB-объектов.

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные примеры приведены в любой из следующих статей:

- [Get started with AzCopy](storage-use-azcopy-v10.md) (Начало работы с AzCopy)

- [Transfer data with AzCopy and blob storage](storage-use-azcopy-blobs.md) (Передача данных с помощью AzCopy и хранилища BLOB-объектов)

- [Transfer data with AzCopy and file storage](storage-use-azcopy-files.md) (Передача данных с помощью AzCopy и хранилища файлов)

- [Configure, optimize, and troubleshoot AzCopy](storage-use-azcopy-configure.md) (Настройка, оптимизация и устранение неполадок с AzCopy)