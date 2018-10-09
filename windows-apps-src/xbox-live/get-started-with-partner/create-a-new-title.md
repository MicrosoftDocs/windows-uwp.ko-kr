---
title: 새 타이틀 만들기
author: KevinAsgari
description: Windows 개발자 센터 UDC (유니버설)를 사용 하 여 Xbox Live에 대 한 새 타이틀을 만드는 방법을 알아봅니다.
ms.assetid: b8bd69e6-887a-4b1f-a42d-8affdbec0234
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 72126b51c4e155babad6cfee737e21d6102b03e5
ms.sourcegitcommit: 49aab071aa2bd88f1c165438ee7e5c854b3e4f61
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/09/2018
ms.locfileid: "4465264"
---
# <a name="create-a-new-title-for-xbox-live"></a>Xbox Live에 대 한 새 타이틀 만들기

## <a name="introduction"></a>소개

코드를 작성 하기 전에 새 제목을 서비스 구성 포털에 설치 해야 합니다.  [Xbox Live 서비스 구성](../xbox-live-service-configuration.md) 에서 서비스 구성에 대해 자세히 알아볼 수 있습니다.

이 문서에서는 다음을 가정을 사용 하 여이 프로세스를 설명 됩니다.

1. 유니버설 Windows 플랫폼 (UWP) 타이틀을 개발 하는 합니다.  Xbox One, Windows 10 데스크톱 Pc 및 모바일에 UWP 타이틀이 실행
2. 타이틀을 구성 하는 Windows 개발자 센터에서 [http://dev.windows.com/](http://dev.windows.com).  잘 모르겠으면, Windows 개발자 센터를 사용 해야 합니다.
3. Visual Studio를 사용 하는 사용자 지정 게임 엔진은 Unity와 합니다.
4. 개발 컴퓨터는 Windows 10을 실행 합니다.

해당 위의 않는 경우이 문서의 나머지 부분에서는 Windows 개발자 센터, 새 프로젝트를 만든 및 Xbox Live 로그인 코드 작성 되어 테스트 된에 구성 된 제목 하는 데 필요한 모든 안내 합니다.

> [!NOTE]
> Xbox Live 크리에이터 스 프로그램의 일부인 경우 위의 가정 하에 적용 하 고이 문서와 함께 따라야 합니다.

## <a name="dev-center-setup"></a>개발자 센터 설치

모든 Xbox Live 기능 작업을 필수 조건으로 [Windows 개발자 센터](http://dev.windows.com) 에서 생성 하는 Xbox Live가 지원 제목이 필요 합니다.

### <a name="create-a-microsoft-account"></a>Microsoft 계정 만들기
처음에 하나씩 만들 해야 Microsoft 계정 (MSA 라고도)를 설정 하지 않은 경우 [https://go.microsoft.com/fwlink/p/?LinkID=254486](https://go.microsoft.com/fwlink/p/?LinkID=254486).  Office 365 계정이, Outlook.com, 사용 또는 Xbox Live 계정을-MSA 이미 있어야 합니다.

### <a name="register-as-an-app-developer"></a>앱 개발자로 등록
개발자 센터에서 새 타이틀을 만들 수 앱 개발자로 등록 해야 합니다.

Go 등록을 https://developer.microsoft.com/en-us/store/register 등록 프로세스를 따릅니다.

### <a name="create-a-new-uwp-title"></a>새 UWP 타이틀 만들기
그런 다음 개발자 센터에서 정의 하는 UWP 제목을 해야 합니다.  이렇게 하면 첫 번째 진행 하 여 대시보드에

![](../images/getting_started/first_xbltitle_dashboard.png)

<p>
</p>
<br>
<p>
</p>

대시보드를 클릭 한 후 새 타이틀을 만듭니다.  이름을 예약 해야 합니다.

![](../images/getting_started/first_xbltitle_newapp.png)

그런 다음 앱에 대 한 *앱 개요* 페이지로 이동 됩니다.  여기서 하면 합니다 구성할 Xbox Live 기본 페이지는 서비스에는 아래 표시 된 Xbox Live 메뉴-> 합니다.

![](../images/getting_started/first_xbltitle_leftnav.png)

<div id="createxblaccount"></div>

## <a name="create-an-xbox-live-account"></a>Xbox Live 계정 만들기
Xbox Live 계정에 로그인 Xbox Live를 해야 합니다.  이미 있는 경우 Windows 10의 Xbox 앱 또는 Xbox One 본체에 로그인을 사용 하는 하나를 하나를 사용할 수 있습니다.

그렇지 않은 경우 Microsoft 계정으로 PC 및 로그인에 Xbox 앱을 열어야 합니다.  Xbox live 사용 하기 위해 다음 활성화 됩니다.

*시작 메뉴* 및 아래와 같이 "Xbox"를 입력 하 여 Xbox 앱을 찾을 수 있습니다.

![](../images/getting_started/first_xbltitle_xboxapp.png)

## <a name="next-steps"></a>다음 단계
만든 새 타이틀 했으므로 이제 게임 엔진, Visual Studio 또는 선택한 빌드 환경에는 Xbox Live가 지원 타이틀을 설정할 수 있습니다.

[Xbox Live를 통합 하는 데 단계별 가이드](partners-step-by-step-guide.md) 를 참조 하세요.
