---
title: Xbox Live 샌드박스
description: Xbox Live 샌드박스 소개
ms.assetid: e7daf845-e6cb-4561-9dfa-7cfba882f494
ms.date: 10/30/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 3f2e5496c9aab2629591a121850685e7f5a1b2fe
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57590228"
---
# <a name="xbox-live-sandboxes-introduction"></a>Xbox Live 샌드박스 소개

에 [Xbox Live 서비스 구성을](xbox-live-service-configuration-creators.md) 문서에 제목에 대 한 정보를 구성 해야 하 고 설명 했습니다 [파트너 센터](https://partner.microsoft.com/dashboard)합니다. 이 정보는 통계, 순위표, 지역화 등 등을 포함합니다. Xbox Live 서비스 구성 변경 전에 변경 내용을 Xbox Live의 나머지 부분에 의해 적용 됩니다 제목에 액세스할 수 있습니다 개발 샌드박스에 파트너 센터에서 게시 해야 합니다.

개발 샌드박스가 사용 하면 격리 된 환경에서 프로그램 제목 변경 내용에 대해 작업할 수 있습니다. 샌드박스는 다음과 같은 몇 가지 장점이 있습니다.

1. 프로덕션 환경에서 유지 되는 버전에 영향을 주지 않고을 제목에 대 한 업데이트의 변경 내용에 대해 반복할 수 있습니다.
2. 보안상의 이유로으로 일부 도구는 개발 샌드박스가으로 작동합니다.
3. 다른 게시자는 샌드박스에 대 한 액세스를 부여 하지 않고 작업할 무엇을 볼 수 없습니다.

기본적으로 Windows 10 Pc 및 Xbox One 콘솔 소매 샌드박스에서 됩니다. 개발 샌드박스가 Xbox Live 서비스 구성의 해당 버전에 액세스 하 여 PC 및/또는 Xbox One 전환 해야 합니다. 소매점에서 테스트 또는 즐겨 찾는 Xbox Live 재생에 중단을 게임을 하는 경우 장치 소매 샌드박스에 다시 변경 해야 하는 것이 반드시 합니다.

## <a name="finding-out-about-your-sandbox"></a>샌드박스에 대해 알아보기

제목을 만들면 샌드박스를 만들어집니다. 제품을 열어 샌드박스 ID를 찾을 수 있습니다 **파트너 센터** 로 이동 하 고 **Services** > **Xbox Live**합니다. 합니다 **샌드박스 ID** 페이지의 맨 위에 있는 나열 됩니다.

![](../images/getting_started/devcenter_sandbox_id.png)

## <a name="switch-your-pcs-development-sandbox"></a>PC의 개발 샌드박스가 전환
Unity, Windows Device Portal WPD ()를 사용 하 여 또는 명령줄을 통해 개발 샌드박스가에 PC를 전환할 수 있습니다.

### <a name="unity"></a>Unity

#### <a name="prerequisites"></a>필수 구성 요소
Unity에서 개발 샌드박스가 내부 및 외부로 전환 하기 전에 다음 요구 사항:

1. [구성 Xbox Live Unity에서](configure-xbox-live-in-unity.md)

#### <a name="switch-sandboxes"></a>샌드박스를 전환 합니다.
기본 제공에서 Xbox Live 구성 창 개발 및 소매 샌드박스 간에 쉽게 전환할 수 있습니다. 를 시작 하려면로 이동 **Xbox Live** > **구성** 메뉴에서. 현재 샌드박스를 볼 수 있습니다 합니다 **개발자 모드 구성** 섹션입니다.

1. 하는 경우 **개발자 모드** 라는 **사용 하도록 설정**, 연결 된 게임 개발 샌드박스에서 현재 됩니다. 클릭할 수는 **소매 모드로 다시 전환** 단추를 전환 합니다.
2. 하는 경우 **개발자 모드** 라는 **사용 하지 않도록 설정**, 소매 샌드박스에서 현재 됩니다. 클릭할 수는 **개발자 모드를 전환할** 스위치 인에 단추입니다.

![XBL 사용 하도록 설정](../images/unity/unity-xbl-dev-mode.PNG)

### <a name="windows-device-portal"></a>Windows Device Portal

#### <a name="prerequisites"></a>필수 구성 요소
샌드박스에의 Windows Device Portal WPD ()를 전환 하기 전에 다음 요구 사항:

1. [Windows 바탕 화면에서 장치 포털 설정](https://msdn.microsoft.com/en-us/windows/uwp/debug-test-perf/device-portal-desktop)

#### <a name="switch-sandboxes"></a>샌드박스를 전환 합니다.

1. 오픈 **Windows 개발자 포털** 에 설명 된 대로 웹 브라우저에서에 연결 하 여 합니다 [Windows 바탕 화면에서 장치 포털 설치](https://msdn.microsoft.com/en-us/windows/uwp/debug-test-perf/device-portal-desktop) 문서.
2. 클릭할 **Xbox Live**합니다.
3. 개발 샌드박스에 텍스트 필드에 입력 하 고 클릭 **변경**합니다.

![](../images/getting_started/wdp_switch_sandbox.png)

일반 정품으로 다시 전환 하려면 여기 소매를 입력할 수 있습니다.

### <a name="command-line"></a>명령줄

#### <a name="prerequisites"></a>필수 구성 요소
명령줄을 통해 개발 샌드박스가 내부 및 외부로 전환 하기 전에 다음 요구 사항:

1. Xbox Live 도구 패키지를 다운로드 [ https://aka.ms/xboxliveuwptools ](https://aka.ms/xboxliveuwptools) 압축을 풉니다.

#### <a name="switch-sandboxes"></a>샌드박스를 전환 합니다.
1. SwitchSandbox.cmd 배치 파일에서 실행할 **관리자 모드**합니다.

샌드박스에 전환 하려면 관리자 모드에서이 실행 합니다. 첫 번째 인수가 샌드박스를 보여 줍니다. 예를 들어 MJJSQH.58 샌드박스도 전환 하려는 경우이 명령을 사용 합니다.

```cmd
SwitchSandbox.cmd MJJSQH.58
```

일반 정품으로 전환할 제공 하기만 하면 됩니다 하는 두 번째 인수로 합니다.

```cmd
SwitchSandbox.cmd RETAIL
```

## <a name="switch-your-xbox-one-console-development-sandbox"></a>Xbox One 콘솔 개발 샌드박스에 전환

### <a name="using-xbox-dev-portal"></a>Xbox 개발자 포털을 사용 하 여

콘솔에 샌드박스를 변경 하려면 Xbox 개발자 포털을 사용할 수 있습니다. 이렇게 하려면로 이동 [개발 홈](https://docs.microsoft.com/windows/uwp/xbox-apps/dev-home) 콘솔에 및 [장치 포털을 사용 하도록 설정](https://docs.microsoft.com/windows/uwp/debug-test-perf/device-portal-xbox)합니다. 한 번 Xbox 개발자 포털 열기를 해야합니다.

2. 클릭할 **Xbox Live**합니다.
3. 개발 샌드박스에 병아리 텍스트 필드에 입력 **변경**합니다.

![](../images/getting_started/xdp_switch_sandbox.png)

### <a name="using-xbox-one-console-ui"></a>Xbox One 콘솔 UI를 사용 하 여

사용할 수 있습니다 [개발 홈](https://docs.microsoft.com/windows/uwp/xbox-apps/dev-home) 샌드박스 콘솔에 직접 변경 하려면:

1. 클릭 **변경 샌드박스**아래에 있는 **빠른 작업**합니다.
2. 샌드박스 ID를 입력 한 다음 클릭 **저장 및 다시 시작**합니다.

### <a name="sign-in-with-the-xbox-app"></a>Xbox 앱으로 로그인

개발을 제목에 대 한 적절 한 샌드박스를 사용 하는 PC 전환한 후 적합 한 테스트 계정을 사용 하 여 Xbox live에 로그인 하 고 확인 하는 것이 좋습니다. 에 로그인 하 여 이렇게 합니다 [Xbox Live 앱](https://www.xbox.com/en-US/xbox-app)합니다. 개발 환경을 원하는 샌드박스 Xbox 앱을 사용 하 여 시작 되 면 로그인 사용자가 동일한 제약 조건을 사용 하 여 모든 다른 Xbox Live로 서비스할 샌드박스를 실행 합니다. 따라서를 샌드박스에 대 한 올바른 계정을 사용 하 고 있는지 확인 하는 데 유용 합니다.
