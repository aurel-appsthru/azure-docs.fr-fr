---
title: Ouverture des ports de pare-feu requis pour une application de proxy d’application | Microsoft Docs
description: Découvrez quels ports ouvrir pour le bon fonctionnement du proxy d’application Azure AD
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/21/2018
ms.author: mimart
ms.reviewer: asteen
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3e69f2e5049ca290a17c058c9d18dc7c6ec91f49
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65783565"
---
# <a name="how-to-open-the-firewall-ports-required-for-an-application-proxy-application"></a>Ouverture des ports de pare-feu requis pour une application de proxy d’application

Pour voir la liste complète des ports requis et la fonction de chaque port, consultez la section dédiée aux conditions préalables de la [documentation du proxy d’application](application-proxy-add-on-premises-application.md). Le proxy d’application utilise uniquement des ports de sortie.

Vous pouvez également vérifier si vous disposez de tous les ports requis sont ouverts en ouvrant le [Connector Ports Test Tool](https://aadap-portcheck.connectorporttest.msappproxy.net/) à partir de votre réseau local. Un nombre plus élevé de coches vertes signifie une résilience accrue. 

## <a name="app-proxy-regions"></a>Régions du proxy d’application

Nous travaillons sur un moyen de vous informer des régions qui doivent vous apparaître en vert. Pour l’instant, assurez-vous que toutes le sont. La région USA Centre est également requise, quelle que soit la région où vous vous trouvez.

Pour vous assurer que l’outil fournit des résultats corrects, veillez à :

-   Ouvrir l’outil sur un navigateur à partir du serveur sur lequel vous avez installé le connecteur.

-   Vous assurer que les proxys ou pare-feu applicables à votre connecteur sont également appliqués à cette page. Cela est possible dans Internet Explorer en accédant à **paramètres**  - &gt; **Options Internet**  - &gt; **connexions**  - &gt; **Paramètres LAN**. Cette page comporte le champ Utiliser un serveur proxy pour votre réseau local. Activez cette case et entrez l’adresse proxy dans le champ Adresse.

## <a name="next-steps"></a>Étapes suivantes
[Présentation des connecteurs de proxy d’application Azure AD](application-proxy-connectors.md)
