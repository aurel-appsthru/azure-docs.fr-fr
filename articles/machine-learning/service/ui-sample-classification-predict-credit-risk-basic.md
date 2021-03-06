---
title: 'Classification : Prédire le risque de crédit'
titleSuffix: Azure Machine Learning service
description: Découvrez comment créer un classifieur d’apprentissage sans écrire une seule ligne de code à l’aide de l’interface visuelle.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: article
author: xiaoharper
ms.author: zhanxia
ms.reviewer: sgilley
ms.date: 05/10/2019
ms.openlocfilehash: f37c945758cfbd03889d79acf764e7f67022267a
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65789399"
---
# <a name="sample-3---classification-predict-credit-risk"></a>Exemple 3 - Classification : Prédire le risque de crédit

Découvrez comment créer un classifieur d’apprentissage sans écrire une seule ligne de code à l’aide de l’interface visuelle. Cet exemple effectue l’apprentissage d’un **arbre de décision optimisé à deux classes** pour prédire le crédit risque (élevée ou faible) selon les informations d’application de crédit telles que l’historique de crédit, âge et nombre de cartes de crédit.

Étant donné que nous essayons de répondre à la question « Quel one ? » Il s’agit d’un problème de classification. Toutefois, vous pouvez appliquer le même processus fondamentaux pour faire face à n’importe quel type de problème d’apprentissage automatique qu’il s’agisse de régression, la classification, clustering et ainsi de suite.

Voici le graphique terminé pour cette expérience :

![Graphique de l’expérience](media/ui-sample-classification-predict-credit-risk-basic/overall-graph.png)

## <a name="prerequisites"></a>Conditions préalables

[!INCLUDE [aml-ui-prereq](../../../includes/aml-ui-prereq.md)]

4. Sélectionnez le **Open** bouton de l’expérience de l’exemple 3 :

    ![Ouvrez l’expérience](media/ui-sample-classification-predict-credit-risk-basic/open-sample3.png)

## <a name="related-sample"></a>Exemple

[Exemple 4 : Classification : (Coût de la casse) prédiction du risque de crédit](ui-sample-classification-predict-credit-risk-cost-sensitive.md) fournit une expérience avancée qui résout le problème même en tant que cette expérience. Il montre comment effectuer _coût sensible_ classification à l’aide un **exécuter le Script Python** module et comparer les performances des deux algorithmes de classification binaire. Vous y reporter si vous souhaitez en savoir plus sur la création d’expériences de classification.

## <a name="data"></a>Données

Nous utilisons le jeu de données de carte de crédit allemande à partir du référentiel de UC Irvine.
Le jeu de données contient des exemples de 1 000 avec 20 caractéristiques et 1 étiquette. Chaque exemple représente une personne. Les fonctionnalités incluent des caractéristiques numériques et catégorielles. Consultez le [site Web UCI](https://archive.ics.uci.edu/ml/datasets/Statlog+%28German+Credit+Data%29) pour la signification des fonctionnalités catégorielles. La dernière colonne est l’étiquette, qui désigne le risque de crédit et a uniquement deux valeurs possibles : risque de crédit élevé = 2 et risque de crédit faible = 1.

## <a name="experiment-summary"></a>Résumé de l’expérience

Nous suivons ces étapes pour créer l’expérience :

1. Dans la zone de dessin de l’expérience, faites glisser le module de jeu de données de données UCI de carte de crédit allemand.
1. Ajouter un **modifier les métadonnées** module afin de pouvoir ajouter des noms explicites pour chaque colonne.
1. Ajouter un **fractionner les données** module permettant de créer les jeux d’apprentissage et de test. Définissez la fraction de lignes dans le premier jeu de données de sortie à 0,7. Ce paramètre spécifie que 70 % des données sera sortie au port de gauche du module et le reste pour le port de droite. Nous utilisons le jeu de données de gauche pour la formation et de celui qui convient pour le test.
1. Ajouter un **Two-Class Boosted Decision Tree** module pour initialiser un classifieur d’arbre de décision optimisé.
1. Ajouter un **former le modèle** module. Connecter le classifieur de l’étape précédente pour le port d’entrée gauche de la **former le modèle**. Ajouter le jeu d’apprentissage (port de sortie de gauche du **fractionner les données**) pour le port d’entrée de droite le **former le modèle**. Le **former le modèle** allons former le classifieur.
1. Ajouter un **noter le modèle** module et connecter le **former le modèle** module à celui-ci. Puis ajoutez le jeu de test (le port de droite le **fractionner les données**) pour le **noter le modèle**. Le **noter le modèle** rendra les prédictions. Vous pouvez sélectionner les ports de sortie pour afficher les prédictions et les probabilités de classe positive.
1. Ajouter un **évaluer le modèle** module et connecter le jeu de données évalué à son port d’entrée gauche. Pour afficher les résultats d’évaluation, sélectionnez le port de sortie le **évaluer le modèle** module et sélectionnez **visualiser**.

Voici le graphique d’expérience complète :

![Graphique de l’expérience](media/ui-sample-classification-predict-credit-risk-basic/overall-graph.png)

## <a name="results"></a>Résultats

![Évaluer les résultats](media/ui-sample-classification-predict-credit-risk-basic/evaluate-result.png)

Dans les résultats d’évaluation, vous pouvez voir que l’AUC du modèle est 0.776. Au seuil de 0,5, la précision est 0.621, le rappel est 0,456 et le score F1 est 0.526.

## <a name="clean-up-resources"></a>Supprimer des ressources

[!INCLUDE [aml-ui-cleanup](../../../includes/aml-ui-cleanup.md)]

## <a name="next-steps"></a>Étapes suivantes

Explorez les autres exemples disponibles pour l’interface visuelle :

- [Exemple 1 : régression : Prédire les prix d’un véhicule automobile](ui-sample-regression-predict-automobile-price-basic.md)
- [Exemple 2 - régression : Comparer des algorithmes pour la prédiction de prix des véhicules automobiles](ui-sample-regression-predict-automobile-price-compare-algorithms.md)
- [Exemple 4 : Classification : Prédire le risque de crédit (coût sensible)](ui-sample-classification-predict-credit-risk-cost-sensitive.md)
- [Exemple 5 - Classification : Prédire l’évolution](ui-sample-classification-predict-churn.md)