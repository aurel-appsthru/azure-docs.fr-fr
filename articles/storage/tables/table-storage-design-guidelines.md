---
title: Conseils de conception de table de stockage Azure | Microsoft Docs
description: Concevez votre service de Table Azure pour prendre en charge efficacement les opérations de lecture.
services: storage
author: WenJason
ms.service: storage
ms.topic: article
origin.date: 04/23/2018
ms.date: 12/10/2018
ms.author: v-jay
ms.component: tables
ms.openlocfilehash: d056d29469ad9a60fceeee307aca3c0e1319283c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61269831"
---
# <a name="guidelines-for-table-design"></a>Conseils pour la conception de table

La conception de tables à utiliser avec le service de table de stockage Azure est très différente de la conception d’une base de données relationnelle. Cet article présente des conseils de conception pour votre solution de service de Table pour une lecture et une écriture efficaces.

## <a name="design-your-table-service-solution-to-be-read-efficient"></a>Concevoir votre solution de service de Table pour une lecture efficace

* ***Pensez votre conception pour l’interrogation dans des applications à lecture intensive.***  Lorsque vous concevez vos tables, pensez aux requêtes que vous allez exécuter (en particulier celles sensibles à la latence) avant de réfléchir à la méthode de mise à jour de vos entités. Cela permet généralement d’élaborer une solution efficace et performante.  
* ***Spécifiez les valeurs de PartitionKey et de RowKey dans vos requêtes.*** *requêtes de pointage* telles que celles-ci sont les requêtes de service de Table les plus efficaces.  
* ***Envisagez de stocker des copies dupliquées des entités.*** Le stockage de table est bon marché. Vous pourriez donc stocker la même entité plusieurs fois (avec différentes clés) pour rendre les requêtes plus efficaces.  
* ***Envisagez de dénormaliser vos données.*** Le stockage de tables est bon marché. Nous vous recommandons donc d’envisager la dénormalisation de vos données. Par exemple, stockez des entités de résumé pour que les requêtes d’agrégation de données aient seulement besoin d’accéder à une entité unique.  
* ***Utilisez des valeurs de clé composées.*** Les seules clés dont vous disposez sont **PartitionKey** et **RowKey**. Par exemple, utilisez des valeurs de clé composées pour activer les chemins d’accès de clé de substitution pour les entités.  
* ***Utilisez la projection de requête.***  Vous pouvez réduire la quantité de données que vous transférez sur le réseau en utilisant des requêtes qui sélectionnent uniquement les champs dont vous avez besoin.  

## <a name="design-your-table-service-solution-to-be-write-efficient"></a>Concevoir votre solution de service de Table pour une écriture efficace  

* ***Ne créez pas de partitions à chaud.***  Choisissez des clés qui vous permettent de répartir vos requêtes sur plusieurs partitions à tout moment.  
* ***Évitez les pics de trafic.***  Fluidifiez le trafic sur une période de temps raisonnable et évitez les pics de trafic.
* ***Ne créez pas nécessairement une table distincte pour chaque type d’entité.***  Lorsque vous avez besoin d’utiliser des transactions atomiques sur des types d’entité, vous pouvez stocker ces types d’entité multiples dans la même partition de la même table.
* ***Prévoyez le débit maximal nécessaire.***  Vous devez connaître les objectifs d'extensibilité du service de Table et vous assurer que votre conception n'entraînera pas de dépassement.  

En lisant ce guide, vous rencontrerez des exemples mettant ces principes en pratique. 

## <a name="next-steps"></a>Étapes suivantes

- [Modèles de conception de table](table-storage-design-patterns.md)
- [Conception pour l’interrogation](table-storage-design-for-query.md)
- [Chiffrer des données de table](table-storage-design-encrypt-data.md)
- [Conception pour la modification de données](table-storage-design-for-modification.md)