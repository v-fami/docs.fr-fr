---
title: 'Procédure : Obtenir un ListBoxItem'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- ListBox controls [WPF], getting a ListBoxItem
- ListBoxItem [WPF]
ms.assetid: da877c6f-5fd8-40cb-8909-225cbfd99aa5
ms.openlocfilehash: 27a435feb4a941c77af221ab25bd63ea98b16e92
ms.sourcegitcommit: 9b552addadfb57fab0b9e7852ed4f1f1b8a42f8e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61910323"
---
# <a name="how-to-get-a-listboxitem"></a>Procédure : Obtenir un ListBoxItem
Si vous avez besoin obtenir un spécifique <xref:System.Windows.Controls.ListBoxItem> à un index particulier dans un <xref:System.Windows.Controls.ListBox>, vous pouvez utiliser un <xref:System.Windows.Controls.ItemContainerGenerator>.  
  
## <a name="example"></a>Exemple  
 L’exemple suivant montre un <xref:System.Windows.Controls.ListBox> et ses éléments.  
  
 [!code-xaml[ListBoxItems#1](~/samples/snippets/csharp/VS_Snippets_Wpf/ListBoxItems/CSharp/Window1.xaml#1)]  
  
 L’exemple suivant montre comment récupérer l’élément en spécifiant l’index de l’élément dans le <xref:System.Windows.Controls.ItemContainerGenerator.ContainerFromIndex%2A> propriété de la <xref:System.Windows.Controls.ItemContainerGenerator>.  
  
 [!code-csharp[ListBoxItems#2](~/samples/snippets/csharp/VS_Snippets_Wpf/ListBoxItems/CSharp/Window1.xaml.cs#2)]
 [!code-vb[ListBoxItems#2](~/samples/snippets/visualbasic/VS_Snippets_Wpf/ListBoxItems/VisualBasic/Window1.xaml.vb#2)]  
  
 Une fois que vous avez extrait l’élément de zone de liste, vous pouvez afficher le contenu de l’élément, comme indiqué dans l’exemple suivant.  
  
 [!code-csharp[ListBoxItems#3](~/samples/snippets/csharp/VS_Snippets_Wpf/ListBoxItems/CSharp/Window1.xaml.cs#3)]
 [!code-vb[ListBoxItems#3](~/samples/snippets/visualbasic/VS_Snippets_Wpf/ListBoxItems/VisualBasic/Window1.xaml.vb#3)]
