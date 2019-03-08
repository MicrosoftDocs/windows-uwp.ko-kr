---
title: 멀티 플레이어 게임에 AppXManifest 구성
description: 멀티 플레이 초대 Xbox Live를 사용 하도록 설정 하 여 UWP AppXManifest를 구성 하는 방법에 알아봅니다.
ms.assetid: 72f179e7-4705-4161-9b8a-4d6a1a05b8f7
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, 하나는 xbox, 프로토콜 활성화, 멀티 플레이 게임
ms.localizationpriority: medium
ms.openlocfilehash: 13b04a86fdc4e4f661dd1c181dda7d9c9e4c1c8a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57646028"
---
# <a name="configure-your-appxmanifest-for-multiplayer"></a>멀티 플레이어 게임에 AppXManifest 구성

다음 조건이 true 인 경우 Visual Studio 프로젝트에서.appxmanifest 파일에 일부 업데이트를 수행 해야 합니다.
- UWP 개발 하는
- 플레이어에 제목에 다른 사용자를 초대할 수 있는 기능을 구현.

이 단계를 수행 하지 않으면 제목 프로토콜 재생을 초대를 수락 하는 받는 사람 플레이어 경우 활성화를 받지 못합니다.

## <a name="open-your-packageappxmanifest"></a>프로그램 Package.appxmanifest를 열으십시오

Package.appxmanifest 파일은 일반적으로 Visual Studio 프로젝트의 솔루션 파일과 동일한 디렉터리에 있습니다.  또는 솔루션 탐색기에서 찾을 수 있습니다.

![](../../images/multiplayer/multiplayer_open_appxmanifest.png)

## <a name="add-new-entry"></a>새 항목 추가

다음을 추가 해야 합니다 ```<Extensions>``` 요소 아래에 있는 ```<Applications>``` Package.appxmanifest 파일에서

```
<Extensions>
  <uap:Extension Category="windows.protocol">
    <uap:Protocol Name="ms-xbl-multiplayer" />
  </uap:Extension>
</Extensions>
```

예:

![](../../images/multiplayer/multiplayer_appxmanifest_changes.png)

저장 하 고 제목을 빌드하십시오.  멀티 플레이 게임 관리자를 사용 하 여 플레이어에 제목에 초대 하는 기능을 구현 하는 방법에 알아보려면 하세요 [친구와 멀티 플레이 게임을 재생](../multiplayer-manager/play-multiplayer-with-friends.md)
