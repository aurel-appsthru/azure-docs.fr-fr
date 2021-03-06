---
title: Architecture et les principaux concepts
titleSuffix: Azure Machine Learning service
description: En savoir plus sur l’architecture, les termes du contrat, les concepts et les flux de travail qui composent le service Azure Machine Learning.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: larryfr
author: Blackmist
ms.date: 04/15/2019
ms.custom: seodec18
ms.openlocfilehash: cb716e0d9f97d3ea2e9584a9fc3d7a6f57da9179
ms.sourcegitcommit: 1d257ad14ab837dd13145a6908bc0ed7af7f50a2
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/09/2019
ms.locfileid: "65502105"
---
# <a name="how-azure-machine-learning-service-works-architecture-and-concepts"></a>Voici comment Azure Machine Learning service fonctionne : Architecture et concepts

En savoir plus sur l’architecture, les concepts et les flux de travail pour le service Azure Machine Learning. Le diagramme suivant représente les principaux composants du service et illustre le workflow général lors de l’utilisation du service :

[![Architecture et workflow du service Azure Machine Learning](./media/concept-azure-machine-learning-architecture/workflow.png)](./media/concept-azure-machine-learning-architecture/workflow.png#lightbox)

## <a name="workflow"></a>Workflow

Le flux de travail machine learning suit généralement cette séquence :

1. Développez les scripts d’entraînement de machine learning avec **Python**.
1. Créez et configurez une **cible de calcul**.
1. **Envoyez les scripts** à la cible de calcul configurée en vue de leur exécution dans cet environnement. Pendant l’entraînement, les scripts peuvent lire ou écrire dans un **magasin de données**. De plus, les enregistrements de l’exécution sont enregistrés en tant que **séries** dans l’**espace de travail** et regroupés dans des **expériences**.
1. **Interrogez cette expérience** pour obtenir les métriques journalisées relatives aux exécutions actuelles et passées. Si les métriques ne montrent pas le résultat souhaité, retournez à l’étape 1, puis itérez vos scripts.
1. Une fois qu’une exécution satisfaisante est trouvée, inscrivez le modèle persistant dans le **registre de modèles**.
1. Développer un script de notation qui utilise le modèle et **déployer le modèle** comme un **service web** dans Azure, ou à un **appareil IoT Edge**.


> [!NOTE]
> Cet article définit les termes et les concepts utilisés par le service Azure Machine Learning, et non pas ceux relatifs à la plateforme Azure. Pour plus d’informations sur la terminologie relative à la plateforme Azure, consultez le [glossaire Microsoft Azure](https://docs.microsoft.com/azure/azure-glossary-cloud-terminology).

## <a name="workspace"></a>Espace de travail

L’espace de travail est la ressource de niveau supérieur du service Azure Machine Learning. Il fournit un emplacement centralisé dans lequel utiliser tous les artefacts que vous créez lorsque vous utilisez le service Azure Machine Learning.

L’espace de travail conserve une liste de cibles de calcul que vous pouvez utiliser pour entraîner votre modèle. Il conserve également un historique des exécutions d’entraînement, y compris les journaux d’activité, métriques, sorties et instantanés de vos scripts. Vous utilisez ces informations pour déterminer quelle exécution d’entraînement produit le meilleur modèle.

Vous inscrivez les modèles dans l’espace de travail. Vous utilisez un modèle inscrit et scripts de calcul de score pour déployer un modèle sur Azure Container Instances, Azure Kubernetes Service, ou à un tableau de portes à champ programmable (FPGA) comme point de terminaison HTTP basé sur REST. Vous pouvez également déployer l’image en tant que module sur un appareil Azure IoT Edge. En interne, une image docker est créée pour héberger l’image déployée. Si nécessaire, vous pouvez spécifier votre propre image.

Vous pouvez créer plusieurs espaces de travail, et chacun d’eux peut être partagé par plusieurs personnes. Lorsque vous partagez un espace de travail, vous pouvez contrôler l’accès à ce dernier en assignant des utilisateurs aux rôles suivants :

* Propriétaire
* Contributeur
* Lecteur

Pour plus d’informations sur ces rôles, consultez le [gérer l’accès à un espace de travail Azure Machine Learning](how-to-assign-roles.md) article.

Lorsque vous créez un nouvel espace de travail, celui-ci crée automatiquement plusieurs ressources Azure qui sont utilisées par l’espace de travail :

* [Azure Container Registry](https://azure.microsoft.com/services/container-registry/) : Enregistre les conteneurs docker que vous utilisez pendant la formation et lorsque vous déployez un modèle.
* [Compte de stockage Azure](https://azure.microsoft.com/services/storage/) : Utilisé comme magasin de données par défaut pour l’espace de travail.
* [Azure Application Insights](https://azure.microsoft.com/services/application-insights/) : Stocke les informations de supervision concernant les modèles.
* [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) : Stocke les secrets qui sont utilisés par les cibles de calcul, ainsi que d’autres informations sensibles dont a besoin l’espace de travail.

> [!NOTE]
> En plus de créer de nouvelles versions, vous pouvez utiliser les services Azure existants.

Le diagramme suivant représente une taxonomie de l’espace de travail :

[![Taxonomie de l’espace de travail](./media/concept-azure-machine-learning-architecture/azure-machine-learning-taxonomy.png)](./media/concept-azure-machine-learning-architecture/azure-machine-learning-taxonomy.png#lightbox)

## <a name="experiment"></a>Expérience

Une expérience est un regroupement d’exécutions provenant d’un script spécifié. Elle appartient toujours à un espace de travail. Lorsque vous envoyez une exécution, vous fournissez un nom pour l’expérience. Les informations concernant l’exécution sont stockées sous cette expérience. Si vous envoyez une exécution et spécifiez un nom d’expérience qui n’existe pas, une nouvelle expérience portant ce nom nouvellement spécifié est automatiquement créée.

Pour obtenir un exemple de l’utilisation d’une expérience, consultez le [Guide de démarrage rapide : Prise en main d’Azure Machine Learning service](quickstart-run-cloud-notebook.md).

## <a name="model"></a>Modèle

Le modèle le plus simple est un morceau de code qui accepte une entrée et produit une sortie. La création d’un modèle Machine Learning implique la sélection d’un algorithme auquel vous devez fournir des données, ainsi que l’optimisation des hyperparamètres. L’entraînement est un processus itératif qui génère un modèle entraîné, lequel encapsule ce qu’il a appris au cours du processus d’entraînement.

Un modèle est généré par une exécution effectuée dans Azure Machine Learning. Vous pouvez également utiliser un modèle qui est entraîné en dehors d’Azure Machine Learning. Vous pouvez inscrire un modèle dans un espace de travail Azure Machine Learning service.

Le service Azure Machine Learning est indépendant de l’architecture. Lorsque vous créez un modèle, vous pouvez utiliser toute infrastructure d’apprentissage machine populaires, tels que Scikit-learn, XGBoost, PyTorch, TensorFlow et programme de chaînage.

Pour obtenir un exemple d’apprentissage d’un modèle, consultez [didacticiel : Effectuer l’apprentissage d’un modèle de classification d’images avec Azure Machine Learning Service](tutorial-train-models-with-aml.md).

### <a name="model-registry"></a>Registre de modèles

Le registre de modèles garde une trace de tous les modèles de l’espace de travail correspondant au service Azure Machine Learning.

Les modèles sont identifiés par leur nom et par leur version. Chaque fois que vous inscrivez un modèle portant le même nom qu’un modèle existant, le registre suppose qu’il s’agit d’une nouvelle version. La version est incrémentée et le nouveau modèle est inscrit sous le même nom.

Lorsque vous inscrivez le modèle, vous pouvez fournir des étiquettes de métadonnées supplémentaires, puis utiliser les étiquettes lorsque vous recherchez des modèles.

Vous ne pouvez pas supprimer les modèles qui sont utilisés par un déploiement actif.

Pour obtenir un exemple d’inscription de modèle, consultez [Entraîner un modèle de classification d’images avec Azure Machine Learning](tutorial-train-models-with-aml.md).

## <a name="run-configuration"></a>Configuration de série de tests

Une configuration d’exécution est un ensemble d’instructions qui définit la façon dont un script doit être exécuté dans une cible de calcul spécifiée. La spécification comprend un vaste ensemble de définitions de comportement, par exemple, s’il faut utiliser un environnement Python existant ou utiliser un environnement Conda créé à partir des spécifications.

Une configuration d’exécution peut être rendue persistante dans un fichier du répertoire qui contient votre script d’entraînement, ou peut être construite comme un objet en mémoire et utilisée pour envoyer une exécution.

Pour obtenir des exemples de configurations d’exécutions, consultez [Sélectionner et utiliser une cible de calcul pour entraîner votre modèle](how-to-set-up-training-targets.md).

## <a name="dataset"></a>Jeu de données

Azure Machine Learning jeux de données (version préliminaire) facilitent l’accéder et travailler avec vos données. Jeux de données gérer les données dans divers scénarios tels que la formation de modèle et la création du pipeline. À l’aide du Kit de développement logiciel Azure Machine Learning, vous pouvez accéder au stockage sous-jacent, Explorer et préparer des données, gérer le cycle de vie de différentes définitions de jeu de données et comparer entre les jeux de données utilisés dans la formation et de production.

Jeux de données fournit des méthodes pour manipuler des données dans des formats courants, comme l’utilisation de `from_delimited_files()` ou `to_pandas_dataframe()`.

Pour plus d’informations, consultez [créer et inscrire les jeux de données Azure Machine Learning](how-to-create-register-datasets.md).

Pour obtenir un exemple d’utilisation de jeux de données, consultez le [exemples de blocs-notes](https://aka.ms/dataset-tutorial).

## <a name="datastore"></a>Magasin de données

Un magasin de données est une abstraction de stockage d’un compte de stockage Azure. Le magasin de données peut utiliser un conteneur d’objets blob Azure ou un partage de fichiers Azure en tant que stockage backend. Chaque espace de travail comprend un magasin de données par défaut, et peut inscrire des magasins de données supplémentaires.

Utilisez l’API du SDK Python ou l’interface CLI Azure Machine Learning pour stocker et récupérer des fichiers à partir du magasin de données.

## <a name="compute-target"></a>Cible de calcul

Une cible de calcul est une ressource de calcul que vous utilisez pour exécuter votre script de formation ou pour héberger votre déploiement de service. Les cibles de calcul prises en charge sont les suivantes :

| Cible de calcul | Formation | Déploiement |
| ---- |:----:|:----:|
| Votre ordinateur local | ✓ | &nbsp; |
| Capacité de calcul Azure Machine Learning | ✓ | &nbsp; |
| Une machine virtuelle Linux dans Azure</br>(par exemple Data Science Virtual Machine) | ✓ | &nbsp; |
| Azure Databricks | ✓ | &nbsp; |
| Service Analytique Azure Data Lake | ✓ | &nbsp; |
| Apache Spark pour HDInsight | ✓ | &nbsp; |
| Azure Container Instances | &nbsp; | ✓ |
| Azure Kubernetes Service | &nbsp; | ✓ |
| Azure IoT Edge | &nbsp; | ✓ |
| FPGA (Field-Programmable Gate Array) | &nbsp; | ✓ |

Les cibles de calcul sont attachées à un espace de travail. Les cibles de calcul autres que l’ordinateur local sont partagées par les utilisateurs de l’espace de travail.

### <a name="managed-and-unmanaged-compute-targets"></a>Cibles de calcul managées et non managées

* **Managées** : Cibles de calcul qui sont créées et managées par le service Azure Machine Learning. Ces cibles de calcul sont optimisées pour les charges de travail Machine Learning. À l’heure actuelle, la capacité de calcul Azure Machine Learning est la seule cible de calcul managée à partir du 4 décembre 2018. Il se peut que d’autres cibles de calcul managées soient ajoutées à l’avenir.

    Vous pouvez créer des instances de capacité de calcul ML directement via l’espace de travail à l’aide du Portail Azure, du SDK Azure Machine Learning ou d’Azure CLI. Toutes les autres cibles de calcul doivent être créées hors de l’espace de travail avant d’y être jointes.

* **Non managées** : Les cibles de calcul ne sont *pas* managées par Azure Machine Learning service. Il se peut que vous deviez les créer hors d’Azure Machine Learning, puis les joindre à votre espace de travail avant utilisation. Il se peut que des étapes supplémentaires soient requises pour ces cibles de calcul non managées afin de maintenir ou d’améliorer les performances des charges de travail Machine Learning.

Pour plus d’informations sur la sélection d’une cible de calcul pour l’entraînement, consultez [Sélectionner et utiliser une cible de calcul pour entraîner votre modèle](how-to-set-up-training-targets.md).

Pour plus d’informations sur la sélection d’une cible de calcul pour le déploiement, consultez [Déployer des modèles avec le service Azure Machine Learning](how-to-deploy-and-where.md).

## <a name="training-script"></a>Script d’entraînement

Pour entraîner un modèle, vous devez spécifier le répertoire qui contient le script d’entraînement et les fichiers associés. Vous pouvez également spécifier un nom pour l’expérience, qui est utilisée pour stocker les informations qui sont collectées au cours de l’entraînement. Pendant l’entraînement, l’intégralité du répertoire est copiée dans l’environnement d’entraînement (la cible de calcul) et le script qui est spécifié par la configuration d’exécution est démarré. Un instantané du répertoire est également stocké sous l’expérience, dans l’espace de travail.

Pour obtenir un exemple, consultez [Didacticiel : Effectuer l’apprentissage d’un modèle de classification d’images avec Azure Machine Learning Service](tutorial-train-models-with-aml.md).

## <a name="run"></a>Exécuter

Une exécution est un enregistrement qui contient les informations suivantes :

* Les métadonnées relatives à l’exécution (horodatage, durée, et ainsi de suite)
* Les métriques qui sont journalisées par votre script
* Les fichiers de sortie qui sont collectés automatiquement par l’expérience ou chargés explicitement par l’utilisateur
* Un instantané du répertoire qui contient vos scripts, avant l’exécution

Vous déclenchez une exécution lorsque vous envoyez un script pour entraîner un modèle. Une exécution peut avoir zéro, une ou plusieurs exécutions enfants. Par exemple, l’exécution de niveau supérieur peut avoir deux exécutions enfants, et chacune d’elles peut avoir sa propre exécution enfant.

Pour obtenir un exemple de l’affichage des exécutions qui sont produites par l’apprentissage d’un modèle, consultez le [Guide de démarrage rapide : Prise en main d’Azure Machine Learning service](quickstart-run-cloud-notebook.md).

## <a name="snapshot"></a>Instantané

Lorsque vous envoyez une exécution, Azure Machine Learning compresse le répertoire qui contient le script dans un fichier zip, puis l’envoie à la cible de calcul. Le fichier zip est ensuite décompressé, et le script y est exécuté. Azure Machine Learning stocke également le fichier zip en tant qu’instantané dans l’enregistrement d’exécution. Toute personne ayant accès à l’espace de travail peut parcourir un enregistrement d’exécution et télécharger l’instantané.

## <a name="activity"></a>Activité

Une activité représente une opération de longue durée. Les opérations suivantes sont des exemples d’activités :

* Création ou suppression d’une cible de calcul
* Exécution d’un script sur une cible de calcul

Les activités peuvent envoyer des notifications via le SDK ou l’interface utilisateur web, ce qui vous permet de superviser facilement la progression de ces opérations.

## <a name="image"></a>Image

Les images constituent un moyen fiable de déployer un modèle et tous les composants dont vous avez besoin pour son utilisation. Une image contient les éléments suivants :

* Un modèle
* Un script ou une application de scoring. Vous utilisez le script pour passer l’entrée au modèle et retourner la sortie du modèle.
* Les dépendances dont ont besoin le modèle ou l’application/le script de scoring. Par exemple, vous pouvez inclure un fichier d’environnement Conda qui répertorie les dépendances du package Python.

Azure Machine Learning peut créer deux types d’image :

* **Image FPGA** : Utilisée lorsque vous effectuez un déploiement vers un tableau FPGA (field programmable gate array) dans Azure.
* **Image Docker** : Utilisée lorsque vous effectuez un déploiement vers des cibles de calcul autres que les tableaux FPGA. Il peut s’agir, par exemple, d’Azure Container Instances et d’Azure Kubernetes Service.

Le service Azure Machine Learning fournit une image de base, qui est utilisée par défaut. Vous pouvez également fournir vos propres images personnalisées.

Pour obtenir un exemple de création d’image, consultez [Déployer un modèle de classification d’images dans Azure Container Instances](tutorial-deploy-models-with-aml.md).

### <a name="image-registry"></a>Registre d’images

Le registre d’images effectue le suivi des images qui sont créées à partir de vos modèles. Vous pouvez fournir des étiquettes de métadonnées supplémentaires lorsque vous créez l’image. Les étiquettes de métadonnées sont stockées par le registre d’images, et vous pouvez les interroger pour trouver votre image.

## <a name="deployment"></a>Déploiement

Un déploiement est une instanciation de votre modèle dans un service web qui peut être hébergé dans le cloud ou un module IoT pour les déploiements de l’appareil intégré.

### <a name="web-service"></a>Service Web

Un service web déployé peut utiliser Azure Container Instances, Azure Kubernetes Service ou des tableaux FPGA. Vous créez le service à partir de votre modèle, les scripts et les fichiers associés. Ceux-ci sont encapsulées dans une image, qui fournit l’environnement d’exécution pour le service web. L’image a un point de terminaison HTTP à charge équilibrée qui reçoit les requêtes de scoring qui sont envoyées au service web.

Azure vous permet de superviser le déploiement de votre service web en collectant les données de télémétrie Application Insights ou les données de télémétrie des modèles, si vous avez choisi d’activer cette fonctionnalité. Vous seul pouvez accéder aux données de télémétrie qui sont stockées dans votre instance Application Insights et votre instance de compte de stockage.

Si vous avez activé la mise à l’échelle automatique, Azure met automatiquement à l’échelle votre déploiement.

Pour obtenir un exemple de déploiement de modèle en tant que service, consultez [Déployer un modèle de classification d’images dans Azure Container Instances](tutorial-deploy-models-with-aml.md).

### <a name="iot-module"></a>Module IoT

Un module IoT déployé est un conteneur Docker qui inclut votre modèle, ainsi que le script ou l’application associés et toutes les dépendances supplémentaires. Vous déployez ces modules à l’aide d’Azure IoT Edge sur périphériques de périmètre.

Si vous avez activé la supervision, Azure collecte les données de télémétrie à partir du modèle qui se trouve dans le module Azure IoT Edge. Vous seul pouvez accéder aux données de télémétrie qui sont stockées dans votre instance de compte de stockage.

Azure IoT Edge garantit l’exécution de votre module et supervise l’appareil qui l’héberge.

## <a name="pipeline"></a>Pipeline

Vous utilisez des pipelines de machine learning pour créer et gérer des flux de travail qui combinent les phases de machine learning. Par exemple, un pipeline peut inclure les phases de préparation des données, de formation du modèle, de déploiement du modèle et d’inférence. Chaque phase peut englober plusieurs étapes, chacune d’elles pouvant s’exécuter sans assistance dans différentes cibles de calcul.

Pour plus d’informations sur les pipelines Machine Learning disposant de ce service, consultez [Pipelines et Azure Machine Learning](concept-ml-pipelines.md).

## <a name="logging"></a>Journalisation

Lorsque vous développez votre solution, utilisez le SDK Python Azure Machine Learning dans votre script Python pour journaliser les métriques arbitraires. Après l’exécution, interrogez les métriques pour vérifier si celle-ci a produit le modèle que vous souhaitez déployer.

## <a name="next-steps"></a>Étapes suivantes

Pour prendre en main le service Azure Machine Learning, consultez :

* [Qu’est-ce que le service Azure Machine Learning ?](overview-what-is-azure-ml.md)
* [Créer un espace de travail du service Azure Machine Learning](setup-create-workspace.md)
* [Tutoriel : Effectuer l'apprentissage d’un modèle](tutorial-train-models-with-aml.md)
* [Créer un espace de travail avec un modèle Resource Manager](how-to-create-workspace-template.md)
