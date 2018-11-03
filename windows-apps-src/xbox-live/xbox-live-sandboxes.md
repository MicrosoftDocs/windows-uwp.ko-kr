---
title: Xbox Live 샌드박스
author: KevinAsgari
description: Xbox Live 개발 샌드박스를 알아봅니다.
ms.assetid: a5acb5bf-dc11-4dff-aa94-6d1f01472d2a
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 35d2e102e80e7d415562e2917d626152331f3fff
ms.sourcegitcommit: 144f5f127fc4fbd852f2f6780ef26054192d68fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/03/2018
ms.locfileid: "5992873"
---
# <a name="xbox-live-sandboxes-intro"></a>Xbox Live 샌드박스 소개

[Xbox Live 서비스 구성](xbox-live-service-configuration.md)에서 정보를 구성 해야 온라인으로 타이틀에 대 한 일반적으로 [Windows 개발자 센터](http://dev.windows.com)에서 설명 된 것입니다.  이 정보 타이틀에서 플레이어의 잠금을 해제할 수 있는 도전 과제를 표시 하려는 순위표, 매치 메이 킹 구성 등이 포함 됩니다.

서비스 구성을 변경한 경우 이러한 변경 내용을 Xbox Live의 나머지 부분에서 획득 하 고 타이틀 볼 수 있습니다 전에 개발자 센터에서 게시 해야 합니다.

개발 샌드박스 섞어에 게시 합니다.  이러한 격리 된 환경에서 타이틀 변경 작업할 수 있습니다.  이러한에 설명 된 몇 가지 이점이 제공는 섹션 아래 합니다.

기본적으로 Windows 10 Pc 및 Xbox One 본체 소매 샌드박스에서 됩니다.

## <a name="benefits"></a>이점

개발 샌드박스 몇 가지 이점을 제공합니다.


1. 현재 사용 가능한 버전 영향을 주지 않고 타이틀에 대 한 업데이트에 대 한 변경 내용에 반복할 수 있습니다.
2. 일부 도구에서 개발 샌드박스 보안상의 이유로 에서만 작동합니다.
3. 일부 개발자 팀에 게 기본 개발 중인 서비스 구성에 영향을 주지 않고 "분기" 및 테스트 서비스 구성 변경 하고자 수 있습니다.
4. 다른 게시자에 샌드박스에 대 한 액세스를 부여 하지 않고 작업할 무엇을 볼 수 없습니다.

**또한 수** 테스트 계정을 만듭니다.  타이틀을 테스트 하는 것에 대 한 일반 Xbox Live 계정을 사용 하지 않으려면 또는 여러 계정을 소셜 상호 작용 등의 시나리오를 테스트 해야 하는 경우이 사용할 수 있습니다 (예: 친구의 통계를 보려면) 또는 멀티 플레이 합니다.

테스트 계정 로그인 할 수만 개발 샌드박스를 및 아래 섹션에서 설명 합니다.

## <a name="finding-out-about-your-sandbox"></a>샌드박스에 대해 알아보기

대부분의 개발자 샌드박스 하나만 필요 합니다.  다행히 샌드박스 제목을 만들 때에 만들어집니다.

1. 개발자 센터 대시보드에서 여기로 이동 하 여 샌드박스에 대해 알아보려면:
![](images/getting_started/first_xbltitle_dashboard.png)

1. 타이틀에서 클릭 합니다.
![](images/getting_started/first_xbltitle_dashboard_overview.png)

1. 마지막으로 서비스 클릭-> Xbox Live 왼쪽된 메뉴에서
![](images/getting_started/first_xbltitle_leftnav.png)

1. 이제 다음에 샌드박스를 볼 수 있습니다.
![](images/getting_started/devcenter_sandbox_id.png)

## <a name="how-your-sandbox-impacts-your-workflow"></a>샌드박스에 워크플로에 미치는 영향

일반적으로 작업할 샌드박스를 사용 하 여 다음과 같은 방식:

1. (일회성) PC 또는 Xbox One 개발 샌드박스를 전환 합니다.
2. (여러 번) 서비스 구성을 변경할 때 변경 개발 샌드박스를 게시 합니다.  서비스 구성 변경이 같은 정의 도전 과제, 순위표, 추가 또는 멀티 플레이어 세션 템플릿을 수정 합니다.
3. (몇 시간) 다른 팀 구성원을 사용 하는 경우 사용자에 게 배포할 수 액세스에 샌드박스
4. (일회성) 일반 정품에서 무언가 테스트 하거나 즐겨 찾는 Xbox 게임을 플레이할 취하도록 하려면 해야 하는 경우에 샌드박스 정품으로 다시 전환 해야 합니다.

이러한 시나리오는 아래에 자세히 설명 합니다.  프로세스는 각각에 대 한 섹션이 되므로 PC와 콘솔에 몇 가지 차이점이 있습니다.

## <a name="switch-your-pcs-development-sandbox"></a>PC의 개발 샌드박스를 전환 합니다.

PC의 개발 샌드박스를 전환 하려는 경우 이렇게 하는 권장된 방법이 Windows 장치 포털 (WDP)를 사용 합니다.  명령줄을 통해도 실행할 수 있습니다.  두 방법을 설명 합니다.

### <a name="windows-device-portal"></a>Windows Device Portal

PC에서 WDP를 이미 설정 하지 않은 경우 이렇게 하려면 다음이 지침에 따릅니다. [Windows 바탕 화면에서 디바이스 포털 설정](https://msdn.microsoft.com/en-us/windows/uwp/debug-test-perf/device-portal-desktop)

이 작업을 수행한 후 위의 문서에 설명 된 대로 웹 브라우저에서 연결 하 여 Windows 개발자 포털을 엽니다.

아래와 같이 적절 한 섹션을 이동 "Xbox Live"를 클릭 합니다.

![](images/getting_started/wdp_switch_sandbox.png)

그런 다음 *Your 샌드박스 아웃 찾기* 의 단계를 통해 얻은에 샌드박스를 입력 하 고 "변경"를 클릭 수 있습니다.

일반 정품으로 다시 전환 하려면 여기 소매를 입력할 수 있습니다.

### <a name="powershell-module"></a>PowerShell 모듈

[Xbox Live PowerShell 모듈](https://github.com/Microsoft/xbox-live-powershell-module/blob/master/docs/XboxLivePsModule.md) XboxlivePSModule PC 또는 콘솔에서 샌드박스를 변경 하는 등 Xbox Live 개발 하는 데 다양 한 유틸리티를 포함 합니다.

* [PowerShell 갤러리](https://www.powershellgallery.com/packages/XboxlivePSModule)에서 사용 하려면 PowerShell 창을 엽니다.
    1. 다운로드 및 모듈을 설치 합니다. `Install-Module XboxlivePSModule -Scope CurrentUser`
    2. 실행 하 여 사용 하 여 시작 `Import-Module XboxlivePSModule`
    3. 즉, 설정 XblSandbox XDKS.1 또는 Get XblSandbox cmdlet 실행

* zip 파일에서 사용 하도록 [https://aka.ms/xboxliveuwptools](https://aka.ms/xboxliveuwptools), PowerShell 창을 열고
    1. Run `Import-Module <path to unzipped folder>\XboxLivePsModule\XboxLivePsModule.psd1`
    2. 즉, 설정 XblSandbox XDKS.1 또는 Get XblSandbox cmdlet 실행

### <a name="command-prompt-script"></a>명령 프롬프트 스크립트

Xbox Live 도구에 있는 패키지를 다운로드 [https://aka.ms/xboxliveuwptools](https://aka.ms/xboxliveuwptools) 의 압축을 풉니다.  내 SwitchSandbox.cmd 배치 파일을 찾을 수 있습니다.

관리자 모드 전환에 샌드박스를에서이 호출을 실행 합니다.  첫 번째 인수가 샌드박스를 보여 줍니다.  예를 들어 수행할 계산과 XDKS.1 샌드박스도 전환 하려는 경우:

```
SwitchSandbox.cmd XDKS.1
```

일반 정품으로 다시 전환 하려면 간단 하 게 제공 하는 두 번째 인수로 합니다.

```
SwitchSandbox.cmd RETAIL
```

## <a name="switch-your-xbox-one-console-development-sandbox"></a>Xbox One 콘솔 개발 샌드박스를 전환 합니다.

### <a name="using-windows-dev-portal"></a>Windows 개발자 포털을 사용 하 여

콘솔에 샌드박스를 변경 하는 Windows 개발자 포털을 사용할 수 있습니다.  이 작업을 수행 하려면 콘솔에서 "개발자 홈"로 이동 하 고 사용 하도록 설정 합니다.

그 후 콘솔에 연결 하려면 PC에서의 웹 브라우저에 IP 주소를 입력할 수 있습니다.  "Xbox Live" 클릭 하 고 있는 텍스트 상자에 샌드박스를 입력 한 다음 있습니다.

### <a name="using-xbox-one-manager"></a>Xbox One 관리자를 사용 하 여

Xbox One 관리자를 사용 하면 PC에서 콘솔의 특정 측면을 관리할 수 있습니다.  다시 부팅, 설치 된 앱 관리 및 샌드박스에 변경 포함 됩니다.

샌드박스를 변경 하 고 "설정..."로 이동 하 고 싶은 콘솔에서 마우스 오른쪽 단추로 클릭

그런 다음 있습니다 샌드박스를 입력할 수 있습니다.

### <a name="using-xbox-one-console-ui"></a>Xbox One 콘솔 UI를 사용 하 여

콘솔 오른쪽에서 개발 샌드박스를 변경 하려는 경우에 "설정"을 이동할 수 있습니다.  다음 "개발자 설정"로 이동 하 고에 샌드박스를 변경 하는 옵션이 표시 됩니다.

## <a name="sandbox-uses"></a>샌드박스 사용

### <a name="data-that-is-sandboxed"></a>샌드박스가 적용 되는 데이터
개발 프로세스 동안 팀에서 개발자 간의 액세스를 관리 하는 샌드박스 기능을 사용할 수 있습니다.  예를 들어 다음 개발 팀 및 테스터 간에 데이터를 분리 하는 것이 좋습니다.

샌드박스가 적용 된 데이터에는 다음이 포함 됩니다.
- 도전 과제, 순위표 및 통계 사용자 합니다.  다른 샌드박스를 하나의 샌드박스 내에서 사용자에 대 한 누적 도전 과제 번역 하지 마십시오.
- 멀티 플레이어와 연결 합니다.  사용자는 멀티 플레이어를 재생할 수 없고 사람과 게임이 다른 샌드박스 합니다.
- 서비스 구성 합니다.  하나의 샌드박스에서 제목에는 새 도전 과제를 추가 하는 경우 다른 샌드박스에서 표시 되지 않습니다.  이 모든 서비스 구성 데이터에 적용 됩니다.

비 샌드박스가 적용 된 데이터는 주로 소셜 정보입니다.  예를 들어 사용자가 다른 사용자와 같이, 샌드박스 독립적 관계는 작업입니다.

### <a name="examples"></a>예
예를 들면 여러 샌드박스를 사용 하는 이유의 이점을 이해를 돕기 위해 아래 제공 됩니다.

> **참고**: Xbox 크리에이터 스 프로그램에 있는 경우 하나의 샌드박스만 있을 수 있습니다.  여러 샌드박스를 만들 필요가 있는 경우 하세요에 적용 합니다 ID@Xbox 프로그램.

#### <a name="service-config-isolation"></a>서비스 구성 격리
위에서 설명한 대로 서비스 구성 특정 샌드박스입니다.  따라서 *개발* 샌드박스를 및 *테스트* 샌드박스를 할 수 있습니다.  테스터로 타이틀 빌드를 제공 하면 [서비스 구성](xbox-live-service-configuration.md) *테스트* 샌드박스를 게시 합니다.

그런 다음 한편,를 추가할 수 도전 과제, 또는 다른 멀티 플레이 세션 형식 *개발* 샌드박스 테스터에 게 표시 되는 서비스 구성을 영향을 주지 않고 합니다.

#### <a name="multiplayer"></a>멀티 플레이
*개발* 및 *테스트* 샌드박스를 사용 하 여 위의 예제를 수행 합니다.  아마도 서비스 구성을 샌드박스, 간에 동일 이지만, 개발자가 멀티 플레이 기능 만드는 서로 매치 메이 킹 테스트 하려면.  테스터는 멀티 플레이 테스트도 합니다.

다음과 같은 경우 별도로 문제를 디버깅 하는 것 있기 때문에 개발자가 테스터와 일치 하도록 Xbox Live 매치 메이 킹 서비스를 고 수 있습니다.  이러한 문제를 방지 하는 좋은 방법 *개발* 샌드박스를 별도 *테스트* 샌드박스에서 되도록 테스터 되도록 개발자를 위한 것입니다.  이렇게 하면 두 그룹 모두 격리 됩니다.

## <a name="advanced"></a>고급

개발 프로세스를 단순하게 유지 하기 위해 기본 샌드박스에으로 시작 하 고 새 샌드박스를 신중 하 게 추가 합니다.

[고급 Xbox Live 샌드박스](advanced-xbox-live-sandboxes.md) 를 볼 수 사용자 액세스 제어 찾아 데이터 격리 성장 필요한 되 면 문서.  
