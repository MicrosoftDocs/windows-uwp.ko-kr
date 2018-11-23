---
title: 파트너 센터에 대 한 single sign-on 구성
author: KevinAsgari
description: 구성 하는 방법 대 한 single sign-on 제목을 사용자를 자신의 Xbox Live id입니다.를 사용 하 여 서비스에 로그인 할 수 있도록 파트너 센터에서 설명 합니다.
ms.assetid: ''
ms.author: kevinasg
ms.date: 02/21/2018
ms.topic: article
ms.localizationpriority: medium
keywords: xbox live, xbox, 게임, uwp, windows 10, 하나는 xbox, udc, 유니버설 개발자 센터, 대 한 single sign-on
ms.openlocfilehash: 01714289ff12d096a17c5474f6a7619680481aa3
ms.sourcegitcommit: 93c0a60cf531c7d9fe7b00e7cf78df86906f9d6e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/23/2018
ms.locfileid: "7568247"
---
# <a name="configure-single-sign-on-in-partner-center"></a>파트너 센터에 대 한 single sign-on 구성

대 한 single sign-on 타이틀을 사용 하 여 자신의 Xbox Live 로그인 사용 하 여 서비스에 로그인 하는 플레이어 수 있습니다. Xbox Live 서비스에 특정 다른 계정 자격 증명을 사용 하 여 한 번 로그인 할 필요 없이 앱 또는 서비스에 대 한 게임 실행에 서명 하는 플레이어 수 있습니다.

> [!NOTE]
> 이 항목에서는 Xbox Live 크리에이터 스 프로그램 제목에 적용 되지 않습니다.

예를 들어 타이틀으로 서비스를 사용 하 여 유효한 계정을 있는 콘텐츠 (동영상, 음악, 등)가 장치에 서비스를 활성화 하는 앱을 수 있습니다. 사용자가 자신의 Xbox Live 계정에 로그인을 하는 경우 이러한 있어야 콘텐츠 때마다 서비스 로그인 할 필요 없이 합니다.

또한 경우 Kinect 데이터 외부 서비스를 전송 하는 앱을 구성할 수 있습니다는 다음과 같습니다.

사용자가 자신의 Xbox Live 계정으로 서비스에 대 한 계정과 연결 하는 방법을 제공 해야 서비스는 사용자 계정을 만듭니다 Xbox Live에서 별도 데이터에 필요한 경우 작업 시간입니다.

대 한 single sign-on 구성 Url과의 신뢰 당사자를 지정할 수 있습니다. 앱을 지정 하는 Url 중 하나를 호출 하는 언제 든 지 Xbox Live Xbox 보안 토큰 서비스 (XSTS) 토큰을 자동으로 첨부 하세요 됩니다. 서비스 키를 수신 하는 *신뢰 당사자*를 이라고 하 고 대 한 single sign-on을 구성 하려면 먼저 [신뢰 당사자를](https://developer.microsoft.com/en-US/xboxconfig/relyingparties/index) 구성 해야 합니다. 각 신뢰 당사자 구성 당사자 XSTS 토큰을 디코딩하는 데 사용할 수 있는 고유한 암호화 키 뿐만 아니라 XSTS 토큰에 포함 된 정보를 지정 합니다.

다음을 수행 하 여 구성을 추가 합니다.

1. **서비스**에 이동 타이틀 [파트너 센터](https://partner.microsoft.com/dashboard)에서 선택한 후 > **Xbox Live**합니다.

2. **Xbox Live 대 한 single sign-on에**대 한 링크를 클릭 합니다.

3. 항목을 만드는 새로운 단일 로그온 **URL 추가** 단추를 클릭 합니다. 이렇게 하면 새 행 구성의 목록 맨 아래에 추가 됩니다.

4. URL 상자에 정규화 된 도메인 이름을 사용 하 여 서비스에 대 한 URL을 입력 합니다. 와일드 카드 문자를 사용 하 여 낮은 수준의 하위 도메인을 대체할 수 있습니다 ('\ *'). 이 동일한 상위 수준 도메인에 있는 모든 URL 일치 합니다. 예를 들어, "*. example.com&quot; "bar.example.com"또는"foo.bar.example.com"일치 합니다.

5. Relying 타사 상자에 XSTS 토큰 인코딩 되는 방식을 지정 하는 신뢰 당사자 구성을 선택 합니다.

6. 변경 내용을 저장 하 여 **저장** 단추를 클릭 합니다.

![단일 로그온 구성 페이지의 스크린샷](../../images/dev-center/single-signon.png)
