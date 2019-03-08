---
title: 교차 play 게임 시작
description: PC 및 Xbox One 콘솔에서 실행 되는 교차 플레이 게임을 개발 하는 방법에 알아봅니다.
ms.assetid: 6c8e9d08-a3d2-4bfc-90ee-03c8fde3e66d
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox 하나 간 play, 어디서 나 재생
ms.localizationpriority: medium
ms.openlocfilehash: 79b0c211291d8a27456126ea0378f9093d1dccab
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57646728"
---
# <a name="get-started-with-cross-play-games"></a>교차 play 게임 시작

## <a name="summary"></a>요약

Windows 10의 시작을 사용 하 여 게임 개발자에 게 ("XDK 게임을)으로 Xbox One 및 Windows 10 UWP 게임) (으로 모두에서 단일 제품을 해제할 수 됩니다. 경우에 따라 개발자가 이러한 게임을 Xbox Live 서비스 멀티 플레이 게임, 게임 저장, 성과, 등과 같은 게임의 Xbox One 및 Windows 10 버전은 통합 하는 위치에 대 한 교차 플레이 사용 하도록 설정 합니다. 간 재생을 사용 하려면 이러한 게임 게임 XDK 및 UWP 버전 간에 단일 제목 ID 및 Xbox Live 서비스 구성을 공유 합니다.

XDK + UWP 게임 지금 수집 4 개의 주요 단계가 필요 합니다.

1.  파트너 센터에서 UWP 제품 만들기

2.  XDP, XBL 구성에서 공유 하려는 플랫폼을 선택 하면에서 XDK 게임 만들기

3.  UWP 제품 정보는 XDK 제품 XDP 바인딩합니다

4.  구성 및 Xbox Live XDP를 통해 게시

이 문서의 목적은 추가로 수집 XDK + UWP 간 구획 게임에 Xbox 직원에 대 한 최대한 쉽게 확인 하려면 다음 4 단계에 자세히 설명 하는 것

## <a name="terminology"></a>용어

### <a name="scenario-terms"></a>시나리오 조건

1.  크로스-재생: 둘 이상의 플랫폼에 해제가 있지만 단일 Xbox를 공유 하는 게임 제목 ID 및 서비스 구성. 최종 결과는 게임의 두 버전 모두 동일한 Xbox Live 구성을 공유-도전 과제, 순위표, 게임 저장, 다중 접속, 등입니다.

2.  파트너 센터: 합니다 [포털](https://partner.microsoft.com/dashboard) UWP 개발 하 고 설치 Xbox Live 구성에서 UWP에 대 한 사용에 대 한 앱 id를 예약할 수 있습니다.

3.  XDP: Xbox 개발자 포털에 현재 존재 하는 수집, 구성 및 Xbox One XDK 및 SRA 게임을 게시 하 고 수집, 구성 및 XDK + UWP 간 플레이 게임을 게시 하려면 추가 사용 하 여 표시 됩니다.

### <a name="identity-terms"></a>Id 용어

1.  제목 ID: Xbox live 게임을 식별 하는 데 사용 하는 Xbox 제목 ID입니다. 제목 ID는 여러 플랫폼에 걸쳐 존재할 수 있는 단일 제품에 매핑됩니다.

2.  서비스 구성 ID (서비스 안내). 각 Xbox title (제목 ID로 식별 됨)에 해당 서비스 구성 ID (즉, 서비스 안내). 이 ID를 사용 하면 Xbox Live 고유 하 게 식별 규칙을 제목 상호 작용할 때 사용 하는 구성 /입니다.

3.  패키지 제품군 이름 (PFN): 파트너 센터에서 만든 각 제품에 할당 된 id입니다. 이 파트너 센터 제품의 id에 UWP 바인딩할 되 면이 PFN에 걸립니다. PFNs는 여러 플랫폼에 걸쳐 존재할 수 있는 고유 제품 식별자입니다. PFNs 1:1 Xbox 제목 Id를 사용 하 여 됩니다.

4.  MSA 앱 ID: 라고도 MSA 클라이언트 ID와 이것이 MSA에서 파트너 센터에서 제품 생성 시 할당 하는 다른 앱 id입니다. 이 id는 Microsoft 서비스 앱을 식별 하는 데 도움이 됩니다. MSA 앱 Id는 1:1 PFNs로 (및 그에 따라 Xbox 제목 Id를 사용 하 여).

## <a name="scenario-overview"></a>시나리오 개요

### <a name="what-is-cross-play"></a>교차 Play 란?

Windows 10 환경을 한 쇼케이스; 교차 플레이 단일 Xbox Live 구성 시나리오가 장치 간 멀티 플레이 게임, 성과 및 순위표를 명확 하 게 장치 버전 간에 공유 되는 게임을 사용 하 여 Xbox One 및 PC 간에 장치 간 게임 게임 저장 합니다.

### <a name="what-are-the-pros-and-cons-of-cross-play"></a>장점 및 단점 간 플레이?

교차 플레이 게임을 XDK 및 UWP 버전 하려는 경우를 적합 한 접근 방식을 가능성이:

-   장치 간 멀티 플레이 게임 (및 Xbox One에 참여 PC) 이상의 멀티 플레이 게임 모드

-   사용자 장치 모두에서 사용할 수 있는 단일 게임 저장 공유

-   성과 게임의 집합을 하나씩 사용할 문제 / / 장치 모두에 대해 입자 진행할 수 있는 순위표

교차 Play 아닙니다 가능성이 하기 적합 한 접근 방식을 경우:

-   PC 및 Xbox One 플레이어 게임의 모든 멀티 플레이 게임 모드에서는 장치에서 멀티 플레이 게임에 관여 하지 못하게.

-   Xbox One을 유지 하려는 및 PC 게임 등의 이유로 보안 또는 신뢰 별도 저장

-   Xbox One 및 PC 게임을 별도 게임 (즉, Xbox One 및 PC를 구입 하는 사용자 수 수신 공유 1000 대신 각 플랫폼용 게임 1000)가 버전을 원하는

일반적으로 교차 플레이에 가장 많은 가치를 추가합니다.

-   Play 무료 Xbox 게임을 플레이 원격에 게임의 Xbox One 및 PC 버전 사이의 연속성을 강조 하는 /

-   Xbox One 및 PC 간에 장치 간 멀티 플레이어를 포함 하는 게임

**참고**: 모두는 XDK 이미 발송 되어 있지만 UWP 버전을 추가 하는 게임 뿐만 아니라 게임 XDK 및 UWP 버전을 동시에 출시할는 새 게임 플레이 간 제품은입니다.

### <a name="what-are-the-restrictions-of-cross-play"></a>간 재생 제한 이란?

Xbox One UWP 게임을 지원 될 때까지 간 플레이 게임 (Xbox One 콘솔용) XDK 및 게임의 UWP (Windows 10 PC)에 대 한 버전을 필요로 합니다.

모든 XDK + UWP 간 게임 중요 한 몇 가지 제한 사항이 수반 됩니다.

1.  **XDP XDK 제목을 수집할 수 해야**합니다. 모두 서비스 구성 및 게시 메인 라인 환경에서 파트너 센터 기반 XDK 타이틀을 지원 하도록 되어 있지 않은 합니다.

2.  **XDP에서 만든 단일 서비스 구성을 수 게임 XDK 및 UWP 버전에서 모두 사용**합니다. XDP 게임 XDK 및 UWP 버전 간에 단일 서비스 구성을 공유할 수 있도록 하려면 새 기능이 추가 되었습니다. 패키지에 대 한 파트너 센터에 게시 해야 UWP 버전 / XDP에서 카탈로그에 있지만 모든 서비스 구성 게시를 수행할 수 있습니다.

3.  **서비스 구성 XDP 사이의 파트너 센터 분할할 수 없는**합니다. 파트너 센터 및 XDP 서로 – 기존 다른에서 게시 한 과도 한 쓰기의 게시 인식 하지 않습니다. 복구 서비스 구성을 나누고 끔찍한 사용자 환경을 (소실점 성과, 게임 저장 등 손실)을 만들 수 있으며 결과적으로 100 %XDK + UWP 간 play 게임 XDP에서 수행 해야 하는 서비스 구성의 필요 합니다.

### <a name="create-your-uwp-product-in-partner-center"></a>파트너 센터에서 UWP 제품 만들기

이 가이드를 수행 하 여 파트너 센터에서 UWP 제품을 만듭니다. [새 또는 기존 UWP 프로젝트에 Xbox Live를 추가합니다.](get-started-with-visual-studio-and-uwp.md)

## <a name="setup-your-xdk-product-in-xdp"></a>XDP에서 XDK 제품 설정

이제 프로그램 UWP를 만들었으므로 XDP에서 XDK 제품 설치 준비가 된 것입니다. XDK 제목 아직 없는 경우 만들어야 합니다.

### <a name="create-your-xdp-product"></a>XDP 제품 만들기

XDP의 게시자에서 새 제품을 만들려면 사용자 계정 관리자를 사용 하 여 작업 ([https://xdp.xboxlive.com/](https://xdp.xboxlive.com/User/Publisher)).

XDP에서 제품을 만들 때 플랫폼을 선택 하려면 UI의 왼쪽된 섹션 맨 아래로 스크롤하면 있는지 확인 합니다. 모든 플랫폼을 확인 했는지 **언젠가 하려는** 간 play 통합 Xbox Live에서 게임의 릴리스 합니다.

플랫폼을 선택 하면 게임 (대개 XDK 제목)에 대 한 리소스 액세스의 유형을 지정 하 고 의도 한이이 제품에 대 한 메커니즘을 릴리스 합니다.


![](../images/ingesting_crossplay_games_xdp/image4.png)
### <a name="update-your-xdp-product-platforms"></a>XDP 제품 플랫폼 업데이트

XDP에 기존 XDK 제품 이미 있는 경우 PC 플랫폼을 지원 하도록 업데이트 해야 합니다. 이 위해 한 번 제품으로 이동 제품 설치 &gt; 플랫폼 유형입니다.

![](../images/ingesting_crossplay_games_xdp/image10.png)

이 페이지에서 지원 하려는 플랫폼을 선택 합니다 (옵션은 Xbox One, 임계값 PC 및 Windows Mobile). 즐거운 선택 되 면 "Platforms 제출" 단추를 선택 합니다.

이 즉시 변경 되지 것입니다 라이브 (적용 하려면이 옵션에 대 한 게시 서비스 구성, 카탈로그 또는 이진 필요 하지 않습니다). Note는이 구성에 걸쳐 샌드박스 – 게임에 대 한 다른 플랫폼 형식 당 샌드박스를 사용할 수 없습니다.

### <a name="enter-your-msa-app-id"></a>MSA 앱 ID를 입력 합니다.

XDP 제품을 만든 후 이전에 만든 MSA 앱 ID를 입력 한 제품에 대 한 제품 설치 페이지로 이동 합니다. 맨 왼쪽 클릭 하 여 제품 설치에 연결할 수 "상태 상자" XDP, 제품에 대 한 작업 표시줄에서 다음과 같이 합니다.

![](../images/ingesting_crossplay_games_xdp/image11.png)

제품 설정 페이지를 수행한 후에 "응용 프로그램 ID 설정" 섹션을 선택 합니다. 이 영역에서는 검색 MSA 앱 ID를 입력 하 고 아래와 같이 "응용 프로그램 ID" 필드에 배치 합니다.

![](../images/ingesting_crossplay_games_xdp/image12.png)

**있습니다** **특성 이름 및 게시자를 입력 하지 않아도**를 **특별히 페이지에서 "Get Application ID" 링크를 사용 하지 않도록 해야**처럼 이미이 입력 해야 하는 MSA 앱 ID 필드 및 응용 프로그램에 대해 생성 된 새 항목을 원하지 않는 합니다.

MSA 앱 ID "응용 프로그램 ID" 필드에 입력 한 후에 "응용 프로그램 ID 설치 제출" 단추를 클릭 합니다. Xbox Live 보안-사용 하 여 MSA 앱 ID 정보를 저장 하이는 XToken 프로그램 UWP에서 검색할 요청할 때마다 내에 포함 된 제목 클레임 이제 매핑될이 XDP 제품 (으로 UWP는 만든 AppX 매니페스트 Id를 사용 하 여 제대로 파트너 센터 d!)

### <a name="enter-the-partner-center-pfn-into-xdp"></a>파트너 센터 PFN XDP에 입력

위의 단계는 인증 및 Xbox Live를 사용 하 여 서비스 구성을 만들고 XDP에서 게시를 사용 하 여 UWP 게임을 주기에 충분 한, 올바르게 작동 하려면 UWP 게임 PFN 알아야 할 특정 Xbox Live 기능 (예: 멀티 플레이 초대) 해야 합니다.

이 작업을 수행 하려면 제품 설치로 이동 &gt; Dev Center 바인딩

![](../images/ingesting_crossplay_games_xdp/image13.png)

이 페이지에서 (4.1.1 섹션에서 검색)으로 UWP 앱에 대 한 PFN을 입력 한 다음 "저장" 단추를 선택 합니다.

이 구성은 즉시 라이브 이루어지지 않습니다. 미래를 통해 라이브 상태가 되는 샌드박스에 게시 서비스 구성. 따라서이 정보가 샌드박스가 적용 되며 사용 가능 하도록 각 샌드박스에 게시 해야 합니다.

### <a name="flag-your-app-for-xbox-cert-in-partner-center"></a>파트너 센터에서 Xbox 인증서에 대 한 응용 프로그램 플래그

현재, Xbox Live Xbox 인증서에 대 한 설정으로 인식 하 여 게임을 가져오는 몇 가지 수동 개입이 필요 합니다. 릴리스 관리자에 앱을 플래그를 사용 하 여 작업 경우 SmartCert 검색에 대 한 파트너 센터에서 활성화 Xbox Live

### <a name="update-your-uwp-game-in-the-xbox-app"></a>UWP 게임을 Xbox 앱 업데이트

일반적으로 Xbox 앱 Xbox 앱 내에서 환경을 Xbox Live를 모든 UWP 게임에 대 한 제목 자동 생성 된 ID를 사용 합니다. 파트너 센터 내에서 필요한 데이터 업데이트를 제대로 제목 XDP-생성 ID를 사용 하 여 Xbox 앱에서 UWP 게임을 가지려면 **프로그램 UWP 릴리스에 대 한 게임을 제출 하기 전에 합니다.**

이 작업을 수행 하 여 파악 문의 하 고 제목 이름에 Xbox 앱 제목 ID를 업데이트 하려는 알립니다 하세요.  XDP에서 만든 제목 ID를 포함 해야 (제품 설치 프로그램에서 XDP 나타나지 &gt; 제품 세부 정보) 및 URL에 대 한 Windows 10에 UWP 용 (파트너 센터에서 앱 관리 아래에 표시 &gt; 앱 id)입니다.

## <a name="configure-xbox-live-in-xdp"></a>Xbox 구성 XDP 거주

### <a name="service-configuration"></a>서비스 구성

XDP 및 파트너 센터 제품 제대로 구성 하 고 바인딩된는 XDK 제목의 평소 대로 XDP 내에서 Xbox Live 공유 구성을 설정 하는 무료 됩니다.

**미리 알림** 바인딩된 – XDK + UWP 게임 해야, 어떠한 상황을 사용 하도록 설정, 구성 또는 파트너 센터를 통해 Xbox Live 서비스 구성을 게시 합니다. 이 지침을 따르지 게임에 Xbox Live 구성이 손상 영구적으로 수 있습니다.

### <a name="catalog-configuration"></a>카탈로그 구성

XDK + UWP 게임을 카탈로그 구성 해야 설치의 두 배가 됩니다: 프로그램 XDK에 대 한 XDP에 한 번 및 프로그램 UWP에 대 한 파트너 센터에서.

XDP 구성의 경우 이것이 정상 XDK 제품 동일입니다. 파트너 센터 구성에 대 한 자세한 단계를 확인할 수 있습니다 [여기](https://dev.windows.com/en-us/publish)합니다.

<table>
  <tr>
    <td>
      UWP 전용 게임에 대 한 카탈로그 구성의 제한 된 집합만 설치 해야 합니다. 특히, 마케팅 정보 채워야 UWP 전용 게임에 Xbox Live 게임을 구성 하는 모든 XDP 여기에 입력 된 문자열을 사용 하는 서비스 구성으로 합니다. 또한 가용성 "사용할 수 없음"을 사용 하 여 설치 해야는 게임에에서 표시 되지 것입니다 Xbox One 카탈로그 경우 카탈로그 정보를 게임 가져옵니다 게시 되도록 합니다.
    </td>
  </tr>
</table>

### <a name="binary-configuration"></a>이진 구성

XDK + UWP 게임을 이진 구성 해야 설치의 두 배가 됩니다: 프로그램 XDK에 대 한 XDP에 한 번 및 프로그램 UWP에 대 한 파트너 센터에서.

XDP 구성의 경우 이것이 정상 XDK 제품 동일입니다. 파트너 센터 구성에 대 한 자세한 단계를 확인할 수 있습니다 [여기](https://dev.windows.com/en-us/publish)합니다.

<table>
  <tr>
    <td>
      UWP 전용 게임에 대 한 이진 구성 XDP에 대 한 않아도가 됩니다. 이 단계를 완전히 건너뛸 수 있습니다.
    </td>
  </tr>
</table>

## <a name="publish-in-xdp"></a>XDP에 게시

일반적으로 XDK + UWP 게임을 게시 하는 XDK 제목 XDP 및 파트너 센터에서 UWP에 별도로 게시와 같은 프로세스를 따릅니다. 따라서는 아래 고유 프로세스 또는 크로스 play 게임에 대 한 사항을 고려해 야에 초점을 맞춥니다.

### <a name="dev-sandbox-publishing"></a>개발 샌드박스 게시

### <a name="no-dev-sandbox-catalog-equivalent-for-microsoft-store"></a>Microsoft Store 개발 샌드박스 카탈로그 동등한 옵션이 없습니다

XDP를 사용 하면 Xbox One 카탈로그의 샌드박스 개발 버전으로 사용자의 카탈로그 및 이진 파일을 게시할 수 있습니다, 있지만 Microsoft Store 카탈로그에 샌드박스 지원 되지 않습니다. 따라서 개발 샌드박스에서 UWP 테스트는 UWP 테스트용으로 로드 하는 데 필요한을 직접 재생 합니다. 이 테스트 Xbox Live, 영향을 주지 않습니다 하지만 표준 테스트 프로세스를 변경할 수 있습니다.

<table>
  <tr>
    <td>
      UWP 전용 게임에 여전히 필요는 XDK 게시할 서비스 구성의 차단을 해제 하려면 제목 카탈로그 정보를 게시 UWP 전용 게임에 Xbox One 카탈로그 존재 하지도 합니다.
    </td>
  </tr>
</table>

### <a name="cert-sandbox-publishing"></a>인증서 샌드박스 게시

### <a name="coordination-between-xdp-publishing-and-partner-center-publishing"></a>XDP 게시 및 파트너 센터 게시 조정

XDP, 일반 정품 인증서에서 이동에 두 가지 별도, 명시적 작업. 그러나 파트너 센터에서 제출 프로세스가 자동으로 이동 및 정품 인증을 통해 게임입니다. 따라서 준수 하도록 작업 순서가 있습니다.

인증을 위해 이동할 준비 된 경우 다음 단계를 순서 대로 따라야 합니다.

1.  인증서 (카탈로그, 이진 및 서비스 구성 포함)에 XDP에서 XDK 제품 게시

2.  UWP 제품에 대 한 파트너 센터 제출을 시작합니다

    1.  **"이이 앱을 수동으로 게시"를 선택 해야 합니다. 또는 "이후에 \[날짜\]" 게시 날짜 필드에!** 이렇게 하지 않으면 UWP 게임 수에 릴리스되는 경우 사용자 개입 없이 자동으로 일반 정품

<table>
  <tr>
    <td>
      UWP 전용 게임에서 여전히 게시 하는 XDK 제목 이진 없는 경우에 파트너 센터 제출물을 시작 하기 전에 카탈로그 및 서비스 config XDP에서 인증서를 게시 해야 합니다.
    </td>
  </tr>
</table>

### <a name="retail-sandbox-publishing"></a>소매 샌드박스 게시

### <a name="coordination-between-xdp-publishing-and-partner-center-publishing"></a>XDP 게시 및 파트너 센터 게시 조정

인증을 XDK 제목의 완료에 UWP 인증 고 게시할 준비가 되 고 일반 정품으로 앱을 게시 준비가 된 것입니다. 마찬가지로 것이 중요 XDK 제목 및 UWP 정렬 유지 되도록 특정 순서로이 프로세스를 따라야 합니다.

1.  준비가 되 면 XDK 제품 XDP에서 일반 정품 버전 (카탈로그, 이진 및 서비스 구성 포함)에 게시

2.  파트너 센터에서 인증 페이지에서 제품의 선택 "지금 게시" "이이 앱을 수동으로 게시", 이전에 선택한 경우 또는 선택한 경우 전송에 게시 될 때까지 기다리는 "이후에 \[날짜\]"입니다.

다음이 단계를 완료 한 후에 XDK 제목 및 UWP 게임 소매점에서 공유 서비스 구성 사용 하 여 전 세계에 게시 해야 합니다. 축하합니다.

<table>
  <tr>
    <td>
      UWP 전용 게임에서 카탈로그를 게시 하는 데 필요 하 고 서비스 구성 완료 되기 전에 게시 파트너 센터에서이 고, 그렇지 릴리스된 UWP XDP에서 일반 정품으로 Xbox Live에 액세스할 수 없습니다.
    </td>
  </tr>
</table>
