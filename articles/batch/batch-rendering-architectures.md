---
title: Эталонные архитектуры рендеринга в Azure — пакетная служба Azure
description: Архитектуры для применения пакетной службы Azure и других служб Azure для расширения локальных ферм рендеринга в облако
services: batch
ms.service: batch
author: davefellows
manager: evansma
ms.author: labrenne
ms.date: 02/07/2019
ms.topic: conceptual
ms.custom: seodec18
ms.openlocfilehash: 20442a6618ca9357bb3be95879b68bffca45a40d
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/05/2020
ms.locfileid: "77022959"
---
# <a name="reference-architectures-for-azure-rendering"></a>Эталонные архитектуры для рендеринга в Azure

В этой статье представлены обобщенные схемы нескольких архитектур, позволяющих расширить локальную ферму рендеринга в Azure. В этих примерах описаны несколько комбинаций служб Azure для вычислений, сетевого взаимодействия и хранения.

## <a name="hybrid-with-nfs-or-cfs"></a>Гибридное развертывание с NFS или CFS

На следующей схеме показан гибридный сценария, который включает в себя следующие службы Azure:

* **Служба вычислений** — пул пакетной службы Azure или масштабируемый набор виртуальных машин.

* **Сеть** — Azure ExpressRoute или VPN в локальной сети. Виртуальная сеть Azure.

* **Хранилище** — входные и выходные файлы NFS или CFS на базе виртуальных машин Azure, синхронизируемые с локальным хранилищем через службу синхронизации файлов Azure или RSync. Кроме того, Авере Вфкст к входным или выходным файлам с локальных устройств NAS с помощью NFS.

  ![Выход в облако — гибридное развертывание с NFS или CFS](./media/batch-rendering-architectures/hybrid-nfs-cfs-avere.png)

## <a name="hybrid-with-blobfuse"></a>Гибридное развертывание с Blobfuse

На следующей схеме показан гибридный сценария, который включает в себя следующие службы Azure:

* **Служба вычислений** — пул пакетной службы Azure или масштабируемый набор виртуальных машин.

* **Сеть** — Azure ExpressRoute или VPN в локальной сети. Виртуальная сеть Azure.

* **Хранилище** — входные и выходные файлы в хранилище BLOB-объектов, подключенном к вычислительным ресурсам через Azure Blobfuse.

  ![Выход в облако — гибридное развертывание с Blobfuse](./media/batch-rendering-architectures/hybrid-blob-fuse.png)

## <a name="hybrid-compute-and-storage"></a>Гибридное развертывание служб вычислений и хранилища

На следующей схеме представлен полностью подключенный гибридный сценарий для служб вычислений и хранилища, в который входят следующие службы Azure:

* **Служба вычислений** — пул пакетной службы Azure или масштабируемый набор виртуальных машин.

* **Сеть** — Azure ExpressRoute или VPN в локальной сети. Виртуальная сеть Azure.

* **Хранилище** — Avere vFXT между локальными средами. Необязательная архивация локальных файлов через Azure Data Box в хранилище BLOB-объектов или использование локального экземпляра Avere FXT для ускорения запоминающего устройства, подключаемого к сети.

  ![Выход в облако — гибридное развертывание служб вычислений и хранения](./media/batch-rendering-architectures/hybrid-compute-storage-avere.png)


## <a name="next-steps"></a>Дальнейшие действия

* Изучите сведения об использовании [диспетчеров рендеринга](batch-rendering-render-managers.md) с пакетной службой Azure.

* Изучите сведения о возможностях для [рендеринга в Azure](batch-rendering-service.md).