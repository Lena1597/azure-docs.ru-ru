---
title: Отображение математических функций в иммерсивное средство чтения
titleSuffix: Azure Cognitive Services
description: В этой статье показано, как отобразить математические вычисления в иммерсивное средство чтения.
author: pasta
manager: guillasi
ms.service: cognitive-services
ms.subservice: immersive-reader
ms.topic: conceptual
ms.date: 01/14/2020
ms.author: pasta
ms.openlocfilehash: e01cc9e8cedb6c38da0b56e04419c706d5d0566e
ms.sourcegitcommit: 49e14e0d19a18b75fd83de6c16ccee2594592355
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/14/2020
ms.locfileid: "75946125"
---
# <a name="how-to-display-math-in-the-immersive-reader"></a>Отображение математических функций в иммерсивное средство чтения

Иммерсивное средство чтения может отображать математические выражения при условии, что оно представлено в форме языка математической разметки ([масмл](https://developer.mozilla.org/docs/Web/MathML)).
Тип MIME можно задать с помощью иммерсивного [блока](../reference.md#chunk)чтения. Дополнительные сведения см. в разделе [Поддерживаемые типы MIME](../reference.md#supported-mime-types) .

## <a name="send-math-to-the-immersive-reader"></a>Отправка математических вычислений в иммерсивное средство чтения
Чтобы отправить математические данные в иммерсивное средство чтения, укажите блок, содержащий Масмл, и задайте для типа MIME значение ```application/mathml+xml```.

Например, если содержимое было следующим:

```html
<div id='ir-content'>
    <math xmlns='http://www.w3.org/1998/Math/MathML'>
        <mfrac>
            <mrow>
                <msup>
                    <mi>x</mi>
                    <mn>2</mn>
                </msup>
                <mo>+</mo>
                <mn>3</mn>
                <mi>x</mi>
                <mo>+</mo>
                <mn>2</mn>
            </mrow>
            <mrow>
                <mi>x</mi>
                <mo>−</mo>
                <mn>3</mn>
            </mrow>
        </mfrac>
        <mo>=</mo>
        <mn>4</mn>
    </math>
</div>
```

Затем можно отобразить содержимое с помощью следующего кода JavaScript.

```javascript
const data = {
    title: 'My Math',
    chunks: [{
        content: document.getElementById('ir-content').innerHTML.trim(),
        mimeType: 'application/mathml+xml'
    }]
};

ImmersiveReader.launchAsync(YOUR_TOKEN, YOUR_SUBDOMAIN, data, YOUR_OPTIONS);
```

При запуске иммерсивное средство чтения должно отобразиться следующее:

![Математические вычисления в иммерсивное средство чтения](../media/how-tos/1-math.png)

## <a name="next-steps"></a>Дальнейшие действия

* Ознакомьтесь с разделом о [пакете SDK для иммерсивного средства чтения](https://github.com/microsoft/immersive-reader-sdk) и [справочнике по этому пакету](../reference.md).