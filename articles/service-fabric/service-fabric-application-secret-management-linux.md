---
title: Настройка сертификата шифрования в кластерах Linux
description: Узнайте, как настроить сертификат шифрования и шифровать секреты в кластерах Linux.
author: shsha
ms.topic: conceptual
ms.date: 01/04/2019
ms.author: shsha
ms.openlocfilehash: 350718e4ce890fcbfaa7f2b10cc4c47dfac4da90
ms.sourcegitcommit: 003e73f8eea1e3e9df248d55c65348779c79b1d6
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/02/2020
ms.locfileid: "75614712"
---
# <a name="set-up-an-encryption-certificate-and-encrypt-secrets-on-linux-clusters"></a>Настройка сертификата шифрования и шифрование секретов в кластерах Linux
В этой статье описывается, как настроить сертификат шифрования и использовать его для шифрования секретов в кластерах Linux. Сведения о кластерах Windows см. [в разделе Настройка сертификата шифрования и шифрование секретов в кластерах Windows][secret-management-windows-specific-link].

## <a name="obtain-a-data-encipherment-certificate"></a>Получение сертификата шифрования данных
Сертификат о шифровании данных используется исключительно для шифрования и расшифровки [параметров][parameters-link] в параметрах. XML и [переменные среды][environment-variables-link] в службе ServiceManifest. XML. Он не применяется для аутентификации или подписывания зашифрованного текста. Сертификат должен отвечать приведенным ниже требованиям.

* Сертификат должен содержать закрытый ключ.
* Использование ключа сертификата должно включать шифрование данных (10) и не должно включать аутентификацию сервера или клиента.

  Например, для создания требуемого сертификата с помощью OpenSSL можно использовать следующие команды.
  
  ```console
  user@linux:~$ openssl req -newkey rsa:2048 -nodes -keyout TestCert.prv -x509 -days 365 -out TestCert.pem
  user@linux:~$ cat TestCert.prv >> TestCert.pem
  ```

## <a name="install-the-certificate-in-your-cluster"></a>Установка сертификата в кластере
Этот сертификат должен быть установлен в папке `/var/lib/sfcerts` на каждом узле в кластере. У учетной записи пользователя, с помощью которой выполняется служба (по умолчанию — sfuser), должен быть **доступ на чтение** к установленному сертификату (то есть `/var/lib/sfcerts/TestCert.pem` в текущем примере).

## <a name="encrypt-secrets"></a>Шифрование секретов
Следующий фрагмент кода может использоваться для шифрования секрета. Он только шифрует значение, но **не** подписывает зашифрованный текст. Чтобы создать зашифрованные данные для значений секретов, **необходимо использовать** тот же сертификат шифрования, установленный в кластере.

```console
user@linux:$ echo "Hello World!" > plaintext.txt
user@linux:$ iconv -f ASCII -t UTF-16LE plaintext.txt -o plaintext_UTF-16.txt
user@linux:$ openssl smime -encrypt -in plaintext_UTF-16.txt -binary -outform der TestCert.pem | base64 > encrypted.txt
```
Результирующая строка в кодировке Base-64 в файле encrypted.txt содержит как зашифрованные данные секрета, так и сведения о сертификате, с помощью которого выполнено шифрование. Можно проверить его действительность, расшифровав его с помощью OpenSSL.
```console
user@linux:$ cat encrypted.txt | base64 -d | openssl smime -decrypt -inform der -inkey TestCert.prv
```

## <a name="next-steps"></a>Дальнейшие действия
Узнайте, как [указать зашифрованные секреты в приложении.][secret-management-specify-encrypted-secrets-link]

<!-- Links -->
[parameters-link]:service-fabric-how-to-parameterize-configuration-files.md
[environment-variables-link]: service-fabric-how-to-specify-environment-variables.md
[secret-management-windows-specific-link]: service-fabric-application-secret-management-windows.md
[secret-management-specify-encrypted-secrets-link]: service-fabric-application-secret-management.md#specify-encrypted-secrets-in-an-application
