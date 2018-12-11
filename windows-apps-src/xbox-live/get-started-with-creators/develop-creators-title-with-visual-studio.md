---
title: Visual Studio로 크리에이터 스 타이틀 개발
description: Visual Studio를 사용 하 여 Xbox Live 크리에이터 스 프로그램 타이틀 개발 시작
ms.assetid: 6952dac0-66ff-4717-b3c7-8b3792e834e3
ms.date: 11/28/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, 하나는 xbox, xbox live 크리에이터 스, visual studio
ms.localizationpriority: medium
ms.openlocfilehash: e52555afd94edda3fc7cefe7a46e51be175b26d9
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/11/2018
ms.locfileid: "8918128"
---
# <a name="get-started-developing-an-xbox-live-creators-program-title-with-visual-studio"></a>Visual Studio를 사용 하 여 Xbox Live 크리에이터 스 프로그램 타이틀 개발 시작

> [!NOTE]
> Unity를 사용 하 여 개발 중인 게임에 사용할 수 있는 플러그 인 경우. 자세한 내용은 [Unity로 크리에이터 스 타이틀 개발](develop-creators-title-with-unity.md) 문서를 참조 하세요.

## <a name="requirements"></a>요구 사항

1. **[파트너 센터 개발자 프로그램](https://developer.microsoft.com/store/register)** 에 등록 합니다.
2. **[Windows 10](https://microsoft.com/windows)** 입니다.
3. **[Visual Studio 2015](https://www.visualstudio.com/)** (이상)는 **유니버설 Windows 앱 개발 도구**입니다.
4. ** [Windows 10 SDK](https://developer.microsoft.com/windows/downloads/windows-10-sdk) v10.0.10586.0** 이상.

> [!IMPORTANT]
> Windows 10 SDK 버전 10.0.15063.0 (크리에이터 스 업데이트 라고도 함)를 사용 하는 경우 visual Studio 2017이 필요 이상.

## <a name="create-a-new-product-in-partner-center"></a>파트너 센터에서 새 제품 만들기

모든 Xbox Live 타이틀 전에 로그인 및 Xbox Live 서비스를 호출 하는 수 있게 됩니다 [파트너 센터](https://partner.microsoft.com/dashboard) 에서 만든 제품이 있어야 합니다. 자세한 내용은 [새 크리에이터 스 타이틀 만들기](create-and-test-a-new-creators-title.md) 를 참조 하세요.

## <a name="configuring-your-development-device"></a>개발 장치 구성

성공적으로 수 있도록 장치에서 필요한 다음 사전 설정 단계는 Xbox Live와 다양 한 Xbox Live 서비스 호출을 사용 하 여 로그인 합니다.

### <a name="set-your-sandbox"></a>에 샌드박스를 설정 합니다.

샌드박스 [Xbox Live 서비스 구성](xbox-live-service-configuration-creators.md) 타이틀을 해제할 준비가 될 때까지 정품에서 격리를 유지 하는 방법을 제공 합니다. 누적 될 수 있는 약간의 데이터는 샌드박스 관련이 있습니다. 예를 들어 타이틀 라는 *헤드샷*는 통계를 정의 하 고 타이틀을 테스트 하는 동안 사용자 계정에 헤드샷 개의 누적 가정해 봅니다. 이 값은 타이틀의 개발 샌드박스를 특정 되며는 헤드샷을 통해 수행 하지는 타이틀의 일반 정품 버전으로 전환 하는 경우.

에 샌드박스를 설정 하는 방법을 확인 하 고 자세한 [Xbox Live 샌드박스](xbox-live-sandboxes-creators.md) 문서를 참조 하세요.

### <a name="sign-in-with-an-xbox-live-account-that-has-been-authorized-for-testing"></a>로그인 권한이 있는 Xbox Live 계정으로 테스트

에 로그인 하 여 개발 샌드박스를 하는 일반 MSA (Microsoft 계정)에 대 한 액세스에 샌드박스를 제공 해야 합니다. 이 일부 기타 혜택 뿐만 아니라 개발, 제목에 향상 된 보안을 제공합니다.

테스트 계정 및 만드는 방법에 대 한 자세한 내용은 참조 [환경에서 테스트에 대 한 Xbox Live 계정에 권한을 부여](authorize-xbox-live-accounts.md)합니다.

## <a name="visual-studio-project-setup"></a>Visual Studio 프로젝트 설정

### <a name="1-open-a-uwp-project"></a>1. UWP 프로젝트를 엽니다.
기존 UWP 프로젝트 아직 없는 경우 다음을 실행 하 여 만들 수 있습니다.

1. Visual Studio, **파일**에서 > **새** > **프로젝트**입니다.
2. **새 프로젝트** 대화 상자를 선택 하는 **Visual C#** > **Windows** > **유니버설** 노드 왼쪽된 창에서 오른쪽 창에서 **빈 앱 (유니버설 Windows)를** 클릭 합니다.
3. 대화 상자 아래쪽에는 프로젝트에 이름을 지정 하 고 프로젝트의 위치를 지정 합니다.
4. Windows 10의 최소 버전과 대상 버전을 지정 SDK입니다. [UWP 버전 선택](https://docs.microsoft.com/windows/uwp/updates-and-versions/choose-a-uwp-version) 에 대 한 자세한 내용은 참조 하세요.

![VS에서 프로젝트 만들기](../images/getting_started/vs-create-project.gif)

> [!NOTE]
> > Xbox Live API (XSAPI)는 최소 버전 10.0.10586.0 필요 이상.

### <a name="2-add-references-to-the-xbox-live-api-xsapi-in-your-project"></a>2. 프로젝트에서 Xbox Live API (XSAPI)에 대 한 참조를 추가 합니다.
Xbox 서비스 API c + + 및 WinRT 특성은 있고 해당 명명 **로 구성 된 Microsoft.Xbox.Live.SDK.*. UWP**. 자세한 내용은에서 Xbox One의 UWP를 실행 하는 방법에 대 한 [https://docs.microsoft.com/windows/uwp/xbox-apps/getting-started](https://docs.microsoft.com/windows/uwp/xbox-apps/getting-started). WinRT SDK로 작업 하는 경우 c + + 게임 엔진에 사용할 수는 c + + SDK c + +, C# 또는 JavaScript로 작성 된 게임 엔진입니다. C + + 엔진을 사용 하 여 WinRT을 사용할 때 사용 하 여 C + + CX hat (^)을 사용 합니다. C + +는 c + + 게임 엔진에 사용 하도록 권장된 API입니다.  

프로젝트에서 Xbox Live API를 사용 하려면 NuGet 패키지를 사용 하거나 API 소스 추가 하거나 이진 파일에 대 한 참조를 추가할 수 있습니다. NuGet 패키지를 추가하면 쉽게 컴파일하고 소스를 추가하면 쉽게 디버깅할 수 있습니다. 이 문서에서는 NuGet 패키지를 사용 하 여 안내 합니다. 소스를 사용 하려는 경우 다음 참조 [는 Xbox Live Api 소스 UWP 프로젝트의 컴파일](../get-started-with-partner/add-xbox-live-apis-source-to-a-uwp-project.md)합니다. 하 여 Xbox Live SDK NuGet 패키지를 추가할 수 있습니다.

1. Visual Studio에서 **도구**를 이동 > **NuGet 패키지 관리자** > **... 솔루션용 NuGet 패키지 관리**합니다.
2. NuGet 패키지 관리자에서 **찾아** 클릭 하 고 **Xbox.Live.SDK** 검색 상자에 입력 합니다.
3. 왼쪽의 목록에서 사용할 수 있는 Xbox Live sdk 버전을 선택 합니다. 이 경우 Microsoft.Xbox.Live.SDK.WinRT.UWP 패키지를 사용 합니다.
3. 창의 오른쪽에서 프로젝트 옆의 확인란을 선택 하 고 **설치**를 클릭 합니다.

![NuGet 통해 XBL 추가](../images/getting_started/vs-add-nuget-xbl.gif)

#### <a name="optionally-include-xsapi-header-in-your-project"></a>필요에 따라 프로젝트에 XSAPI 헤더를 포함

포함 해야 Microsoft.Xbox.Live.SDK.Cpp.* 기반 프로젝트에 대 한 `xsapi\\services.h` Xbox Live 서비스 API (XSAPI) NuGet에 대 한 헤더에서를 c + + 프로젝트에서 패키지를 합니다. 정의 해야 XSAPI 헤더를 포함 하기 전에 `XBOX_LIVE_CREATORS_SDK`. Xbox Live 크리에이터 스 프로그램 개발자가 사용할 수 있는 api만 API 노출 영역을 제한 합니다. 예를 들면 다음과 같습니다.

```c++
#define XBOX_LIVE_CREATORS_SDK
#include "xsapi\services.h"
```
### <a name="3-optional-using-connected-storage"></a>3. (선택 사항)를 사용 하 여 연결 된 저장소
액세스 해야 [연결 된 저장소](../storage-platform/connected-storage/connected-storage-technical-overview.md) 서비스를 사용 하려는 경우는 `Windows.Gaming.XboxLive.Storage` 네임 스페이스입니다. 사용 중인 Windows sdk 버전에 따라 추가 콘텐츠를 설치 하거나 수동으로 사용 하도록 프로젝트에 대 한 참조를 추가 해야 합니다. Windows 10 SDK 10.0.16299를 대상으로 한 하거나 이상 다음 됩니다 추가 작업을 수행 하지 않고 연결 된 저장소 네임 스페이스에 액세스할 수 있습니다.

#### <a name="windows-10-sdk-version-10015063-or-lower"></a>Windows 10 SDK 버전 10.0.15063 또는 아래
연결 된 저장소를 사용 하려면 프로젝트에 대 한 참조를 추가 하기 전에 Xbox Live 플랫폼 확장 SDK를 설치 해야 합니다. 하 여이 수행할 수 있습니다.

1. 다운로드 하 고 [Xbox Live 플랫폼 확장 SDK](http://aka.ms/xblextsdk)를 추출 합니다.
2. 추출 되 면 Windows 10 SDK 버전을 사용 하는 일치 하는 포함 된 MSI 파일을 실행 합니다.

Xbox Live 플랫폼 확장 SDK를 설치한 후 Visual Studio에서에 대 한 참조를 추가 해야 합니다. 하 여이 수행할 수 있습니다.

1. **솔루션 탐색기**에서 **참조** 노드를 마우스 오른쪽 단추로 클릭 하 고 **참조 추가...** 를 선택 합니다.
2. **참조 관리자** 대화 상자의 왼쪽에서 **유니버설 Windows**선택 > **확장**합니다.
3. 표시 되는 목록에서 **UWP 용 Windows 데스크톱 확장** 에 대 한 검색 하 고 Windows 10 SDK와 일치 하는 버전 옆에 있는 확인란을 선택 합니다.
4. **확인**을 클릭합니다.

![VS에서 새 참조 추가](../images/getting_started/get-started-vs-add-ref.png)

### <a name="4-associate-your-visual-studio-project-with-your-uwp-app"></a>4. UWP 앱을 사용 하 여 Visual Studio 프로젝트를 연결 합니다.

게임 수에 대 한 로그인을 파트너 센터에서 만든 제품 연결 되어야 합니다. 스토어 연결 마법사를 사용 하 여 Visual Studio에서 게임을 연결할 수 있습니다. Visual Studio에서 다음을 수행 합니다.

1.  기본 프로젝트 (시작 프로젝트)를 마우스 오른쪽 단추로 클릭, **저장소**를 클릭 > **... 스토어에 앱 연결**
2.  로그인 요청 하 고 지시에 따라 앱을 만드는 데 **Windows 개발자 계정** 으로 합니다.

> [!TIP]
> [앱 패키징](https://docs.microsoft.com/windows/uwp/packaging/) 게임을 준비 하 고 Windows 스토어에 대 한 자세한 내용은 참조 하세요.

### <a name="5-add-internet-capabilities-to-your-visual-studio-project"></a>5. Visual Studio 프로젝트에 인터넷 기능을 추가 합니다.
UWP 프로젝트를 인터넷 Xbox Live와 통신 하는 기능을 지정 해야 합니다. 하 여 이러한 속성을 설정할 수 있습니다.

1. **매니페스트 디자이너**를 엽니다 Visual Studio에서 **package.appxmanifest** 파일을 두 번 클릭 합니다.
2. **기능** 탭 클릭 하 고 **인터넷 (클라이언트)를**확인 합니다.

![VS에서 새 참조 추가](../images/getting_started/get-started-vs-add-capability.png)

### <a name="6-associate-your-visual-studio-project-with-your-xbox-live-enabled-title"></a>6. Xbox Live가 지원 타이틀을 Visual Studio 프로젝트를 연결 합니다.

Xbox Live 서비스에 게 서비스 구성 파일을 프로젝트에 추가 해야 합니다. 이렇게 하 여 쉽게 수행할 수 있습니다.

1. 시작 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 **추가**선택 > **새 항목**입니다.
2. **텍스트 파일** 형식을 선택 하 고 **xboxservices.config**로 이름을 지정 합니다.
3. 파일을 마우스 오른쪽 단추로 클릭 하 고 **속성** 선택 되었는지 확인 합니다.
    1. **빌드 작업** 은 **콘텐츠**를 설정 하 고  
    2. **출력 디렉터리로 복사** **항상 복사로**설정 됩니다.
5.  타이틀을 적용할 수 있는 값을 사용 하 여 **TitleId** 및 **PrimaryServiceConfigId** 대체 다음 템플릿 사용 하 여 구성 파일을 편집 합니다. 파트너 센터에서 루트 Xbox Live 페이지에서 올바른 값을 가져올 수 있습니다. **PrimaryServiceConfigId** **서비스 안내**으로 파트너 센터에 표시 됩니다.

```json
    {
       "TitleId" : "your title ID (JSON number in decimal)",
       "PrimaryServiceConfigId" : "your primary service config ID",
       "XboxLiveCreatorsTitle" : true
    }
```

예를 들면 다음과 같습니다.

```json
    {
        "TitleId" : 1563044810,
        "PrimaryServiceConfigId" : "12200100-88da-4d8b-af88-e38f5d2a2bca",
        "XboxLiveCreatorsTitle" : true
    }
```

> [!TIP]
> Xboxservices.config 내의 모든 값은 대/소문자 구분 합니다. TitleID와 PrimaryServiceConfigId 얻기 대 한 자세한 내용은 [서비스 구성을](../xbox-live-service-configuration.md) 참조 하세요.

## <a name="learn-more"></a>자세히 알아보기

Xbox Live 크리에이터 스 프로그램의 개발자 들에 사용할 수 있는 Api 쇼케이스에서 [Xbox Live SDK 샘플](https://github.com/Microsoft/xbox-live-samples/tree/master/Samples/CreatorsSDK) 입니다. 샘플을 사용 하려면 XDKS.1 샌드박스에 변경 해야 합니다.
