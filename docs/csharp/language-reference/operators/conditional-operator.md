---
title: '?: , opérateur - Référence C#'
ms.custom: seodec18
ms.date: 11/20/2018
f1_keywords:
- ?:_CSharpKeyword
- ?_CSharpKeyword
- :_CSharpKeyword
helpviewer_keywords:
- '?: operator [C#]'
- conditional operator (?:) [C#]
ms.assetid: e83a17f1-7500-48ba-8bee-2fbc4c847af4
ms.openlocfilehash: a40dd4addfaf8a505cf334876192f0b2ccf66a09
ms.sourcegitcommit: 4c10802ad003374641a2c2373b8a92e3c88babc8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/08/2019
ms.locfileid: "65452393"
---
# <a name="-operator-c-reference"></a>?: Opérateur (référence C#)

L’opérateur conditionnel `?:`, souvent appelé opérateur conditionnel ternaire, évalue une expression booléenne et retourne le résultat de l’évaluation d’une des deux expressions, selon que l’expression booléenne donne `true` ou `false`. À partir de C# 7.2, [l’expression ref conditionnelle](#conditional-ref-expression) retourne la référence au résultat d’une des deux expressions.

Voici la syntaxe de l'opérateur conditionnel :

```csharp
condition ? consequent : alternative
```

L'expression `condition` doit donner `true` ou `false`. Si `condition` prend la valeur `true`, l’expression `consequent` est évaluée et son résultat devient le résultat de l’opération. Si `condition` prend la valeur `false`, l’expression `alternative` est évaluée et son résultat devient le résultat de l’opération. Soit `consequent`, soit `alternative` est évaluée.

Il faut que `consequent` et `alternative` soient du même type ou bien qu’une conversion implicite existe d’un type à l’autre.

L’opérateur conditionnel est associatif à droite ; autrement dit, une expression de la forme :

```csharp
a ? b : c ? d : e
```

est évaluée comme étant

```csharp
a ? b : (c ? d : e)
```

Pour vous rappeler de la valeur de cet opérateur, posez-vous la question suivante :

```text
is this condition true ? yes : no
```

Le « ? » de l’opérateur sert de point d’interrogation pour l’instruction précédente, et le suivant sert de réponse logique à cette question.

L’exemple suivant illustre l’utilisation de l’opérateur conditionnel :

[!code-csharp[non ref conditional](~/samples/snippets/csharp/language-reference/operators/ConditionalExamples.cs#ConditionalValue)]

## <a name="conditional-ref-expression"></a>Expression ref conditionnelle

À partir de C# 7.2, il est possible d’utiliser l’expression ref conditionnelle pour retourner la référence au résultat d’une des deux expressions. Vous pouvez affecter cette référence à une variable [locale ref](../keywords/ref.md#ref-locals) ou [locale ref readonly](../keywords/ref.md#ref-readonly-locals), ou l’utiliser comme [valeur de retour de référence](../keywords/ref.md#reference-return-values) ou comme [paramètre de méthode `ref`](../keywords/ref.md#passing-an-argument-by-reference).

Voici la syntaxe de l'expression ref conditionnelle :

```csharp
condition ? ref consequent : ref alternative
```

Comme l’opérateur conditionnel d’origine, l’expression ref conditionnelle n’évalue qu’une des deux expressions : soit `consequent`, soit `alternative`.

Dans le cas de l’expression ref conditionnelle, `consequent` et `alternative` doivent être du même type.

L’exemple suivant illustre l’utilisation de l’expression ref conditionnelle :

[!code-csharp[conditional ref](~/samples/snippets/csharp/language-reference/operators/ConditionalExamples.cs#ConditionalRef)]

Pour plus d’informations, voir la [proposition de fonctionnalité](../../../../_csharplang/proposals/csharp-7.2/conditional-ref.md).

## <a name="conditional-operator-and-an-ifelse-statement"></a>Opérateur conditionnel et instruction `if..else`

Sur une instruction [if-else](../keywords/if-else.md), l’opérateur conditionnel permet de rester concis dans l’écriture du code lorsque l’objectif est de calculer une valeur sous condition. L’exemple suivant montre deux façons de classer un entier comme négatif ou non :

[!code-csharp[conditional and if-else](~/samples/snippets/csharp/language-reference/operators/ConditionalExamples.cs#CompareWithIf)]

## <a name="operator-overloadability"></a>Capacité de surcharge de l’opérateur

L'opérateur conditionnel ne peut pas être surchargé.

## <a name="c-language-specification"></a>spécification du langage C#

Pour plus d’informations, voir la section [Opérateur conditionnel](~/_csharplang/spec/expressions.md#conditional-operator) de la [spécification du langage C#](../language-specification/index.md).

## <a name="see-also"></a>Voir aussi

- [Référence C#](../index.md)
- [Guide de programmation C#](../../programming-guide/index.md)
- [Opérateurs C#](index.md)
- [Instruction if-else](../keywords/if-else.md)
- [Opérateurs ?. et ?[]](member-access-operators.md#null-conditional-operators--and-)
- [?? Opérateur](null-coalescing-operator.md)
- [ref, mot clé](../keywords/ref.md)
