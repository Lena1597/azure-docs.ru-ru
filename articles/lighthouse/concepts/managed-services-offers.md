---
title: Предложения управляемых служб в Azure Marketplace
description: Предложения управляемых служб позволяют поставщикам услуг продавать предложения по управлению ресурсами клиентам в Azure Marketplace.
ms.date: 12/16/2019
ms.topic: conceptual
ms.openlocfilehash: 1b4f0d7457a74afe710a48f429cfe47535a9b122
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/25/2019
ms.locfileid: "75453586"
---
# <a name="managed-services-offers-in-azure-marketplace"></a>Предложения управляемых служб в Azure Marketplace

В этой статье описывается новый тип предложения **управляемых служб** в [Azure Marketplace](https://azuremarketplace.microsoft.com). Предложения управляемых служб позволяют предлагать службы управления ресурсами клиентам с делегированным управлением ресурсами Azure. Эти предложения можно сделать доступными для всех потенциальных клиентов или только для одного или нескольких конкретных клиентов. Майкрософт плату не взымает, поскольку вы напрямую выставляете счет клиентам за расходы, связанные с этими управляемыми службами.

## <a name="understand-managed-services-offers"></a>Основные сведения о предложениях управляемых служб

Управляемые службы позволяют упростить процесс подключения клиентов до делегированного управления ресурсами Azure. После того как клиент приобретет предложение в Azure Marketplace, он сможет указать, какие подписки и/или группы ресурсов должны быть включены. Обратите внимание, что каждая подписка сначала должна быть авторизована для подключения путем регистрации поставщика ресурсов **Microsoft.ManagedServices** вручную.

После этого пользователи в вашей организации смогут выполнять задачи администрирования этих ресурсов из клиента организации в соответствии с типом доступа, определенным при создании предложения на [Портале Microsoft Cloud Partner](https://cloudpartner.azure.com/). Это делается с помощью манифеста, в котором указываются пользователи, группы и принципы службы Azure AD, которые будут иметь доступ к ресурсам клиента с помощью делегированного управления ресурсами Azure, вместе с ролями, которые определяют уровни доступа. Назначая разрешения группе Azure AD, а не серии отдельных учетных записей пользователей или приложений, при изменении требований к доступу вы сможете добавлять или удалять отдельных пользователей.

## <a name="public-and-private-offers"></a>Общедоступные и частные предложения

Каждое предложение управляемых служб включает в себя один или несколько планов. Планы могут быть либо частными, либо общедоступными. 

Чтобы ограничить предложение конкретными клиентами, можно опубликовать частный план. Когда это будет сделано, план может быть приобретен только для определенных] предоставленных вами кодов подписки. Дополнительные сведения см. в разделе [Private offers](../../marketplace/private-offers.md) (Частные предложения).

Общедоступные планы позволяют продвигать службы новым клиентам. Обычно они более уместны, когда требуется только ограниченный доступ к арендатору клиента. После установления связи с клиентами, если они решат предоставить организации дополнительный доступ, можете осуществить его, опубликовав новый частный план только для этого клиента, или [подключив клиента к делегированному управлению ресурсами Azure](../how-to/onboard-customer.md).

При необходимости в одно и то же предложение можно включить как общественные, так и частные планы.

> [!IMPORTANT]
> После публикации плана как общедоступного вы не сможете изменить его на частный. Чтобы контролировать, какие клиенты могут принимать ваши предложения и делегировать ресурсы, используйте частный план. С помощью общедоступного плана вы не сможете ограничить доступность определенными клиентами или даже определенным числом клиентов (хотя вы можете не допустить продажи плана полностью, если вы решили это сделать). В настоящее время нет механизма отклонять или удалять делегирования, когда клиент принимает предложение, хотя вы всегда можете обратиться к клиенту и попросить его [Удалить доступ](../how-to/view-manage-service-providers.md#add-or-remove-service-provider-offers).

## <a name="publish-managed-service-offers"></a>Публикация предложений управляемых служб

Чтобы узнать, как опубликовать предложение управляемых служб, см. раздел [Publish a managed services offer to Azure Marketplace](../how-to/publish-managed-services-offers.md) (Публикация предложений управляемых служб в Azure Marketplace). Общие сведения о публикации в Azure Marketplace с помощью портала Cloud Partner см. раздел [Azure Marketplace and AppSource publishing guide](../../marketplace/marketplace-publishers-guide.md) (Руководство по публикации в Azure Marketplace и AppSource) и [Manage Azure and AppSource Marketplace offers](../../marketplace/cloud-partner-portal/manage-offers/cpp-manage-offers.md) (Управление предложениями Azure и AppSource Marketplace).

## <a name="next-steps"></a>Дальнейшие действия

- Дополнительные сведения о [делегированном управлении ресурсами Azure](azure-delegated-resource-management.md) и [интерфейсах управления для различных клиентов](cross-tenant-management-experience.md).
- [Publish a managed services offer](../how-to/publish-managed-services-offers.md) (Публикация предложения управляемых служб) в Azure Marketplace.
