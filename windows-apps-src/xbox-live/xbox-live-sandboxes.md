---
title: Xbox Live 샌드박스
description: Xbox Live 개발에 대 한 샌드박스를 알아봅니다.
ms.assetid: a5acb5bf-dc11-4dff-aa94-6d1f01472d2a
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: ee284550a9b508a8d46556bf0353bd75d55014f3
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57599478"
---
# <a name="xbox-live-sandboxes-intro"></a>Xbox Live 샌드박스 소개

[Xbox Live 서비스 구성을](xbox-live-service-configuration.md)에서 정보를 온라인으로 프로그램 타이틀에 대 한 일반적으로 구성 해야 하는 것이 설명한 [파트너 센터](https://partner.microsoft.com/dashboard)합니다.  이 정보 등을 제목 플레이어 잠금을 해제할 수 있는 업적을 표시 하려는 순위표, 결혼 정보 회사 연결 구성 등을 포함 합니다.

서비스 구성을 변경한 경우 변경 내용을 Xbox Live의 나머지 부분에 의해 적용 됩니다 하 고 제목에서 확인할 수 있습니다 파트너 센터에서 게시 해야 이러한 합니다.

개발 샌드박스가 이라고에 게시 합니다.  이러한 속성을 사용 하면 격리 된 환경에서 프로그램 제목 변경 내용에 대해 작업을 수 있습니다.  에 설명 된 몇 가지 이점을 제공 이러한는 아래 섹션입니다.

기본적으로 하나의 콘솔 Xbox 및 Windows 10 Pc의에서 경우 소매 샌드박스

## <a name="benefits"></a>장점

개발 샌드박스는 다음과 같은 몇 가지 장점이 있습니다.


1. 현재 사용 가능한 버전을 영향을 주지 않고을 제목에 대 한 업데이트의 변경 내용에 대해 반복할 수 있습니다.
2. 일부 도구는 보안상의 이유로 개발 샌드박스에서 에서만 작동합니다.
3. 일부 개발자 팀의 기본 개발에서 서비스 구성에 영향을 주지 않고 "분기" 및 테스트 서비스 구성 변경 하려고 수 있습니다.
4. 다른 게시자는 샌드박스에 대 한 액세스를 부여 하지 않고 작업할 무엇을 볼 수 없습니다.

할 수도 있습니다 **필요에 따라** 테스트 계정을 만듭니다.  제목으로 테스트에 대 한 일반 Xbox Live 계정을 사용 하지 않으려는 경우 소셜 상호 작용 등의 시나리오를 테스트 하려면 여러 계정을 해야 사용할 수 있습니다 (예: 친구의 통계를 보려면) 또는 멀티 플레이입니다.

테스트 수만 로그인 개발 샌드박스 계정과 아래 섹션에서 설명 합니다.

## <a name="finding-out-about-your-sandbox"></a>샌드박스에 대해 알아보기

대부분의 개발자가 하나만 sandbox가 필요 합니다.  다행히 샌드박스 제목을 만들면를 만들어집니다.

1. 여기에 파트너 센터로 이동 하 여 샌드박스에 대 한 확인 합니다. ![](images/getting_started/first_xbltitle_dashboard.png)

1. 에 제목을 클릭 합니다. ![](images/getting_started/first_xbltitle_dashboard_overview.png)

1. 마지막으로 서비스 클릭-> Xbox Live 왼쪽된 메뉴에서 ![](images/getting_started/first_xbltitle_leftnav.png)

1. 이제 다음과 같이 나열 하 여 샌드박스를 볼 수 있습니다. ![](images/getting_started/devcenter_sandbox_id.png)

## <a name="how-your-sandbox-impacts-your-workflow"></a>샌드박스에 워크플로 미치는 영향을

일반적으로 작업할 샌드박스를 사용 하 여 다음과 같은 방법으로:

1. (1 회) PC 또는 Xbox One 개발 샌드박스에 전환 합니다.
2. (여러 번) 서비스 구성 변경에 개발 샌드박스에 변경 내용을 게시 합니다.  서비스 구성 변경이 등 정의 도전 과제, 순위표를 추가 또는 멀티 플레이 게임 세션 템플릿을 수정 합니다.
3. (몇 시간) 다른 팀 멤버와 작업 하는 경우 있습니다 수 액세스할 수 있도록 샌드박스에
4. (1 회) 일반 정품, 테스트 또는 좋아하는 Xbox 게임 플레이를 중단 해야 하는 경우 샌드박스에 일반 정품으로 다시 전환 해야 합니다.

이러한 시나리오는 아래에서 자세히 설명 합니다.  프로세스 PC 및 콘솔에 몇 가지 차이점이 있으므로 각각에 대해 별도 섹션이 있습니다.

## <a name="switch-your-pcs-development-sandbox"></a>PC의 개발 샌드박스가 전환

PC의 개발 샌드박스가 전환 하려는 경우 이렇게 하는 권장된 방법은 Windows Device Portal (WDP)를 사용 합니다.  또한 명령줄을 통해 수행할 수 있습니다.  두 가지 방법을 모두 설명 하겠습니다.

### <a name="windows-device-portal"></a>Windows Device Portal

PC에 WDP를 이미 사용 하지 않았다면 이렇게 하려면 다음이 지침을 수행 하십시오. [Windows 바탕 화면에서 장치 포털 설정](https://msdn.microsoft.com/en-us/windows/uwp/debug-test-perf/device-portal-desktop)

이 작업을 수행한 후 위의 문서에 설명 된 대로 웹 브라우저에서 연결 하 여 Windows 개발자 포털을 엽니다.

다음에서 아래와 같이 적절 한 섹션을 이동 하려면 "Xbox Live"를 클릭 합니다.

![](images/getting_started/wdp_switch_sandbox.png)

단계를 통해 얻은 샌드박스에 입력할 수 있습니다 다음 합니다 *찾기 아웃, 즉 샌드박스에* [변경]을 클릭 합니다.

일반 정품으로 다시 전환 하려면 여기 소매를 입력할 수 있습니다.

### <a name="powershell-module"></a>PowerShell 모듈

[Xbox Live PowerShell 모듈](https://github.com/Microsoft/xbox-live-powershell-module/blob/master/docs/XboxLivePsModule.md) XboxlivePSModule Xbox Live 개발 PC 또는 콘솔에 대 한 샌드박스를 변경 하는 등 다양 한 유틸리티가 포함 되어 있습니다.

* 사용할 [PowerShell 갤러리](https://www.powershellgallery.com/packages/XboxlivePSModule), PowerShell 창을 엽니다.
    1. 다운로드 하 고 모듈을 설치 합니다. `Install-Module XboxlivePSModule -Scope CurrentUser`
    2. 실행 하 여 시작 `Import-Module XboxlivePSModule`
    3. 즉, 집합 XblSandbox XDKS.1 또는 Get XblSandbox cmdlet 실행

* Zip 파일에서 사용할 [ https://aka.ms/xboxliveuwptools ](https://aka.ms/xboxliveuwptools), PowerShell 창을 열고
    1. `Import-Module <path to unzipped folder>\XboxLivePsModule\XboxLivePsModule.psd1`를 실행합니다.
    2. 즉, 집합 XblSandbox XDKS.1 또는 Get XblSandbox cmdlet 실행

### <a name="command-prompt-script"></a>명령 프롬프트 스크립트

Xbox Live 도구 패키지를 다운로드 [ https://aka.ms/xboxliveuwptools ](https://aka.ms/xboxliveuwptools) 압축을 풉니다.  내 SwitchSandbox.cmd 배치 파일을 찾을 수 있습니다.

샌드박스에 전환 하려면 관리자 모드에서이 실행 합니다.  첫 번째 인수가 샌드박스를 보여 줍니다.  예를 들어 하듯이 XDKS.1 샌드박스도 전환 하려는 경우:

```
SwitchSandbox.cmd XDKS.1
```

일반 정품으로 전환할 제공 하기만 하면 됩니다 하는 두 번째 인수로 합니다.

```
SwitchSandbox.cmd RETAIL
```

## <a name="switch-your-xbox-one-console-development-sandbox"></a>Xbox One 콘솔 개발 샌드박스에 전환

### <a name="using-windows-dev-portal"></a>Windows 개발자 포털을 사용 하 여

콘솔에 샌드박스를 변경 하려면 Windows 개발자 포털을 사용할 수 있습니다.  이렇게 하려면 콘솔에 "개발자 홈"으로 이동 하 고 사용 하도록 설정 합니다.

그 후에 콘솔에 연결 하려면 PC에서 웹 브라우저에서 IP 주소를 입력할 수 있습니다.  그런 다음 "Xbox Live" 클릭 하 고 샌드박스 있는 텍스트 상자에 입력 수 있습니다.

### <a name="using-xbox-one-manager"></a>Xbox One Manager를 사용 하 여

Xbox One Manager를 사용 하면 PC에서 콘솔의 특정 측면을 관리할 수 있습니다.  여기에, 다시 부팅 하 고, 설치 된 앱을 관리 하 고, 샌드박스에 변경 합니다.

콘솔에 대 한 샌드박스를 변경 하 고 "설정..."로 이동 하려면 마우스 오른쪽 단추로 클릭

샌드박스를 입력할 수 있습니다.

### <a name="using-xbox-one-console-ui"></a>Xbox One 콘솔 UI를 사용 하 여

콘솔에서 바로 개발 샌드박스에 변경 하려는 경우에 "설정"을 이동할 수 있습니다.  "개발자 설정"으로 이동 하 고 샌드박스에 변경 하는 옵션이 표시 됩니다.

## <a name="sandbox-uses"></a>샌드박스 사용

### <a name="data-that-is-sandboxed"></a>샌드박스가 적용 된 데이터
팀 개발 프로세스 중에 개발자 간에 액세스를 관리 하는 샌드박스 기능을 사용할 수 있습니다.  예를 들어, 다음 개발 팀 및 테스터 간에 데이터를 격리 하는 것이 좋습니다.

샌드박스가 적용 된 데이터에 포함 됩니다.
- 도전 과제, 순위표 및 사용자에 대 한 통계입니다.  하나의 sandbox의 사용자에 대 한 누적 된 성과 다른 샌드박스를 변환 하지 않습니다.
- 멀티 플레이 게임 및 결혼 정보 회사 연결 합니다.  사용자는 멀티 플레이 게임을 재생할 수 없는 사용자는 게임은 다른 샌드박스를 합니다.
- 서비스 구성입니다.  새 성과 달성 하셨습니다 하나 샌드박스에서 제목으로 추가 하는 경우 다른 샌드박스에서 표시 되지 않습니다.  이 모든 서비스 구성 데이터에 적용 됩니다.

비-샌드박스가 적용 된 데이터는 주로 소셜 정보입니다.  예를 들어 사용자 다른 사용자와 같이, 샌드박스를 알 수 없는 관계 인지 하도록 합니다.

### <a name="examples"></a>예
예로 여러 샌드박스를 사용 하는 이유의 이점 중 일부를 보여 주기 위해 아래 제공 됩니다.

> **참고**: Xbox 크리에이터 스 프로그램에 있는 경우만 하나의 샌드박스를 해야 합니다.  여러 샌드박스를 만들 필요가 있는 경우에 적용 된 ID@Xbox 프로그램입니다.

#### <a name="service-config-isolation"></a>서비스 구성 격리
위에서 설명 했 듯이 서비스 구성은 특정 샌드박스입니다.  있을 *개발* 샌드박스, 및 *테스트* 샌드박스입니다.  테스터에 제목의 빌드를 부여 하는 경우 게시할 프로그램 [서비스 구성](xbox-live-service-configuration.md) 에 *테스트* 샌드박스입니다.

그동안 성과 추가할 수 있습니다 하거나 다른 멀티 플레이 세션 형식을 프로그램 *개발* 테스터 표시 되는 서비스 구성에 영향을 주지 않고 샌드박스입니다.

#### <a name="multiplayer"></a>멀티 플레이
사용 하 여 위의 예는 *개발* 하 고 *테스트* 샌드박스입니다.  아마도 서비스 구성 샌드박스에 사이 같다고 이지만 개발자 멀티 플레이 기능 만들고 서로 결혼 정보 회사 연결을 테스트 하려면.  테스터는 또한 멀티 플레이 테스트 됩니다.

다음과 같은 경우에는 별도로 문제 디버깅 하기 때문에 테스터를 Xbox Live 매 치메이 킹 서비스를 개발자에 게 좋습니다.  이 문제를 방지 하는 좋은 방법은에 개발자를 위한 것은 *개발* 샌드박스 및 별도의 되도록 테스터 *테스트* 샌드박스.  이 격리 된 그룹을 모두 유지 합니다.

## <a name="advanced"></a>고급

개발 프로세스를 간단히 유지 하기 기본 샌드박스에부터 시작 하 고 새 샌드박스를 신중 하 게 추가 합니다.

요구 사항 증가 대 한 액세스 제어 및 데이터 격리를 찾으면 보시 [Xbox Live 샌드박스 고급](advanced-xbox-live-sandboxes.md) 문서.  
