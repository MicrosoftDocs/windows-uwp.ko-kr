---
ms.assetid: 9ca1f880-2ced-46b4-8ea7-aba43d2ff863
description: Microsoft Advertising SDK의 현재 릴리스에 대 한 알려진 문제에 대해 알아봅니다.
title: 응용 프로그램의 광고에 대 한 알려진 문제 및 문제 해결
ms.date: 02/18/2020
ms.topic: article
keywords: windows 10, uwp, 광고, 광고, 알려진 문제, 문제 해결
ms.localizationpriority: medium
ms.openlocfilehash: c69c61cc1db0796edbaedb2f8e2970e1100c5774
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89158537"
---
# <a name="known-issues-and-troubleshooting-for-ads-in-apps"></a>응용 프로그램의 광고에 대 한 알려진 문제 및 문제 해결

>[!WARNING]
> 2020 년 6 월 1 일부 터 Windows UWP 앱 용 Microsoft Ad 수익 화 플랫폼이 종료 됩니다. [자세한 정보](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/db8d44cb-1381-47f7-94d3-c6ded3fea36f/microsoft-ad-monetization-platform-shutting-down-june-1st?forum=aiamgr)

이 항목에서는 Microsoft Advertising SDK의 현재 릴리스에 대 한 알려진 문제를 나열 합니다. 추가 문제 해결 지침은 다음 항목을 참조 하세요.

* [HTML 및 JavaScript 문제 해결 가이드](html-and-javascript-troubleshooting-guide.md)
* [XAML과 C# 문제 해결 가이드](xaml-and-c-troubleshooting-guide.md)

## <a name="adcontrol-interface-unknown-in-xaml"></a>XAML에서 AdControl 인터페이스를 알 수 없음

[Adcontrol](/uwp/api/microsoft.advertising.winrt.ui.adcontrol) 의 XAML 태그는 인터페이스를 알 수 없다는 파란색 곡선으로 잘못 표시 될 수 있습니다. 이는 x 86을 대상으로 하는 경우에만 발생 하며 무시할 수 있습니다.

## <a name="lasterror-from-previous-ad-request"></a>이전 ad 요청에서 lastError

이전 ad 요청에서 남겨진 **lastError** 있는 경우 다음 ad 호출 중에 이벤트가 두 번 발생 될 수 있습니다. 새 ad 요청은 계속 생성 되며 유효한 ad를 생성할 수 있지만이 동작으로 인해 혼란이 발생할 수 있습니다.

## <a name="interstitial-ads-and-navigation-buttons-on-phones"></a>휴대폰의 중간 광고 및 탐색 단추

하드웨어 단추 대신 소프트웨어 **뒤로**, **시작**및 **검색** 단추가 있는 휴대폰 (또는 에뮬레이터)에서 카운트다운 타이머와 중간 광고에 대 한 클릭 단추를 흐리게 표시할 수 있습니다.

## <a name="recently-created-ads-are-not-being-served-to-your-app"></a>최근에 만든 광고가 앱에 제공 되지 않습니다.

최근에 ad (하루 미만)를 만든 경우 즉시 사용 하지 못할 수 있습니다. Ad가 편집 콘텐츠에 대해 승인 되 면 광고 서버가이를 처리 하 고 ad를 인벤토리에 사용할 수 있게 되 면 제공 됩니다.

## <a name="no-ads-are-shown-in-your-app"></a>앱에 광고가 표시 되지 않음

네트워크 오류를 비롯 하 여 광고를 볼 수 없는 많은 이유가 있습니다. 다른 이유는 다음과 같습니다.

* 앱 코드에서 **Adcontrol** 크기 보다 크거나 작은 크기의 파트너 센터에서 ad 단위를 선택 합니다.

* 라이브 앱을 실행할 때 ad 단위 ID에 대해 [테스트 모드 값](set-up-ad-units-in-your-app.md#test-ad-units) 을 사용 하는 경우 광고가 나타나지 않습니다.

* 지난 30 분 동안 새 ad 단위 ID를 만든 경우 서버에서 시스템을 통해 새 데이터를 전파 하기 전 까지는 광고가 표시 되지 않을 수 있습니다. 이전에 광고를 표시 한 기존 Id는 광고를 즉시 표시 해야 합니다.

앱에서 테스트 광고를 볼 수 있는 경우 코드가 작동 하 고 광고를 표시할 수 있습니다. 문제가 발생 하는 경우 [기술 지원](https://developer.microsoft.com/windows/support)서비스에 문의 하십시오. 해당 페이지에서 **문의처**를 선택 합니다.

[포럼](https://social.msdn.microsoft.com/forums/windowsapps/en-US/home?category=windowsapps)에 질문을 게시할 수도 있습니다.

## <a name="test-ads-are-showing-in-your-app-instead-of-live-ads"></a>테스트 광고가 라이브 광고 대신 앱에 표시 됩니다.

라이브 광고를 기대 하는 경우에도 테스트 광고를 표시할 수 있습니다. 이 문제는 다음과 같은 시나리오에서 발생할 수 있습니다.

* Microsoft의 광고 플랫폼은 스토어에서 사용 된 라이브 응용 프로그램 ID를 확인 하거나 찾을 수 없습니다. 이 경우 사용자가 ad 단위를 만들 때 상태는 라이브 (비 테스트)로 시작 될 수 있지만 첫 번째 ad 요청 후 6 시간 이내에 테스트 상태로 전환 됩니다. 10 일 동안 테스트 앱에서 요청을 받지 않은 경우 다시 라이브로 변경 됩니다.

* 테스트용으로 로드 된 앱 또는 에뮬레이터에서 실행 중인 앱은 라이브 광고를 표시 하지 않습니다.

라이브 ad 단위가 테스트 광고를 처리 하는 경우 ad 단위의 상태는 파트너 센터에서 **활성 및 테스트 광고** 를 표시 합니다. 이는 현재 휴대폰 앱에는 적용 되지 않습니다.


<span id="reference_errors"/>

## <a name="reference-errors-caused-by-targeting-any-cpu-in-your-project"></a>프로젝트의 모든 CPU를 대상으로 하 여 발생 한 참조 오류

Microsoft Advertising SDK를 사용 하는 경우 프로젝트의 **모든 CPU** 를 대상으로 할 수 없습니다. 프로젝트가 **모든 CPU** 플랫폼을 대상으로 하는 경우에는 참조를 추가한 후에도 경고가 표시 될 수 있습니다.

![referenceerror \- solutionexplorer](images/13-19629921-023c-42ec-b8f5-bc0b63d5a191.jpg)

이 경고를 제거 하려면 아키텍처 관련 빌드 출력 (예: **x86**)을 사용 하도록 프로젝트를 업데이트 합니다. **Configuration Manager** 를 사용 하 여 디버그 및 릴리스 구성에 대 한 플랫폼 대상을 설정 합니다.

![configurationmanagerwin10](images/13-87074274-c10d-4dbd-9a06-453b7184f8de.png)

다음 이미지와 같이 스토어 전송용 앱 패키지를 만들 때 대상으로 할 아키텍처를 포함 해야 합니다. X64 OS에서 x86 빌드를 실행 하려는 경우 x 64를 건너뛰도록 선택할 수 있습니다.

![projectstorecreateapppackages](images/13-a99b05a4-8917-4c53-822e-2548fadf828a.png)

![createapppackages](images/13-16280cb1-a838-42b9-9256-eac7f33f5603.png)

## <a name="z-order-in-javascripthtml-apps"></a>JavaScript/HTML 앱에서 Z 순서

JavaScript/HTML 앱은 z 순서의 예약 된 최대-10 범위에 요소를 넣지 않아야 합니다. 유일한 예외는 Skype 앱에 대 한 인바운드 호출 알림과 같은 인터럽트 오버레이입니다.

<span id="bkmk-ui"/>

## <a name="do-not-use-borders"></a>테두리 사용 안 함

**Adcontrol** 에서 상속 된 테두리 관련 속성을 부모 클래스에서 설정 하면 ad 배치가 잘못 됩니다.

## <a name="more-information"></a>추가 정보

알려진 최신 문제에 대 한 자세한 내용 및 Microsoft Advertising SDK와 관련 된 질문을 게시 하려면 [포럼](https://social.msdn.microsoft.com/forums/windowsapps/en-US/home?category=windowsapps)을 방문 하세요.

 

 