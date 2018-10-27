---
title: 만들고 새 크리에이터 스 타이틀 테스트
author: KevinAsgari
description: 새 Xbox Live 크리에이터 스 프로그램 타이틀 만들기 및 테스트 환경에 게시 하는 방법을 알아봅니다.
ms.assetid: ced4d708-e8c0-4b69-aad0-e953bfdacbbf
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, 하나는 xbox, 크리에이터 스, 테스트
ms.localizationpriority: medium
ms.openlocfilehash: 1e51364ee87ee592420c88ac5808d24d010cfa25
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/27/2018
ms.locfileid: "5686645"
---
# <a name="create-a-new-xbox-live-creators-program-title-and-publish-to-the-test-environment"></a>새 Xbox Live 크리에이터 스 프로그램 타이틀 만들기 및 테스트 환경에 게시

## <a name="introduction"></a>소개

Xbox Live 코드를 작성 하기 전에 서비스 구성 포털에 새 타이틀을 설정 해야 합니다.  [Xbox Live 서비스 구성](../xbox-live-service-configuration.md)에서 서비스 구성에 대해 자세히 알아볼 수 있습니다.

이 문서는 Windows 개발자 센터에서 구성 하는 제목, 생성, 새 프로젝트 및 Xbox Live 테스트를 위해 준비 하는 데 필요한 모든 안내 합니다. 이 문서에서는 다음을 가정합니다.

1. Xbox Live 크리에이터 스 프로그램을 사용 하는 합니다.
2. 유니버설 Windows 플랫폼 (UWP) 타이틀을 개발 하는 합니다.  UWP 타이틀이 Xbox One, Windows 10 데스크톱 Pc 및 모바일에서 실행할 수 있습니다.
3. 타이틀을 구성 하는 Windows 개발자 센터에서 [http://dev.windows.com/](http://dev.windows.com).  잘 모르겠으면, Windows 개발자 센터를 사용 해야 합니다.
4. 개발 컴퓨터는 Windows 10을 실행 합니다.

> [!NOTE]
> Xbox Live 크리에이터 스 프로그램의 일부인 경우 위의 가정 하는 귀하에 게 적용 하 고이 문서와 함께 따라야 합니다.

## <a name="dev-center-setup"></a>개발자 센터 설치

모든 Xbox Live 기능을 사용 하 여 필수 조건으로 [Windows 개발자 센터](http://dev.windows.com) 에서 생성 하는 Xbox Live가 지원 제목이 필요 합니다.

### <a name="create-a-microsoft-account"></a>Microsoft 계정 만들기
Microsoft 계정 (MSA 라고도)를 설정 하지 않은 경우 먼저 [Microsoft 계정으로 로그인](https://go.microsoft.com/fwlink/p/?LinkID=254486)에서 새로 만들 해야 합니다. Office 365 계정이, Outlook.com, 사용 또는 Xbox Live 계정을-MSA 이미 있어야 합니다.

### <a name="register-as-an-app-developer"></a>앱 개발자로 등록
개발자 센터에서 새 타이틀 만들기 전에 앱 개발자로 등록 해야 합니다.

을 등록 하려면 [앱 개발자로 등록](https://developer.microsoft.com/store/register) 으로 이동 하 고 등록 프로세스를 수행 합니다.

### <a name="create-a-new-uwp-title"></a>새 UWP 타이틀 만들기
개발자 센터에 정의 된 UWP 제목을 할 수 있습니다. 이렇게 하면 첫 번째 진행 하 여 [Windows 개발자 센터 대시보드](https://developer.microsoft.com/dashboard/).

다음으로 새 타이틀을 만듭니다. 이름을 예약 해야 합니다.

![](../images/getting_started/first_xbltitle_newapp.png)

스크린샷 있는 있습니다 됩니다 구성할 Xbox Live **서비스**아래에 있는 기본 페이지 표시 > 메뉴**Xbox Live** 합니다.

![](../images/creators_udc/creators_udc_xboxlive_page.png)

## <a name="enable-xbox-live-services"></a>Xbox Live 서비스를 사용 하도록 설정
제품에 대 한 처음으로 **서비스** 에서 **Xbox Live** 링크를 클릭할 때 활성화 Xbox Live 크리에이터 스 프로그램 페이지로 이동 합니다.  Xbox Live 설정 대화 상자를 불러오는 데 **사용** 하는 단추를 클릭 합니다.

![](../images/creators_udc/creators_udc_xboxlive_enable.png)

설정 대화 상자에서 플랫폼에 대 한 Xbox Live 서비스를 사용 하도록 설정 하려는 선택 (Xbox One 및 Windows 10 PC는 기본적으로 선택).  게임에 대 한 Xbox Live 크리에이터 스 프로그램을 사용 하도록 설정 하려면 **확인** 단추를 클릭 합니다.

> [!IMPORTANT]
> Xbox Live 게임에만 지원 됩니다. Xbox live 게임을 게시 하기 위해 제출의 제품 유형 속성 섹션에서 "게임"으로 설정 해야 합니다.

![](../images/creators_udc/creators_udc_xboxlive_enable_dialog.png)

## <a name="test-xbox-live-service-configuration-in-your-game"></a>게임에서 Xbox Live 서비스 구성 테스트
레코드를 게임에 대 한 Xbox Live 구성과 변경 하는 경우 Xbox Live의 나머지 부분에서 획득 게임에서 볼 수 전에 특정 환경에 변경 내용을 게시 해야 합니다.

게임에서 작업 하는 여전히 자체 개발 샌드박스를 게시 합니다.  개발 샌드박스를 사용 하면 격리 된 환경에서 게임에 대 한 변경 내용에 작업할 수 있습니다.

공개적으로 게임 릴리스된 Xbox Live 구성과 소매 샌드박스에 자동으로 게시 됩니다.

기본적으로 Windows 10 Pc 및 Xbox One 본체 소매 샌드박스에서 됩니다.

### <a name="publish-xbox-live-configuration-to-the-test-environment"></a>Xbox Live 구성을 테스트 환경에 게시

Xbox Live 서비스를 사용 하도록 설정 하 고 Xbox Live 서비스 구성을 변경할 때마다 변경을 적용, 하려면 개발 샌드박스를 이러한 변경 내용을 게시 합니다.

Xbox Live 구성 페이지에서 개발 샌드박스를 현재 Xbox Live 구성을 게시 하려면 **테스트** 단추를 클릭 합니다.

![](../images/creators_udc/creators_udc_xboxlive_config_test.png)

### <a name="authorize-devices-and-users-for-the-development-sandbox"></a>장치 및 개발 샌드박스에 대 한 사용자에 게 권한을 부여합니다

승인 된 장치와 사용자가 개발 샌드박스 내에서 게임에 대 한 Xbox Live 구성과 액세스할 수 있습니다.

기본적으로 모든 Xbox One 개발 콘솔 개발자 센터 계정에 추가한는 개발 샌드박스에 액세스할을 수 있습니다.  Xbox One 콘솔에 추가 하려면 [Xbox One 본체 관리](https://partner.microsoft.com/XboxDevices)으로 이동 합니다. 개발자 센터 계정에 이미 있는 경우 **계정 설정**으로 이동할 수 있습니다 > **계정 설정** > **개발자 장치** > **Xbox One 개발 콘솔**.

또한 개발 샌드박스에 액세스할 수 있는 일반 Xbox Live 계정 권한을 부여할 수 있습니다.  개발 샌드박스를 Xbox Live 계정 액세스 권한을 부여, [계정 관리](https://developer.microsoft.com/xboxtestaccounts/configurecreators)로 이동 합니다.

## <a name="next-steps"></a>다음 단계
만든 새 제목을 했으므로 이제 게임 엔진, Visual Studio 또는 선택한 빌드 환경에는 Xbox Live가 지원 타이틀을 설정할 수 있습니다.

[Xbox Live를 통합 하는 단계별 가이드](creators-step-by-step-guide.md) 를 참조 하세요.
