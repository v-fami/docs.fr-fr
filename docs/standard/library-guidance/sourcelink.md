---
title: Bibliothèques Source Link et .NET
description: Bonnes pratiques relatives à l’utilisation de Source Link pour améliorer le débogage des bibliothèques .NET
author: jamesnk
ms.author: mairaw
ms.date: 01/15/2019
ms.openlocfilehash: 9d3e2b0b3aedbab150072bf6eebff4acb5f8a0b7
ms.sourcegitcommit: 0d0a6e96737dfe24d3257b7c94f25d9500f383ea
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/07/2019
ms.locfileid: "65211673"
---
# <a name="source-link"></a>Source Link

Source Link est une technologie qui permet aux développeurs de déboguer le code source des assemblys .NET dans NuGet. Source Link s’exécute lors de la création du package NuGet et incorpore des métadonnées de contrôle de code source à l’intérieur des assemblys et du package. Les développeurs qui téléchargent le package et activent Source Link dans Visual Studio peuvent effectuer un pas à pas détaillé dans son code source. Source Link fournit des métadonnées de contrôle de code source qui améliorent grandement le débogage.

## <a name="source-link-demo"></a>Démonstration de Source Link

> [!VIDEO https://www.youtube.com/embed/gyRGhCQPkB4?start=61]

## <a name="using-source-link"></a>Utilisation de Source Link

Vous trouverez des instructions sur l’utilisation de Source Link dans le dépôt GitHub [dotnet/sourcelink](https://github.com/dotnet/sourcelink/blob/master/README.md).

Vous pouvez utiliser [NuGet Package Explorer](https://github.com/NuGetPackageExplorer/NuGetPackageExplorer) pour vérifier que les métadonnées Source Link ont été correctement incorporées dans le package. Vérifiez que les métadonnées `Repository` sont présentes avec un identificateur de commentaire et que les fichiers .pdb se trouvent avec le fichier .dll de chaque cible.

![Source Link dans NuGet Package Explorer](./media/sourcelink/nuget-package-explorer-sourcelink.png "Source Link dans NuGet Package Explorer")

**✔️ À ENVISAGER** : Utiliser Source Link pour ajouter des métadonnées de contrôle de code source à vos assemblys et packages NuGet.

> [!TIP]
> Vous pouvez améliorer davantage l’expérience de débogage d’un développeur en ajoutant des attributs de débogueur à vos types.
> * <xref:System.Diagnostics.DebuggerDisplayAttribute> peut personnaliser la façon dont une classe ou un champ s’affiche dans les fenêtres de variables du débogueur.
> * <xref:System.Diagnostics.DebuggerStepThroughAttribute> demande au débogueur de parcourir le code au lieu d’y effectuer un pas à pas détaillé.
> * <xref:System.Diagnostics.DebuggerBrowsableAttribute> contrôle si un membre est affiché dans les fenêtres de variables du débogueur.

**✔️ À ENVISAGER** : publication des fichiers de symboles (`*.pdb`).

> Pour une meilleure expérience de débogage, votre bibliothèque doit publier les fichiers de symboles et utiliser Source Link. Pour plus d’informations sur les fichiers de symboles et les packages de symboles, consultez [Packages de symboles](./nuget.md#symbol-packages).

>[!div class="step-by-step"]
>[Précédent](dependencies.md)
>[Suivant](publish-nuget-package.md)
