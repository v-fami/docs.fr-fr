---
ms.openlocfilehash: 8db115a46df3fcea103e8fa6896542d0116aa256
ms.sourcegitcommit: 5b6d778ebb269ee6684fb57ad69a8c28b06235b9
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59236734"
---
### <a name="vbnet-no-longer-supports-partial-namespace-qualification-for-systemwindows-apis"></a>VB.NET ne prend plus en charge la qualification partielle des espaces de noms pour les API System.Windows

|   |   |
|---|---|
|Détails|À compter de .NET Framework 4.5.2, les projets VB.NET ne peuvent plus spécifier d’API System.Windows avec des espaces de noms partiellement qualifiés. Par exemple, la référence à <code>Windows.Forms.DialogResult</code> échoue. Le code doit référencer le nom qualifié complet (<xref:System.Windows.Forms.DialogResult>) ou importer l’espace de noms et référencer <xref:System.Windows.Forms.DialogResult?displayProperty=name>.|
|Suggestion|Le code doit être mis à jour pour référencer les API <code>System.Windows</code> avec des noms simples (en important l’espace de noms nécessaire) ou avec des noms qualifiés complets.|
|Portée|Mineur|
|Version|4.5.2|
|Type|Reciblage|
