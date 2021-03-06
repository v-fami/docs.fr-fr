---
title: set, mot clé - Référence C#
ms.custom: seodec18
ms.date: 03/10/2017
f1_keywords:
- set
- set_CSharpKeyword
helpviewer_keywords:
- set keyword [C#]
ms.assetid: 30d7e4e5-cc2e-4635-a597-14a724879619
ms.openlocfilehash: 0322f1bb94174dd3a0cdd2089c8626d25a80cc1c
ms.sourcegitcommit: 4ac80713f6faa220e5a119d5165308a58f7ccdc8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/09/2019
ms.locfileid: "54147991"
---
# <a name="set-c-reference"></a>set (référence C#)

Le mot clé `set` définit une méthode *accessor* dans une propriété ou un indexeur qui assigne une valeur à l’élément de la propriété ou de l’indexeur. Pour plus d’informations et des exemples, consultez [Propriétés](../../programming-guide/classes-and-structs/properties.md), [Propriétés implémentées automatiquement](../../programming-guide/classes-and-structs/auto-implemented-properties.md) et [Indexeurs](../../programming-guide/indexers/index.md).

L’exemple suivant définit un accesseur `get` et un accesseur `set` pour une propriété nommée `Seconds`. Il utilise un champ privé nommé `_seconds` pour stocker la valeur de la propriété.

[!code-csharp[set#1](~/samples/snippets/csharp/language-reference/keywords/get/get-1.cs)]

Souvent, l’accesseur `set` se compose d’une seule instruction qui assigne une valeur, comme dans l’exemple précédent. À compter de C# 7.0, vous pouvez implémenter l’accesseur `set` comme membre expression-bodied. L’exemple suivant implémente l’accesseur `get` et l’accesseur `set` en tant que membres expression-bodied.

[!code-csharp[set#3](~/samples/snippets/csharp/language-reference/keywords/get/get-3.cs)]
  
Pour les cas simples dans lesquels les accesseurs `get` et `set` d’une propriété n’effectuent aucune autre opération que la définition ou la récupération d’une valeur dans un champ de stockage privé, vous pouvez tirer parti de la prise en charge par le compilateur C# des propriétés implémentées automatiquement. L’exemple suivant implémente `Hours` en tant que propriété implémentée automatiquement. 

[!code-csharp[set#2](~/samples/snippets/csharp/language-reference/keywords/get/get-2.cs)]
  
## <a name="c-language-specification"></a>spécification du langage C#

[!INCLUDE[CSharplangspec](~/includes/csharplangspec-md.md)]

## <a name="see-also"></a>Voir aussi

- [Référence C#](../../language-reference/index.md)
- [Guide de programmation C#](../../programming-guide/index.md)
- [Mots clés C#](index.md)
- [Propriétés](../../programming-guide/classes-and-structs/properties.md)
