---
author: aahill
ms.author: aahi
ms.service: cognitive-services
ms.topic: include
ms.date: 10/08/2019
ms.openlocfilehash: 5089af4a4e1714d49b844a1b6823487a3f6a8dcf
ms.sourcegitcommit: 8cf199fbb3d7f36478a54700740eb2e9edb823e8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/25/2019
ms.locfileid: "74483018"
---
Начните использовать службу "Детектор аномалий", создав один из ресурсов Azure ниже.

* <a href="https://azure.microsoft.com/try/cognitive-services/#decision" target="_blank" rel="noopener">Создание пробного ресурса (открывается на новой вкладке)</a>
    * Подписка Azure не требуется: 
    * срок действия —7 дней, бесплатно. После регистрации ключ пробной версии и конечная точка будут доступными на [веб-сайте Azure](https://azure.microsoft.com/try/cognitive-services/my-apis/). 
    * Это отличный вариант, если вы хотите опробовать Детектор аномалий, но у вас нет подписки Azure.

* <a href="https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesAnomalyDetector" target="_blank" rel="noopener">Создание ресурса "Детектор аномалий" (открывается на новой вкладке)</a>
    * доступен на портале Azure до удаления.
    * Используйте бесплатную ценовую категорию, чтобы опробовать службу, а затем выполните обновление до платного уровня для рабочей среды.



### <a name="create-an-environment-variable"></a>Создание переменной среды

>[!NOTE]
> Конечные точки для ресурсов не из пробной версии, созданных после 1июля 2019 г., поддерживают пользовательский формат поддомена, показанный ниже. Дополнительные сведения и полный список региональных конечных точек см. в статье [Custom subdomain names for Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-custom-subdomains) (Пользовательские имена поддоменов для Cognitive Services). 

Используя ключ и конечную точку из созданного ресурса, создайте две переменные среды для проверки подлинности:

* `ANOMALY_DETECTOR_KEY` — ключ ресурса для проверки подлинности запросов.
* `ANOMALY_DETECTOR_ENDPOINT` — конечная точка ресурса для отправки запросов API. Она должна выглядеть так: 
  * `https://<your-custom-subdomain>.api.cognitive.microsoft.com` 

Используйте инструкции для своей операционной системы.

#### <a name="windowstabwindows"></a>[Windows](#tab/windows)

```console
setx ANOMALY_DETECTOR_KEY <replace-with-your-anomaly-detector-key>
setx ANOMALY_DETECTOR_ENDPOINT <replace-with-your-anomaly-detector-endpoint>
```

Добавив переменную среды, перезапустите окно консоли.

#### <a name="linuxtablinux"></a>[Linux](#tab/linux)

```bash
export ANOMALY_DETECTOR_KEY=<replace-with-your-anomaly-detector-key>
export ANOMALY_DETECTOR_ENDPOINT=<replace-with-your-anomaly-detector-endpoint>
```

После добавления переменной среды запустите `source ~/.bashrc` из окна консоли, чтобы применить изменения.

#### <a name="macostabunix"></a>[macOS](#tab/unix)

Измените `.bash_profile` и добавьте переменную среды:

```bash
export ANOMALY_DETECTOR_KEY=<replace-with-your-anomaly-detector-key>
export ANOMALY_DETECTOR_ENDPOINT=<replace-with-your-anomaly-detector-endpoint>
```

После добавления переменной среды запустите `source .bash_profile` из окна консоли, чтобы применить изменения.

***