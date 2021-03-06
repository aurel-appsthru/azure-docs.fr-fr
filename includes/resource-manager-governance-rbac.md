---
title: Fichier Include
description: Fichier Include
services: azure-resource-manager
author: rockboyfor
manager: digimobile
ms.service: azure-resource-manager
ms.topic: include
origin.date: 02/16/2018
ms.date: 04/30/2018
ms.author: v-yeche
ms.custom: include file
ms.openlocfilehash: f77a5d482c3f8632a3d86bd8e027fbb4418168c3
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62122906"
---
Vous devez vous assurer que les utilisateurs de votre organisation disposent du niveau d’accès approprié à ces ressources. Il n’est pas question de leur accorder un accès illimité, mais de faire en sorte qu’ils puissent accomplir leur travail. Le contrôle d’accès en fonction du rôle (RBAC) vous permet de définir les utilisateurs autorisés à effectuer des actions spécifiques dans une étendue. Un rôle définit un ensemble d’actions autorisées. Vous attribuez le rôle à une étendue et spécifiez les utilisateurs qui appartiennent à ce rôle pour l’étendue.

Lorsque vous planifiez votre stratégie de contrôle d’accès, vous pouvez accorder aux utilisateurs les privilèges minimaux pour effectuer leur travail. L’image suivante illustre un modèle suggéré pour l’attribution de RBAC.

![Étendue](./media/resource-manager-governance-rbac/role-examples.png)

Il existe trois rôles qui s’appliquent à toutes les ressources : Propriétaire, Contributeur et Lecteur. Les comptes affectés au rôle Propriétaire doivent être étroitement contrôlés et rarement utilisées. Les utilisateurs qui ont seulement besoin d’observer l’état de solutions doivent être dotés du rôle Lecteur.

La plupart des utilisateurs obtiennent des [rôles propres aux ressources](../articles/role-based-access-control/built-in-roles.md) ou des [rôles personnalisés](../articles/role-based-access-control/custom-roles.md) au niveau du groupe de ressources ou de l’abonnement. Ces rôles définissent précisément les actions autorisées. En affectant des utilisateurs à ces rôles, vous leur accordez l’accès requis sans leur donner trop de contrôle. Vous pouvez affecter un compte à plusieurs rôles ; l’utilisateur obtient alors les autorisations combinées des rôles. L’octroi d’un accès au niveau de la ressource est souvent trop restrictif pour les utilisateurs, mais cela peut fonctionner dans le cadre d’un processus automatisé conçu pour une tâche spécifique.

### <a name="who-can-assign-roles"></a>Qui peut attribuer des rôles ?

Pour créer et supprimer des attributions de rôles, les utilisateurs doivent disposer d’un accès `Microsoft.Authorization/roleAssignments/*`. Cet accès est accordé par le biais du rôle Propriétaire ou Administrateur de l’accès utilisateur.
<!--ms.date: 04/30/2018-->