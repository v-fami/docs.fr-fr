---
title: GetCORSystemDirectory, fonction
ms.date: 03/30/2017
api_name:
- GetCORSystemDirectory
api_location:
- mscoree.dll
- mscoreei.dll
api_type:
- DLLExport
f1_keywords:
- GetCORSystemDirectory
helpviewer_keywords:
- GetCORSystemDirectory function [.NET Framework hosting]
ms.assetid: 3dcd16a7-dafc-4ca8-b5cd-20ffb37db91d
topic_type:
- apiref
author: rpetrusha
ms.author: ronpet
ms.openlocfilehash: 567e6533a9a9ac718f8b5acac769295c104f7f3c
ms.sourcegitcommit: 9b552addadfb57fab0b9e7852ed4f1f1b8a42f8e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61628095"
---
# <a name="getcorsystemdirectory-function"></a>GetCORSystemDirectory, fonction
Retourne le répertoire d’installation du common language runtime (CLR) qui est chargé dans le processus. Le répertoire d’installation est qualifié complet, par exemple, « c:\windows\microsoft.net\framework\v1.0.3705 ».  
  
 Cette fonction est déconseillée. Il est remplacé par le [ICLRRuntimeInfo::GetRuntimeDirectory](../../../../docs/framework/unmanaged-api/hosting/iclrruntimeinfo-getruntimedirectory-method.md) méthode fourni dans le [!INCLUDE[net_v40_long](../../../../includes/net-v40-long-md.md)].  
  
## <a name="syntax"></a>Syntaxe  
  
```  
HRESULT GetCORSystemDirectory (   
    [out] LPWSTR  pbuffer,     
    [in]  DWORD   cchBuffer,   
    [out] DWORD*  dwlength  
);   
```  
  
## <a name="parameters"></a>Paramètres  
 `pbuffer`  
 [out] Une mémoire tampon dans laquelle le runtime retourne une chaîne qui contient le nom qualifié complet du répertoire d’installation pour le runtime est chargé dans le processus. Si le runtime n’a pas encore été chargé dans le processus, la fonction retourne les informations de répertoire approprié pour la dernière version du runtime installée sur l’ordinateur.  
  
 `cchBuffer`  
 [in] La taille, en octets, de `pbuffer`.  
  
 `dwLength`  
 [out] Le nombre de caractères retournés dans `pbuffer`.  
  
## <a name="remarks"></a>Notes  
  
> [!CAUTION]
>  N’utilisez pas cette fonction dans les processus qui exécutent la version 4 du CLR. Si une version antérieure du CLR est installée sur l’ordinateur, cette fonction retourne le répertoire d’installation pour cette version.  
  
## <a name="requirements"></a>Configuration requise  
 **Plateformes :** Consultez [Configuration requise](../../../../docs/framework/get-started/system-requirements.md).  
  
 **En-tête :** MSCorEE.h  
  
 **Bibliothèque :** MSCorEE.dll  
  
 **Versions du .NET Framework :** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>Voir aussi

- [Fonctions d’hébergement CLR dépréciées](../../../../docs/framework/unmanaged-api/hosting/deprecated-clr-hosting-functions.md)
