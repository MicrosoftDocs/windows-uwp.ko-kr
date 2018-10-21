---
title: 크로스 플레이 게임 시작
author: KevinAsgari
description: PC와 Xbox One 콘솔에서 실행 되는 크로스 플레이 게임을 개발 하는 방법을 알아봅니다.
ms.assetid: 6c8e9d08-a3d2-4bfc-90ee-03c8fde3e66d
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, 크로스 플레이 한 xbox, 어디 재생
ms.localizationpriority: medium
ms.openlocfilehash: 14f6e895ed98804fa965ee6d9ef6cadcde6f220f
ms.sourcegitcommit: 72835733ec429a5deb6a11da4112336746e5e9cf
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/21/2018
ms.locfileid: "5158778"
---
# <a name="get-started-with-cross-play-games"></a>크로스 플레이 게임 시작

## <a name="summary"></a>요약

Windows 10 일 출시를 사용 하 여 게임 개발자는 XDK 게임) (으로 Xbox One 및 Windows 10 (로 UWP 게임)에 단일 제품을 해제할 수 됩니다. 경우에 따라 개발자 크로스 플레이 이러한 게임의 경우 멀티 플레이어, 게임 저장, 도전 과제, 등과 같은 Xbox Live 서비스에서 해당 게임의 Xbox One 및 Windows 10 버전은 통합 하는 위치를 사용 하도록 설정 합니다. 크로스 플레이 사용 하려면 이러한 게임을 단일 제목 ID 및 Xbox Live 서비스 구성 게임의 XDK와 UWP 버전 간에 공유 합니다.

XDK + UWP 게임 오늘날 ingesting 4 개의 주요 단계가 필요 합니다.

1.  Windows 개발자 센터에서 UWP 제품 만들기

2.  XBL 구성에서 공유 하려는 플랫폼을 선택 하면, XDP에서 XDK 게임 만들기

3.  UWP 제품 정보 XDP에서 XDK 제품에 바인딩

4.  구성 및 Xbox Live XDP 통해 게시

이 문서에서는 구체화 하는 데 추가로 Xbox XDK + UWP 간 같은 게임을 수집 하는 직원을 위한 가능한 한 쉽게 확인 하려면 다음 4 단계

## <a name="terminology"></a>용어

### <a name="scenario-terms"></a>시나리오 용어

1.  크로스 플레이: 둘 이상의 플랫폼에서 해제 하지만 단일 Xbox를 공유 하는 게임 ID 제목 및 서비스 구성 합니다. 최종 결과 게임의 두 버전 모두 동일한 Xbox Live 구성을 공유-도전 과제, 순위표, 게임 저장, 멀티 플레이어, 등에 됩니다.

2.  Windows 개발자 센터: 포털 UWP 개발 오늘과 설치 Xbox Live 구성에서 UWP에 대 한 사용에 대 한 앱 id를 예약할 수 있습니다.

3.  XDP:를 수집할 현재 존재 하는 Xbox 개발자 포털을 구성 및 Xbox One XDK 및 SRA 게임을 게시 하 고 수집, 구성 및 XDK + UWP 크로스 플레이 게임을 게시 하는 데 추가 사용 표시 됩니다.

### <a name="identity-terms"></a>Identity 용어

1.  제목 ID:이 값은 각 게임을 Xbox Live를 식별 하는 데는 Xbox 제목 ID입니다. 제목 ID가 여러 플랫폼으로 확장 될 수 있는 단일 제품을 매핑합니다.

2.  서비스 구성 ID (서비스 안내): 각 Xbox 제목 (제목 ID로 식별 됨)에 해당 서비스 구성 ID (일명 서비스 안내). 이 ID를 사용 하면 고유 하 게 식별 규칙 Xbox Live 타이틀와 상호 작용할 때 사용할 구성 / 합니다.

3.  PFN (패키지 패밀리 이름): 개발자 센터에서 만든 각 제품에 할당 하는 id입니다. UWP 개발자 센터 제품의 id를 바인딩할 되 면이 PFN에 걸립니다. PFNs는 여러 플랫폼으로 확장 될 수 있는 고유한 제품 식별자입니다. PFNs은 Xbox 제목 Id를 사용 하 여 1:1입니다.

4.  MSA 앱 ID: 라고도 MSA 클라이언트 ID, 개발자 센터에서 제품 생성 시 MSA에서 할당 된 다른 응용 프로그램 id입니다. 이 id는 Microsoft 서비스 앱을 식별 하는 데 도움이 됩니다. MSA 앱 Id는 PFNs 사용 하 여 1:1 (및 그에 따라 Xbox 제목 Id를 사용 하 여).

## <a name="scenario-overview"></a>시나리오 개요

### <a name="what-is-cross-play"></a>크로스 플레이 란?

Windows 10 환경을 소개가 있습니다. 크로스 플레이 게임 단일 Xbox Live 구성을 장치 간 멀티 플레이어, 도전 과제 및 순위표와 같은 시나리오를 돋보이게 하려면 장치 버전 간에 공유를 사용 하 여 Xbox One 및 PC 간에 장치 간 게임 이며 게임 저장 합니다.

### <a name="what-are-the-pros-and-cons-of-cross-play"></a>크로스 플레이의 장단점 사항은 무엇입니까?

크로스 플레이 XDK 및 UWP 버전을 게임의 하려는 경우 올바르게 접근 하실 수 가능성이 높습니다.

-   하나 이상의 멀티 플레이 게임 모드에서 장치 간 멀티 플레이어 (Xbox One 및 PC)에 참여

-   사용자가 두 장치에서 사용할 수 있는 단일 게임 저장 공유

-   도전 과제 및 게이머 점수는 단일 집합이 어려움이 / / 두 디바이스에 대해 additively 진행 수 순위표

크로스 플레이 될 가능성이 하지 않는 것을 올바르게 접근 하실 경우.

-   PC 및 Xbox One 플레이어 게임의 멀티 플레이 게임 모드 모든 장치 간에 멀티 플레이어의 참여를 유도 하는 것을 방지.

-   Xbox One을 유지 하 고 PC 게임 (아마도 이유로 보안 또는 신뢰) 별도 저장

-   Xbox One 및 PC 버전 (일명 사용자 Xbox One 및 PC 모두 구입 수를 받을 공유 1000 대신 각 플랫폼에 대 한 1000 게이머) 별도 게이머 점수를 게임의 원하는

일반적으로 교차 플레이 대부분의 값을 추가합니다.

-   무료 플레이 게임, 게임의 Xbox One 및 PC 버전 간의 연속성을 강조 하는 어디 Xbox 재생 /

-   Xbox One 및 PC 간에 장치 간 멀티 플레이어 피처 링 한 게임

**참고**: 크로스 플레이 모두 동시에 게임의 UWP 및 XDK 버전을 릴리스 하는 새로운 게임 뿐만 아니라는 XDK 이미 제공 하지만 UWP 버전을 추가 하는 게임에 사용할 수 있습니다.

### <a name="what-are-the-restrictions-of-cross-play"></a>크로스 플레이의 제한 사항은 무엇입니까?

Xbox One UWP 게임을 지원 될 때까지 XDK (Xbox One 본체)에 대 한 및 게임의 UWP (Windows 10 PC 용) 버전 크로스 플레이 게임 필요 합니다.

모든 XDK + UWP 크로스 플레이 게임 몇 가지 중요 한 제한 사항과 함께 제공 됩니다.

1.  **XDK 타이틀 XDP에서 수집 되려면 해야 합니다**. 모두 서비스 구성 및 주요 게시 환경에서 볼 때, 개발자 센터는 되지 않았다면 기반 XDK 타이틀을 지원 합니다.

2.  **XDP에서 만든 단일 서비스 구성 될 수 있는 게임의 UWP 및 XDK 버전에서 사용 되는**. UWP 및 XDK 버전 간에 단일 서비스 구성을 공유 하는 게임을 허용 하도록 XDP 새로운 기능 추가 되었습니다. UWP 버전 패키지에 대 한 개발자 센터에 게시 해야 여전히 / 카탈로그, 하지만 모든 서비스 구성 게시 XDP에서 수행할 수 있습니다.

3.  **서비스 구성 XDP와 개발자 센터 사이의 분할할 수 없습니다**. XDP 및 개발자 센터의 다른 – 다른에서 게시 기존 과도 하 게 작성에서 게시 인식 하지 않습니다. 되어 회복할 수 없게 서비스 구성을 중단 하 고 (소실점 도전 과제, 손실 등 게임 저장)를 무시 무시 한 사용자 환경을 만들 수 있으며 따라서에서 수행 해야 할 XDP XDK + UWP 크로스 플레이 게임에 대 한 서비스 구성의 100% 주의 해야 합니다.

### <a name="create-your-uwp-product-in-the-windows-dev-center"></a>Windows 개발자 센터에서 UWP 제품 만들기

이 가이드를 따라 Windows 개발자 센터에서 UWP 제품 만들기: [새로운 또는 기존 UWP 프로젝트에 Xbox Live 추가](get-started-with-visual-studio-and-uwp.md)

## <a name="setup-your-xdk-product-in-xdp"></a>XDP에서 XDK 제품 설치

이제 UWP을 만들었으므로 XDP에서 XDK 제품 설치 준비가 되었습니다. XDK 제목 아직 없는 경우 만들어야 합니다.

### <a name="create-your-xdp-product"></a>XDP 제품 만들기

XDP에서 게시자에서 새 제품을 만드는 사용자 계정 관리자를 사용 하 여 작업 ([https://xdp.xboxlive.com/](https://xdp.xboxlive.com/User/Publisher)).

XDP에서 제품을 만들 때 플랫폼을 선택 하려면 UI의 왼쪽된 섹션의 맨 아래를 스크롤할 때 있는지 확인 합니다. 크로스 플레이 통합 Xbox Live를 사용 하 여 게임에서 해제 **언젠가 하고자** 하는 모든 플랫폼을 확인 합니다.

플랫폼을 선택 하면 게임 (대개 XDK 제목)에 대 한 리소스 액세스 및이 제품에 대 한 의도 한 릴리스 메커니즘 유형을 지정 합니다.


![](../images/ingesting_crossplay_games_xdp/image4.png)
### <a name="update-your-xdp-product-platforms"></a>XDP 제품 플랫폼 업데이트

이미 있는 경우 기존 XDK 제품 XDP에서, PC 플랫폼을 지원 하도록 업데이트 해야 합니다. 이렇게 하려면 한 번에서 제품을 이동 하 여 제품 설치 &gt; 플랫폼 유형입니다.

![](../images/ingesting_crossplay_games_xdp/image10.png)

이 페이지에서 지원 하려는 플랫폼 선택 (옵션은 Xbox One, 임계값 PC 및 Windows Mobile). 마음에 선택 된 되 면 "제출 플랫폼" 단추를 선택 합니다.

이 즉시 변경 라이브 (적용 하려면이 옵션에 대 한 게시할 서비스 구성, 카탈로그, 또는 이진 파일이 필요 하지 않음). Note는이 구성을 걸쳐 샌드박스-게임에 대 한 다른 플랫폼 형식 당 샌드박스를 가질 수 없습니다.

### <a name="enter-your-msa-app-id"></a>MSA 앱 ID를 입력 합니다.

XDP 제품 생성 되 면 앞에서 만든 MSA 앱 ID를 입력 하 고 제품에 대 한 제품 설치 페이지로 이동 합니다. 제품 설치 맨 왼쪽 클릭 하 여 연결할 수 "상태"에 있는 확인란 XDP, 제품에 대 한 작업 표시줄을 다음과 같이 합니다.

![](../images/ingesting_crossplay_games_xdp/image11.png)

제품 설치 페이지를 만들면 "응용 프로그램 ID 설정" 섹션을 선택 합니다. 이 영역에서 검색 MSA 앱 ID를 입력 하 고 아래와 같이 "응용 프로그램 ID" 필드에 배치 수 있습니다.

![](../images/ingesting_crossplay_games_xdp/image12.png)

**있습니다** 이 필드와 응용 프로그램에 대 한 새 제출을 생성 하는 원하는 되지 않음에 입력 해야 MSA 앱 ID가 **특성 이름과 게시자를 입력할 필요가 없습니다**, **특히 페이지에서 "응용 프로그램 ID 가져오기" 링크를 사용 하지 않아야 하 고**, 다른 이름으로 이미 있습니다.

MSA 앱 ID "응용 프로그램 ID" 필드에서를 입력 한 후에 "응용 프로그램 ID 설정 제출" 단추를 클릭 합니다. 이 Xbox Live 보안 – 사용 하 여 MSA 앱 ID 정보 저장에 UWP에서는 XToken을 검색 하려면 요청 때마다 내에 포함 된 제목 클레임 이제에 매핑됩니다이 XDP 제품 (으로 UWP는 만든 AppX 매니페스트 Id를 사용 하 여 제대로 d Windows 개발자 센터에서!)

### <a name="enter-the-dev-center-pfn-into-xdp"></a>개발자 센터 PFN XDP에 입력

위의 단계를 인증 하 고 만들고 XDP에서 게시 하면 서비스 구성을 사용 하 여 Xbox Live를 사용 하 여 UWP 게임을 가져올 수 있을 정도로 인 제대로 작동 하기 위해 UWP 게임의 PFN 고려해 야 할 특정 Xbox Live 기능 (예: 멀티 플레이 초대) 해야 합니다.

이렇게 하려면 제품 설치로 이동 &gt; 개발자 센터 바인딩

![](../images/ingesting_crossplay_games_xdp/image13.png)

이 페이지에서 (4.1.1 섹션에서 검색)으로 UWP 앱의 PFN을 입력 한 다음 "저장" 단추를 선택 합니다.

이 구성은 즉시 라이브 이루어지지 않습니다. 미래를 통해 라이브 상태로 서비스 구성 샌드박스에 게시 합니다. 따라서이 정보 샌드박스가 적용 되며 가능 하도록 각 샌드박스에 게시 해야 합니다.

### <a name="flag-your-app-for-xbox-cert-in-the-dev-center"></a>앱 개발자 센터에서 Xbox 인증서에 대 한 플래그

현재 Xbox Live Xbox 인증서에 대 한 설정으로 인식 게임을 가져오는 데 필요한 몇 가지 수동 개입이 필요 합니다. 앱 플래그를 릴리스 관리자를 사용 하 여 작업은 Xbox Live SmartCert 검색에 대 한 개발자 센터에서 활성화 됩니다.

### <a name="update-your-uwp-game-in-the-xbox-app"></a>Xbox 앱에서 UWP 게임 업데이트

일반적으로 Xbox 앱을 Xbox 앱 내 환경을 Xbox Live 전원 모든 UWP 게임에 대 한 자동 생성 된 제목 ID를 사용 합니다. 제대로 XDP 생성 제목 ID를 사용 하 여 Xbox 앱에서 UWP 게임을 얻으려면 데이터 업데이트는 Windows 개발자 센터 내에서 수행 해야 **릴리스에 대 한 게임의 UWP를 제출 하기 전에.**

이렇게 하려면 댐을 사용자에 게 문의 하 고 제목 이름에 대 한 Xbox 앱 제목 ID를 업데이트 하려는 알려 하십시오.  XDP에서 만든 제목 ID를 포함 해야 (XDP 제품 설정 아래에 표시 &gt; 제품 세부 정보) 및 URL에 대 한 Windows 10의 UWP에 대 한 (앱 관리에서 개발자 센터에 표시 &gt; 앱 Id).

## <a name="configure-xbox-live-in-xdp"></a>구성 Xbox Live XDP에서

### <a name="service-configuration"></a>서비스 구성

XDP 및 개발자 센터 제품과 제대로 구성 하 고 바인딩된 무료 XDK 제목에 대 한 평소 대로 XDP 내에서 공유 Xbox Live 구성과 설정 됩니다.

**미리 알림** – 바인딩된 XDK + UWP 게임 해야, 어떠한 경우 활성화, 구성, 또는 개발자 센터를 통해 Xbox Live 서비스 구성 게시 합니다. 이 지침을 따르지 않으면 게임에 대 한 Xbox Live 구성과 손상 영구적으로 수도 있습니다.

### <a name="catalog-configuration"></a>카탈로그 구성

XDK + UWP 게임에 대 한 카탈로그 구성 설치를 두 번 되어야 할: 한 번에 XDK 용 XDP 뿐만 아니라 UWP에 대 한 개발자 센터에서.

XDP 구성의 경우 이것이 일반 XDK 제품와 동일 합니다. 개발자 센터 구성에 대 한 자세한 단계를 확인할 수 있습니다 [다음과 같습니다](https://dev.windows.com/en-us/publish).

<table>
  <tr>
    <td>
      UWP 전용 게임의 경우는 제한 된 집합 카탈로그 구성 설정 해야 합니다. 특히, 마케팅 정보 해야 채워야 UWP 전용 게임의 경우 서비스 구성 모든 XDP Xbox Live 게임 구성에 대 한 여기에 입력 된 문자열을 사용 하는 대로 합니다. 또한 가용성 "사용할 수 없음"으로 설치 되어야 게임 됩니다 되지 표시 하려면 Xbox One 카탈로그에서 경우 카탈로그 정보에 게임 게시 된 가져옵니다.
    </td>
  </tr>
</table>

### <a name="binary-configuration"></a>이진 구성

XDK + UWP 게임용 이진 구성 설치를 두 번 되어야 할: 한 번에 XDK 용 XDP 뿐만 아니라 UWP에 대 한 개발자 센터에서.

XDP 구성의 경우 이것이 일반 XDK 제품와 동일 합니다. 개발자 센터 구성에 대 한 자세한 단계를 확인할 수 있습니다 [다음과 같습니다](https://dev.windows.com/en-us/publish).

<table>
  <tr>
    <td>
      UWP 전용 게임의 경우 필요는 없습니다 XDP에서 이진 구성 합니다. 완전히이 단계를 건너뛸 수 있습니다.
    </td>
  </tr>
</table>

## <a name="publish-in-xdp"></a>XDP에 게시

일반적으로 XDK + UWP 게임을 게시 별도로 XDK 제목 XDP 및 개발자 센터에서의 UWP에 게시 하는 것과 동일한 프로세스를 따릅니다. 따라서는 아래 중점적으로 고유한 프로세스 또는 크로스 플레이 게임에 대 한 사항을 고려해 야 합니다.

### <a name="dev-sandbox-publishing"></a>개발자 샌드박스 게시

### <a name="no-dev-sandbox-catalog-equivalent-for-microsoft-store"></a>Microsoft Store에 대 한 개발자 샌드박스 카탈로그 항목이 없는

XDP 카탈로그 및 이진 Xbox One 카탈로그의 개발자 샌드박스 버전을 게시할 수, Microsoft Store 카탈로그에 샌드박스를 지원 하지 않습니다. 따라서 UWP 개발자 샌드박스에서 테스트 해당 UWP 테스트용으로 로드 하는 데 필요한 및 직접 재생 해야 합니다. 이 Xbox Live 테스트에 영향을 주지 않지만 프로세스를 테스트 하 여 표준을 변경할 수 있습니다.

<table>
  <tr>
    <td>
      UWP 전용 게임을 위한 것이 여전히 필요는 XDK 게시 하려면 서비스 구성의 차단을 제목 카탈로그 정보를 게시 UWP 전용 게임에 Xbox One 카탈로그 존재 하지도 합니다.
    </td>
  </tr>
</table>

### <a name="cert-sandbox-publishing"></a>인증서 샌드박스 게시

### <a name="coordination-between-xdp-publishing-and-dev-center-publishing"></a>XDP 게시 및 게시 개발자 센터 간 조정

XDP, 소매 인증서에서 이동은 두 개의 별도, 명시적 작업입니다. 그러나 개발자 센터에서 제출 프로세스 자동으로 이동 소매에 로그온 하 고 인증을 통해 게임 합니다. 따라서 존중 연산 순서가 있습니다.

인증으로 이동 하려면 준비를 마쳤으면 순서로 다음 단계를 따라야 합니다.

1.  XDK 제품 (카탈로그, 이진, 및 서비스 구성 포함) 인증서를 XDP에 게시

2.  UWP 제품에 대 한 개발자 센터 제출을 시작합니다

    1.  **게시 날짜 필드에 "이이 앱을 수동으로 게시" 또는 "아니요 보다 일찍 \[date\]"를 선택 해야!** 이렇게 하지 않으면 사용자 개입 없이 자동으로 소매 UWP 게임 해제 수 있습니다.

<table>
  <tr>
    <td>
      UWP 전용 게임에서는 여전히 게시 하는 XDK 제목 이진 파일이 없는 경우에 개발자 센터 제출 시작 하기 전에 카탈로그 및 서비스 config XDP에서 인증서를 게시 해야 합니다.
    </td>
  </tr>
</table>

### <a name="retail-sandbox-publishing"></a>소매 샌드박스 게시

### <a name="coordination-between-xdp-publishing-and-dev-center-publishing"></a>XDP 게시 및 게시 개발자 센터 간 조정

XDK 타이틀의 인증을 완료 하 고 UWP가 떠났음을 인증 및 게시할 준비가 되 면 정품에 앱을 게시할 준비가 되었습니다. 다시이 XDK 제목 및 UWP 유지 하는 정렬 되도록 특정 순서로이 프로세스를 수행 하는 것이 중요 합니다.

1.  준비가 되 면 XDK 제품 XDP 일반 정품 (카탈로그, 이진, 및 서비스 구성 포함)에 게시

2.  제품의 인증 페이지에서 개발자 센터에서 이전에 선택한 "이이 앱을 수동으로 게시", "지금 게시" 선택 또는 "\[date\ 보다 없음 빠르게]"를 선택 하는 경우 발생에 게시 될 때까지 기다립니다.

다음이 단계를 완료 한 후 XDK 제목 및 소매점에서 공유 서비스 구성 사용 하 여 전 세계에 게시 하는 UWP 게임에 있어야 합니다. 축하합니다!

<table>
  <tr>
    <td>
      UWP 전용 게임에서는 더 카탈로그를 게시 하려면 여전히 필요 및 서비스 구성 XDP 완료 되기 전에 게시 개발자 센터에서 그렇지 릴리스된 UWP에서에서 일반 정품 Xbox Live에 액세스할 수 없게 됩니다.
    </td>
  </tr>
</table>
