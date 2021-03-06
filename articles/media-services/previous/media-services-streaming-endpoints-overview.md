---
title: Vue d’ensemble des points de terminaison de streaming Azure Media Services | Microsoft Docs
description: Cette rubrique fournit une vue d’ensemble des points de terminaison de streaming Azure Media Services.
services: media-services
documentationcenter: ''
author: Juliako
writer: juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2019
ms.author: juliako
ms.openlocfilehash: a45e2af6f2cb9c105c084585a03a6de615fa1397
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64573040"
---
# <a name="streaming-endpoints-overview"></a>Vue d’ensemble des points de terminaison de streaming  

> [!NOTE]
> Aucune nouvelle fonctionnalité ni fonction n’est ajoutée à Media Services v2. <br/>Découvrez la dernière version, [Media Services v3](https://docs.microsoft.com/azure/media-services/latest/). Consultez également [conseils de migration de v2 vers v3](../latest/migrate-from-v2-to-v3.md)

Dans Microsoft Azure Media Services (AMS), un **point de terminaison de streaming** représente un service de streaming qui peut fournir du contenu directement à une application de lecteur cliente ou à un réseau de distribution de contenu (CDN) pour être redistribué. Media Services fournit également une intégration transparente au CDN Azure. Le flux sortant d’un service StreamingEndpoint peut être un flux dynamique, une vidéo à la demande ou un téléchargement progressif de votre ressource dans votre compte Media Services. Chaque compte Azure Media Services comprend une valeur de point de terminaison de streaming par défaut. Vous pouvez créer d’autres points de terminaison de streaming sous votre compte. Il existe deux versions du point de terminaison de streaming : 1.0 et 2.0. À compter du 10 janvier 2017, les nouveaux comptes AMS incluront la version 2.0 du point de terminaison de streaming **par défaut**. Les autres points de terminaison que vous ajoutez à ce compte seront également de la version 2.0. Cette modification n’aura aucune incidence sur les comptes existants ; les StreamingEndpoint existants auront la version 1.0 et pourront être mis à niveau vers la version 2.0. Avec cette modification, il y aura des changements de comportement, de facturation et de fonctionnalités (pour plus d’informations, consultez la section **Types et versions de streaming** ci-dessous).

Azure Media Services a ajouté les propriétés suivantes à l’entité de point de terminaison de streaming : **CdnProvider**, **CdnProfile**, **FreeTrialEndTime**, **StreamingEndpointVersion**. Pour une présentation détaillée de ces propriétés, consultez [ceci](https://docs.microsoft.com/rest/api/media/operations/streamingendpoint). 

Lorsque vous créez un compte Azure Media Services, un point de terminaison de streaming par défaut est créé pour vous dans l’état **Arrêté**. Il n’est pas possible de supprimer le point de terminaison de streaming par défaut. En fonction de la disponibilité d’Azure CDN dans la région ciblée, le point de terminaison de streaming par défaut qui vient d’être créé comprendra également l’intégration du fournisseur CDN « StandardVerizon ». 
                
> [!NOTE]
> L’intégration d’Azure CDN peut être désactivée avant de démarrer le point de terminaison de streaming. Le `hostname` et l’URL de diffusion en continu restent identiques que vous activiez le CDN ou non.

Cette rubrique donne une vue d’ensemble des fonctionnalités principales offertes par les points de terminaison de streaming.

## <a name="naming-conventions"></a>Conventions d’affectation de noms

Pour le point de terminaison par défaut : `{AccountName}.streaming.mediaservices.windows.net`

Pour tous les autres points de terminaison : `{EndpointName}-{AccountName}.streaming.mediaservices.windows.net`

## <a name="streaming-types-and-versions"></a>Types et versions de streaming

### <a name="standardpremium-types-version-20"></a>Types Standard/Premium (version 2.0)

À compter de la version publiée en janvier 2017 de Media Services, il existe deux types de streaming : **Standard** (version préliminaire) et **Premium**. Ces types font partie de la version de la version « 2.0 » du point de terminaison de streaming.


|Type|Description|
|--------|--------|  
|**Standard**|La valeur par défaut est le point de terminaison de diffusion en continu un **Standard** tapez, peut être converti en type Premium en ajustant les unités de diffusion en continu.|
|**Premium** |Cette option convient aux scénarios professionnels qui nécessitent plus de mise à l’échelle ou de contrôle. Vous déplacer vers un **Premium** type en ajustant les unités de diffusion en continu.<br/>Dédié points de terminaison de diffusion en continu en direct dans un environnement isolé et ne se concurrencent pas pour les ressources.|

Pour les clients qui souhaitent pour fournir du contenu à un large public internet, nous vous recommandons d’activer le CDN sur le point de terminaison de diffusion en continu.

Pour plus d’informations, consultez la section [Comparaison des types de streaming](#comparing-streaming-types) qui suit.

### <a name="classic-type-version-10"></a>Type classique (version 1.0)

Pour les utilisateurs qui ont créé des comptes AMS avant la version du 10 janvier 2017, vous avez un type **classique** de point de terminaison de streaming. Ce type fait partie de la version de la version « 1.0 » du point de terminaison de streaming.

Si votre point de terminaison de streaming **version « 1.0 »** continu a 1 ou plusieurs unités de streaming (SU) Premium, le point de terminaison sera Premium et fournira toutes les fonctionnalités AMS (tout comme le **Standard/Premium**) sans aucune étape de configuration supplémentaire.

>[!NOTE]
>Les points de terminaison de streaming de type **Classique** continu (version « 1.0 » et 0 SU), fournissent des fonctionnalités limitées et n’incluent pas de contrat de niveau de service. Il est recommandé de migrer vers le type **Standard** pour obtenir une meilleure expérience et utiliser des fonctionnalités telles que l’empaquetage dynamique ou le chiffrement et d’autres fonctionnalités qui accompagnent le type **Standard**. Pour migrer vers le type **Standard**, accédez au [portail Azure](https://portal.azure.com/) et sélectionnez **Opt-in to Standard** (Abonnement Standard). Pour plus d’informations sur la migration, consultez la section [Migration](#migration-between-types).
>
>Faites attention, car cette opération ne peut pas être restaurée et a un impact sur la tarification.
>
 
## <a name="comparing-streaming-types"></a>Comparaison des types de streaming

### <a name="versions"></a>Versions

|Type|StreamingEndpointVersion|ScaleUnits|CDN|Facturation|
|--------------|----------|-----------------|-----------------|-----------------|
|Classique|1.0|0|N/D|Gratuit|
|Terminaison de Streaming standard (version préliminaire)|2.0|0|Oui|Payant|
|Unités de streaming Premium|1.0|>0|Oui|Payant|
|Unités de streaming Premium|2.0|>0|Oui|Payant|

### <a name="features"></a>Caractéristiques

Fonctionnalité|standard|Premium
---|---|---
Gratuit les 15 premiers jours <sup>1</sup>| Oui |Non 
Débit |Jusqu'à 600 Mbits/s et peut fournir un débit bien plus efficace important lors de l’utilisation d’un CDN.|200 Mbits/s par unité de streaming (SU). Peut de fournir un débit bien plus efficace important lors de l’utilisation d’un CDN.
CDN|Azure CDN, CDN tiers ou sans CDN.|Azure CDN, CDN tiers ou sans CDN.
La facturation est calculée sur la base d'un taux| Quotidien|Quotidien
Chiffrement dynamique|Oui|Oui
l’empaquetage dynamique|Oui|Oui
Scale|Mise à l’échelle automatique vers le débit cible.|Unités de diffusion en continu supplémentaires.
Hôte de filtrage/G20/personnalisé IP <sup>2</sup>|Oui|Oui
Téléchargement progressif|Oui|Oui
Utilisation recommandée |Recommandé pour la plupart des scénarios de streaming.|Utilisation professionnelle. 

<sup>1</sup> l’essai gratuit s’applique uniquement aux comptes de services de média nouvellement créé et la valeur par défaut point de terminaison de diffusion en continu.<br/>
<sup>2</sup> uniquement utilisés directement sur le point de terminaison de diffusion en continu lorsque le CDN n’est pas activé sur le point de terminaison.<br/>

Pour plus d’informations de contrat SLA, consultez [tarification et les contrats SLA](https://azure.microsoft.com/pricing/details/media-services/).

## <a name="migration-between-types"></a>Migration entre les types

À partir | À | Action
---|---|---
Classique|standard|Abonnement nécessaire
Classique|Premium| Échelle (unités de diffusion en continu supplémentaires)
Standard/Premium|Classique|Non disponible (si la version du point de terminaison de streaming est 1.0. Il est possible de passer en mode classique en définissant ScaleUnits sur « 0 »)
Standard (avec/sans CDN)|Premium avec les mêmes configurations|Autorisé dans l’état **démarré**. (via le portail Azure)
Premium (avec/sans CDN)|Standard avec les mêmes configurations|Autorisé dans l’état **démarré** (via le portail Azure)
Standard (avec/sans CDN)|Premium avec une configuration différente|Autorisé dans l’état **arrêté** (via le portail Azure). Non autorisé en état En cours d’exécution.
Premium (avec/sans CDN)|Standard avec une configuration différente|Autorisé dans l’état **arrêté** (via le portail Azure). Non autorisé en état En cours d’exécution.
Version 1.0 avec SU > = 1 avec CDN|Standard/Premium avec aucun CDN|Autorisé dans l’état **arrêté**. Non autorisé en état **démarré**.
Version 1.0 avec SU > = 1 avec CDN|Standard, avec/sans CDN|Autorisé dans l’état **arrêté**. Non autorisé en état **démarré**. Le CDN version 1.0 sera supprimé et le nouveau créé et démarré.
Version 1.0 avec SU > = 1 avec CDN|Premium avec/sans CDN|Autorisé dans l’état **arrêté**. Non autorisé en état **démarré**. Le CDN classique sera supprimé et le nouveau créé et démarré.

## <a name="next-steps"></a>Étapes suivantes
Consultez les parcours d’apprentissage de Media Services.

[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Fournir des commentaires
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

