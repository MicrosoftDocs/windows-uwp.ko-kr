---
description: Visual Studio에서 새 Windows 10 프로젝트를 만들고 파일을 복사 하 여 포팅 프로세스를 시작 합니다.
title: Windows Phone Silverlight 프로젝트를 UWP 프로젝트로 포팅
ms.assetid: d86c99c5-eb13-4e37-b000-6a657543d8f4
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 73f90cb52f0bb5756c4e9b67783dbfd286289377
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89167457"
---
# <a name="porting-windowsphone-silverlight-projects-to-uwp-projects"></a>Windows Phone Silverlight 프로젝트를 UWP 프로젝트로 포팅


이전 항목은 [네임 스페이스 및 클래스 매핑입니다](wpsl-to-uwp-namespace-and-class-mappings.md).

Visual Studio에서 새 Windows 10 프로젝트를 만들고 파일을 복사 하 여 포팅 프로세스를 시작 합니다.

## <a name="create-the-project-and-copy-files-to-it"></a>프로젝트를 만들고 파일을 복사 합니다.

1.  Microsoft Visual Studio 2015를 시작 하 고 새 빈 응용 프로그램 (Windows 유니버설) 프로젝트를 만듭니다. 자세한 내용은 [템플릿을 사용 하 여 Windows 런타임 8.x 앱 시작 (c #, c + +, Visual Basic)](/previous-versions/windows/apps/hh768232(v=win.10))을 참조 하세요. 새 프로젝트는 모든 장치 제품군에서 실행 되는 앱 패키지 (appx 파일)를 빌드합니다.
2.  Windows Phone Silverlight 앱 프로젝트에서 다시 사용할 모든 소스 코드 파일 및 시각적 자산 파일을 식별 합니다. 파일 탐색기를 사용 하 여 데이터 모델, 뷰 모델, 시각적 자산, 리소스 사전, 폴더 구조 및 다시 사용 하려는 기타 항목을 새 프로젝트에 복사 합니다. 필요에 따라 디스크에 하위 폴더를 복사 하거나 만듭니다.
3.  뷰 (예: MainPage 및)를 새 프로젝트 노드에 복사 합니다. 필요에 따라 새 하위 폴더를 만들고 프로젝트에서 기존 뷰를 제거 합니다. 그러나 Visual Studio에서 생성 한 뷰를 오버 작성 하거나 제거 하기 전에 나중에 참조 하는 것이 유용할 수 있으므로 복사본을 유지 합니다. Windows Phone Silverlight 앱을 이식 하는 첫 번째 단계는 한 장치 제품군에서 잘 작동 하 고 잘 작동 하도록 하는 데 중점을 두는 것입니다. 나중에 보기가 모든 폼 팩터에 제대로 어울리도록 조정하고 선택적으로 적응 코드를 추가하여 특정 디바이스 패밀리를 최대한 활용하도록 하는 데 집중할 수 있습니다.
4.  **솔루션 탐색기**에서 **모든 파일 표시**가 설정되어 있는지 확인합니다. 복사한 파일을 선택 하 고 마우스 오른쪽 단추를 클릭 한 다음 **프로젝트에 포함**을 클릭 합니다. 그러면 포함 하는 폴더가 자동으로 포함 됩니다. 그런 다음 원하는 경우 **모든 파일 표시** 를 설정/해제할 수 있습니다. 다른 워크플로를 사용 하는 것이 좋습니다. **기존 항목 추가** 명령을 사용 하 여 Visual Studio **솔루션 탐색기**에서 필요한 하위 폴더를 만들었습니다. 시각적 자산에서 **빌드 작업** 을 **내용** 으로 설정 하 고 **출력 디렉터리로 복사** 를 **복사 안 함**으로 설정 했는지 두 번 선택 합니다.
5.  네임 스페이스와 클래스 이름의 차이는이 단계에서 많은 빌드 오류를 생성 합니다. 예를 들어, Visual Studio에서 생성 한 보기를 열면 해당 보기는 **PhoneApplicationPage**가 아니라 [**페이지**](/uwp/api/Windows.UI.Xaml.Controls.Page)형식 이라고 표시 됩니다. 이 포팅 가이드의 다음 항목에서는 많은 XAML 태그와 명령적 코드의 차이점이 자세히 설명 되어 있습니다. 그러나 XAML 태그의 네임 스페이스 접두사 선언에서 "clr-namespace"를 "using"으로 변경 하는 것은 다음과 같은 일반적인 단계를 수행 하는 것과 같이 신속 하 게 진행 됩니다. [네임 스페이스 및 클래스 매핑](wpsl-to-uwp-namespace-and-class-mappings.md) 항목과 Visual Studio의 **찾기 및 바꾸기** 명령을 사용 하 여 소스 코드를 대량으로 변경 합니다 (예: "system.string"을 "system.xml"로 바꾸기). 그리고 Visual Studio의 명령적 코드 편집기에서 상황에 맞는 메뉴의 **Using** 확인 및 구성 명령을 사용 하 여 보다 구체적인 변경 내용을 **확인** 합니다.

## <a name="extension-sdks"></a>확장 SDK

이식 된 앱이 호출 하는 UWP (대부분의 유니버설 Windows 플랫폼) Api는 유니버설 장치 제품군 이라는 Api 집합에서 구현 됩니다. 하지만 일부는 확장 Sdk에서 구현 되며, Visual Studio는 앱의 대상 장치 제품군 또는 참조 한 확장 Sdk에 의해 구현 된 Api만 인식 합니다.

찾을 수 없는 네임 스페이스 또는 형식 또는 멤버에 대 한 컴파일 오류가 발생 하는 경우 이것이 원인일 수 있습니다. Api 참조 설명서에서 API 항목을 열고 요구 사항 섹션으로 이동 합니다. 그러면 구현 하는 장치 패밀리가 무엇 인지 알 수 있습니다. 대상 장치 패밀리가 아닌 경우 프로젝트에서 API를 사용할 수 있도록 하려면 해당 장치 제품군에 대 한 확장 SDK에 대 한 참조가 필요 합니다.

**프로젝트** &gt; **참조 추가** &gt; **Windows 유니버설** &gt; **확장** 을 클릭 하 고 적절 한 확장 SDK를 선택 합니다. 예를 들어 호출 하려는 Api가 모바일 장치 제품군 에서만 사용 가능 하 고 버전 10.0. x. y에서 도입 된 경우 **UWP에 대 한 Windows Mobile 확장**을 선택 합니다.

그러면 프로젝트 파일에 다음 참조가 추가 됩니다.

```XML
<ItemGroup>
    <SDKReference Include="WindowsMobile, Version=10.0.x.y">
        <Name>Windows Mobile Extensions for the UWP</Name>
    </SDKReference>
</ItemGroup>
```

이름 및 버전 번호는 SDK의 설치 된 위치에 있는 폴더와 일치 합니다. 예를 들어 위의 정보는 다음 폴더 이름과 일치 합니다.

`\Program Files (x86)\Windows Kits\10\Extension SDKs\WindowsMobile\10.0.x.y`

앱이 API를 구현 하는 장치 제품군을 대상으로 하지 않는 한, [**Apiinformation**](/uwp/api/Windows.Foundation.Metadata.ApiInformation) 클래스를 사용 하 여 api를 호출 하기 전에 해당 api의 존재 여부를 테스트 해야 합니다 (이를 적응형 코드 라고 함). 이 조건은 앱이 실행 되는 모든 위치에서 평가 되지만 API가 존재 하 여 호출할 수 있는 장치 에서만 true로 평가 됩니다. 먼저 범용 API가 있는지 확인 한 후 확장 Sdk 및 적응 코드를 사용 합니다. 몇 가지 예는 아래 섹션에 나와 있습니다.

또한 [앱 패키지 매니페스트](#the-app-package-manifest)를 참조 하세요.

## <a name="maximizing-markup-and-code-reuse"></a>태그 및 코드 재사용 극대화

간단한 리팩터링 (아래에 설명 되어 있음)을 추가 하면 모든 장치 패밀리에서 작동 하는 태그와 코드를 최대화할 수 있습니다. 자세한 내용은 다음과 같습니다.

-   모든 장치 패밀리에 공통적인 파일에는 특별 한 고려 사항이 필요 하지 않습니다. 이러한 파일은 앱이 실행 되는 모든 장치 패밀리의 앱에서 사용 됩니다. 여기에는 XAML 태그 파일, 명령적 소스 코드 파일 및 자산 파일이 포함 됩니다.
-   앱이 실행 중인 장치 패밀리를 검색 하 고 해당 장치 제품군 용으로 특별히 설계 된 보기를 탐색할 수 있습니다. 자세한 내용은 [앱이 실행 되는 플랫폼 검색](wpsl-to-uwp-input-and-sensors.md)을 참조 하세요.
-   응용 프로그램이 특정 장치 제품군에서 실행 되는 경우에만 런타임에 자동으로 로드 되도록 마크업 파일 또는 **ResourceDictionary** 파일 (또는 파일을 포함 하는 폴더)을 특수 한 이름으로 지정 하는 것이 유용할 수 있습니다. 이 기법은 [Bookstore1](wpsl-to-uwp-case-study-bookstore1.md) 사례 연구에 설명 되어 있습니다.
-   모든 장치 제품군 (예: 프린터, 스캐너 또는 카메라 단추)에서 사용할 수 없는 기능을 사용 하려면 적응 코드를 작성할 수 있습니다. 이 항목의 [조건부 컴파일 및 적응 코드](#conditional-compilation-and-adaptive-code) 에서 세 번째 예제를 참조 하세요.
-   Windows Phone Silverlight와 Windows 10을 모두 지원 하려는 경우 프로젝트 간에 소스 코드 파일을 공유할 수 있습니다. 방법: Visual Studio에서 **솔루션 탐색기**프로젝트를 마우스 오른쪽 단추로 클릭 하 고, **기존 항목 추가**를 선택 하 고, 공유할 파일을 선택한 후 **링크로 추가**를 클릭 합니다. 소스 코드 파일을 파일 시스템의 공용 폴더에 저장 합니다 .이 폴더에 연결 하는 프로젝트에서 해당 파일을 볼 수 있으며, 소스 제어에 추가 하는 것을 잊지 마세요. 모든 파일이 두 플랫폼에서 모두 작동 하는 경우를 위해 명령적 소스 코드를 지정할 수 있는 경우 두 개의 복사본이 있어야 합니다. 가능 하면 조건부 컴파일 지시문 내의 파일에서 플랫폼별 논리를 래핑할 수 있으며, 필요한 경우 런타임 조건을 래핑할 수 있습니다. 다음 섹션 및 [c # 전처리기 지시문](/dotnet/articles/csharp/language-reference/preprocessor-directives/index)을 참조 하세요.
-   소스 코드 수준 대신 이진 수준에서 다시 사용 하기 위해 Windows Phone Silverlight에서 사용할 수 있는 .NET Api의 하위 집합 및 Windows 10 앱 (.NET Core)의 하위 집합을 지 원하는 이식 가능한 클래스 라이브러리가 있습니다. 이식 가능한 클래스 라이브러리 어셈블리는 이러한 .NET 플랫폼 등의 이진과 호환 됩니다. Visual Studio를 사용 하 여 이식 가능한 클래스 라이브러리를 대상으로 하는 프로젝트를 만듭니다. [이식 가능한 클래스 라이브러리를 사용 하 여 플랫폼 간 개발](/dotnet/standard/cross-platform/cross-platform-development-with-the-portable-class-library)을 참조 하세요.

## <a name="conditional-compilation-and-adaptive-code"></a>조건부 컴파일 및 적응 코드

단일 코드 파일에서 Silverlight 및 Windows 10 Windows Phone를 모두 지원 하려는 경우이 작업을 수행할 수 있습니다. 프로젝트 속성 페이지에서 Windows 10 프로젝트를 살펴보면 프로젝트가 조건부 컴파일 기호로 WINDOWS UAP를 정의 하는 것을 확인할 수 있습니다 \_ . 일반적으로 다음 논리를 사용 하 여 조건부 컴파일을 수행할 수 있습니다.

```csharp
#if WINDOWS_UAP
    // Code that you want to compile into the Windows 10 app.
#else
    // Code that you want to compile into the Windows Phone Silverlight app.
#endif // WINDOWS_UAP
```

Windows Phone Silverlight 앱과 Windows 런타임 8gb 앱 간에 공유 하는 코드가 있는 경우 다음과 같은 논리를 포함 하는 소스 코드가 이미 있을 수 있습니다.

```csharp
#if NETFX_CORE
    // Code that you want to compile into the Windows Runtime 8.x app.
#else
    // Code that you want to compile into the Windows Phone Silverlight app.
#endif // NETFX_CORE
```

그렇다면 이제 Windows 10도 지원 하려는 경우이 작업을 수행할 수 있습니다.

```csharp
#if WINDOWS_UAP
    // Code that you want to compile into the Windows 10 app.
#else
#if NETFX_CORE
    // Code that you want to compile into the Windows Runtime 8.x app.
#else
    // Code that you want to compile into the Windows Phone Silverlight app.
#endif // NETFX_CORE
#endif // WINDOWS_UAP
```

조건부 컴파일을 사용 하 여 Windows Phone 하드웨어 뒤로 단추의 처리를 제한할 수 있습니다. Windows 10에서 뒤로 단추 이벤트는 범용 개념입니다. 하드웨어 또는 소프트웨어에서 구현 된 뒤로 단추는 처리할 [**요청**](/uwp/api/windows.ui.core.systemnavigationmanager.backrequested) 된 이벤트를 모두 발생 시킵니다.

```csharp
       Windows.UI.Core.SystemNavigationManager.GetForCurrentView().BackRequested +=
            this.ViewModelLocator_BackRequested;

...

    private void ViewModelLocator_BackRequested(object sender, Windows.UI.Core.BackRequestedEventArgs e)
    {
        // Handle the event.
    }

```

Windows Phone에 대 한 하드웨어 카메라의 처리를 제한 하기 위해 조건부 컴파일을 사용 했을 수 있습니다. Windows 10에서 하드웨어 카메라 단추는 모바일 장치 제품군에 특정 한 개념입니다. 하나의 앱 패키지는 모든 장치에서 실행 되므로, 적응 코드 라고 하는 기능을 사용 하 여 컴파일 시간 조건을 런타임 조건으로 변경 합니다. 이렇게 하려면 [**Apiinformation**](/uwp/api/Windows.Foundation.Metadata.ApiInformation) 클래스를 사용 하 여 런타임에 [**HardwareButtons**](/uwp/api/Windows.Phone.UI.Input.HardwareButtons) 클래스가 있는지 쿼리 합니다. **HardwareButtons** 는 모바일 확장 sdk에 정의 되어 있으므로이 코드를 컴파일하려면 해당 sdk에 대 한 참조를 프로젝트에 추가 해야 합니다. 그러나 처리기는 모바일 확장 SDK에 정의 된 형식을 구현 하는 장치 에서만 실행 되며 모바일 장치 패밀리입니다. 따라서 다음 코드는 조건부 컴파일과는 다른 방식으로 제공 되는 기능을 사용 하는 경우에만 주의를 기울여야 합니다.

```csharp
       // Note: Cache the value instead of querying it more than once.
        bool isHardwareButtonsAPIPresent = Windows.Foundation.Metadata.ApiInformation.IsTypePresent
            ("Windows.Phone.UI.Input.HardwareButtons");

        if (isHardwareButtonsAPIPresent)
        {
            Windows.Phone.UI.Input.HardwareButtons.CameraPressed +=
                this.HardwareButtons_CameraPressed;
        }

    ...

    private void HardwareButtons_CameraPressed(object sender, Windows.Phone.UI.Input.CameraEventArgs e)
    {
        // Handle the event.
    }
```

또한 [앱이 실행 되는 플랫폼 검색](wpsl-to-uwp-input-and-sensors.md)을 참조 하세요.

## <a name="the-app-package-manifest"></a>앱 패키지 매니페스트

프로젝트의 설정 (확장 Sdk 참조 포함)은 앱이 호출할 수 있는 API 노출 영역을 결정 합니다. 그러나 앱 패키지 매니페스트는 고객이 스토어에서 앱을 설치할 수 있는 실제 장치 집합을 결정 하는 것입니다. 자세한 내용은 [**Targetdevicefamily**](/uwp/schemas/appxpackage/uapmanifestschema/element-targetdevicefamily)의 예제를 참조 하세요.

앱 패키지 매니페스트를 편집 하는 방법에 대 한 자세한 내용은 다음 항목에서 다양 한 선언, 기능 및 일부 기능에 필요한 기타 설정에 대 한 사용에 대해 설명 합니다. Visual Studio 앱 패키지 매니페스트 편집기를 사용 하 여 편집할 수 있습니다. **솔루션 탐색기** 표시 되지 않으면 **보기** 메뉴에서 선택 합니다. **Package.appxmanifest**를 두 번 클릭합니다. 그러면 매니페스트 편집기 창이 열립니다. 적절 한 탭을 선택 하 여 변경한 다음 변경 내용을 저장 합니다. 포팅 된 앱 매니페스트의 **pm: PhoneIdentity** 요소가 포팅 하는 앱의 앱 매니페스트에 있는 것과 일치 하는지 확인할 수 있습니다. 자세한 내용은 [**pm: PhoneIdentity**](/uwp/schemas/appxpackage/uapmanifestschema/element-pm-phoneidentity) 항목을 참조 하세요.

[Windows 10에 대 한 패키지 매니페스트 스키마 참조를](/uwp/schemas/appxpackage/uapmanifestschema/schema-root)참조 하세요.

다음 항목은 [문제 해결](wpsl-to-uwp-troubleshooting.md)항목입니다.