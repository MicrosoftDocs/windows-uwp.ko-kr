---
title: 멀티 플레이어에 AppXManifest 구성
description: Xbox Live 멀티 플레이 초대를 사용 하 여 UWP AppXManifest 구성 하는 방법을 알아봅니다.
ms.assetid: 72f179e7-4705-4161-9b8a-4d6a1a05b8f7
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, 하나는 xbox, 프로토콜 활성화, 멀티 플레이어
ms.localizationpriority: medium
ms.openlocfilehash: 13b04a86fdc4e4f661dd1c181dda7d9c9e4c1c8a
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/11/2018
ms.locfileid: "8897118"
---
# <a name="configure-your-appxmanifest-for-multiplayer"></a>멀티 플레이어에 AppXManifest 구성

다음과 같은 경우 Visual Studio 프로젝트의.appxmanifest 파일에 일부 업데이트를 확인 해야 합니다.
- UWP 개발 하는
- 타이틀을 다른 사용자를 초대 하 여 플레이어에 대 한 기능을 구현.

이 단계를 수행 하지 않는 경우 타이틀 프로토콜 받는 사람 플레이어 재생 하 라는 초대를 수락 하면 활성화를 가져오지 않습니다.

## <a name="open-your-packageappxmanifest"></a>에 Package.appxmanifest 열기

Package.appxmanifest 파일에는 일반적으로 Visual Studio 프로젝트의 솔루션 파일로 동일한 디렉터리에 배치 됩니다.  또는 솔루션 탐색기에서 찾을 수 있습니다.

![](../../images/multiplayer/multiplayer_open_appxmanifest.png)

## <a name="add-new-entry"></a>새 항목 추가

다음을 추가 해야 합니다 ```<Extensions>``` 요소 아래에서 ```<Applications>``` Package.appxmanifest 파일에

```
<Extensions>
  <uap:Extension Category="windows.protocol">
    <uap:Protocol Name="ms-xbl-multiplayer" />
  </uap:Extension>
</Extensions>
```

예::

![](../../images/multiplayer/multiplayer_appxmanifest_changes.png)

저장 하 고 타이틀을 다시 빌드하십시오.  타이틀을 플레이어를 초대 하는 기능을 구현 하는 멀티 플레이어 관리자를 사용 하는 방법에 자세한 참조 [친구와 멀티 플레이어 재생](../multiplayer-manager/play-multiplayer-with-friends.md)
