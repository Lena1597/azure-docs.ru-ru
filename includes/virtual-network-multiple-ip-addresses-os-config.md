---
title: включить файл
description: включить файл
services: virtual-network
author: jimdial
ms.service: virtual-network
ms.topic: include
ms.date: 05/10/2019
ms.author: anavin
ms.custom: include file
ms.openlocfilehash: a9473f69d600a86ff71da69c7efe0dea3f2b0a08
ms.sourcegitcommit: 5bbe87cf121bf99184cc9840c7a07385f0d128ae
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/16/2020
ms.locfileid: "76159027"
---
## <a name="os-config"></a>Добавление IP-адресов в операционную систему виртуальной машины

Подключитесь к виртуальной машине, созданной с несколькими частными IP-адресами, и войдите на нее. Все частные IP-адреса (включая основные), добавленные в виртуальную машину, необходимо добавить вручную. Выполните следующие действия в соответствии с операционной системой виртуальной машины.

### <a name="windows"></a>Windows

1. В командной строке введите *ipconfig /all*.  Отображается только *первичный* частный IP-адрес (через службу DHCP).
2. Введите *ncpa.cpl* в командной строке, чтобы открыть окно **Сетевые подключения**.
3. Откройте свойства для соответствующего адаптера: **Подключение по локальной сети**.
4. Дважды щелкните "Протокол IP версии 4 (IPv4)".
5. Выберите **Использовать следующий IP-адрес** и введите следующие значения:

    * **IP-адрес**— *первичный* частный IP-адрес.
    * **Маска подсети**— в зависимости от своей подсети. Например, если подсеть — /24, то маска подсети — 255.255.255.0.
    * **Шлюз по умолчанию**— первый IP-адрес в подсети. Если подсеть — 10.0.0.0/24, то IP-адрес шлюза по умолчанию — 10.0.0.1.
    * Щелкните **Использовать следующие адреса DNS-серверов** и введите следующие значения:
        * **Предпочитаемый DNS-сервер.** Введите 168.63.129.16, если собственный DNS-сервер не используется.  В противном случае введите IP-адрес этого DNS-сервера.
    * Нажмите кнопку **Дополнительно** и добавьте дополнительные IP-адреса. Добавьте каждый из дополнительных частных IP-адресов, добавленных в сетевой интерфейс Azure на предыдущем шаге, в сетевой интерфейс Windows, которому назначен основной IP-адрес, назначенный сетевому интерфейсу Azure.

        Никогда не следует вручную назначать общедоступный IP-адрес для виртуальной машины Azure в ее операционной системе. Если вы будете вручную устанавливать IP-адрес в операционной системе, убедитесь, что он соответствует частному IP-адресу, назначенному [сетевому интерфейсу](../articles/virtual-network/virtual-network-network-interface-addresses.md#change-ip-address-settings) Azure. Иначе соединение с виртуальной машиной может быть потеряно. Ознакомьтесь с дополнительными сведениями о параметрах [частных IP-адресов](../articles/virtual-network/virtual-network-network-interface-addresses.md#private). Никогда не следует назначать общедоступный IP-адрес Azure в операционной системе.

    * Нажмите кнопку **ОК**, чтобы закрыть настройки TCP/IP, а затем нажмите кнопку **ОК** еще раз, чтобы закрыть параметры адаптера. После этого подключение к удаленному рабочему столу будет восстановлено.

6. В командной строке введите *ipconfig /all*. Отобразятся все добавленные IP-адреса, а DHCP будет отключен.
7. Настройте Windows для использования частного IP-адреса основной IP-конфигурации в Azure в качестве основного IP-адрес для Windows. См. дополнительные сведения об [отсутствии доступа к Интернету из виртуальной машины Windows Azure с несколькими IP-адресами](https://support.microsoft.com/help/4040882/no-internet-access-from-azure-windows-vm-that-has-multiple-ip-addresse). 

### <a name="validation-windows"></a>Проверка (Windows)

Чтобы убедиться, что вы можете подключиться к Интернету с помощью дополнительной IP-конфигурации через связанный с ней общедоступный IP-адрес (если он был добавлен правильно), используйте следующую команду:

```bash
ping -S 10.0.0.5 hotmail.com
```
>[!NOTE]
>Для вторичных IP-конфигураций подключение к Интернету можно проверить, только если с конфигурацией связан общедоступный IP-адрес. В случае с первичными IP-конфигурациями для проверки связи с Интернетом общедоступный IP-адрес не требуется.

### <a name="linux-ubuntu-1416"></a>Linux (Ubuntu 14/16)

Рекомендуется просмотреть последнюю документацию по дистрибутиву Linux. 

1. Откройте окно терминала.
2. Нужно войти с правами привилегированного пользователя. В противном случае введите следующую команду:

   ```bash
   sudo -i
   ```

3. Обновите файл конфигурации сетевого интерфейса (предполагается "eth0").

   * Сохраните имеющийся элемент строки для DHCP. Основной IP-адрес остается настроенным, как и ранее.
   * Добавьте конфигурацию для дополнительного статического IP-адреса с помощью следующих команд:

     ```bash
     cd /etc/network/interfaces.d/
     ls
     ```

     Должен отобразиться CFG-файл.
4. Откройте файл. В конце этого файла будут следующие строки:

   ```bash
   auto eth0
   iface eth0 inet dhcp
   ```

5. Добавьте следующие строки после имеющихся строк в этом файле:

   ```bash
   iface eth0 inet static
   address <your private IP address here>
   netmask <your subnet mask>
   ```

6. Сохраните файл с помощью следующей команды:

   ```bash
   :wq
   ```

7. Сбросьте сетевой интерфейс, выполнив следующую команду:

   ```bash
   sudo ifdown eth0 && sudo ifup eth0
   ```

   > [!IMPORTANT]
   > Если используется удаленное подключение, запустите скрипты ifdown и ifup в одной и той же строке.
   >

8. Проверьте, добавлен ли IP-адрес к сетевому интерфейсу, с помощью следующей команды:

   ```bash
   ip addr list eth0
   ```

   Должен отобразиться IP-адрес, добавленный как часть списка.

### <a name="linux-ubuntu-1804"></a>Linux (Ubuntu 18.04 +)

Ubuntu 18,04 и выше изменились на `netplan` для управления сетью ОС. Рекомендуется просмотреть последнюю документацию по дистрибутиву Linux. 

1. Откройте окно терминала.
2. Нужно войти с правами привилегированного пользователя. В противном случае введите следующую команду:

    ```bash
    sudo -i
    ```

3. Создайте файл для второго интерфейса и откройте его в текстовом редакторе:

    ```bash
    vi /etc/netplan/60-static.yaml
    ```

4. Добавьте в файл следующие строки, заменив `10.0.0.6/24` на IP-адрес или сетевую маску:

    ```bash
    network:
        version: 2
        ethernets:
            eth0:
                addresses:
                    - 10.0.0.6/24
    ```

5. Сохраните файл с помощью следующей команды:

    ```bash
    :wq
    ```

6. Протестируйте изменения с помощью [нетплан попробуйте](http://manpages.ubuntu.com/manpages/cosmic/man8/netplan-try.8.html) проверить синтаксис:

    ```bash
    netplan try
    ```

> [!NOTE]
> `netplan try` применит эти изменения временно и извлечет изменения назад через 120 секунд. В случае потери подключения подождите 120 секунд, а затем снова подключитесь. В это время изменения будут отменены.

7. При отсутствии проблем с `netplan try`примените изменения конфигурации:

    ```bash
    netplan apply
    ```

8. Проверьте, добавлен ли IP-адрес к сетевому интерфейсу, с помощью следующей команды:

    ```bash
    ip addr list eth0
    ```

    Должен отобразиться IP-адрес, добавленный как часть списка. Пример:

    ```bash
    1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
        link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
        inet 127.0.0.1/8 scope host lo
        valid_lft forever preferred_lft forever
        inet6 ::1/128 scope host
        valid_lft forever preferred_lft forever
    2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP group default qlen 1000
        link/ether 00:0d:3a:8c:14:a5 brd ff:ff:ff:ff:ff:ff
        inet 10.0.0.6/24 brd 10.0.0.255 scope global eth0
        valid_lft forever preferred_lft forever
        inet 10.0.0.4/24 brd 10.0.0.255 scope global secondary eth0
        valid_lft forever preferred_lft forever
        inet6 fe80::20d:3aff:fe8c:14a5/64 scope link
        valid_lft forever preferred_lft forever
    ```
    
### <a name="linux-red-hat-centos-and-others"></a>Linux (Red Hat, CentOS и другие)

1. Откройте окно терминала.
2. Нужно войти с правами привилегированного пользователя. В противном случае введите следующую команду:

    ```bash
    sudo -i
    ```

3. Введите пароль и следуйте инструкциям. После того как вы станете привилегированным пользователем, перейдите к сетевой папке скриптов, используя следующую команду:

    ```bash
    cd /etc/sysconfig/network-scripts
    ```

4. Выведите список связанных IFCFG-файлов, используя следующую команду:

    ```bash
    ls ifcfg-*
    ```

    В списке файлов должен быть файл *ifcfg-eth0* .

5. Чтобы добавить IP-адрес, создайте для него файл конфигурации, как показано ниже. Обратите внимание, что такой файл должен быть создан для каждой IP-конфигурации.

    ```bash
    touch ifcfg-eth0:0
    ```

6. Откройте файл *ifcfg-eth0:0*, используя следующую команду.

    ```bash
    vi ifcfg-eth0:0
    ```

7. Добавьте содержимое в файл *eth0:0* (в этом случае) с помощью следующей команды. Не забудьте обновить данные на основании IP-адреса.

    ```bash
    DEVICE=eth0:0
    BOOTPROTO=static
    ONBOOT=yes
    IPADDR=192.168.101.101
    NETMASK=255.255.255.0
    ```

8. Сохраните файл, используя следующую команду:

    ```bash
    :wq
    ```

9. Перезапустите сетевые службы и убедитесь, что изменения вступили в силу, выполнив следующие команды:

    ```bash
    /etc/init.d/network restart
    ifconfig
    ```

    В возвращенном списке должен содержаться добавленный IP-адрес — *eth0:0*.

### <a name="validation-linux"></a>Проверка (Linux)

Чтобы убедиться, что вы можете подключиться к Интернету с помощью дополнительной IP-конфигурации через связанный с ней общедоступный IP-адрес, используйте следующую команду:

```bash
ping -I 10.0.0.5 hotmail.com
```
>[!NOTE]
>Для вторичных IP-конфигураций подключение к Интернету можно проверить, только если с конфигурацией связан общедоступный IP-адрес. В случае с первичными IP-конфигурациями для проверки связи с Интернетом общедоступный IP-адрес не требуется.

При проверке исходящего подключения из дополнительной сетевой карты для виртуальных машин Linux может потребоваться добавить соответствующие маршруты. Это можно сделать несколькими способами. Дополнительные сведения по дистрибутиву Linux см. в соответствующей документации. Ниже приведен один из методов добавления маршрутов.

```bash
echo 150 custom >> /etc/iproute2/rt_tables 

ip rule add from 10.0.0.5 lookup custom
ip route add default via 10.0.0.1 dev eth2 table custom

```
- Значения, которые требуется заменить:
    - **10.0.0.5** — на частный IP-адрес, у которого есть связанный с ним общедоступный IP-адрес;
    - **10.0.0.1** — на шлюз по умолчанию;
    - **eth2** — на имя дополнительной сетевой карты.
