---
title: 파트너 센터에서 single sign on 구성
description: 구성 하는 방법-single sign-on 제목을 Xbox Live ID를 사용 하 여 서비스에 사용자를 로그인 할 수 있도록 파트너 센터에서 설명 합니다.
ms.assetid: ''
ms.date: 02/21/2018
ms.topic: article
ms.localizationpriority: medium
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox 하나, udc, 유니버설 개발자 센터, single sign on
ms.openlocfilehash: 32f06edd407d8c1fa74795d0a230c7d56ba8e838
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57632878"
---
# <a name="configure-single-sign-on-in-partner-center"></a>파트너 센터에서 single sign on 구성

Single sign on을 제목을 사용 하 여 Xbox Live 로그인 사용 하 여 서비스에 로그인 하는 플레이어 수 있습니다. 이렇게 하면 Xbox Live 서비스에 특정 다른 계정 자격 증명을 사용 하 여 두 번 로그인 할 필요 없이 응용 프로그램 또는 서비스에 대 한 게임 실행에 서명한 플레이어가 있습니다.

> [!NOTE]
> 이 항목에서는 Xbox Live 크리에이터 스 프로그램 제목에 적용 되지 않습니다.

예를 들어을 제목으로 서비스를 사용 하 여 유효한 계정이 해당 콘텐츠를 스트림할 (비디오, 음악 등)는 장치에 서비스를 사용 하도록 설정 하는 앱을 수 있습니다. 사용자가 Xbox Live 계정에 로그인 하는 경우 이러한 있어야 콘텐츠를 스트림할 때마다 서비스에 로그인 하지 않고도 합니다.

또한 외부 서비스에 Kinect 데이터를 전송 하는 앱을 구성할 수 있습니다는 같습니다.

Xbox Live 계정으로 서비스에서 해당 계정과 연결 하려면 사용자에 대 한 방법을 제공 해야 서비스에는 사용자 계정을 만들고 별도의 Xbox Live가 필요한 경우 일회성 작업입니다.

Single sign on를 구성한 경우 Url 및 해당 신뢰 당사자를 지정할 수 있습니다. 앱을 지정 된 Url 중 하나를 호출 하는 언제 든 지 Xbox Live는 자동으로 연결 Xbox 보안 토큰 서비스 (XSTS) 토큰을 합니다. 키를 받는 서비스 라고 한 *신뢰 당사자*를 구성 해야 합니다 [신뢰 당사자](https://developer.microsoft.com/en-US/xboxconfig/relyingparties/index) 에서 single sign-on을 구성 하려면. 신뢰 당사자 구성이 각 신뢰 당사자를 XSTS 토큰 디코딩하는 데 사용할 수 있는 고유한 암호화 키를 비롯 하 여 XSTS 토큰에 포함 된 정보를 지정 합니다.

다음을 수행 하 여 구성을 추가 합니다.

1. 제목을 선택한 후 [파트너 센터](https://partner.microsoft.com/dashboard), 이동할 **Services** > **Xbox Live**합니다.

2. 링크를 클릭 **Xbox Live에서 single sign-on**합니다.

3. 클릭 합니다 **URL 추가** 단추를 새 항목을 만들려면 단일 로그온 합니다. 이렇게 하면 구성 목록 맨 아래에 새 행을 추가 됩니다.

4. URL 상자에 정규화 된 도메인 이름을 사용 하 여 서비스에 대 한 URL을 입력 합니다. 가장 낮은 수준의 하위 도메인 와일드 카드 문자를 사용 하 여 바꿀 수 있습니다 ('\*'). 이 동일한 상위 수준 도메인에 있는 모든 URL 일치 합니다. 예를 들어, "*. example.com&quot; "bar.example.com"또는"foo.bar.example.com"와 일치 합니다.

5. 신뢰 당사자 상자에서 XSTS 토큰 인코딩 되는 방법을 지정 하는 신뢰 당사자 구성이 선택 합니다.

6. 클릭 합니다 **저장** 변경 내용을 저장 하는 단추입니다.

![Single sign-on 구성 페이지의 스크린샷](../../images/dev-center/single-signon.png)
