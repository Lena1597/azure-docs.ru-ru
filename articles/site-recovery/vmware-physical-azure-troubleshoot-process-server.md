---
title: Устранение неполадок сервера обработки Azure Site Recovery
description: В этой статье описывается, как устранять неполадки Azure Site Recovery сервере обработки.
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: troubleshooting
ms.date: 09/09/2019
ms.author: raynew
ms.openlocfilehash: 812cd0293f9627b7438e9870d8985e71dae1d147
ms.sourcegitcommit: fa4852cca8644b14ce935674861363613cf4bfdf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/09/2019
ms.locfileid: "70813406"
---
# <a name="troubleshoot-the-process-server"></a>Устранение неполадок сервера обработки

[Site Recovery](site-recovery-overview.md) сервер обработки используется при настройке аварийного восстановления в Azure для локальных виртуальных машин VMware и физических серверов. В этой статье описывается, как устранять неполадки сервера обработки, в том числе проблемы с репликацией и подключением.

Дополнительные [сведения](vmware-physical-azure-config-process-server-overview.md) о сервере обработки.

## <a name="before-you-start"></a>Перед началом

Перед началом устранения неполадок:

1. Убедитесь, что вы понимаете, как [отслеживать серверы обработки](vmware-physical-azure-monitor-process-server.md).
2. Ознакомьтесь с рекомендациями ниже.
3. Убедитесь, что соблюдены [требования к емкости](site-recovery-plan-capacity-vmware.md#capacity-considerations)и используется руководство по выбору размеров для [сервера конфигурации](site-recovery-plan-capacity-vmware.md#size-recommendations-for-the-configuration-server-and-inbuilt-process-server) или [изолированных серверов обработки](site-recovery-plan-capacity-vmware.md#size-recommendations-for-the-process-server).

## <a name="best-practices-for-process-server-deployment"></a>Рекомендации по развертыванию сервера обработки

Для достижения оптимальной производительности серверов обработки мы обобщены некоторые общие рекомендации.

**Рекомендации** | **Сведения**
--- |---
**Использование** | Убедитесь, что сервер конфигурации или изолированный сервер обработки используются только в целях назначения. Не запускайте никаких других действий на компьютере.
**IP-адрес** | Убедитесь, что сервер обработки имеет статический IPv4-адрес и не настроен NAT.
**Контроль использования памяти и ЦП** |Обеспечьте использование ЦП и памяти в течение 70%.
**Обеспечение свободного места** | Свободное место — это дисковое пространство кэша на сервере обработки. Данные репликации хранятся в кэше перед их отправкой в Azure.<br/><br/> Сохраняет свободное место свыше 25%. Если он превысит 20%, репликация для реплицированных компьютеров, связанных с сервером обработки, регулируется.

## <a name="check-process-server-health"></a>Проверка работоспособности сервера обработки

Первым шагом в устранении неполадок является проверка работоспособности и состояния сервера обработки. Для этого просмотрите все оповещения, проверьте, выполняются ли необходимые службы, и убедитесь в наличии пульса от сервера обработки. Эти действия приведены на приведенном ниже рисунке, за которым следуют процедуры, помогающие выполнить эти шаги.

![Устранение неполадок работоспособности сервера обработки](./media/vmware-physical-azure-troubleshoot-process-server/troubleshoot-process-server-health.png)

## <a name="step-1-troubleshoot-process-server-health-alerts"></a>Шаг 1. Устранение неполадок с оповещениями о работоспособности сервера обработки

Сервер обработки создает ряд оповещений о работоспособности. Эти предупреждения и рекомендуемые действия приведены в следующей таблице.

**Тип оповещения** | **Ошибка** | **Устранение неполадок**
--- | --- | --- 
![Работоспособна][green] | Отсутствуют  | Сервер обработки подключен и является работоспособным.
![Предупреждение][yellow] | Указанные службы не работают. | 1. Проверьте, работают ли службы.<br/> 2. Если службы работают должным образом, следуйте приведенным ниже инструкциям, чтобы устранить неполадки [подключения и репликации](#check-connectivity-and-replication).
![Предупреждение][yellow]  | Загрузка ЦП > 80% за последние 15 минут. | 1. Не добавляйте новые компьютеры.<br/>2. Убедитесь, что количество виртуальных машин, использующих сервер обработки, соответствует [заданным ограничениям](site-recovery-plan-capacity-vmware.md#capacity-considerations)и рассмотрите возможность настройки [дополнительного сервера обработки](vmware-azure-set-up-process-server-scale.md).<br/>3. Следуйте приведенным ниже инструкциям, чтобы устранить неполадки [подключения и репликации](#check-connectivity-and-replication).
![Critical][red] |  Загрузка ЦП > 95% за последние 15 минут. | 1. Не добавляйте новые компьютеры.<br/>2. Убедитесь, что количество виртуальных машин, использующих сервер обработки, соответствует [заданным ограничениям](site-recovery-plan-capacity-vmware.md#capacity-considerations)и рассмотрите возможность настройки [дополнительного сервера обработки](vmware-azure-set-up-process-server-scale.md).<br/>3. Следуйте приведенным ниже инструкциям, чтобы устранить неполадки [подключения и репликации](#check-connectivity-and-replication).<br/> 4. Если проблема не исчезнет, запустите [планировщик развертывания](https://aka.ms/asr-v2a-deployment-planner) для репликации VMware или физического сервера.
![Предупреждение][yellow] | Использование памяти > 80% за последние 15 минут. |  1. Не добавляйте новые компьютеры.<br/>2. Убедитесь, что количество виртуальных машин, использующих сервер обработки, соответствует [заданным ограничениям](site-recovery-plan-capacity-vmware.md#capacity-considerations)и рассмотрите возможность настройки [дополнительного сервера обработки](vmware-azure-set-up-process-server-scale.md).<br/>3. Выполните все инструкции, связанные с предупреждением.<br/> 4. Если проблема не исчезнет, следуйте приведенным ниже инструкциям, чтобы устранить неполадки [подключения и репликации](#check-connectivity-and-replication).
![Critical][red] | Использование памяти > 95% за последние 15 минут. | 1. Не добавляйте новые компьютеры и просматривая настройку [дополнительного сервера обработки](vmware-azure-set-up-process-server-scale.md).<br/> 2. Выполните все инструкции, связанные с предупреждением.<br/> 3.4. Если проблема сохраняется, следуйте приведенным ниже инструкциям, чтобы устранить неполадки [подключения и репликации](#check-connectivity-and-replication).<br/> 4. Если проблема будет повторяться, выполните [планировщик развертывания](https://aka.ms/asr-v2a-deployment-planner) проблемы репликации VMware или физического сервера.
![Предупреждение][yellow] | Свободное место в папке кэша < 30% за последние 15 минут. | 1. Не добавляйте новые компьютеры и рассмотрите возможность настройки [дополнительного сервера обработки](vmware-azure-set-up-process-server-scale.md).<br/>2. Убедитесь, что количество виртуальных машин, использующих сервер обработки, соответствует [рекомендациям](site-recovery-plan-capacity-vmware.md#capacity-considerations).<br/> 3. Следуйте приведенным ниже инструкциям, чтобы устранить неполадки [подключения и репликации](#check-connectivity-and-replication).
![Critical][red] |  Свободное место < 25% за последние 15 минут | 1. Следуйте инструкциям, связанным с предупреждением для этой проблемы.<br/> 2.3. Следуйте приведенным ниже инструкциям, чтобы устранить неполадки [подключения и репликации](#check-connectivity-and-replication).<br/> 3. Если проблема не исчезнет, запустите [планировщик развертывания](https://aka.ms/asr-v2a-deployment-planner) для репликации VMware или физического сервера.
![Critical][red] | Нет пульса от сервера обработки в течение 15 минут или более. Служба тмансвс не взаимодействует с сервером конфигурации. | 1) убедитесь, что сервер обработки работает.<br/> 2. Убедитесь, что тмассвк работает на сервере обработки.<br/> 3. Следуйте приведенным ниже инструкциям, чтобы устранить неполадки [подключения и репликации](#check-connectivity-and-replication).


![Ключ таблицы](./media/vmware-physical-azure-troubleshoot-process-server/table-key.png)


## <a name="step-2-check-process-server-services"></a>Шаг 2. Проверка служб сервера обработки

Службы, которые должны выполняться на сервере обработки, приведены в следующей таблице. В службах существуют небольшие различия в зависимости от способа развертывания сервера обработки. 

Для всех служб, кроме агента Службы восстановления Microsoft Azure (obengine), убедитесь, что для Старттипе задано значение **автоматически** или **автоматически (отложенный запуск)** .
 
**Развертывание** | **Работающие службы**
--- | ---
**Сервер обработки на сервере конфигурации** | ProcessServer Процесссервермонитор; cxprocessserver Inmage PushInstall; Служба отправки журналов (Логуплоад); Служба приложения Inmage Scout; Агент Службы восстановления Microsoft Azure (obengine); Inmage Scout VX Agent — Sentinel/of POST (сважентс); tmansvc Служба веб-публикаций (W3SVC); MySQL Служба Site Recovery Microsoft Azure (DRA)
**Сервер обработки, работающий в качестве изолированного сервера** | ProcessServer Процесссервермонитор; cxprocessserver Inmage PushInstall; Служба отправки журналов (Логуплоад); Служба приложения Inmage Scout; Агент Службы восстановления Microsoft Azure (obengine); Inmage Scout VX Agent — Sentinel/of POST (сважентс); tmansvc.
**Сервер обработки, развернутый в Azure для восстановления размещения** | ProcessServer Процесссервермонитор; cxprocessserver Inmage PushInstall; Служба отправки журналов (Логуплоад)


## <a name="step-3-check-the-process-server-heartbeat"></a>Шаг 3. Проверка пульса сервера обработки

Если нет пульса от сервера обработки (код ошибки 806), выполните следующие действия.

1. Убедитесь, что виртуальная машина сервера обработки запущена.
2. Проверьте эти журналы на наличие ошибок.

    К:\програмдата\аср\хоме\свсистемс\евентманажер *. log C\ProgramData\ASR\home\svsystems\monitor_protection*. log

## <a name="check-connectivity-and-replication"></a>Проверка подключения и репликации

 Первоначальные и текущие сбои репликации часто вызываются проблемами подключения между исходными компьютерами и сервером обработки, а также между сервером обработки и Azure. Эти действия приведены на приведенном ниже рисунке, за которым следуют процедуры, помогающие выполнить эти шаги.

![Устранение неполадок подключения и репликации](./media/vmware-physical-azure-troubleshoot-process-server/troubleshoot-connectivity-replication.png)


## <a name="step-4-verify-time-sync-on-source-machine"></a>Шаг 4. Проверка синхронизации времени на исходном компьютере

Убедитесь, что системные дата и время для реплицируемого компьютера синхронизированы. [Подробнее](https://docs.microsoft.com/windows-server/networking/windows-time-service/accurate-time)

## <a name="step-5-check-anti-virus-software-on-source-machine"></a>Шаг 5. Проверка антивирусного по на исходном компьютере

Убедитесь, что антивирусная программа на реплицированном компьютере не блокирует Site Recovery. Если необходимо исключить Site Recovery из антивирусных программ, ознакомьтесь с [этой статьей](vmware-azure-set-up-source.md#azure-site-recovery-folder-exclusions-from-antivirus-program).

## <a name="step-6-check-connectivity-from-source-machine"></a>Шаг 6. Проверка подключения с исходного компьютера


1. При необходимости установите [клиент Telnet](https://technet.microsoft.com/library/cc771275(v=WS.10).aspx) на исходном компьютере. Не используйте команду ping.
2. С исходного компьютера проверьте связь с сервером обработки на HTTPS порт с помощью Telnet. По умолчанию 9443 — это HTTPS-порт для трафика репликации.

    `telnet <process server IP address> <port>`

3. Проверьте, успешно ли установлено соединение.


**Соединение** | **Сведения** | **Действие**
--- | --- | ---
**Удалось** | В Telnet отображается пустой экран, и сервер обработки доступен. | Дальнейшие действия не требуются.
**Неудачных** | Невозможно подключиться | Убедитесь, что на сервере обработки разрешен входящий порт 9443. Например, если у вас есть сеть периметра или экранная подсеть. Проверьте подключение еще раз.
**Частично успешно** | Вы можете подключиться, но на исходном компьютере появится сообщение о том, что сервер обработки недоступен. | Перейдите к следующей процедуре устранения неполадок.

## <a name="step-7-troubleshoot-an-unreachable-process-server"></a>Шаг 7. Устранение неполадок с недостижимым сервером обработки

Если сервер обработки недоступен с исходного компьютера, отобразится сообщение об ошибке 78186. Если не устранить эту ошибку, эта проблема приведет к тому, что точки восстановления с постоянными приложениями и сбоями не будут создаваться должным образом.

Для устранения неполадок проверьте, может ли исходный компьютер получить IP-адрес сервера обработки, и запустите средство ккспсклиент на исходном компьютере, чтобы проверить сквозное подключение.


### <a name="check-the-ip-connection-on-the-process-server"></a>Проверка IP-подключения на сервере обработки

Если Telnet выполняется успешно, но исходный компьютер сообщает о том, что сервер обработки недоступен, проверьте, доступен ли IP-адрес сервера обработки.

1. В веб-браузере попробуйте обратиться по IP-адресу https://< PS_IP >: < PS_Data_Port >/.
2. Если эта проверка показывает ошибку сертификата HTTPS, это нормально. Если вы пропустите эту ошибку, появится запрос 400-Bad. Это означает, что сервер не может обслуживать запрос браузера, а стандартное подключение по протоколу HTTPS — нормальное.
3. Если эта проверка не работает, обратите внимание на сообщение об ошибке в браузере. Например, ошибка 407 указывает на ошибку проверки подлинности прокси.

### <a name="check-the-connection-with-cxpsclient"></a>Проверка подключения с помощью ккспсклиент

Кроме того, можно запустить средство ккспсклиент, чтобы проверить сквозное подключение.

1. Запустите средство следующим образом:

    ```
    <install folder>\cxpsclient.exe -i <PS_IP> -l <PS_Data_Port> -y <timeout_in_secs:recommended 300>
    ```

2. На сервере обработки проверьте созданные журналы в следующих папках:

    К:\програмдата\аср\хоме\свсистемс\транспорт\лог\ккспс.ЕРР К:\програмдата\аср\хоме\свсистемс\транспорт\лог\ккспс.ксфер



### <a name="check-source-vm-logs-for-upload-failures-error-78028"></a>Проверьте журналы исходной виртуальной машины на наличие ошибок отправки (ошибка 78028).

Проблемы с передачей данных, заблокированных на исходных компьютерах в службе обработки, могут привести к тому, что точки восстановления с последовательным сбоем и целостностью приложений не создаются. 

1. Чтобы устранить неполадки при сетевой передаче, можно просмотреть ошибки в этом журнале:

    C:\Program Files (x86) \Microsoft Azure сайт Рековери\ажент\сважентс *. log 

2. Используйте остальные процедуры, описанные в этой статье, чтобы устранить проблемы, связанные с отправкой данных.



## <a name="step-8-check-whether-the-process-server-is-pushing-data"></a>Шаг 8. Проверка того, отправляет ли сервер обработки данные

Убедитесь, что сервер обработки активно отправляет данные в Azure.

  1. Откройте диспетчер задач на сервере обработки (нажмите клавиши CTRL+SHIFT+ESC).
  2. Выберите вкладку **производительность** > **откройте монитор ресурсов**.
  3. На странице **Монитор ресурсов** выберите вкладку **сеть** . В разделе **процессы с сетевой активностью**проверьте, отправляют ли CBengine. exe большой объем данных.

       ![Тома в процессах с сетевой активностью](./media/vmware-physical-azure-troubleshoot-process-server/cbengine.png)

  Если файл cbengine.exe не отправляет большие объемы данных, выполните действия, указанные в следующих разделах.

## <a name="step-9-check-the-process-server-connection-to-azure-blob-storage"></a>Шаг 9. Проверка подключения сервера обработки к хранилищу BLOB-объектов Azure

1. В монитор ресурсов выберите **CBengine. exe**.
2. В разделе **TCP-подключения**проверьте наличие подключения сервера обработки к службе хранилища Azure.

  ![Подключение между CBengine. exe и URL-адресом хранилища больших двоичных объектов Azure](./media/vmware-physical-azure-troubleshoot-process-server/rmonitor.png)

### <a name="check-services"></a>Проверка служб

Если нет подключения сервера обработки к URL-адресу хранилища больших двоичных объектов Azure, проверьте, работают ли службы.

1. На панели управления выберите **службы**.
2. Убедитесь, что выполняются следующие службы:

    - cxprocessserver;
    - InMage Scout VX Agent – Sentinel/Outpost;
    - Агент служб восстановления Microsoft Azure
    - служба Microsoft Azure Site Recovery;
    - tmansvc.

3. Запустите или перезапустите службы, которые не запущены.
4. Убедитесь, что сервер обработки подключен и доступен. 

## <a name="step-10-check-the-process-server-connection-to-azure-public-ip-address"></a>Шаг 10. Проверка подключения сервера обработки к общедоступному IP-адресу Azure

1. На сервере обработки в **%ProgramFiles%\Microsoft служб восстановления Azure ажент\темп**откройте последний файл журнал cbenginecurr. errlog.
2. В файле найдите значение **443**или, чтобы **попытка подключения строки завершилась неудачно**.

  ![Журналы ошибок во временной папке](./media/vmware-physical-azure-troubleshoot-process-server/logdetails1.png)

3. Если вы видите вопросы, расположение общедоступного IP-адреса Azure в файле журнал cbenginecurr. Куррлог с помощью порта 443:

  `telnet <your Azure Public IP address as seen in CBEngineCurr.errlog>  443`

5. В командной строке на сервере обработки выполните команду Telnet для проверки связи с общедоступным IP-адресом Azure.
6. Если не удается подключиться, выполните следующую процедуру.

## <a name="step-11-check-process-server-firewall-settings"></a>Шаг 11. Проверьте параметры брандмауэра сервера обработки. 

Проверьте, не блокирует ли доступ брандмауэр на основе IP-адресов на сервере обработки.

1. Для правил брандмауэра на основе IP-адресов:

    a) Скачайте полный список [диапазонов IP-адресов центра обработки данных Microsoft Azure](https://www.microsoft.com/download/details.aspx?id=41653).

    б) добавьте диапазоны IP-адресов в конфигурацию брандмауэра, чтобы убедиться, что брандмауэр разрешает обмен данными с Azure (и с портом HTTPS по умолчанию 443).

    в) разрешить диапазоны IP-адресов для региона Azure вашей подписки и для региона Azure "Западная часть США" (используется для управления доступом и удостоверениями).

2. Для брандмауэров на основе URL-адресов добавьте в конфигурацию брандмауэра URL-адреса, перечисленные в следующей таблице.

    [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]  


## <a name="step-12-verify-process-server-proxy-settings"></a>Шаг 12. Проверка параметров прокси-сервера обработки 

1. Если вы используете прокси-сервер, убедитесь, что его имя разрешается DNS-сервером. Проверьте значение, указанное при настройке сервера конфигурации в разделе реестра **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Azure site рековери\проксисеттингс**.
2. Убедитесь, что в агенте Azure Site Recovery используются одни и те же параметры для отправки данных.

    а) найдите **Microsoft Azure Backup**.

    б) откройте **Microsoft Azure Backup**и выберите **действие** > **изменить свойства**.

    в) на вкладке " **Конфигурация прокси** -сервера" адрес прокси-сервера должен совпадать с адресом прокси-сервера, показанным в параметрах реестра. В противном случае измените его на тот адрес.

## <a name="step-13-check-bandwidth"></a>Шаг 13. Проверить пропускную способность

Увеличьте пропускную способность между сервером обработки и Azure, а затем проверьте, не возникает ли проблема.


## <a name="next-steps"></a>Следующие шаги

Если вам нужна дополнительная помощь, задайте свой вопрос на [форуме Azure Site Recovery](https://social.msdn.microsoft.com/Forums/azure/home?forum=hypervrecovmgr). 

[green]: ./media/vmware-physical-azure-troubleshoot-process-server/green.png
[yellow]: ./media/vmware-physical-azure-troubleshoot-process-server/yellow.png
[red]: ./media/vmware-physical-azure-troubleshoot-process-server/red.png
