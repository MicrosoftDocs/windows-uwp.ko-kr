---
title: XDK 프로젝트에 Xbox Live 추가
author: KevinAsgari
description: 새로운 또는 기존 Xbox 개발자 키트 (XDK) 프로젝트에 Xbox Live를 추가 하는 방법을 알아봅니다.
ms.assetid: fc6f987c-1a87-4ff5-b063-891591aa6653
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, 하나는 xbox, xdk
ms.localizationpriority: medium
ms.openlocfilehash: 5563966c6f877bf02b5e58751173e6c425b25bfa
ms.sourcegitcommit: e6daa7ff878f2f0c7015aca9787e7f2730abcfbf
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/04/2018
ms.locfileid: "4339771"
---
# <a name="add-xbox-live-to-a-new-or-existing-xdk-project"></a>새로운 또는 기존 XDK 프로젝트에 Xbox Live 추가

이 항목에서는 Xbox Live 새로운 또는 기존 XDK 프로젝트에 추가 하는 방법을 설명 합니다.

프로세스는.

- Xbox One 개발 환경 구성 설정
- 사용자의 Id 가져오기
- 개발 콘솔 구성
- TitleID 및 서비스 안내 하려면 이진 파일에 추가


## <a name="setup-up-your-xbox-one-development-environment"></a>Xbox One 개발 환경 구성 설정
먼저 XDK 설명서에서 "Xbox One 개발 환경 설정을" 섹션에 따라 콘솔 설정

## <a name="get-your-ids"></a>사용자의 Id 가져오기

Xbox Live 서비스를 사용 하려면 개발 키트 및 타이틀을 구성 하는 몇 가지 Id를 가져오는 해야 합니다. 이러한 동일한 프로세스를 사용 하 여 수행할 수 있습니다.

[Xbox Live 서비스 구성](../xbox-live-service-configuration.md) 하는 프로세스를 수행 하 여 사용자 Id를 가져오는 됩니다.

## <a name="configure-your-development-console"></a>개발 콘솔 구성

사용자 Id가 있으면 개발 콘솔을 설치 하려면 [개발 콘솔 구성](configure-your-development-console.md) 가이드를 따릅니다.

## <a name="add-the-titleid-and-scid-to-your-binary"></a>TitleID 및 서비스 안내 하려면 이진 파일에 추가
플랫폼 수준에서 각 개발 키트에 대 한 샌드박스를 구성 하는 동안 TitleID 및 서비스 안내 특정 이진에 바인딩됩니다. TitleID 및 서비스 안내 하려면 이진 파일을 추가 하려면 해당 이진 파일에 대 한 Package.appxmanifest에서 새 노드를 추가 하 여 수정 합니다 <Extensions> 노드 아래와 같이:

```
<Applications>
    ...
    <Application ...>
      ...
      <Extensions>
        <mx:Extension Category="xbox.live">
           <mx:XboxLive TitleId="<your titleID>" PrimaryServiceConfigId="<your SCID>" RequireXboxLive="<boolean indicating Live requirement>" />
        </mx:Extension>
      </Extensions>
   </Application>
</Applications>
```

AppxManifest.xml 파일에 대 한 자세한 내용은 Xbox One 개발에 대 한 Visual Studio에서 프로젝트 템플릿을 참조 합니다.

응용 프로그램 매니페스트 스키마에 대 한 설명은 응용 프로그램 매니페스트 스키마 참조 하세요.

**RequireXboxLive 플래그** Windows.Networking.Connectivity 연결 수준 하지 않는 한 플래그가 제목을 true로 설정 되어 RequireXboxLive 시작 되지 않는 XboxLiveAccess로 반환 하 고 제목 인증 Xbox live를 지웁니다. 그러면 제목 콘텐츠 최신 업데이트를 수행 했습니다. 제목을 실행 되는 동안 연결이 끊기면, 제목 일시 중단 됩니다.

유일한 "인터넷 필수" 제목 true 및 타이틀이 방식으로 표시 되지는지 않습니다 들이 제목에 대 한 필수 서비스는 실행 RequireXboxLive 표시 해야 합니다.
