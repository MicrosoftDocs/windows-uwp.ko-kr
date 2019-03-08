---
title: 새 제목 만들기
description: 파트너 센터를 사용 하 여 Xbox Live에 대 한 새 제목을 만드는 방법에 알아봅니다.
ms.assetid: b8bd69e6-887a-4b1f-a42d-8affdbec0234
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 1aa2447a2044bec9b2013b30c05e45342b763fc3
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57656568"
---
# <a name="create-a-new-title-for-xbox-live"></a>Xbox Live에 대 한 새 제목 만들기

## <a name="introduction"></a>소개

코드를 작성 하기 전에 서비스 구성 포털의 새 제목을 설정 해야 합니다.  서비스 구성에 대 한 자세히 알아볼 수 있습니다 [Xbox Live 서비스 구성](../xbox-live-service-configuration.md)

이 문서에서는 안내이 프로세스는 다음과 같은 가정 사용 하 여

1. 유니버설 Windows 플랫폼 (UWP) title을 개발 하는 합니다.  Xbox One, Windows 10 데스크톱 Pc 및 모바일에서 UWP 타이틀을 실행합니다.
2. 제목에서 구성한 [파트너 센터](https://partner.microsoft.com/dashboard)합니다.
3. Visual Studio를 사용 하 여 게임 엔진을 사용자 지정 또는 Unity를 사용 하 여 됩니다.
4. 개발 컴퓨터에 Windows 10을 실행 합니다.

위의 조건이 true는이 문서의 나머지 부분에서는 제목을 파트너 센터, 만든 새 프로젝트 및 Xbox Live 로그인 코드 작성 및 테스트 구성 하는 데 필요한 모든 안내 합니다.

> [!NOTE]
> Xbox Live 크리에이터 스 프로그램의 일부인 경우 위의 가정에 적용 하 고이 문서와 함께 수행 해야 합니다.

## <a name="partner-center-setup"></a>파트너 센터 설치

생성 하는 Xbox Live를 사용 하도록 설정 제목 해야 [파트너 센터](https://partner.microsoft.com/dashboard) 모든 Xbox Live 기능 작업에 필수 조건으로 합니다.

### <a name="create-a-microsoft-account"></a>Microsoft 계정 만들기
에 하나씩 만들어야 할 경우 Microsoft 계정 (MSA 라고도)가 없는 [ https://go.microsoft.com/fwlink/p/?LinkID=254486 ](https://go.microsoft.com/fwlink/p/?LinkID=254486)합니다.  Office 365 계정이, Outlook.com, 사용 또는-Xbox Live 계정이 이미 있으면 MSA입니다.

### <a name="register-as-an-app-developer"></a>앱 개발자로 등록
파트너 센터에서 새 제목을 만들을 수 전에 앱 개발자로 등록 해야 합니다.

Go를 등록 하려면에 https://developer.microsoft.com/en-us/store/register 등록 프로세스를 수행 합니다.

### <a name="create-a-new-uwp-title"></a>새 UWP 제목 만들기
그런 다음 UWP 제목을 파트너 센터에서 정의 해야 합니다.  이렇게 하면 첫 번째 이동 하 여 대시보드로

![](../images/getting_started/first_xbltitle_dashboard.png)

<p>
</p>
<br>
<p>
</p>

그런 다음 새 제목을 만듭니다.  이름을 예약 하는 것이 필요 합니다.

![](../images/getting_started/first_xbltitle_newapp.png)

있습니다 다음 이동 된 *앱 개요* 페이지 앱에 대 한 합니다.  여기서 구성할 것 Xbox Live 기본 페이지의 서비스-> 아래에 표시 하는 Xbox Live 메뉴.

![](../images/getting_started/first_xbltitle_leftnav.png)

<div id="createxblaccount"></div>

## <a name="create-an-xbox-live-account"></a>Xbox Live 계정 만들기
Xbox Live 계정을 Xbox Live에 로그인 해야 합니다.  에 이미 있는 Xbox One 콘솔, 또는 Windows 10의 Xbox 앱에서 로그인을 사용 하는 경우 하나를 사용할 수 있습니다.

그렇지 않은 경우 Microsoft 계정으로 PC와 로그인에서 Xbox 앱을 열어야 합니다.  Xbox Live를 사용한 사용에 대 한 다음 활성화 됩니다.

로 이동 하 여 Xbox 앱을 찾을 수 있습니다 합니다 *시작 메뉴* 아래와 같이 "Xbox"을 입력 합니다.

![](../images/getting_started/first_xbltitle_xboxapp.png)

## <a name="next-steps"></a>다음 단계
만든 새 제목을 설정 했으므로 이제는 Xbox Live를 사용 하도록 설정 제목 게임 엔진, Visual Studio 또는 선택한 빌드 환경에서을 설정할 수 있습니다.

참조 [Xbox Live를 통합 하 단계별 가이드](partners-step-by-step-guide.md)
