---
title: 'Tutoriel : Classer les problèmes de support : classification multiclasse'
description: Découvrez comment utiliser ML.NET dans un scénario de classification multiclasse pour classer des problèmes GitHub et les affecter à une zone donnée.
ms.date: 05/16/2019
ms.topic: tutorial
ms.custom: mvc, title-hack-0516
ms.openlocfilehash: d47522bef632de1aac890d4de384c1b2c16b7a50
ms.sourcegitcommit: c4e9d05644c9cb89de5ce6002723de107ea2e2c4
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2019
ms.locfileid: "65877340"
---
# <a name="tutorial-categorize-support-issues-using-multiclass-classification-with-ml-net"></a>Tutoriel : Classer les problèmes de support à l’aide de la classification multiclasse avec ML.NET

Ce tutoriel montre comment utiliser ML.NET pour créer un classifieur de problèmes GitHub afin d’entraîner un modèle qui classe et prédit l’étiquette Area d’un problème GitHub par le biais d’une application console .NET Core en C# dans Visual Studio.

Dans ce didacticiel, vous apprendrez à :
> [!div class="checklist"]
> * Préparer vos données
> * Transformer les données
> * Effectuer l’apprentissage du modèle
> * Évaluer le modèle
> * Prédire avec le modèle entraîné
> * Déployer et prédire avec un modèle chargé

Vous trouverez le code source de ce tutoriel dans le référentiel [dotnet/samples](https://github.com/dotnet/samples/tree/master/machine-learning/tutorials/GitHubIssueClassification).

## <a name="prerequisites"></a>Prérequis

* [Visual Studio 2017 15.6 ou version ultérieure](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2017), avec la charge de travail « Développement multiplateforme .Net Core » installée.

* [Fichier de problèmes GitHub séparés par des tabulations (issues_train.tsv)](https://raw.githubusercontent.com/dotnet/samples/master/machine-learning/tutorials/GitHubIssueClassification/Data/issues_train.tsv).
* [Fichier de test des problèmes GitHub séparés par des tabulations (issues_test.tsv)](https://raw.githubusercontent.com/dotnet/samples/master/machine-learning/tutorials/GitHubIssueClassification/Data/issues_test.tsv).

## <a name="create-a-console-application"></a>Créer une application console

### <a name="create-a-project"></a>Créer un projet

1. Ouvrez Visual Studio 2017. Sélectionnez **Fichier** > **Nouveau** > **Projet** dans la barre de menus. Dans la boîte de dialogue **Nouveau projet**, sélectionnez le nœud **Visual C#** suivi du nœud **.NET Core**. Ensuite, sélectionnez le modèle de projet **Application console (.NET Core)**. Dans la zone de texte **Nom**, tapez « GitHubIssueClassification », puis cliquez sur le bouton **OK**.

2. Créez un répertoire nommé *Données* dans votre projet pour enregistrer vos fichiers de jeu de données :

    Dans l'**Explorateur de solutions**, cliquez avec le bouton droit sur votre projet, puis sélectionnez **Ajouter** > **Nouveau dossier**. Tapez « Données » et appuyez sur Entrée.

3. Créez un répertoire nommé *Models* dans votre projet pour enregistrer votre modèle :

    Dans l'**Explorateur de solutions**, cliquez avec le bouton droit sur votre projet, puis sélectionnez **Ajouter** > **Nouveau dossier**. Tapez « Models » et appuyez sur Entrée.

4. Installez le **package NuGet Microsoft.ML** :

    Dans l'Explorateur de solutions, cliquez avec le bouton droit sur votre projet, puis sélectionnez **Gérer les packages NuGet**. Choisissez « nuget.org » comme source du package, sélectionnez l’onglet Parcourir, recherchez **Microsoft.ML**, sélectionnez le package **v 1.0.0** dans la liste, puis sélectionnez le bouton **Installer**. Cliquez sur le bouton **OK** dans la boîte de dialogue **Aperçu des modifications**, puis sur le bouton **J’accepte** dans la boîte de dialogue **Acceptation de la licence** si vous acceptez les termes du contrat de licence pour les packages répertoriés.

### <a name="prepare-your-data"></a>Préparer vos données

1. Téléchargez les jeux de données [issues_train.tsv](https://raw.githubusercontent.com/dotnet/samples/master/machine-learning/tutorials/GitHubIssueClassification/Data/issues_train.tsv) et [issues_test.tsv](https://raw.githubusercontent.com/dotnet/samples/master/machine-learning/tutorials/GitHubIssueClassification/Data/issues_test.tsv), puis enregistrez-les dans le dossier *Données* créé précédemment. Le premier jeu de données effectue l’apprentissage automatique du modèle, et le second peut servir à évaluer la précision de votre modèle.

2. Dans l'Explorateur de solutions, cliquez avec le bouton droit sur chacun des fichiers \*.tsv, puis sélectionnez **Propriétés**. Sous **Avancé**, définissez la valeur **Copier dans le répertoire de sortie** sur **Copier si plus récent**.

### <a name="create-classes-and-define-paths"></a>Créer des classes et définir des chemins

Ajoutez les instructions `using` supplémentaires suivantes en haut du fichier *Program.cs* :

[!code-csharp[AddUsings](~/samples/machine-learning/tutorials/GitHubIssueClassification/Program.cs#AddUsings)]

Créez trois champs globaux pour accueillir les chemins des fichiers récemment téléchargés, puis des variables globales pour `MLContext`, `DataView` et `PredictionEngine` :

* `_trainDataPath` contient le chemin d’accès au jeu de données utilisé pour l’apprentissage du modèle.
* `_testDataPath` contient le chemin d’accès au jeu de données utilisé pour évaluer le modèle.
* `_modelPath` contient le chemin d’accès où le modèle formé est enregistré.
* `_mlContext` est le <xref:Microsoft.ML.MLContext> qui fournit le contexte de traitement.
* `_trainingDataView` est le <xref:Microsoft.ML.IDataView> utilisé pour traiter le jeu de données d’entraînement.
* `_predEngine` est le <xref:Microsoft.ML.PredictionEngine%602> utilisé pour des prédictions uniques.

Ajoutez le code suivant à la ligne juste au-dessus de la méthode `Main` pour spécifier ces chemins et les autres variables :

[!code-csharp[DeclareGlobalVariables](~/samples/machine-learning/tutorials/GitHubIssueClassification/Program.cs#DeclareGlobalVariables)]

Créez des classes pour vos données d’entrée et vos prévisions. Ajoutez une nouvelle classe à votre projet :

1. Dans l **’Explorateur de solutions**, cliquez avec le bouton de droite sur le projet, puis sélectionnez **Ajouter** > **Nouvel élément**.

1. Dans la boîte de dialogue **Ajouter un nouvel élément**, sélectionnez **Classe**, puis remplacez la valeur du champ **Nom** par *GitHubIssueData.cs*. Ensuite, sélectionnez le bouton **Ajouter**.

    Le fichier *GitHubIssueData.cs* s’ouvre dans l’éditeur de code. Ajoutez l’instruction `using` suivante en haut de *GitHubIssueData.cs* :

[!code-csharp[AddUsings](~/samples/machine-learning/tutorials/GitHubIssueClassification/GitHubIssueData.cs#AddUsings)]

Supprimez la définition de classe existante et ajoutez le code suivant, lequel contient deux classes (`GitHubIssue` et `IssuePrediction`), au fichier *GitHubIssueData.cs* :

[!code-csharp[DeclareGlobalVariables](~/samples/machine-learning/tutorials/GitHubIssueClassification/GitHubIssueData.cs#DeclareTypes)]

`label` est la colonne à prédire. Les valeurs `Features` identifiées sont les entrées que vous fournissez au modèle pour prédire l’étiquette.

Utilisez [LoadColumnAttribute](xref:Microsoft.ML.Data.LoadColumnAttribute) pour spécifier les index des colonnes sources dans le jeu de données.

`GitHubIssue`, la classe de jeu de données d’entrée, comprend les champs <xref:System.String> suivants :

* La première colonne `ID` est l’ID du problème GitHub.
* La deuxième colonne `Area` est la prédiction pour l’entraînement.
* La troisième colonne `Title` (titre du problème GitHub) est la première caractéristique (`feature`) utilisée pour prédire l’étiquette `Area`
* La quatrième colonne `Description` est la deuxième caractéristique (`feature`) utilisée pour prédire `Area`

`IssuePrediction` représente la classe utilisée pour la prédiction, une fois le modèle formé. Il comporte une valeur `string` unique (`Area`) et un attribut `ColumnName` `PredictedLabel`.  L’attribut `PredictedLabel` est utilisé pendant la prédiction et l’évaluation. L’évaluation utilise une entrée avec les données d’apprentissage, les valeurs prédites et le modèle.

Toutes les opérations de ML.NET démarrent dans la classe [MLContext](xref:Microsoft.ML.MLContext). L’initialisation de `mlContext` crée un environnement ML.NET qui peut être partagé par les objets du workflow de création de modèle. Sur le plan conceptuel, cette classe est similaire à `DBContext` dans `Entity Framework`.

### <a name="initialize-variables-in-main"></a>Initialiser les variables dans Principal

Initialisez la variable globale `_mlContext` à l’aide d’une nouvelle instance de `MLContext` avec une valeur de départ aléatoire (`seed: 0`) pour obtenir des résultats reproductibles/déterministes dans différents entraînements.  Remplacez la ligne `Console.WriteLine("Hello World!")` par le code suivant dans la méthode `Main` :

[!code-csharp[CreateMLContext](~/samples/machine-learning/tutorials/GitHubIssueClassification/Program.cs#CreateMLContext)]

## <a name="load-the-data"></a>Charger les données

ML.NET utilise la [classe IDataView](xref:Microsoft.ML.IDataView) comme un moyen flexible et efficace de décrire des données tabulaires au format numérique ou texte. `IDataView` peut charger des fichiers texte ou en temps réel (par exemple, une base de données SQL ou des fichiers journaux).

Pour initialiser et charger la variable globale `_trainingDataView` afin de l’utiliser pour le pipeline, ajoutez le code suivant après l’initialisation de `mlContext` :

[!code-csharp[LoadTrainData](~/samples/machine-learning/tutorials/GitHubIssueClassification/Program.cs#LoadTrainData)]

[LoadFromTextFile()](xref:Microsoft.ML.TextLoaderSaverCatalog.LoadFromTextFile%60%601%28Microsoft.ML.DataOperationsCatalog,System.String,System.Char,System.Boolean,System.Boolean,System.Boolean,System.Boolean%29) définit le schéma de données et lit le fichier. Elle prend les variables de chemin de données et retourne un `IDataView`.

Ajoutez le code suivant comme prochaine ligne dans la méthode `Main` :

[!code-csharp[CallProcessData](~/samples/machine-learning/tutorials/GitHubIssueClassification/Program.cs#CallProcessData)]

La méthode `ProcessData` exécute les tâches suivantes :

* Extrait et transforme les données.
* Retourne le pipeline de traitement.

Créez la méthode `ProcessData` juste après la méthode `Main`, en utilisant le code suivant :

```csharp
public static IEstimator<ITransformer> ProcessData()
{

}
```

## <a name="extract-features-and-transform-the-data"></a>Extraire des caractéristiques et transformer les données

Pour prédire l’étiquette Area GitHub pour un `GitHubIssue`, utilisez la méthode [MapValueToKey()](xref:Microsoft.ML.ConversionsExtensionsCatalog.MapValueToKey%2A) afin de transformer la colonne `Area` en une colonne `Label` de type clé numérique (un format accepté par les algorithmes de classification) et ajoutez-la comme nouvelle colonne au jeu de données :

[!code-csharp[MapValueToKey](~/samples/machine-learning/tutorials/GitHubIssueClassification/Program.cs#MapValueToKey)]

Ensuite, appelez `mlContext.Transforms.Text.FeaturizeText` qui transforme les colonnes de texte (`Title` et `Description`) en vecteur numérique pour chaque `TitleFeaturized` et `DescriptionFeaturized` appelé. Utilisez le code suivant pour ajouter la caractérisation des deux colonnes au pipeline :

[!code-csharp[FeaturizeText](~/samples/machine-learning/tutorials/GitHubIssueClassification/Program.cs#FeaturizeText)]

La dernière étape de préparation des données consiste à combiner toutes les colonnes de caractéristiques dans la colonne **Features** à l’aide de la méthode [Concatenate()](xref:Microsoft.ML.TransformExtensionsCatalog.Concatenate%2A). Par défaut, un algorithme d’apprentissage traite uniquement les caractéristiques issues de la colonne **Features**. Utilisez le code suivant pour ajouter cette transformation au pipeline :

[!code-csharp[Concatenate](~/samples/machine-learning/tutorials/GitHubIssueClassification/Program.cs#Concatenate)]

 Ajoutez ensuite un <xref:Microsoft.ML.Data.EstimatorChain%601.AppendCacheCheckpoint%2A> pour mettre en cache le DataView. Ainsi, quand vous itérerez plusieurs fois sur les données, l’utilisation du cache vous fera peut-être gagner en performances, à l’instar du code suivant :

[!code-csharp[AppendCache](~/samples/machine-learning/tutorials/GitHubIssueClassification/Program.cs#AppendCache)]

> [!WARNING]
> Utilisez AppendCacheCheckpoint pour les jeux de données petits/moyens afin de réduire le temps d’apprentissage. Ne l’utilisez PAS (supprimez .AppendCacheCheckpoint()) lors du traitement de jeux de données très volumineux.

Retournez le pipeline à la fin de la méthode `ProcessData`.

[!code-csharp[ReturnPipeline](~/samples/machine-learning/tutorials/GitHubIssueClassification/Program.cs#ReturnPipeline)]

Cette étape gère le prétraitement/la caractérisation. L’utilisation des composants supplémentaires disponibles dans ML.NET peut améliorer les résultats de votre modèle.

## <a name="build-and-train-the-model"></a>Générer et entraîner le modèle

Ajoutez l’appel suivant à la méthode `BuildAndTrainModel` comme prochaine ligne de code dans la méthode `Main` :

[!code-csharp[CallBuildAndTrainModel](~/samples/machine-learning/tutorials/GitHubIssueClassification/Program.cs#CallBuildAndTrainModel)]

La méthode `BuildAndTrainModel` exécute les tâches suivantes :

* Crée la classe d’algorithme d’entraînement.
* Effectue l’apprentissage du modèle.
* Prédit la zone en fonction des données d’entraînement.
* Retourne le modèle.

Créez la méthode `BuildAndTrainModel` juste après la méthode `Main`, en utilisant le code suivant :

```csharp
public static IEstimator<ITransformer> BuildAndTrainModel(IDataView trainingDataView, IEstimator<ITransformer> pipeline)
{

}
```

### <a name="about-the-classification-task"></a>À propos de la tâche de classification

La classification est une tâche de machine learning qui utilise des données pour **déterminer** la catégorie, le type ou la classe d’un élément ou d’une ligne de données. Cette tâche est souvent d’un de ces types :

* Binaire : A ou B.
* Multiclasse : plusieurs catégories pouvant être prédites à l’aide d’un modèle unique.

Pour ce type de problème, utilisez un algorithme de machine learning de classification multiclasse, dans la mesure où votre prédiction de catégorie de problème peut appartenir à une catégorie parmi plusieurs possibles (multiclasse) plutôt qu’à une catégorie parmi deux possibles (binaire).

Ajoutez l’algorithme de machine learning aux définitions de transformation de données en ajoutant la première ligne de code suivante dans `BuildAndTrainModel()` :

[!code-csharp[AddTrainer](~/samples/machine-learning/tutorials/GitHubIssueClassification/Program.cs#AddTrainer)]

[SdcaMaximumEntropy](xref:Microsoft.ML.Trainers.SdcaMaximumEntropyMulticlassTrainer) est l’algorithme de machine learning de classification multiclasse. Il est ajouté à `pipeline` et accepte les caractéristiques `Title` et `Description` (`Features`) ainsi que les paramètres d’entrée `Label` pour apprendre à partir des données d’historique.

### <a name="train-the-model"></a>Effectuer l’apprentissage du modèle

Ajustez le modèle aux données `splitTrainSet` et retournez le modèle entraîné en ajoutant la ligne de code suivante dans la méthode `BuildAndTrainModel()` :

[!code-csharp[TrainModel](~/samples/machine-learning/tutorials/GitHubIssueClassification/Program.cs#TrainModel)]

La méthode `Fit()` entraîne votre modèle en transformant le jeu de données et en appliquant l’entraînement.

[PredictionEngine](xref:Microsoft.ML.PredictionEngine%602) est une API utile qui vous permet de passer et d’exécuter une prédiction sur une seule instance de données. Ajoutez ce contenu à la ligne suivante dans la méthode `BuildAndTrainModel()` :

[!code-csharp[CreatePredictionEngine1](~/samples/machine-learning/tutorials/GitHubIssueClassification/Program.cs#CreatePredictionEngine1)]

### <a name="predict-with-the-trained-model"></a>Prédire avec le modèle entraîné

Ajoutez un problème GitHub pour tester la prédiction du modèle entraîné dans la méthode `Predict` en créant une instance de `GitHubIssue` :

[!code-csharp[CreateTestIssue1](~/samples/machine-learning/tutorials/GitHubIssueClassification/Program.cs#CreateTestIssue1)]

Utilisez la fonction [Predict()](xref:Microsoft.ML.PredictionEngine%602.Predict%2A) pour effectuer une prédiction sur une seule ligne de données :

[!code-csharp[Predict](~/samples/machine-learning/tutorials/GitHubIssueClassification/Program.cs#Predict)]

### <a name="using-the-model-prediction-results"></a>Utilisation des résultats du modèle : prédiction

Affichez `GitHubIssue` et la prédiction d’étiquette `Area` correspondante pour partager les résultats et prendre les mesures nécessaires.  Créez une vue des résultats à l’aide du code <xref:System.Console.WriteLine?displayProperty=nameWithType> suivant :

[!code-csharp[OutputPrediction](~/samples/machine-learning/tutorials/GitHubIssueClassification/Program.cs#OutputPrediction)]

### <a name="return-the-model-trained-to-use-for-evaluation"></a>Retourner le modèle entraîné à utiliser pour l’évaluation

Retournez le modèle à la fin de la méthode `BuildAndTrainModel`.

[!code-csharp[ReturnModel](~/samples/machine-learning/tutorials/GitHubIssueClassification/Program.cs#ReturnModel)]

## <a name="evaluate-the-model"></a>Évaluer le modèle

Maintenant que vous avez créé et effectué l’apprentissage du modèle, vous devez l’évaluer avec un jeu de données différent à des fins d’assurance qualité et de validation. Dans la méthode `Evaluate`, le modèle créé dans `BuildAndTrainModel` est passé pour évaluation. Créez la méthode `Evaluate` juste après `BuildAndTrainModel`, comme dans le code suivant :

```csharp
public static void Evaluate()
{

}
```

La méthode `Evaluate` exécute les tâches suivantes :

* Charge le jeu de données de test.
* Crée l’évaluateur multiclasse.
* Évalue le modèle et crée des métriques.
* Affiche les métriques.

Ajoutez un appel à la nouvelle méthode à partir de la méthode `Main`, juste sous l’appel à la méthode `BuildAndTrainModel`, en utilisant le code suivant :

[!code-csharp[CallEvaluate](~/samples/machine-learning/tutorials/GitHubIssueClassification/Program.cs#CallEvaluate)]

Comme vous l’avez fait précédemment avec le jeu de données d’entraînement, chargez le jeu de données de test en ajoutant le code suivant à la méthode `Evaluate` :

[!code-csharp[LoadTestDataset](~/samples/machine-learning/tutorials/GitHubIssueClassification/Program.cs#LoadTestDataset)]

La méthode [Evaluate()](xref:Microsoft.ML.MulticlassClassificationCatalog.Evaluate%2A) calcule les métriques de qualité pour le modèle à l’aide du jeu de données spécifié. Elle retourne un objet <xref:Microsoft.ML.Data.MulticlassClassificationMetrics> contenant les métriques globales calculées par les évaluateurs de classification multiclasse.
Pour afficher les métriques permettant de déterminer la qualité du modèle, vous devez d’abord les obtenir.
Notez l’utilisation de la méthode [Transform()](xref:Microsoft.ML.ITransformer.Transform%2A) de la variable globale de machine learning `_trainedModel` (un [ITransformer](xref:Microsoft.ML.ITransformer)) pour entrer les caractéristiques et retourner les prédictions. Ajoutez comme nouvelle ligne le code suivant à la méthode `Evaluate` :

[!code-csharp[Evaluate](~/samples/machine-learning/tutorials/GitHubIssueClassification/Program.cs#Evaluate)]

Les métriques suivantes sont évaluées pour la classification multiclasse :

* Micro-précision : chaque paire exemple-classe contribue de manière égale à la métrique de précision.  Vous voulez que la micro-précision soit aussi proche de 1 que possible.

* Macro-précision : chaque classe contribue de manière égale à la métrique de précision. Les classes minoritaires sont aussi importantes que les classes plus grandes. Vous voulez que la macro-précision soit aussi proche de 1 que possible.

* Perte logarithmique : consultez [Perte logarithmique](../resources/glossary.md#log-loss). Vous voulez que la perte logarithmique soit aussi proche de zéro que possible.

* Réduction de la perte logarithmique : comprise entre [-inf, 100], où 100 correspond à des prédictions parfaites et 0 à des prédictions moyennes. Vous voulez que la réduction de la perte logarithmique soit aussi proche de zéro que possible.

### <a name="displaying-the-metrics-for-model-validation"></a>Affichage des métriques pour la validation du modèle

Utilisez le code suivant pour afficher les métriques, partager les résultats et agir dessus :

[!code-csharp[DisplayMetrics](~/samples/machine-learning/tutorials/GitHubIssueClassification/Program.cs#DisplayMetrics)]

## <a name="deploy-and-predict-with-a-model"></a>Déployer et prédire avec un modèle

Ajoutez un appel à la nouvelle méthode à partir de la méthode `Main`, juste sous l’appel à la méthode `Evaluate`, en utilisant le code suivant :

[!code-csharp[CallPredictIssue](~/samples/machine-learning/tutorials/GitHubIssueClassification/Program.cs#CallPredictIssue)]

Créez la méthode `PredictIssue`, juste après la méthode `Evaluate` (et juste avant la méthode `SaveModelAsFile`), en utilisant le code suivant :

```csharp
private static void PredictIssue()
{

}
```

La méthode `PredictIssue` exécute les tâches suivantes :

* Crée un problème unique de données de test.
* Prédit Area en fonction des données de test.
* Combine les données de test et les prédictions pour générer des rapports.
* Affiche les résultats prédits.

Ajoutez un problème GitHub pour tester la prédiction du modèle entraîné dans la méthode `Predict` en créant une instance de `GitHubIssue` :

[!code-csharp[AddTestIssue](~/samples/machine-learning/tutorials/GitHubIssueClassification/Program.cs#AddTestIssue)]

Comme vous l’avez fait précédemment, créez une instance `PredictionEngine` avec le code suivant :

[!code-csharp[CreatePredictionEngine](~/samples/machine-learning/tutorials/GitHubIssueClassification/Program.cs#CreatePredictionEngine)]
  
Utilisez `PredictionEngine` pour prédire l’étiquette Area GitHub en ajoutant le code suivant à la méthode `PredictIssue` servant à la prédiction :

[!code-csharp[PredictIssue](~/samples/machine-learning/tutorials/GitHubIssueClassification/Program.cs#PredictIssue)]

### <a name="using-the-loaded-model-for-prediction"></a>Utilisation du modèle chargé pour la prédiction

Affichez `Area` pour catégoriser le problème et agir en conséquence. Créez une vue des résultats à l’aide du code <xref:System.Console.WriteLine?displayProperty=nameWithType> suivant :

[!code-csharp[DisplayResults](~/samples/machine-learning/tutorials/GitHubIssueClassification/Program.cs#DisplayResults)]

## <a name="results"></a>Résultats

Vos résultats doivent être similaires à ce qui suit. Lors du traitement, le pipeline affiche des messages. Vous pouvez voir des avertissements ou des messages de traitement. Ces messages ont été supprimés des résultats suivants par souci de clarté.

```console
=============== Single Prediction just-trained-model - Result: area-System.Net ===============
*************************************************************************************************************
*       Metrics for Multi-class Classification model - Test Data
*------------------------------------------------------------------------------------------------------------
*       MicroAccuracy:    0.738
*       MacroAccuracy:    0.668
*       LogLoss:          .919
*       LogLossReduction: .643
*************************************************************************************************************
=============== Single Prediction - Result: area-System.Data ===============
```

Félicitations ! Vous venez de créer un modèle d’apprentissage automatique pour la classification et la prédiction d’une étiquette Area d’un problème GitHub. Vous trouverez le code source de ce tutoriel dans le référentiel [dotnet/samples](https://github.com/dotnet/samples/tree/master/machine-learning/tutorials/GitHubIssueClassification).

## <a name="next-steps"></a>Étapes suivantes

Dans ce didacticiel, vous avez appris à :
> [!div class="checklist"]
> * Préparer vos données
> * Transformer les données
> * Effectuer l’apprentissage du modèle
> * Évaluer le modèle
> * Prédire avec le modèle entraîné
> * Déployer et prédire avec un modèle chargé

Passer au tutoriel suivant pour en savoir plus
> [!div class="nextstepaction"]
> [Prédiction du prix d’une course de taxi](taxi-fare.md)
