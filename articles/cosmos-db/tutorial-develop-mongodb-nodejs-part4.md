---
title: Créer une application Angular avec l’API Azure Cosmos DB pour MongoDB - Créer un compte Cosmos
titleSuffix: Azure Cosmos DB
description: Il s’agit de la partie 4 de cette série de didacticiels sur la création d’une application MongoDB avec Angular et Node sur Azure Cosmos DB à l’aide des mêmes API que celles utilisées pour MongoDB
author: johnpapa
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.devlang: nodejs
ms.topic: tutorial
ms.date: 12/06/2018
ms.author: jopapa
ms.custom: seodec18
ms.reviewer: sngun
ms.openlocfilehash: 8320204f75e583dae0449f83e7c38f6638371c2a
ms.sourcegitcommit: 8330a262abaddaafd4acb04016b68486fba5835b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/04/2019
ms.locfileid: "54035112"
---
# <a name="create-an-angular-app-with-azure-cosmos-dbs-api-for-mongodb---create-a-cosmos-account"></a>Créer une application Angular avec l’API Azure Cosmos DB pour MongoDB - Créer un compte Cosmos

Ce tutoriel en plusieurs parties montre comment créer une application écrite en Node.js avec Express et Angular, puis comment la connecter à votre [compte Cosmos configuré avec l’API de Cosmos DB pour MongoDB](mongodb-introduction.md).

La partie 4 de ce didacticiel est basée sur la [partie 3](tutorial-develop-mongodb-nodejs-part3.md) et aborde les tâches suivantes :

> [!div class="checklist"]
> * Créer un groupe de ressources Azure à l’aide d’Azure CLI
> * Créer un compte Cosmos à l’aide d’Azure CLI

## <a name="video-walkthrough"></a>Vidéo de procédure pas à pas

> [!VIDEO https://www.youtube.com/embed/hfUM-AbOh94]

## <a name="prerequisites"></a>Prérequis

Avant de commencer cette partie du didacticiel, assurez-vous d’avoir effectué les étapes de la [partie 3](tutorial-develop-mongodb-nodejs-part3.md) du didacticiel. 

Dans cette section du tutoriel, vous pouvez utiliser Azure Cloud Shell (dans votre navigateur Internet) ou [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli) installé localement.

[!INCLUDE [cloud-shell-try-it](../../includes/cloud-shell-try-it.md)]

[!INCLUDE [Log in to Azure](../../includes/login-to-azure.md)]

[!INCLUDE [Create resource group](../../includes/app-service-web-create-resource-group.md)]

> [!TIP]
> Ce didacticiel vous guide à travers les étapes pour générer l’application pas à pas. Si vous souhaitez télécharger le projet terminé, vous pouvez obtenir l’application complète à partir du [référentiel cosmosdb-angular](https://github.com/Azure-Samples/angular-cosmosdb) sur GitHub.

## <a name="create-an-azure-cosmos-db-account"></a>Création d’un compte Azure Cosmos DB

Créez un compte Azure Cosmos DB en exécutant la commande [`az cosmosdb create`](/cli/azure/cosmosdb#az-cosmosdb-create).

```azurecli-interactive
az cosmosdb create --name <cosmosdb-name> --resource-group myResourceGroup --kind MongoDB
```

* Dans `<cosmosdb-name>`, utilisez le nom de votre compte Azure Cosmos DB. Il doit être unique parmi tous les noms de compte Azure Cosmos DB présents dans Azure.
* Le paramètre `--kind MongoDB` active les connexions MongoDB pour Azure Cosmos DB.

L’exécution de la commande peut durer quelques minutes. Une fois la commande terminée, la fenêtre du terminal affiche les informations de la nouvelle base de données. 

Après avoir créé le compte Azure Cosmos DB :
1. Ouvrez une nouvelle fenêtre dans votre navigateur et accédez à [https://portal.azure.com](https://portal.azure.com)
1. Cliquez sur le logo Azure Cosmos DB ![dans le portail Azure](./media/tutorial-develop-mongodb-nodejs-part4/azure-cosmos-db-icon.png) dans la barre de gauche pour afficher toutes les bases de données Azure Cosmos DB que vous possédez.
1. Cliquez sur le compte Azure Cosmos DB que vous venez de créer, sélectionnez l’onglet **Vue d’ensemble** et faites défiler vers le bas pour voir où se trouve la base de données. 

    ![Nouveau compte Azure Cosmos DB dans le portail Azure](./media/tutorial-develop-mongodb-nodejs-part4/azure-cosmos-db-angular-portal.png)

4. Faites défiler vers le bas dans la barre de navigation située à gauche, puis cliquez sur l’onglet **Répliquer les données mondialement** pour afficher un mappage indiquant les différentes zones dans lesquelles vous pouvez répliquer des données. Par exemple, vous pouvez cliquer sur Australie Sud-Est ou Australie Est pour répliquer vos données en Australie. Pour en savoir plus sur la réplication mondiale, consultez [Distribution mondiale des données avec Azure Cosmos DB](distribute-data-globally.md). Pour le moment, conservons cette instance et lorsque nous voudrons répliquer, nous saurons comment procéder.

    ![Nouveau compte Azure Cosmos DB dans le portail Azure](./media/tutorial-develop-mongodb-nodejs-part4/azure-cosmos-db-replicate-portal.png)

## <a name="next-steps"></a>Étapes suivantes

Dans cette partie du didacticiel, vous avez :

> [!div class="checklist"]
> * Créé un groupe de ressources Azure à l’aide d’Azure CLI
> * Créé un compte Azure Cosmos DB à l’aide d’Azure CLI

Vous pouvez maintenant passer à la partie suivante du didacticiel afin de connecter Azure Cosmos DB et votre application à l’aide de Mongoose.

> [!div class="nextstepaction"]
> [Utiliser Mongoose pour se connecter à Azure Cosmos DB](tutorial-develop-mongodb-nodejs-part5.md)
