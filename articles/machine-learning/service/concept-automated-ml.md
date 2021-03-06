---
title: Sélection et optimisation de l’algorithme de machine learning automatisé
titleSuffix: Azure Machine Learning service
description: Découvrez comment le service Azure Machine Learning peut choisir automatiquement un algorithme pour vous et générer un modèle à partir de celui-ci pour vous permettre de gagner du temps en utilisant les paramètres et les critères que vous fournissez de façon à sélectionner le meilleur algorithme pour votre modèle.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
author: nacharya1
ms.author: nilesha
ms.date: 05/02/2019
ms.custom: seodec18
ms.openlocfilehash: 9736cc3ab20d43cc3731bc237bed9eb9b5370cb4
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65800782"
---
# <a name="what-is-automated-machine-learning"></a>Qu’est-ce que le machine learning automatisé ?

Apprentissage automatique, également appelé AutoML, permet aux chercheurs de données, les analystes et les développeurs à générer des modèles ML à grande échelle, l’efficacité et la productivité tout en maintenant la qualité du modèle. 

Automatisés ML génère un jeu de modèles ML automatiquement, intelligemment sélection de modèles pour l’apprentissage, puis recommande le mieux adapté pour vous. Développement d’un modèle d’apprentissage traditionnel consomme des ressources nécessitant une connaissance du domaine significatif et l’heure pour produire et comparer des dizaines de modèles. Avec ML automatisé, vous allez ainsi accélérer le temps que nécessaire pour obtenir des modèles ML de prêt pour la production avec l’efficacité et une grande facilité.

Dans les coulisses, vos données d’apprentissage sont effectuées avec une fonctionnalité de cible défini et intelligemment parcourues lors de combinaisons d’algorithmes ML et les sélections de fonctionnalité. Ensuite, selon les scores de formation, le modèle ajusté est identifié et recommandé de vous. 

Vous avez toujours contrôler votre expérience et la transparence quant à ce qui se passe. Vous pouvez définir des contraintes et faire des essais objectifs basées sur la durée, précision ou le nombre d’itérations, par exemple. Vous pouvez voir chaque modèle a été généré pour votre expérience, le flux de formation pour chaque itération et les fonctionnalités les plus influentes pour un modèle donné.

## <a name="how-automated-ml-works"></a>Comment automatisés works ML

À l’aide de **service Azure Machine Learning**, vous pouvez concevoir et exécuter vos expériences d’apprentissage automatisés ML avec ces étapes :

1. **Identifier le problème de ML** à résoudre : classification, de prévision ou de régression
   
1. **Spécifier la source et le format des données d’apprentissage étiqueté**: Tableaux Numpy ou données Pandas

1. **Configurer la cible de calcul pour l’apprentissage du modèle**, tel que votre [ordinateur local, Azure Machine Learning calcule, machines virtuelles à distance ou Azure Databricks](how-to-set-up-training-targets.md).  En savoir plus sur la formation automatisée [sur une ressource distante](how-to-auto-train-remote.md).

1. **Configurer les paramètres d’apprentissage automatique** qui déterminent le nombre d’itérations sur les différents modèles, paramètres d’hyperparamètres, des fonctions avancées de prétraitement/caractérisation et les mesures à prendre en compte pour déterminer le meilleur modèle.  Vous pouvez configurer les paramètres de l’expérience d’apprentissage automatique [dans le portail Azure](how-to-create-portal-experiments.md) ou [avec le SDK](how-to-configure-auto-train.md).

1. **Envoyer la formation à exécuter.** 


[![Apprentissage automatique](./media/how-to-automated-ml/automated-machine-learning.png)](./media/how-to-automated-ml/automated-machine-learning.png#lightbox)

Pendant l’entraînement, le service Azure Machine Learning crée plusieurs pipelines qui essaient différents paramètres et algorithmes. Il s’arrête une fois qu’il atteint les critères de sortie définis dans l’expérience. 

Vous pouvez également inspecter les informations d’exécution connectées, qui contient des mesures collectées pendant l’exécution. L’exécution de la formation génère un objet sérialisé de Python (`.pkl` fichier) qui contient le modèle et le prétraitement des données.


Bien que la création de modèles est automatisée, vous pouvez également [Apprenez comment importantes ou pertinentes de fonctionnalités sont](how-to-configure-auto-train.md#explain) pour les modèles générés. 

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2Xc9t]

<a name="preprocess"></a>

## <a name="preprocessing"></a>Prétraitement

Dans chaque automatisé expérience d’apprentissage, vos données sont prétraitées à l’aide de méthodes par défaut et éventuellement via avancées de prétraitement.

### <a name="automatic-preprocessing-standard"></a>Automatique de prétraitement (standard)
Dans chaque automatisé expérience d’apprentissage, vos données sont automatiquement mis à l’échelle ou normalisées pour aider les algorithmes fonctionnent bien.  Pendant l’apprentissage du modèle, une des techniques de mise à l’échelle ou normalisation suivantes s’appliqueront à chaque modèle.

|Mise à l’échelle&nbsp;&&nbsp;normalisation| Description  |
| ------------- | ------------- |
| [StandardScaleWrapper](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.StandardScaler.html)  | Normaliser les fonctionnalités en supprimant la moyenne et de mise à l’échelle variance d’unité  |
| [MinMaxScalar](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.MinMaxScaler.html)  | Transforme les fonctionnalités en mise à l’échelle de chaque fonctionnalité par de cette colonne au minimum et maximum  |
| [MaxAbsScaler](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.MaxAbsScaler.html#sklearn.preprocessing.MaxAbsScaler) |    Mettre à l’échelle de chaque fonctionnalité par sa valeur absolue maximale |  
| [RobustScalar](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.RobustScaler.html) |   Cette fonctionnalité Scaler par leur plage de quantiles |
| [PCA](https://scikit-learn.org/stable/modules/generated/sklearn.decomposition.PCA.html) | Réduction de dimensionnalité linéaire à l’aide de la décomposition de valeur singulière des données à projeter vers un espace dimensionnel inférieur | 
| [TruncatedSVDWrapper](https://scikit-learn.org/stable/modules/generated/sklearn.decomposition.TruncatedSVD.html) |    Ce transformateur effectue la réduction de dimensionnalité linéaire au moyen de décomposition tronquée valeur singulière (SVD). Contrairement aux PCA, cet estimateur ne centre pas les données avant de calculer la décomposition de valeur singulière. Cela signifie qu’il peut travailler efficacement avec des matrices de scipy.sparse | 
| [SparseNormalizer](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.Normalizer.html) | Chaque échantillon (autrement dit, chaque ligne de la matrice de données) au moins un composant non nulle est remises à l’échelle indépendamment des autres exemples afin que sa norme (l1 ou l2) est égal à un | 

### <a name="advanced-preprocessing-optional-featurization"></a>Advanced prétraitement : caractérisation facultative

Prétraitement avancées supplémentaires et la caractérisation sont également disponibles, tels que l’absence d’imputation des valeurs, de codage et de transformations. [En savoir plus sur la caractérisation est incluse](how-to-create-portal-experiments.md#preprocess). Activez ce paramètre avec :
+ Portail Azure : En sélectionnant le **prétraitement** case à cocher dans la **paramètres avancés** [avec ces étapes](how-to-create-portal-experiments.md). 
+ SDK Python : Spécification `"preprocess": True` pour le [ `AutoMLConfig` classe](https://docs.microsoft.com/python/api/azureml-train-automl/azureml.train.automl.automlconfig?view=azure-ml-py).

## <a name="ensemble-models"></a>Modèles d’ensemble

Vous pouvez former des modèles d’ensemble à l’aide d’apprentissage automatique avec le [algorithme de sélection Caruana ensemble avec l’initialisation Ensemble triée](http://www.niculescu-mizil.org/papers/shotgun.icml04.revised.rev2.pdf). Apprentissage d’ensemble améliore les résultats de la machine learning et les performances de prédiction en combinant plusieurs modèles au lieu d’utiliser des modèles uniques. L’itération de l’ensemble s’affiche en tant que la dernière itération de votre série de tests.

## <a name="use-with-onnx-in-c-apps"></a>Utilisation avec ONNX dans C# applications

Avec Azure Machine Learning, vous pouvez utiliser ML automatisé pour générer un modèle de Python et qu’il soit converti au format ONNX. Prend en charge par le runtime ONNX C#, de sorte que vous pouvez utiliser le modèle généré automatiquement dans votre C# applications sans avoir recours aux recoder ou l’un des latences réseau présentant les points de terminaison REST. Essayez un exemple de ce flux [ce Notebook Jupyter](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/classification-with-onnx/auto-ml-classification-with-onnx.ipynb).

## <a name="automated-ml-across-microsoft"></a>ML automatisée au sein de Microsoft

ML automatisé est également disponible dans d’autres solutions de Microsoft telles que :
+ Dans les applications .NET à l’aide de Visual Studio et Visual Studio Code avec [ML.NET](https://docs.microsoft.com/dotnet/machine-learning/automl-overview)
+ [Sur HDInsight](../../hdinsight/spark/apache-spark-run-machine-learning-automl.md), où vous faire évoluer vos travaux de formation automatisée ML sur Spark dans les clusters HDInsight en parallèle. 
+ [Dans Power BI](https://docs.microsoft.com/power-bi/service-machine-learning-automated)

## <a name="next-steps"></a>Étapes suivantes

Consultez les exemples et découvrez comment générer des modèles à l’aide du machine learning automatisé :

+ Suivez le [didacticiel : Entraîner automatiquement un modèle de classification avec le Machine Learning Azure automatisé](tutorial-auto-train-models.md).

+ Configurer les paramètres de l’expérience d’apprentissage automatique : 
   + Dans l’interface du portail Azure, [Utilisez ces étapes](how-to-create-portal-experiments.md).
   + Avec le SDK Python, [Utilisez ces étapes](how-to-configure-auto-train.md).
  
 + Découvrez comment auto former à l’aide des données de série chronologique, [Utilisez ces étapes](how-to-auto-train-forecast.md).

+ Essayer [exemples de bloc-notes Jupyter](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/)
