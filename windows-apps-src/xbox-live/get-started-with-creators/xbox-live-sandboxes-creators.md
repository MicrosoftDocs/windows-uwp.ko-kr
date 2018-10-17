---
title: Xbox Live 샌드박스
author: PhillipLucas
description: Xbox Live 샌드박스 소개
ms.assetid: e7daf845-e6cb-4561-9dfa-7cfba882f494
ms.author: kevinasg
ms.date: 10/30/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: b9175eda1d73a7678ac9fd304dc60ef228a57c7f
ms.sourcegitcommit: 1c6325aa572868b789fcdd2efc9203f67a83872a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/17/2018
ms.locfileid: "4741728"
---
# <a name="xbox-live-sandboxes-introduction"></a>Xbox Live 샌드박스 소개

[Xbox Live 서비스 구성](xbox-live-service-configuration-creators.md) 문서의 [Windows 개발자 센터](http://dev.windows.com)에서 타이틀에 대 한 정보를 구성 해야 하는 것 설명 했습니다. 이 정보는 통계, 순위표, 지역화, 등에 등이 포함 됩니다. 필요한 Xbox Live 서비스 구성 변경 게시 개발자 센터에서 개발 샌드박스에 변경 내용을 Xbox Live의 나머지 부분에서 획득 및 타이틀에 액세스할 수 있습니다.

개발 샌드박스를 사용 하면 격리 된 환경에서 타이틀에 변경 내용에 작업할 수 있습니다. 샌드박스 여러 가지 이점을 제공합니다.

1. 프로덕션 환경에서 유지 되는 버전 영향을 주지 않고 타이틀에 대 한 업데이트에 대 한 변경 내용에 반복할 수 있습니다.
2. 보안상의 이유로 일부 도구 에서만 개발 샌드박스에서 작동합니다.
3. 어떤 작업 중인 샌드박스에 대 한 액세스를 부여 하지 않고 다른 게시자 볼 수 없습니다.

기본적으로 Windows 10 Pc 및 Xbox One 본체 소매 샌드박스에서 됩니다. 사용자 PC 및/또는 Xbox One 개발 샌드박스 Xbox Live 서비스 구성의 해당 버전에 액세스할 수로 전환 해야 합니다. 이기를 취하도록 즐겨찾기 Xbox Live를 재생 하려면 게임 또는 소매점에서 무언가 테스트 하는 경우 장치를 다시 정품 샌드박스 변경 하려면 기억해 야 합니다.

## <a name="finding-out-about-your-sandbox"></a>샌드박스에 대해 알아보기

타이틀 만들기 샌드박스를 만들어집니다. **Windows 개발자 센터** 에서 제품을 열고 **서비스**를 탐색 하 여 샌드박스 ID를 찾을 수 > **Xbox Live**. 페이지 맨 위에 있는 **샌드박스 ID** 를 나열 됩니다.

![](../images/getting_started/devcenter_sandbox_id.png)

## <a name="switch-your-pcs-development-sandbox"></a>PC의 개발 샌드박스를 전환 합니다.
Unity, Windows 장치 포털 (WPD)를 사용 하 여 또는 명령줄을 통해 개발 샌드박스를 PC를 전환할 수 있습니다.

### <a name="unity"></a>Unity

#### <a name="prerequisites"></a>필수 구성 요소
Unity에서 개발 샌드박스 아웃 하기 전에 수행 해야 하는 다음 전환할 수 있습니다.

1. [구성 Xbox Live Unity에서](configure-xbox-live-in-unity.md)

#### <a name="switch-sandboxes"></a>스위치 샌드박스
기본 제공에서 Xbox Live 구성과 창 사용 하면 개발 및 소매 샌드박스 간에 쉽게 전환 합니다. 시작 하려면 **Xbox Live**로 이동 > 메뉴에서**구성** 합니다. **개발자 모드 구성** 섹션에 있는 현재 샌드박스를 볼 수 있습니다.

1. **개발자 모드** **활성화**라는 경우 다음 위치는 현재 연결 된 게임 개발 샌드박스입니다. 아웃 전환 하려면 **정품 모드로 다시 전환** 단추를 클릭할 수 있습니다.
2. **개발자 모드** **비활성화**라는 경우 다음 위치는 현재 정품 샌드박스입니다. 전환 하려면 **개발자 모드로 전환 하려면** 단추를 클릭할 수 있습니다.

![XBL 사용 하도록 설정](../images/unity/unity-xbl-dev-mode.PNG)

### <a name="windows-device-portal"></a>Windows Device Portal

#### <a name="prerequisites"></a>필수 구성 요소
Windows 장치 포털 (WPD)에 샌드박스를 전환 하기 전에 다음 요구 사항:

1. [Windows 바탕 화면에서 디바이스 포털 설정](https://msdn.microsoft.com/en-us/windows/uwp/debug-test-perf/device-portal-desktop)

#### <a name="switch-sandboxes"></a>스위치 샌드박스

1. [Windows 바탕 화면에서 디바이스 포털 설정](https://msdn.microsoft.com/en-us/windows/uwp/debug-test-perf/device-portal-desktop) 문서에 설명 된 대로 웹 브라우저에서 연결 하 여 **Windows 개발자 포털** 을 엽니다.
2. **Xbox Live**를 클릭 합니다.
3. 텍스트 필드에서 개발 샌드박스를 입력 하 고 **변경**을 클릭 합니다.

![](../images/getting_started/wdp_switch_sandbox.png)

일반 정품으로 다시 전환 하려면 여기 소매를 입력할 수 있습니다.

### <a name="command-line"></a>명령줄

#### <a name="prerequisites"></a>필수 구성 요소
명령줄을 통해 개발 샌드박스 아웃 하기 전에 수행 해야 하는 다음 전환할 수 있습니다.

1. Xbox Live 도구에 있는 패키지를 다운로드 [https://aka.ms/xboxliveuwptools](https://aka.ms/xboxliveuwptools) 의 압축을 풉니다.

#### <a name="switch-sandboxes"></a>스위치 샌드박스
1. **관리자**모드로 SwitchSandbox.cmd 배치 파일을 실행 합니다.

관리자 모드에 샌드박스를 전환 하려면이 호출을 실행 합니다. 첫 번째 인수는 샌드박스 합니다. 예를 들어 MJJSQH.58 샌드박스도 전환 하려는 경우이 명령을 사용 합니다.

```cmd
SwitchSandbox.cmd MJJSQH.58
```

일반 정품으로 다시 전환 하려면 간단 하 게 제공 하는 두 번째 인수로 합니다.

```cmd
SwitchSandbox.cmd RETAIL
```

## <a name="switch-your-xbox-one-console-development-sandbox"></a>Xbox One 콘솔 개발 샌드박스를 전환 합니다.

### <a name="using-xbox-dev-portal"></a>Xbox 개발자 포털을 사용 하 여

콘솔에 샌드박스를 변경 하는 Xbox 개발자 포털을 사용할 수 있습니다. 이렇게 하려면 [개발자 홈](https://docs.microsoft.com/windows/uwp/xbox-apps/dev-home) 으로 이동 콘솔 및 [디바이스 포털을 사용](https://docs.microsoft.com/windows/uwp/debug-test-perf/device-portal-xbox)합니다. Xbox 개발자 포털 열기 있는 경우:

2. **Xbox Live**를 클릭 합니다.
3. 텍스트 필드와 병아리 **변경**에서 개발 샌드박스를 입력 합니다.

![](../images/getting_started/xdp_switch_sandbox.png)

### <a name="using-xbox-one-console-ui"></a>Xbox One 콘솔 UI를 사용 하 여

콘솔에서 직접 샌드박스를 변경 하려면 [개발자 홈](https://docs.microsoft.com/windows/uwp/xbox-apps/dev-home) 을 사용할 수 있습니다.

1. **변경 샌드박스**, **바로 가기**아래를 클릭 합니다.
2. 샌드박스 ID를 입력 하 고 **저장 하 고 다시 시작**을 클릭 합니다.
