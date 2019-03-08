---
title: Xbox Live XDK 프로젝트에 추가
description: 새 또는 기존 Xbox 개발자 키트 (XDK) 프로젝트에 Xbox Live를 추가 하는 방법에 알아봅니다.
ms.assetid: fc6f987c-1a87-4ff5-b063-891591aa6653
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, 하나는 xbox, xdk
ms.localizationpriority: medium
ms.openlocfilehash: f17765b09dcb0b6f5c89d168f2d3d9611a60ffa6
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57649048"
---
# <a name="add-xbox-live-to-a-new-or-existing-xdk-project"></a>새 또는 기존 XDK 프로젝트에 Xbox Live를 추가 합니다.

이 항목에서는 기존 또는 새 XDK 프로젝트에 Xbox Live를 추가 하는 방법을 간략하게 설명 합니다.

프로세스는.

- Xbox One 개발 환경 설정
- 사용자 Id를 가져오려면
- 개발 콘솔 구성
- TitleID 및 서비스 안내 이진 파일에 추가


## <a name="setup-up-your-xbox-one-development-environment"></a>Xbox One 개발 환경 설정
먼저 콘솔 XDK 설명서에서 "Xbox 하나의 개발 환경 설정을" 섹션을 따라 설치

## <a name="get-your-ids"></a>사용자 Id를 가져오려면

Xbox Live 서비스를 사용 하려면 개발 키트 및 제목을 구성 하려면 여러 Id를 가져오는 해야 합니다. 이러한 동일한 프로세스를 사용 하 여 수행할 수 있습니다.

프로세스를 수행 하 여 사용자 Id를 가져오는 [Xbox Live 서비스 구성](../xbox-live-service-configuration.md)

## <a name="configure-your-development-console"></a>개발 콘솔 구성

사용자 Id를 만든 후에 따라 합니다 [개발 콘솔 구성](configure-your-development-console.md) 개발 콘솔을 설치 하는 가이드입니다.

## <a name="add-the-titleid-and-scid-to-your-binary"></a>TitleID 및 서비스 안내 이진 파일에 추가
플랫폼 수준에서 각 개발 키트에 대 한 샌드박스를 구성할 서비스 안내 고 TitleID 특정 이진 파일에 바인딩됩니다. TitleID 및 서비스 안내를 위해 이진 파일에 추가 하려면 해당 이진 파일에 대해 Package.appxmanifest에 새 노드를 추가 하 여 수정 된 <Extensions> 다음과 같은 노드:

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

AppxManifest.xml 파일에 대 한 자세한 내용은 Xbox 개발에 대 한 Visual Studio에서 프로젝트 템플릿을 참조 합니다.

응용 프로그램 매니페스트 스키마에 대 한 설명은 응용 프로그램 매니페스트 스키마를 참조 하세요.

**RequireXboxLive 플래그** 플래그가 제목 true로 설정 되어 RequireXboxLive XboxLiveAccess Windows.Networking.Connectivity 연결 수준을 반환 하 고 제목 지웁니다 Xbox Live를 사용한 인증 하지 않으면 시작 되지 않는 경우. 이렇게 하면 제목에는 최신 콘텐츠 업데이트를 수행 했습니다. 제목 실행 되는 동안 연결이 끊어지면, 제목 일시 중단 됩니다.

True는 방식에서에 제목 표시 되지 않을 제목에 필요한 서비스가 실행 중 및 실행만 "인터넷 Required" 제목 RequireXboxLive를 표시 해야 합니다.
