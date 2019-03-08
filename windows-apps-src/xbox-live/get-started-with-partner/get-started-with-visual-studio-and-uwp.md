---
title: UWP 게임을 위한 Visual Studio를 사용 하 여 시작
description: UWP 게임에 Xbox Live를 사용 하려면 Visual Studio 프로젝트를 설정 하는 방법 알아보기
ms.assetid: b53bc91f-79db-4d8f-8919-b9144e2d609b
ms.date: 11/28/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 314d5e937bb8680dc26b7dfdfa15ff90f8f26c81
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57651408"
---
# <a name="get-started-using-visual-studio-for-uwp-games"></a>UWP 게임을 위한 Visual Studio를 사용 하 여 시작

## <a name="requirements"></a>요구 사항

1. 등록 합니다  **[파트너 센터 개발자 프로그램](https://developer.microsoft.com/store/register)** 합니다.
2. **[Windows 10](https://microsoft.com/windows)**.
3. **[Visual Studio](https://www.visualstudio.com/)**  사용 하 여 합니다 **유니버설 Windows 앱 개발 도구**합니다. 최소는 UWP 앱의 버전은 Visual Studio 2015 업데이트 3 필요 합니다. 개발자와 보안 업데이트에 대 한 Visual Studio의 최신 릴리스를 사용 하는 것이 좋습니다. 
4. **[Windows 10 SDK](https://developer.microsoft.com/windows/downloads/windows-10-sdk) v10.0.10586.0** 이상.

> [!IMPORTANT]
> Visual Studio 2017은 Windows 10 SDK 버전 10.0.15063.0 (Creators Update 라고도 함)를 사용 하는 경우에 필요 이상.

## <a name="create-a-new-product-in-partner-center"></a>파트너 센터에서 새 제품 만들기

모든 Xbox Live 제목에서 만든 제품이 있어야 [파트너 센터](https://partner.microsoft.com/dashboard) 로그인 및 Xbox Live 서비스를 호출 하는 일을 할 수 있습니다. 참조 [UDC에 제목을 만들](create-a-new-title.md) 자세한 내용은 합니다.

## <a name="configuring-your-development-device"></a>개발 장치를 구성합니다.

성공적으로 수행할 수 있도록 사용자의 장치에 필요한 다음 예비 설치 단계는 Xbox Live 및 다양 한 Xbox Live 서비스 호출을 사용 하 여 로그인 합니다.

### <a name="set-your-sandbox"></a>샌드박스에 설정

샌드박스 유지 하는 방법을 제공 하 [Xbox Live 서비스 구성을](../xbox-live-service-configuration.md) 을 제목 릴리스 준비가 될 때까지 소매점에서 격리 합니다. 누적 되 면 하는 일부 데이터는 샌드박스와 관련이 있습니다. 예를 들어 제목에는 상태를 정의 하는 호출 *헤드샷*를 하 고 제목을 테스트 하는 동안 특정 개수의 헤드샷 사용자 계정에서를 누적 합니다. 이 값을 제목 개발 샌드박스가 관련 되 고 재생을 제목의 정품 버전으로 전환한 경우를 헤드샷은 전달 되지 합니다.

참조 된 [Xbox Live 샌드박스](../xbox-live-sandboxes.md) 문서를 자세히 알아보고 샌드박스에 설정 하는 방법을 참조 하세요.

### <a name="sign-in-with-a-test-account"></a>테스트 계정으로 로그인

에 로그인 하 여 개발 샌드박스가, 하려면 테스트 계정을 만들려면 하거나 샌드박스에 일반 Microsoft 계정 (MSA) 액세스에 대 한 프로 비전 합니다. 이 프로그램 타이틀 다른 이점을 뿐만 아니라 개발에 대 한 향상 된 보안을 제공합니다.

테스트 계정 및 만드는 방법에 대 한 자세한 내용은를 참조 하세요. [Xbox Live 테스트 계정](../xbox-live-test-accounts.md)

## <a name="visual-studio-project-setup"></a>Visual Studio 프로젝트 설치

### <a name="1-open-a-uwp-project"></a>1. UWP 프로젝트를 열
기존 UWP 프로젝트를 아직 없는 경우 다음을 수행 하 여 만들 수 있습니다.

1. Visual Studio에서 **파일** > **새** > **프로젝트**합니다.
2. 에 **새 프로젝트** 대화 상자를 선택 합니다 **Visual C#**   >  **Windows** > **유니버설** 노드 왼쪽된 창에서 클릭 **비어 있는 앱 (유니버설 Windows)** 오른쪽 창에서.
3. 대화 상자 아래쪽에서 프로젝트 이름을 지정 하 고 프로젝트의 위치를 지정 합니다.
4. 대상 버전 및 Windows 10의 최소 버전 지정 SDK. 참조 [UWP 버전 선택](https://docs.microsoft.com/windows/uwp/updates-and-versions/choose-a-uwp-version) 자세한 내용은 합니다.

![VS에서 프로젝트 만들기](../images/getting_started/vs-create-project.gif)

> [!NOTE]
> 필요한 최소 버전 10.0.10586.0 Xbox Live API (XSAPI) 이상.

### <a name="2-add-references-to-the-xbox-live-api-xsapi-in-your-project"></a>2. 프로젝트에서 Xbox Live API (XSAPI)에 참조를 추가 합니다.

UWP와 XDK, 및 c + + 및 WinRT Xbox 서비스 API는 버전으로 제공 하 고 해당 네임 스페이스도 구조화 **Microsoft.Xbox.Live.SDK.* 합니다. UWP** 고 **Microsoft.Xbox.Live.SDK.* 합니다. XboxOneXDK**합니다.

1. **UWP** PC, Xbox One 또는 Windows Phone 실행할 수 있는 UWP 게임을 작성 중인 개발자입니다.
2. **XboxOneXDK** 용인지 ID@Xbox 및 Xbox One XDK 사용 하는 개발자를 관리 합니다.
3. WinRT SDK로 작업 하는 경우 c + + 게임 엔진에 대 한 c + + SDK를 사용할 수 있습니다는 c + +로 작성 된 게임 엔진에 대 한 C#, 또는 JavaScript입니다.
4. WinRT는 c + + 엔진을 사용 하 여 사용 하는 경우 사용 해야 C + + /cli CX hats (^)를 사용 합니다. C + +는 c + + 게임 엔진을 사용 하도록 권장 되는 API입니다.  

> [!TIP]
> 자세한 내용은 UWP에서 Xbox One에서 실행 하는 방법에 대 한 [Xbox One에서 UWP 앱 개발을 시작 하기](https://docs.microsoft.com/windows/uwp/xbox-apps/getting-started).

프로젝트에서 Xbox Live API를 사용 하려면 NuGet 패키지를 사용 하거나 API 소스를 추가 하거나 이진 파일에 대 한 참조를 추가할 수 있습니다. NuGet 패키지를 추가하면 쉽게 컴파일하고 소스를 추가하면 쉽게 디버깅할 수 있습니다. 이 문서에서는 NuGet 패키지를 사용 하 여 안내 합니다. 원본을 사용 하려는 경우을 참조 하세요 [Xbox Live Api 소스에서 Your UWP 프로젝트를 컴파일](add-xbox-live-apis-source-to-a-uwp-project.md)합니다. Xbox Live SDK NuGet 패키지에서 추가할 수 있습니다.

1. Visual Studio로 이동 **도구가** > **NuGet 패키지 관리자** > **솔루션용 NuGet 패키지 관리...** .
2. NuGet 패키지 관리자에서 클릭 **찾아보기** enter **Xbox.Live.SDK** 검색 상자에 있습니다.
3. 왼쪽 목록에서 사용 하려는 Xbox Live SDK의 버전을 선택 합니다. 이 경우 Microsoft.Xbox.Live.SDK.WinRT.UWP 패키지를 사용 됩니다.
3. 창의 오른쪽에서 프로젝트 옆의 확인란을 클릭 **설치**합니다.

![NuGet 통해 XBL 추가](../images/getting_started/vs-add-nuget-xbl.gif)


> [!IMPORTANT]
> 에 대 한 `Microsoft.Xbox.Live.SDK.Cpp.*` 기반된 프로젝트 헤더를 포함 해야 `#include <xsapi\services.h>` 프로젝트의 원본에 있습니다.

### <a name="3-optional-using-connected-storage-andor-secure-sockets"></a>3. (선택 사항) 연결 된 저장소 및/또는 보안 소켓 사용
사용 하는 Windows SDK의 버전에 따라 추가 콘텐츠를 설치 하거나 수동으로 Xbox Live를 사용 하려면 프로젝트에 대 한 참조를 추가 해야 할 수 있습니다 [연결 된 저장소](../storage-platform/connected-storage/connected-storage-technical-overview.md) 또는 소켓 보호 합니다. 연결 된 저장소 기능을 사용 하려는 경우에 액세스 해야 합니다는 `Windows.Gaming.XboxLive.Storage` 네임 스페이스입니다. 액세스 해야 보안 소켓을 사용 하려는 경우 `Windows.Networking.XboxLive`합니다.

#### <a name="windows-10-sdk-version-10016299-or-higher"></a>Windows 10 SDK 버전 10.0.16299 이상
Windows 10 SDK 10.0.16299를 대상으로 한 경우 그 이상가 됩니다 추가 작업을 수행 하지 않고 저장소 연결 된 네임 스페이스에 액세스할 수 있습니다. 보안 소켓에 액세스 하려면에 대 한 참조를 추가 해야 합니다 **UWP 용 Windows 데스크톱 확장**합니다. 이 수행할 수 있습니다.

1. 에 **솔루션 탐색기**, 마우스 오른쪽 단추로 클릭 합니다 **참조** 노드와 선택 **참조 추가...**
2. 왼쪽에는 **참조 관리자** 대화 상자에서 **유니버설 Windows** > **확장**합니다.
3. 표시 되는 목록에서 검색할 **UWP 용 Windows 데스크톱 확장** 맞는 Windows 10 SDK 버전 옆에 있는 확인란을 선택 합니다.
4. **확인**을 클릭합니다.

![VS에서 새 참조 추가](../images/getting_started/get-started-vs-add-ref.png)

#### <a name="windows-10-sdk-version-10015063-or-lower"></a>Windows 10 SDK 버전 10.0.15063 이하인
저장소 연결 또는 보안 소켓을 사용 하려는 경우 프로젝트에 대 한 참조를 추가 하기 전에 Xbox Live 플랫폼 확장 SDK를 설치 해야 합니다. 이 수행할 수 있습니다.

1. 다운로드 하 고 추출 합니다 [Xbox Live 플랫폼 확장 SDK](https://aka.ms/xblextsdk)합니다.
2. 추출 후 사용 중인 Windows 10 SDK 버전과 일치 하는 포함된 된 MSI 파일을 실행 합니다.

Xbox Live 플랫폼 확장 SDK를 설치한 후에 Visual Studio에서 해당 참조를 추가 해야 합니다. 이 수행할 수 있습니다.

1. 에 **솔루션 탐색기**, 마우스 오른쪽 단추로 클릭 합니다 **참조** 노드와 선택 **참조 추가...**
2. 왼쪽에는 **참조 관리자** 대화 상자에서 **유니버설 Windows** > **확장**합니다.
3. 표시 되는 목록에서 검색할 **UWP 용 Windows 데스크톱 확장** 맞는 Windows 10 SDK 버전 옆에 있는 확인란을 선택 합니다.
4. **확인**을 클릭합니다.

### <a name="4-associate-your-visual-studio-project-with-your-uwp-app"></a>4. UWP 앱을 사용 하 여 Visual Studio 프로젝트에 연결

게임을 할 수에 대 한 로그인을 파트너 센터에서 만든 제품을 사용 하 여 연결 되어야 합니다. 스토어 연결 마법사를 사용 하 여 Visual Studio에서 게임을 연결할 수 있습니다. Visual Studio에서 다음을 수행 합니다.

1.  기본 프로젝트 (시작 프로젝트를) 마우스 오른쪽 단추로 클릭, 클릭 **스토어** > **스토어와 앱을 연결 하는 중...**
2.  으로 로그인 합니다 **Windows 개발자 계정** 요청 하 고 프롬프트에 따라 앱을 만드는 데 사용 합니다.

> [!TIP]
> 참조 [앱 패키징](https://docs.microsoft.com/windows/uwp/packaging/) Windows 스토어 용 게임을 준비 하는 방법은 합니다.

### <a name="5-add-internet-capabilities-to-your-visual-studio-project"></a>5. Visual Studio 프로젝트에 인터넷 기능을 추가 합니다.
UWP 프로젝트는 Xbox Live 통신할 인터넷 기능을 지정 해야 합니다. 이러한 속성을 설정할 수 있습니다.

1. 두 번 클릭 합니다 **package.appxmanifest** 열려면 Visual Studio에서 파일을 **매니페스트 디자이너**합니다.
2. 클릭 합니다 **기능** 탭을 확인 **인터넷 (클라이언트)** 합니다.

![VS에서 새 참조 추가](../images/getting_started/get-started-vs-add-capability.png)

### <a name="6-associate-your-visual-studio-project-with-your-xbox-live-enabled-title"></a>6. Xbox Live를 사용 하도록 설정 제목과 함께 Visual Studio 프로젝트에 연결

Xbox Live 서비스에 게 서비스 구성 파일을 프로젝트에 추가 해야 합니다. 이 하 여 쉽게 수행할 수 있습니다.

1. 시작 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 선택 **추가** > **새 항목**합니다.
2. 선택 된 **텍스트 파일** 입력 하 고 이름을 **xboxservices.config**합니다.
3. 파일을 마우스 오른쪽 단추로 클릭 **속성** 있는지 확인 합니다.
    1. **빌드 작업** 로 설정 된 **콘텐츠**, 및  
    2. **출력 디렉터리로 복사** 로 설정 된 **항상 복사**합니다.
5.  다음 템플릿 사용 하 여 구성 파일을 편집 대체는 **TitleId** 하 고 **PrimaryServiceConfigId** 을 제목에 적용할 값을 사용 하 여 합니다. 파트너 센터에서 루트 Xbox Live 페이지에서 올바른 값을 가져올 수 있습니다. 합니다 **PrimaryServiceConfigId** 파트너 센터에 표시 됩니다 **서비스 안내**합니다.

```json
    {
       "TitleId" : "your title ID (JSON number in decimal)",
       "PrimaryServiceConfigId" : "your primary service config ID"
    }
```

예를 들어 다음과 같은 가치를 제공해야 합니다.

```json
    {
        "TitleId" : 1563044810,
        "PrimaryServiceConfigId" : "12200100-88da-4d8b-af88-e38f5d2a2bca"
    }
```

> [!TIP]
> Xboxservices.config 내 모든 값은 대/소문자 구분입니다. 참조 [서비스 구성](../xbox-live-service-configuration.md) TitleID 및 PrimaryServiceConfigId를 얻는 방법에 대 한 자세한 내용은 합니다.

### <a name="7-optional-add-multiplayer-capabilities"></a>7. (선택 사항) 멀티 플레이 기능 추가

추가 하려는 경우 멀티 플레이 게임을 제목에 지원 하 고 플레이어가 멀티 플레이 게임을 다른 사용자를 초대 하는 기능을 구현 하려면 다음 AppXManifest 파일에 다른 필드를 추가 해야 합니다. 참조 [여 AppXManifest에 대 한 멀티 플레이 게임 구성](../multiplayer/service-configuration/configure-your-appxmanifest-for-multiplayer.md) 자세한 내용은 합니다.

## <a name="learn-more"></a>자세히 알아보기

합니다 [Xbox Live SDK 샘플](https://github.com/Microsoft/xbox-live-samples) Xbox Live Api를 사용 하는 방법을 참조 하는 좋은 방법 이며 개발자에 게 사용 가능한 Api를 소개 합니다 ID@Xbox 프로그램입니다. 샘플을 사용 하려면 샌드박스에 XDKS.1 변경 해야 합니다.
