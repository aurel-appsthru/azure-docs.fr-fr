---
title: Limites de la taille des demandes adressées au pare-feu d’applications web et listes d’exclusions dans Azure Application Gateway - Portail Azure
description: Cet article fournit des informations sur la configuration des limites de la taille des demandes adressées au pare-feu d’applications web et des listes d’exclusions dans Application Gateway avec le portail Azure.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.date: 5/15/2019
ms.author: victorh
ms.topic: conceptual
ms.openlocfilehash: 5ddcdeca41e2f21fa27db25f7e0721c7ef87e491
ms.sourcegitcommit: 3675daec6c6efa3f2d2bf65279e36ca06ecefb41
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/14/2019
ms.locfileid: "65620277"
---
# <a name="web-application-firewall-request-size-limits-and-exclusion-lists"></a>Limites de la taille des demandes adressées au pare-feu d’application web et listes d’exclusions

Le pare-feu d’applications web (WAF) Azure Application Gateway fournit une protection pour les applications web. Cet article décrit la configuration des limites de la taille des demandes adressées au WAF et des listes d’exclusions.

## <a name="waf-request-size-limits"></a>Limites de la taille des demandes adressées au WAF

![Limites de la taille des demandes](media/application-gateway-waf-configuration/waf-requestsizelimit.png)

Le pare-feu d’applications web vous permet de configurer des limites maximale et minimale de la taille des demandes. Les configurations de limites à deux tailles suivantes sont disponibles :

- Le champ de taille de corps de requête maximale est spécifié en kilo-octets et contrôles charge globale limite de taille de demande à l’exclusion de n’importe quel fichier. La valeur de ce champ peut varier de 1 Ko à 128 Ko. La valeur par défaut pour la taille du corps de la demande est de 128 Ko.
- Le champ de la limite de chargement de fichier est spécifié en Mo et régit la taille maximale autorisée pour la limite de chargement de fichier. La valeur de ce champ peut être comprise entre 1 Mo et 500 Mo pour les grandes instances SKU et 100 Mo pour les instances SKU intermédiaires. La valeur par défaut pour la limite de chargement de fichier est de 100 Mo.

Le WAF offre également un bouton configurable qui permet d’activer ou de désactiver l’inspection du corps de la demande. Par défaut, l’inspection du corps de la demande est activée. Si l’inspection du corps de la demande est désactivée, WAF n’évalue pas le contenu du corps du message HTTP. Dans ce cas, le WAF continue d’appliquer ses règles sur les en-têtes, les cookies et les URI. Si l’inspection du corps de la demande est désactivée, le champ de la taille maximale du corps de la demande n’est pas applicable et ne peut pas être défini. La désactivation de l’inspection du corps de la demande permet l’envoi au WAF de messages dont la taille est supérieure à 128 Ko, mais le corps du message n’est pas inspecté.

## <a name="waf-exclusion-lists"></a>Listes d’exclusions du WAF

![waf-exclusion.png](media/application-gateway-waf-configuration/waf-exclusion.png)

Les listes d’exclusion du WAF vous permettent d’omettre certains attributs de la demande dans une évaluation par le WAF. À titre d’exemple courant, citons les jetons Active Directory insérés qui sont utilisés pour les champs d’authentification ou de mot de passe. Ces attributs sont sujets à contenir des caractères spéciaux qui peuvent déclencher un faux positif dans les règles du pare-feu d’applications web. Une fois ajouté à la liste d’exclusions du WAF, un attribut n’est pris en considération par aucune règle du pare-feu d’applications web configurée et active. Les listes d’exclusions ont une portée globale.

Les attributs suivants peuvent être ajoutés aux listes d’exclusion :

* En-têtes de demande
* Cookies de requête
* Demander le nom d’attribut (args)

   * Données de plusieurs parties de formulaire
   * XML
   * JSON
   * Arguments de requête d’URL

Vous pouvez spécifier une correspondance exacte avec l'en-tête ou le corps d'une requête, un cookie ou un attribut de chaîne de requête  ou spécifier des correspondances partielles. L'exclusion porte toujours sur un champ d'en-tête, jamais sur sa valeur. Les règles d'exclusion ont une portée globale, et s'appliquent à toutes les pages et à toutes les règles.

Voici les opérateurs de critères de correspondance pris en charge :

- **Est égal à** :  cet opérateur est utilisé pour une correspondance exacte. Par exemple, pour sélectionner l’en-tête **bearerToken**, utilisez l’opérateur d’égalité avec le sélecteur défini sur **bearerToken**.
- **Commence par** : cet opérateur correspond à tous les champs qui commencent par la valeur de sélecteur spécifiée.
- **Se termine par** :  cet opérateur correspond à tous les champs de demande qui se terminent par la valeur de sélecteur spécifiée.
- **Contient** : cet opérateur correspond à tous les champs de demande qui se contiennent la valeur de sélecteur spécifiée.
- **Est égal à un**: Cet opérateur correspond à tous les champs de demande. * sera la valeur du sélecteur.

Dans tous les cas, la correspondance respecte la casse, et les expressions régulières ne sont pas autorisées en guise de sélecteurs.

### <a name="examples"></a>Exemples

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Les exemples suivants illustrent l’utilisation des exclusions.

### <a name="example-1"></a>Exemple 1

Dans cet exemple, que vous souhaitez exclure de l’en-tête user-agent. L’en-tête de demande de l’agent utilisateur contient une chaîne de caractéristique qui permet aux pairs de protocole réseau identifier le type d’application, système d’exploitation, éditeur de logiciels ou version du logiciel de l’agent utilisateur de logiciels demandeur. Pour plus d’informations, consultez [Agent utilisateur](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/User-Agent).

Il peut y avoir un nombre quelconque de raisons de désactiver l’évaluation de cet en-tête. Il peut être une chaîne qui le WAF voit et supposer qu’il est malveillant. Par exemple, l’attaque SQL classique « x = x » dans une chaîne. Dans certains cas, il peut s’agir du trafic légitime. Par conséquent, vous devrez peut-être exclure cet en-tête de version d’évaluation de WAF.

L’applet de commande Azure PowerShell suivant permet d’exclure l’en-tête user-agent d’évaluation :

```azurepowershell
$exclusion1 = New-AzApplicationGatewayFirewallExclusionConfig `
   -MatchVariable "RequestHeaderNames" `
   -SelectorMatchOperator "Equals" `
   -Selector "User-Agent"
```

### <a name="example-2"></a>Exemple 2

Cet exemple exclut la valeur dans le *utilisateur* paramètre qui est transmis dans la demande via l’URL. Par exemple, qu'il est courant dans votre environnement pour le champ utilisateur contienne une chaîne le WAF vues en tant que contenu malveillant, elle le bloque.  Vous pouvez exclure le paramètre utilisateur dans ce cas, afin que le pare-feu WAF n’évalue pas quoi que ce soit dans le champ.

L’applet de commande Azure PowerShell suivant exclut le paramètre de l’utilisateur à partir de la version d’évaluation :

```azurepowershell
$exclusion2 = New-AzApplicationGatewayFirewallExclusionConfig `
   -MatchVariable "RequestArgNames" `
   -SelectorMatchOperator "Equals" `
   -Selector "user"
```
Par conséquent, si l’URL **http://www.contoso.com/?user=fdafdasfda** est passée pour le pare-feu d’applications Web, il n’évalue pas la chaîne **fdafdasfda**.

## <a name="next-steps"></a>Étapes suivantes

Après avoir configuré vos paramètres du pare-feu d’applications web, vous pouvez apprendre à afficher vos journaux d’activité WAF. Pour plus d’informations, consultez [Diagnostics Application Gateway](application-gateway-diagnostics.md#diagnostic-logging).
