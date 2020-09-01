---
description: 포팅 프로세스를 시작할 때 두 가지 옵션이 있습니다.
title: Windows 런타임 .x 프로젝트를 UWP 프로젝트로 포팅 '
ms.assetid: 2dee149f-d81e-45e0-99a4-209a178d415a
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 5cac5e66fe6d940ec73cc0e898924b4398382466
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89174837"
---
# <a name="porting-a-windows-runtime-8x-project-to-a-uwp-project"></a>Windows 런타임 .x 프로젝트를 UWP 프로젝트로 포팅



포팅 프로세스를 시작할 때 두 가지 옵션이 있습니다. 하나는 앱 패키지 매니페스트를 비롯 하 여 기존 프로젝트 파일의 복사본을 편집 하는 것입니다. 해당 옵션에 대 한 자세한 내용은 [앱을 유니버설 Windows 플랫폼로 마이그레이션 (UWP)](/visualstudio/misc/migrate-apps-to-the-universal-windows-platform-uwp?view=vs-2015)에서 프로젝트 파일을 업데이트 하는 방법에 대 한 정보를 참조 하세요. 다른 옵션은 Visual Studio에서 새 Windows 10 프로젝트를 만들고 파일을 복사 하는 것입니다. 이 항목의 첫 번째 섹션에서는 두 번째 옵션에 대해 설명 하지만이 항목의 나머지 부분에는 두 옵션 모두에 적용 되는 추가 정보가 있습니다. 새 Windows 10 프로젝트를 기존 프로젝트와 동일한 솔루션에 유지 하 고 공유 프로젝트를 사용 하 여 소스 코드 파일을 공유 하도록 선택할 수도 있습니다. 또는 Visual Studio의 연결 된 파일 기능을 사용 하 여 새 프로젝트를 자체 솔루션에 유지 하 고 소스 코드 파일을 공유할 수 있습니다.

## <a name="create-the-project-and-copy-files-to-it"></a>프로젝트를 만들고 파일을 복사 합니다.

이러한 단계는 Visual Studio에서 새 Windows 10 프로젝트를 만들고 파일을 복사 하는 옵션에 초점을 둡니다. 사용자가 만드는 프로젝트 수와 그에 따라 복사 하는 파일에 대 한 자세한 내용은에 설명 된 요소와 결정에 따라 [유니버설 8.1 앱](w8x-to-uwp-root.md) 과 그 다음에 나오는 섹션을 참조 하세요. 이러한 단계는 가장 간단한 경우를 가정 합니다.

1.  Microsoft Visual Studio 2015를 시작 하 고 새 빈 응용 프로그램 (Windows 유니버설) 프로젝트를 만듭니다. 자세한 내용은 [템플릿을 사용 하 여 Windows 런타임 8.x 앱 시작 (c #, c + +, Visual Basic)](/previous-versions/windows/apps/hh768232(v=win.10))을 참조 하세요. 새 프로젝트는 모든 장치 제품군에서 실행 되는 앱 패키지 (appx 파일)를 빌드합니다.
2.  유니버설 8.1 앱 프로젝트에서 다시 사용할 모든 소스 코드 파일 및 시각적 자산 파일을 식별 합니다. 파일 탐색기를 사용 하 여 데이터 모델, 뷰 모델, 시각적 자산, 리소스 사전, 폴더 구조 및 다시 사용 하려는 기타 항목을 새 프로젝트에 복사 합니다. 필요에 따라 디스크에 하위 폴더를 복사 하거나 만듭니다.
3.  뷰 (예: MainPage 및)를 새 프로젝트에도 복사 합니다. 필요에 따라 새 하위 폴더를 만들고 프로젝트에서 기존 뷰를 제거 합니다. 그러나 Visual Studio에서 생성 한 뷰를 오버 작성 하거나 제거 하기 전에 나중에 참조 하는 것이 유용할 수 있으므로 복사본을 유지 합니다. 유니버설 8.1 앱을 이식 하는 첫 번째 단계는 장치 제품군에서 잘 작동 하 고 잘 작동 하도록 하는 데 중점을 두는 것입니다. 나중에 보기가 모든 폼 팩터에 제대로 어울리도록 조정하고 선택적으로 적응 코드를 추가하여 특정 디바이스 패밀리를 최대한 활용하도록 하는 데 집중할 수 있습니다.
4.  **솔루션 탐색기**에서 **모든 파일 표시**가 설정되어 있는지 확인합니다. 복사한 파일을 선택 하 고 마우스 오른쪽 단추를 클릭 한 다음 **프로젝트에 포함**을 클릭 합니다. 그러면 포함 하는 폴더가 자동으로 포함 됩니다. 그런 다음 원하는 경우 **모든 파일 표시** 를 설정/해제할 수 있습니다. 다른 워크플로를 사용 하는 것이 좋습니다. **기존 항목 추가** 명령을 사용 하 여 Visual Studio **솔루션 탐색기**에서 필요한 하위 폴더를 만들었습니다. 시각적 자산에서 **빌드 작업** 을 **내용** 으로 설정 하 고 **출력 디렉터리로 복사** 를 **복사 안 함**으로 설정 했는지 두 번 선택 합니다.
5.  이 단계에서 일부 빌드 오류가 표시 될 수 있습니다. 그러나 변경 해야 할 사항을 알고 있는 경우 Visual Studio의 **찾기 및 바꾸기** 명령을 사용 하 여 소스 코드를 대량으로 변경할 수 있습니다. 그리고 Visual Studio의 명령적 코드 편집기에서 상황에 맞는 메뉴의 Using 확인 및 **구성** 명령을 사용 하 여 보다 구체적인 변경 내용을 **확인** 합니다.

## <a name="maximizing-markup-and-code-reuse"></a>태그 및 코드 재사용 극대화

간단한 리팩터링 (아래에 설명 되어 있음)을 추가 하면 모든 장치 패밀리에서 작동 하는 태그와 코드를 최대화할 수 있습니다. 자세한 내용은 다음과 같습니다.

-   모든 장치 패밀리에 공통적인 파일에는 특별 한 고려 사항이 필요 하지 않습니다. 이러한 파일은 앱이 실행 되는 모든 장치 패밀리의 앱에서 사용 됩니다. 여기에는 XAML 태그 파일, 명령적 소스 코드 파일 및 자산 파일이 포함 됩니다.
-   앱이 실행 중인 장치 패밀리를 검색 하 고 해당 장치 제품군 용으로 특별히 설계 된 보기를 탐색할 수 있습니다. 자세한 내용은 [앱이 실행 되는 플랫폼 검색](w8x-to-uwp-input-and-sensors.md)을 참조 하세요.
-   다른 방법으로는 응용 프로그램이 특정 장치 제품군에서 실행 되는 경우에만 런타임에 자동으로 로드 되도록 태그 파일 또는 **ResourceDictionary** 파일 (또는 파일을 포함 하는 폴더)을 특수 한 이름으로 지정 하는 것이 유용할 수 있습니다. 이 기법은 [Bookstore1](w8x-to-uwp-case-study-bookstore1.md) 사례 연구에 설명 되어 있습니다.
-   Windows 10만 지원 해야 하는 경우 유니버설 8.1 앱의 소스 코드에서 많은 조건부 컴파일 지시문을 제거할 수 있어야 합니다. 이 항목의 [조건부 컴파일 및 적응 코드](#conditional-compilation-and-adaptive-code) 를 참조 하세요.
-   모든 장치 제품군 (예: 프린터, 스캐너 또는 카메라 단추)에서 사용할 수 없는 기능을 사용 하려면 적응 코드를 작성할 수 있습니다. 이 항목의 [조건부 컴파일 및 적응 코드](#conditional-compilation-and-adaptive-code) 에서 세 번째 예제를 참조 하세요.
-   Windows 8.1, Windows Phone 8.1 및 Windows 10을 지원 하려면 동일한 솔루션에 세 개의 프로젝트를 유지 하 고 공유 프로젝트와 코드를 공유할 수 있습니다. 또는 프로젝트 간에 소스 코드 파일을 공유할 수 있습니다. 방법: Visual Studio에서 **솔루션 탐색기**프로젝트를 마우스 오른쪽 단추로 클릭 하 고, **기존 항목 추가**를 선택 하 고, 공유할 파일을 선택한 후 **링크로 추가**를 클릭 합니다. 소스 코드 파일을 연결 하는 프로젝트에서 볼 수 있는 파일 시스템의 공용 폴더에 소스 코드 파일을 저장 합니다. 소스 제어에 추가 하는 것을 잊지 마세요.
-   소스 코드 수준이 아닌 이진 수준에서 다시 사용 하려면 [c #에서 Windows 런타임 구성 요소 만들기 및 Visual Basic](/previous-versions/windows/apps/br230301(v=vs.140))를 참조 하세요. Windows 8.1, Windows Phone 8.1 및 Windows 10 앱 (.NET Core) 및 전체 .NET Framework에 대 한 .NET Framework에서 사용할 수 있는 .NET Api의 하위 집합을 지 원하는 이식 가능한 클래스 라이브러리도 있습니다. 이식 가능한 클래스 라이브러리 어셈블리는 이러한 모든 플랫폼과 이진 호환 됩니다. Visual Studio를 사용 하 여 이식 가능한 클래스 라이브러리를 대상으로 하는 프로젝트를 만듭니다. [이식 가능한 클래스 라이브러리를 사용 하 여 플랫폼 간 개발](/dotnet/standard/cross-platform/cross-platform-development-with-the-portable-class-library)을 참조 하세요.

## <a name="extension-sdks"></a>확장 SDK

유니버설 8.1 앱이 이미 호출 하는 대부분의 Windows 런타임 Api는 유니버설 장치 제품군 이라는 Api 집합에서 구현 됩니다. 하지만 일부는 확장 Sdk에서 구현 되며, Visual Studio는 앱의 대상 장치 제품군 또는 참조 한 확장 Sdk에 의해 구현 된 Api만 인식 합니다.

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

또한 [앱 패키지 매니페스트](#app-package-manifest)를 참조 하세요.

## <a name="conditional-compilation-and-adaptive-code"></a>조건부 컴파일 및 적응 코드

코드 파일이 Windows 8.1 및 Windows Phone 8.1 둘 다에서 작동 하도록 조건부 컴파일을 사용 하는 경우, 이제 Windows 10에서 수행 되는 수렴 작업의 빛에서 해당 조건부 컴파일을 검토할 수 있습니다. 수렴 이란 Windows 10 앱에서 일부 조건을 완전히 제거할 수 있음을 의미 합니다. 아래 예제에서 보여 주는 것 처럼 다른 항목은 런타임 검사를 변경 합니다.

**참고**    Windows 8.1, Windows Phone 8.1 및 Windows 10을 단일 코드 파일에 지원 하려는 경우이 작업을 수행할 수 있습니다. 프로젝트 속성 페이지에서 Windows 10 프로젝트를 살펴보면 프로젝트가 조건부 컴파일 기호로 WINDOWS UAP를 정의 하는 것을 확인할 수 있습니다 \_ . 따라서 WINDOWS \_ 앱 및 windows \_ PHONE 앱과 함께 사용할 수 있습니다 \_ . 이러한 예제에서는 유니버설 8.1 앱에서 조건부 컴파일을 제거 하 고 Windows 10 앱에 대 한 동등한 코드를 대체 하는 간단한 사례를 보여 줍니다.

이 첫 번째 예제에서는 **PickSingleFileAsync** api (Windows 8.1에만 적용 됨) 및 **PickSingleFileAndContinue** api (Windows Phone 8.1에만 적용 됨)에 대 한 사용 패턴을 보여 줍니다.

```csharp
#if WINDOWS_APP
    // Use Windows.Storage.Pickers.FileOpenPicker.PickSingleFileAsync
#else
    // Use Windows.Storage.Pickers.FileOpenPicker.PickSingleFileAndContinue
#endif // WINDOWS_APP
```

[**PickSingleFileAsync**](/uwp/api/windows.storage.pickers.fileopenpicker.picksinglefileasync) API에 대 한 Windows 10 수렴 코드를 다음과 같이 단순화 합니다.

```csharp
    // Use Windows.Storage.Pickers.FileOpenPicker.PickSingleFileAsync
```

이 예제에서는 하드웨어 뒤로 단추를 처리 하지만 Windows Phone에만 해당 합니다.

```csharp
#if WINDOWS_PHONE_APP
        Windows.Phone.UI.Input.HardwareButtons.BackPressed += this.HardwareButtons_BackPressed;
#endif // WINDOWS_PHONE_APP

...

#if WINDOWS_PHONE_APP
    void HardwareButtons_BackPressed(object sender, Windows.Phone.UI.Input.BackPressedEventArgs e)
    {
        // Handle the event.
    }
#endif // WINDOWS_PHONE_APP
```

Windows 10에서 뒤로 단추 이벤트는 범용 개념입니다. 하드웨어 또는 소프트웨어에서 구현 된 뒤로 단추는 처리할 [**요청**](/uwp/api/windows.ui.core.systemnavigationmanager.backrequested) 된 이벤트를 모두 발생 시킵니다.

```csharp
    Windows.UI.Core.SystemNavigationManager.GetForCurrentView().BackRequested +=
        this.ViewModelLocator_BackRequested;

...

private void ViewModelLocator_BackRequested(object sender, Windows.UI.Core.BackRequestedEventArgs e)
{
    // Handle the event.
}
```

이 마지막 예제는 이전 예제와 유사 합니다. 여기서는 하드웨어 카메라 단추를 처리 합니다. 단,이는 Windows Phone 앱 패키지에 컴파일된 코드에만 해당 됩니다.

```csharp
#if WINDOWS_PHONE_APP
    Windows.Phone.UI.Input.HardwareButtons.CameraPressed += this.HardwareButtons_CameraPressed;
#endif // WINDOWS_PHONE_APP

...

#if WINDOWS_PHONE_APP
void HardwareButtons_CameraPressed(object sender, Windows.Phone.UI.Input.CameraEventArgs e)
{
    // Handle the event.
}
#endif // WINDOWS_PHONE_APP
```

Windows 10에서 하드웨어 카메라 단추는 모바일 장치 제품군에 특정 한 개념입니다. 하나의 앱 패키지는 모든 장치에서 실행 되므로, 적응 코드 라고 하는 기능을 사용 하 여 컴파일 시간 조건을 런타임 조건으로 변경 합니다. 이렇게 하려면 [**Apiinformation**](/uwp/api/Windows.Foundation.Metadata.ApiInformation) 클래스를 사용 하 여 런타임에 [**HardwareButtons**](/uwp/api/Windows.Phone.UI.Input.HardwareButtons) 클래스가 있는지 쿼리 합니다. **HardwareButtons** 는 모바일 확장 sdk에 정의 되어 있으므로이 코드를 컴파일하려면 해당 sdk에 대 한 참조를 프로젝트에 추가 해야 합니다. 그러나 처리기는 모바일 확장 SDK에 정의 된 형식을 구현 하는 장치 에서만 실행 되며 모바일 장치 패밀리입니다. 따라서이 코드는 다른 방식으로 제공 되기는 하지만 존재 하는 기능을 사용 하는 경우에만 주의 한다는 점에 유의 하 여 유니버설 8.1 코드와 동일 합니다.

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

또한 [앱이 실행 되는 플랫폼 검색](w8x-to-uwp-input-and-sensors.md)을 참조 하세요.

## <a name="app-package-manifest"></a>앱 패키지 매니페스트

[Windows 10의 변경 내용](/uwp/schemas/appxpackage/uapmanifestschema/what-s-changed-in-windows-10) 항목에는 추가, 제거 및 변경 된 요소를 포함 하 여 windows 10에 대 한 패키지 매니페스트 스키마 참조의 변경 내용이 나열 되어 있습니다. 스키마의 모든 요소, 특성 및 유형에 대한 참조 정보는 [요소 계층 구조](/uwp/schemas/appxpackage/uapmanifestschema/root-elements)를 참조하세요. Windows Phone 스토어 앱을 이식 하는 경우 또는 앱이 Windows Phone 스토어의 앱에 대 한 업데이트 인 경우 **pm: PhoneIdentity** 요소가 이전 앱의 앱 매니페스트에 있는 것과 일치 하는지 확인 합니다. 스토어에서 앱에 할당 된 것과 동일한 guid를 사용 합니다. 이렇게 하면 Windows 10으로 업그레이드 하는 앱의 사용자가 새 앱을 중복 되지 않는 업데이트로 받게 됩니다. 자세한 내용은 [**pm: PhoneIdentity**](/uwp/schemas/appxpackage/uapmanifestschema/element-pm-phoneidentity) 참조 항목을 참조 하세요.

프로젝트의 설정 (확장 Sdk 참조 포함)은 앱이 호출할 수 있는 API 노출 영역을 결정 합니다. 그러나 앱 패키지 매니페스트는 고객이 스토어에서 앱을 설치할 수 있는 실제 장치 집합을 결정 하는 것입니다. 자세한 내용은 [**Targetdevicefamily**](/uwp/schemas/appxpackage/uapmanifestschema/element-targetdevicefamily)의 예제를 참조 하세요.

앱 패키지 매니페스트를 편집 하 여 일부 기능에 필요한 다양 한 선언, 기능 및 기타 설정을 지정할 수 있습니다. Visual Studio 앱 패키지 매니페스트 편집기를 사용 하 여 편집할 수 있습니다. **솔루션 탐색기** 표시 되지 않으면 **보기** 메뉴에서 선택 합니다. **Package.appxmanifest**를 두 번 클릭합니다. 그러면 매니페스트 편집기 창이 열립니다. 적절 한 탭을 선택 하 여 변경 하 고 저장 합니다.

다음 항목은 [문제 해결](w8x-to-uwp-troubleshooting.md)항목입니다.

## <a name="related-topics"></a>관련 항목

* [유니버설 Windows 플랫폼 응용 프로그램 개발](/visualstudio/cross-platform/develop-apps-for-the-universal-windows-platform-uwp?view=vs-2015)
* [템플릿을 사용 하 여 Windows 런타임 .x 앱 시작 (c #, c + +, Visual Basic)](/previous-versions/windows/apps/hh768232(v=win.10))
* [Windows 런타임 구성 요소 만들기](/previous-versions/windows/apps/hh441572(v=vs.140))
* [이식 가능한 클래스 라이브러리로 크로스 플랫폼 개발](/dotnet/standard/cross-platform/cross-platform-development-with-the-portable-class-library)