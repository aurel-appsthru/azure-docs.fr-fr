---
title: Comparaison des préréglages de Video Indexer et d’Azure Media Services v3 | Microsoft Docs
description: Cette rubrique compare les préréglages de Video Indexer et d’Azure Media Services v3.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.service: media-services
ms.subservice: video-indexer
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/15/2019
ms.author: juliako
ms.openlocfilehash: 275178998948e357a6a72fbe5d0b3c9c01485a3a
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65800174"
---
# <a name="compare-azure-media-services-v3-presets-and-video-indexer"></a>Comparer les préréglages d’Azure Media Services v3 et de Video Indexer 

Cet article compare les fonctionnalités des **API Video Indexer** et des **API Media Services v3**. 

Actuellement, il existe un chevauchement entre les fonctionnalités offertes par le [API d’indexeur vidéo](https://api-portal.videoindexer.ai/) et [Media Services v3 API](https://github.com/Azure/azure-rest-api-specs/blob/master/specification/mediaservices/resource-manager/Microsoft.Media/stable/2018-07-01/Encoding.json). Le tableau suivant offre des indications pour comprendre les différences et les similitudes. 

## <a name="compare"></a>Comparer

|Fonctionnalité|API Video Indexer |Préréglages de l’analyseur vidéo et de l’analyseur audio<br/>dans les API Media Services v3|
|---|---|---|
|Insights médias|[Amélioré](video-indexer-output-json-v2.md) |[Fondamentaux](../latest/intelligence-concept.md)|
|Expériences|Consultez la liste complète des fonctionnalités prises en charge : <br/> [Vue d’ensemble](video-indexer-overview.md)|Retourne uniquement des insights vidéo|
|Facturation|[Tarifs de Media Services](https://azure.microsoft.com/pricing/details/media-services/#analytics)|[Tarifs de Media Services](https://azure.microsoft.com/pricing/details/media-services/#analytics)|
|Conformité|[ISO 27001](https://www.microsoft.com/TrustCenter/Compliance/ISO-IEC-27001), [ISO 27018](https://www.microsoft.com/trustcenter/Compliance/ISO-IEC-27018), [SOC 1,2,3](https://www.microsoft.com/TrustCenter/Compliance/SOC), [HIPAA](https://www.microsoft.com/trustcenter/compliance/hipaa), [FedRAMP](https://www.microsoft.com/TrustCenter/Compliance/fedramp), [PCI](https://www.microsoft.com/trustcenter/compliance/pci)et [ HITRUST](https://www.microsoft.com/TrustCenter/Compliance/hitrust) certifié. Pour les mises à jour plus récentes, visitez [état actuel de certifications de Video Indexer](https://gallery.technet.microsoft.com/Overview-of-Azure-c1be3942).|Media Services est compatible avec de nombreuses certifications. Consultez le fichier [Azure Compliance Offerings.pdf](https://gallery.technet.microsoft.com/Overview-of-Azure-c1be3942/file/178110/23/Microsoft%20Azure%20Compliance%20Offerings.pdf) et recherchez « Media Services » pour savoir s’il est conforme à un certificat d’intérêt.|
|Essai gratuit|USA Est|Indisponible|
|Disponibilité dans la région|Est des États-Unis 2, Sud-Centre des États-Unis, ouest des États-Unis 2, Europe du Nord, Europe de l’ouest, Asie du Sud-est, Asie de l’est et est de l’Australie.  Pour les mises à jour plus récentes, visitez le [produits par région](https://azure.microsoft.com/global-infrastructure/services/?products=cognitive-services) page.|Consultez [État Azure](https://azure.microsoft.com/global-infrastructure/services/?products=media-services).|

## <a name="next-steps"></a>Étapes suivantes

[Présentation de Video Indexer](video-indexer-overview.md)

[Vue d’ensemble de Media Services v3](../latest/media-services-overview.md)
