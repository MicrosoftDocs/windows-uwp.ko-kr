---
title: 만들고 새 작성자 제목을 테스트합니다
description: 새 Xbox Live 크리에이터 스 프로그램 제목을 작성 및 테스트 환경에 게시 하는 방법을 알아봅니다.
ms.assetid: ced4d708-e8c0-4b69-aad0-e953bfdacbbf
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, 하나는 xbox, 작성자, 테스트
ms.localizationpriority: medium
ms.openlocfilehash: 5b39a2ed67d2fe77fa8904408b81a329125d73c7
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57594598"
---
# <a name="create-a-new-xbox-live-creators-program-title-and-publish-to-the-test-environment"></a>새 Xbox Live 크리에이터 스 프로그램 제목을 만들기 및 테스트 환경에 게시

## <a name="introduction"></a>소개

Xbox Live 코드를 작성 하기 전에 서비스 구성 포털의 새 제목을 설정 해야 합니다.  서비스 구성에 대 한 자세히 알아볼 수 있습니다 [Xbox Live 서비스 구성을](../xbox-live-service-configuration.md)합니다.

이 문서에 구성 된 제목을 하는 데 필요한 모든 안내 [파트너 센터](https://partner.microsoft.com/dashboard), 새 프로젝트를 만들고 테스트용 Xbox Live를 준비 합니다. 이 문서에서는 다음을 가정합니다.

1. Xbox Live 크리에이터 스 프로그램을 사용 하는
2. 유니버설 Windows 플랫폼 (UWP) title을 개발 하는 합니다.  UWP 제목 Xbox One, Windows 10 데스크톱 Pc 및 모바일에서 실행할 수 있습니다.
3. 제목에서 구성한 [파트너 센터](https://partner.microsoft.com/dashboard)합니다.
4. 개발 컴퓨터에 Windows 10을 실행 합니다.

> [!NOTE]
> 위의 가정 적용 하며이 기사의 따라야 Xbox Live 크리에이터 스 프로그램의 일부인 경우

## <a name="partner-center-setup"></a>파트너 센터 설치

생성 하는 Xbox Live를 사용 하도록 설정 제목 해야 [파트너 센터](https://partner.microsoft.com/dashboard) Xbox Live 기능을 사용 하 여 필수 조건으로 합니다.

### <a name="create-a-microsoft-account"></a>Microsoft 계정 만들기
에 하나씩 만들어야 할 경우 Microsoft 계정 (MSA 라고도)가 없는 [로그인 Microsoft 계정-](https://go.microsoft.com/fwlink/p/?LinkID=254486)합니다. Office 365 계정이, Outlook.com, 사용 또는-Xbox Live 계정이 이미 있으면 MSA입니다.

### <a name="register-as-an-app-developer"></a>앱 개발자로 등록
파트너 센터에서 새 제목을 만들려면 먼저 앱 개발자로 등록 해야 합니다.

를 등록 하려면로 이동 [앱 개발자로 등록](https://developer.microsoft.com/store/register) 등록 프로세스를 수행 합니다.

### <a name="create-a-new-uwp-title"></a>새 UWP 제목 만들기
UWP 제목을 파트너 센터에서 정의 해야 합니다. 첫 번째 이동 하 여 이렇게 [파트너 센터](https://partner.microsoft.com/dashboard)합니다.

다음으로 새 제목을 만듭니다. 이름을 예약 하는 것이 필요 합니다.

![](../images/getting_started/first_xbltitle_newapp.png)

스크린샷에서 기본 페이지를 구성할 것 Xbox Live, 아래에 **Services** > **Xbox Live** 메뉴.

![](../images/creators_udc/creators_udc_xboxlive_page.png)

## <a name="enable-xbox-live-services"></a>Xbox Live 서비스를 사용 하도록 설정
클릭할 때 합니다 **Xbox Live** 링크 **Services** 제품에 대 한 처음 있습니다 됩니다 수 사용 Xbox Live 크리에이터 스 프로그램 페이지로 돌아갑니다.  를 클릭 합니다 **사용 하도록 설정** 단추 Xbox Live 설치 대화 상자를 표시 합니다.

![](../images/creators_udc/creators_udc_xboxlive_enable.png)

설치 대화 상자에서 Xbox Live Services를 사용 하도록 하려는 플랫폼을 선택 합니다 (Xbox One 및 Windows 10 PC를 모두 선택한 경우 기본적으로).  클릭 합니다 **확인** 단추 게임에 Xbox Live 크리에이터 스 프로그램을 사용 하도록 설정 합니다.

> [!IMPORTANT]
> Xbox Live 게임 에서만 지원 됩니다. Xbox Live를 사용한 게임을 게시 하려면 제출의 제품 유형 속성 섹션에 "Game"를 설정 해야 합니다.

![](../images/creators_udc/creators_udc_xboxlive_enable_dialog.png)

## <a name="test-xbox-live-service-configuration-in-your-game"></a>서비스 구성 Xbox Live 게임 테스트
게임에 대 한 변경 내용을 Xbox Live 구성 하는 경우에 Xbox Live의 나머지 부분에서 선택 하 고 게임에서 확인할 수 있습니다 하려면 먼저 특정 환경에 변경 내용을 게시 해야 합니다.

게임에서 계속 작업 하는 경우 사용자 고유의 개발 샌드박스에 게시 합니다.  개발 샌드박스를 사용 하면 격리 된 환경에서 게임에 변경 내용에 작업할 수 있습니다.

게임은 공개적으로 릴리스되면 Xbox Live configuration 소매점 샌드박스에 자동으로 게시 됩니다.

기본적으로 하나의 콘솔 Xbox 및 Windows 10 Pc의에서 경우 소매 샌드박스

### <a name="publish-xbox-live-configuration-to-the-test-environment"></a>Xbox Live 구성 테스트 환경에 게시

Xbox Live 서비스를 사용 하도록 설정 하 고 Xbox Live 서비스 구성을 변경 될 때마다 변경 내용을 적용 되도록 해야 개발 샌드박스에 이러한 변경 내용을 게시 합니다.

Xbox Live 구성 페이지에서 클릭 합니다 **테스트** 개발 샌드박스에 현재 Xbox Live 구성을 게시 하는 단추입니다.

![](../images/creators_udc/creators_udc_xboxlive_config_test.png)

### <a name="authorize-devices-and-users-for-the-development-sandbox"></a>장치 및 개발 샌드박스에 대 한 사용자 권한 부여

승인 된 장치 및 사용자만 게임 개발 샌드박스에 Xbox Live 구성에 액세스할 수 있습니다.

기본적으로 모든 Xbox One 개발 콘솔 파트너 센터 계정에 추가한 개발 샌드박스에 대 한 액세스를 적용 합니다.  Xbox One을 콘솔에 추가 하려면로 이동 [Xbox One 관리 콘솔](https://partner.microsoft.com/xboxconfig/devices)합니다. 파트너 센터 계정에 이미 있는 경우로 이동할 수 있습니다 **계정 설정** > **계정 설정** > **개발 장치**  >  **Xbox One 개발 콘솔**합니다.

또한 개발 샌드박스에 액세스할 수 있도록 일반 Xbox Live 계정 권한을 부여할 수 있습니다.  Xbox Live 계정에 권한을 부여 개발 샌드박스에로 이동 [계정 관리](https://developer.microsoft.com/xboxtestaccounts/configurecreators)합니다.

## <a name="next-steps"></a>다음 단계
만든 새 제목을 설정 했으므로 이제는 Xbox Live를 사용 하도록 설정 제목 게임 엔진, Visual Studio 또는 선택한 빌드 환경에서을 설정할 수 있습니다.

참조 [Xbox Live를 통합 하 단계별 가이드](creators-step-by-step-guide.md)
