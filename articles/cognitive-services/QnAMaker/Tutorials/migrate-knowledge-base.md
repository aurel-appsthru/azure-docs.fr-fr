---
title: Migrer des bases de connaissances - QnA Maker
titleSuffix: Azure Cognitive Services
description: La migration d’une base de connaissances nécessite l’exportation d’une base de connaissances, puis l’importation dans une autre.
services: cognitive-services
author: tulasim88
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 04/08/2019
ms.author: tulasim
ms.custom: seodec18
ms.openlocfilehash: 8ff3c497372a761bd8a02ae81bc897c8ee297bd0
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65794882"
---
# <a name="migrate-a-knowledge-base-using-export-import"></a>Migrer une base de connaissances à l’aide des fonctions d’exportation-importation

La migration d’une base de connaissances nécessite l’exportation d’une base de connaissances, puis l’importation dans une autre. 

## <a name="prerequisites"></a>Conditions préalables

* Avant de commencer, créez un [compte gratuit](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* Configurer un nouveau [service QnA Maker](../How-To/set-up-qnamaker-service-azure.md)

## <a name="migrate-a-knowledge-base-from-qna-maker"></a>Migrer une base de connaissances à partir de QnA Maker
1. Connectez-vous au [portail QnA Maker](https://qnamaker.ai).
1. Sélectionnez la base de connaissances que vous souhaitez migrer.

1. Dans la page **Settings** (Paramètres), sélectionnez **Export knowledge base** (Exporter la base de connaissances) pour télécharger un fichier .tsv qui contient le contenu de votre base de connaissances : questions, réponses, métadonnées et nom des sources de données à partir desquelles elles ont été extraites.

1. Sélectionnez **Créer une base de connaissances** dans le menu supérieur, puis créez une base de connaissances vide. 

    ![Définir les sources de données](../media/qnamaker-how-to-create-kb/set-data-sources.png)

    - Donnez un **nom** à votre service. Les noms dupliqués et les caractères spéciaux sont acceptés.

1. Sélectionnez **Créer**.

    ![Créer la base de connaissances](../media/qnamaker-how-to-create-kb/create-kb.png)

1. Dans cette nouvelle base de connaissances, ouvrez l’onglet **Paramètres**, puis sélectionnez **Importer une base de connaissances**. Cette action importe les questions, les réponses et les métadonnées, tout en conservant le nom des sources de données à partir desquelles elles ont été extraites.

   ![Importer une base de connaissances](../media/qnamaker-how-to-migrate-kb/Import.png)

1. **Testez** la nouvelle base de connaissances à l’aide du panneau de test. Découvrez comment [tester votre base de connaissances](../How-To/test-knowledge-base.md).
1. **Publiez** la base de connaissances. Découvrez comment [publier votre base de connaissances](../Quickstarts/create-publish-knowledge-base.md#publish-the-knowledge-base).
1. Utilisez le point de terminaison dans votre code d’application ou de bot. Découvrez comment [créer un bot QnA](../Tutorials/create-qna-bot.md) ici.

    ![Valeurs QnA Maker](../media/qnamaker-how-to-migrate-kb/qnamaker-settings-kbid-key.png)

    À ce stade, tout le contenu de la base de connaissances (questions, réponses et métadonnées, ainsi que le nom des fichiers sources et les URL) est importé dans la nouvelle base de connaissances. 

## <a name="chat-logs-and-alterations"></a>Conversations et modifications
Les modifications qui ne respectent pas la casse (synonymes) ne sont pas importées automatiquement. Utilisez le [V4 API](https://go.microsoft.com/fwlink/?linkid=2092179) pour déplacer les modifications dans la base de connaissances.

Il n’existe aucun moyen de migrer les conversations, étant donné que la nouvelle base de connaissances utilise Application Insights pour le stockage des conversations. 

## <a name="next-steps"></a>Étapes suivantes

> [!div class="nextstepaction"]
> [Modifier une base de connaissances](../How-To/edit-knowledge-base.md)
