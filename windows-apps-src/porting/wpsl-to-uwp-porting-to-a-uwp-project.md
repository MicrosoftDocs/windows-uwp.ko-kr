---
Visual Studio에서 새 Windows 10 프로젝트를 만든 다음 파일을 프로젝트로 복사하여 포팅 프로세스를 시작합니다.
Windows Phone Silverlight 프로젝트를 UWP 프로젝트로 포팅
ms.assetid: d86c99c5-eb13-4e37-b000-6a657543d8f4
---

# Windows Phone Silverlight 프로젝트를 UWP 프로젝트로 포팅

\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]

이전 항목은 [네임스페이스 및 클래스 매핑](wpsl-to-uwp-namespace-and-class-mappings.md)이었습니다.

Visual Studio에서 새 Windows 10 프로젝트를 만든 다음 파일을 프로젝트로 복사하여 포팅 프로세스를 시작합니다.

## 프로젝트를 만들고 파일을 프로젝트에 복사

1.  Microsoft Visual Studio 2015를 시작하고 비어 있는 새 응용 프로그램(Windows 유니버설) 프로젝트를 만듭니다. 자세한 내용은 [템플릿을 사용하여 Windows 스토어 앱 시작(C#, C++, Visual Basic)](https://msdn.microsoft.com/library/windows/apps/hh768232)을 참조하세요. 새 프로젝트에서는 모든 디바이스 패밀리에서 실행될 앱 패키지(appx 파일)를 빌드합니다.
2.  Windows Phone Silverlight 앱 프로젝트에서 다시 사용하려는 모든 소스 코드 파일 및 시각적 자산 파일을 식별합니다. 파일 탐색기를 사용하여 다시 사용할 데이터 모델, 보기 모델, 시각적 자산, 리소스 사전, 폴더 구조 등을 새 프로젝트에 복사합니다. 필요한 경우 디스크에서 하위 폴더를 복사하거나 만듭니다.
3.  또한 보기(예: MainPage.xaml 및 MainPage.xaml.cs)를 새 프로젝트 노드에 복사합니다. 필요한 경우 새 하위 폴더를 만들고 기존 보기를 프로젝트에서 제거합니다. 하지만 Visual Studio에서 생성된 보기를 덮어쓰거나 제거하기 전에 나중에 유용하게 참조할 수 있도록 복사본을 유지하세요. Windows Phone Silverlight 앱을 포팅하는 첫 단계에서는 한 디바이스 패밀리에서 앱이 제대로 표시되고 잘 작동하도록 하는 데 중점을 둡니다. 나중에 보기가 모든 폼 팩터에 제대로 어울리도록 조정하고 선택적으로 적응 코드를 추가하여 특정 디바이스 패밀리를 최대한 활용하도록 하는 데 집중할 수 있습니다.
4.  **솔루션 탐색기**에서 **모든 파일 표시**가 설정되어 있는지 확인합니다. 복사한 파일을 선택하여 마우스 오른쪽 단추로 클릭하고 **프로젝트에 포함**을 클릭합니다. 그러면 포함하는 폴더가 자동으로 포함됩니다. 원하는 경우 **모든 파일 표시**를 해제할 수 있습니다. 원하는 경우 대체 워크플로로 **기존 항목 추가** 명령을 사용하여 Visual Studio **솔루션 탐색기**에서 필요한 하위 폴더를 만듭니다. 시각적 자산에서 **빌드 작업**이 **콘텐츠**로 설정되어 있고 **출력 디렉터리로 복사**가 **복사 안 함**으로 설정되어 있는지 다시 확인합니다.
5.  네임스페이스와 클래스 이름의 차이로 인해 이 단계에서 많은 빌드 오류가 발생합니다. 예를 들어 Visual Studio에서 생성된 보기를 열 경우 보기는 **PhoneApplicationPage** 형식이 아니라 [**Page**](https://msdn.microsoft.com/library/windows/apps/br227503) 형식으로 표시됩니다. 이 포팅 가이드의 다음 항목에서는 XAML 태그와 명령적 코드의 다양한 차이점에 대해 자세히 설명합니다. 하지만 다음 일반 단계에 따라 빠르게 진행하게 됩니다. XAML 태그의 네임스페이스 접두사 선언에서 "clr-namespace"를 "using"으로 변경하고, [네임스페이스 및 클래스 매핑](wpsl-to-uwp-namespace-and-class-mappings.md) 항목과 Visual Studio의 **Find and Replace** 명령을 사용하여 소스 코드를 대량으로 변경하고(예: "System.Windows"를 "Windows.UI.Xaml"로 바꿈), Visual Studio의 명령적 코드 편집기의 상황에 맞는 메뉴에서 **Resolve** 및 **Organize Usings** 명령을 통해 더 많은 대상을 지정하여 변경합니다.

## 확장 SDK

포팅된 앱이 호출하는 대부분의 UWP(유니버설 Windows 플랫폼)는 유니버설 디바이스 패밀리라고 하는 API의 집합으로 구현됩니다. 그러나 일부는 확장 SDK에서 구현되며, Visual Studio는 앱의 대상 디바이스 패밀리에서 또는 참조했던 확장 SDK에서 구현한 API만 인식합니다.

찾을 수 없는 네임스페이스, 형식 또는 멤버에 대한 컴파일 오류가 발생하는 경우 이 문제가 원인일 가능성이 큽니다. API 참조 설명서에서 API의 항목을 열고 요구 사항 섹션으로 이동합니다. 그러면 구현하는 디바이스 패밀리에 대해 알 수 있습니다. 대상 디바이스 패밀리가 아닌 경우 해당 디바이스 패밀리의 확장 SDK에 대한 참조가 필요한 프로젝트에서 API를 사용할 수 있도록 합니다.

**프로젝트** &gt; **참조 추가** &gt; **Windows 유니버설** &gt; **확장**을 클릭하고 적절한 확장 SDK를 선택합니다. 예를 들어 호출할 API를 모바일 디바이스 패밀리에서만 사용할 수 있으며 버전 10.0.x.y에서 도입한 경우 **UWP용 Windows 모바일 확장**을 확인합니다.

프로젝트 파일에 다음 참조를 추가합니다.

```XML
<ItemGroup>
    <SDKReference Include="WindowsMobile, Version=10.0.x.y">
        <Name>Windows Mobile Extensions for the UWP</Name>
    </SDKReference>
</ItemGroup>
```

이름 및 버전 번호는 SDK 설치 위치의 폴더와 일치합니다. 예를 들어 위 정보는 다음 폴더 이름과 일치합니다.

`\Program Files (x86)\Windows Kits\10\Extension SDKs\WindowsMobile\10.0.x.y`

앱이 API를 구현한 디바이스 패밀리를 대상으로 하지 않는 한 [**ApiInformation**](https://msdn.microsoft.com/library/windows/apps/dn949001) 클래스를 사용하여 API가 있는지 테스트한 후 호출해야 합니다(적응 코드라고 함). 그러면 앱이 실행되는 모든 장치에서 이 조건이 평가되지만, API가 있어 호출에 사용할 수 있는 장치에 대해서만 true로 평가합니다. 먼저 범용 API가 있는지를 확인한 후 확장 SDK 및 적응 코드만 사용합니다. 아래 섹션에 몇 가지 예제가 나와 있습니다.

또한 [앱 패키지 매니페스트](#appxpackage)를 참조하세요.

## 태그 및 코드 재사용 최대화

약간 리팩터링하거나 적응 코드(아래에서 설명)를 추가하면 모든 디바이스 패밀리에서 작동하는 코드 및 태그를 최대화할 수 있습니다. 자세한 내용은 다음과 같습니다.

-   모든 디바이스 패밀리에 공통되는 파일의 경우 특별한 고려 사항이 필요하지 않습니다. 앱이 실행되는 모든 디바이스 패밀리에서 이러한 파일을 사용합니다. 여기에는 XAML 태그 파일, 명령적 소스 코드 파일 및 자산 파일이 포함됩니다.
-   앱은 실행되고 있는 디바이스 패밀리를 검색하고 해당 디바이스 패밀리용으로 특별히 설계된 보기를 탐색할 수 있습니다. 자세한 내용은 [앱이 실행되고 있는 플랫폼 검색](wpsl-to-uwp-input-and-sensors.md#detecting-the-platform)을 참조하세요.
-   대안이 없는 경우 유용하다고 생각할 수 있는 유사한 기술은 앱이 특정 디바이스 패밀리에서 실행될 경우에만 런타임 시 자동으로 로드되도록 태그 파일 또는 **ResourceDictionary** 파일(또는 파일이 들어 있는 폴더)의 특별한 이름을 지정하는 것입니다. [Bookstore1](wpsl-to-uwp-case-study-bookstore1.md#an-optional-adjustment) 사례 연구에서 이 기술을 보여 줍니다.
-   모든 디바이스 패밀리(예: 프린터, 스캐너 또는 카메라 단추)에서 사용할 수 없는 기능을 사용하기 위해 적응 코드를 작성할 수 있습니다. 이 항목의 [조건부 컴파일 및 적응 코드](#conditional-compilation)에서 세 번째 예제를 참조하세요.
-   Windows Phone Silverlight 및 Windows 10을 모두 지원하려는 경우 프로젝트 간에 소스 코드 파일을 공유할 수 있습니다. 방법: Visual Studio의 **솔루션 탐색기**에서 프로젝트를 마우스 오른쪽 단추로 클릭하고, **기존 항목 추가**를 선택하고, 공유할 파일을 선택하고, **링크로 추가**를 클릭합니다. 소스 코드 파일에 연결된 프로젝트에서 해당 파일을 인식할 수 있도록 소스 코드 파일을 파일 시스템의 공통 폴더에 저장하고, 소스 컨트롤에 해당 파일을 추가합니다. 전부는 아니지만 대부분의 파일이 두 플랫폼 모두에서 작동하도록 명령적 소스 코드를 팩터링할 수 있는 경우 코드를 두 번 복사할 필요가 없습니다. 파일의 조건부 컴파일 지시문(가능한 경우) 또는 런타임 조건(필요한 경우) 내에 플랫폼별 논리를 래핑할 수 있습니다. 아래에 나오는 다음 섹션과 [C# 전처리기 지시문](http://msdn.microsoft.com/library/ed8yd1ha.aspx)을 참조하세요.
-   소스 코드 수준이 아닌 이진 수준에서의 재사용을 위한 포팅 가능한 클래스 라이브러리도 있습니다. 이 라이브러리는 Windows Phone Silverlight와 Windows 10 앱(.NET Core)의 하위 집합에서 사용 가능한 .NET API의 하위 집합을 지원합니다. 포팅 가능한 클래스 라이브러리 어셈블리는 이러한 .NET 플랫폼 등과 이진 호환됩니다. Visual Studio를 사용하여 포팅 가능한 클래스 라이브러리를 대상으로 하는 프로젝트를 만듭니다. [포팅 가능한 클래스 라이브러리를 사용한 플랫폼 간 개발](http://msdn.microsoft.com/library/gg597391.aspx)을 참조하세요.

## 조건부 컴파일 및 적응 코드

단일 코드 파일에서 Windows Phone Silverlight 및 Windows 10을 모두 지원하려는 경우 다음을 수행할 수 있습니다. 프로젝트 속성 페이지에서 Windows 10 프로젝트를 살펴보는 경우 프로젝트가 WINDOWS\_UAP를 조건부 컴파일 기호로 정의한 것을 알 수 있습니다. 일반적으로 다음 논리를 사용하여 조건부 컴파일을 수행할 수 있습니다.

```csharp
#if WINDOWS_UAP
    // Code that you want to compile into the Windows 10 app.
#else
    // Code that you want to compile into the Windows Phone Silverlight app.
#endif // WINDOWS_UAP
```

Windows Phone Silverlight 앱과 Windows 스토어 앱 간에 공유되지 않는 코드가 있는 경우 다음과 같은 논리를 가진 소스 코드가 이미 있을 수 있습니다.

```csharp
#if NETFX_CORE
    // Code that you want to compile into the Windows Store app.
#else
    // Code that you want to compile into the Windows Phone Silverlight app.
#endif // NETFX_CORE
```

해당 소스 코드가 있고 이제 Windows 10도 지원하려는 경우 다음도 수행할 수 있습니다.

```csharp
#if WINDOWS_UAP
    // Code that you want to compile into the Windows 10 app.
#else
#if NETFX_CORE
    // Code that you want to compile into the Windows Store app.
#else
    // Code that you want to compile into the Windows Phone Silverlight app.
#endif // NETFX_CORE
#endif // WINDOWS_UAP
```

조건부 컴파일을 사용하여 하드웨어 뒤로 단추의 처리를 Windows Phone으로 제한할 수 있습니다. Windows 10에서 뒤로 단추 이벤트는 범용 개념입니다. 하드웨어에서 구현한 뒤로 단추 또는 소프트웨어에서 구현한 뒤로 단추는 모두 [**BackRequested**](https://msdn.microsoft.com/library/windows/apps/dn893596) 이벤트를 발생시킵니다. 즉, 처리할 이벤트입니다.

```csharp
       Windows.UI.Core.SystemNavigationManager.GetForCurrentView().BackRequested +=
            this.ViewModelLocator_BackRequested;

...

    private void ViewModelLocator_BackRequested(object sender, Windows.UI.Core.BackRequestedEventArgs e)
    {
        // Handle the event.
    }

```

조건부 컴파일을 사용하여 하드웨어 카메라 단추의 처리를 Windows Phone으로 제한할 수 있습니다. Windows 10에서 하드웨어 카메라 단추는 모바일 디바이스 패밀리와 관련된 개념입니다. 한 앱 패키지를 모든 장치에서 실행하므로 적응 코드를 사용하여 컴파일 시간 조건을 런타임 조건으로 변경합니다. 이러한 작업을 수행하기 위해 [**ApiInformation**](https://msdn.microsoft.com/library/windows/apps/dn949001) 클래스를 사용하여 런타임 시 [**HardwareButtons**](https://msdn.microsoft.com/library/windows/apps/jj207557) 클래스가 있는지 쿼리합니다. **HardwareButtons**는 모바일 확장 SDK에서 정의되므로 이 코드를 컴파일할 프로젝트에 해당 SDK에 대한 참조를 추가해야 합니다. 그러나 처리기는 모바일 확장 SDK에서 정의한 형식을 구현하는 디바이스이면서 모바일 디바이스 패밀리인 디바이스에서만 실행됩니다. 따라서 다음 코드가 현재 기능을 신중하게 사용하는 데 적합합니다. 그러나 구현 방식은 조건부 컴파일과 다릅니다.

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

또한 [앱이 실행되고 있는 플랫폼 검색](wpsl-to-uwp-input-and-sensors.md#detecting-the-platform)을 참조하세요.

## 앱 패키지 매니페스트

모든 확장 SDK 참조를 비롯하여 프로젝트의 설정은 앱에서 호출할 수 있는 API 노출 영역을 결정합니다. 하지만 앱 패키지 매니페스트는 고객이 스토어에서 앱을 설치할 수 있는 장치의 실제 집합을 결정합니다. 자세한 내용은 [**TargetDeviceFamily**](https://msdn.microsoft.com/library/windows/apps/dn986903)의 예제를 참조하세요.

다양한 선언, 기능, 일부 기능에 필요한 기타 설정 등에서 앱 패키지 매니페스트를 사용하는 방법을 설명하는 항목이 뒤에 나오므로, 앱 패키지 매니페스트를 편집하는 방법을 알아두는 것이 좋습니다. Visual Studio 앱 패키지 매니페스트 편집기를 사용하여 앱 패키지 매니페스트를 편집할 수 있습니다. **솔루션 탐색기**가 표시되지 않는 경우 **보기** 메뉴에서 선택합니다. **Package.appxmanifest**를 두 번 클릭합니다. 매니페스트 편집기 창이 열립니다. 적절한 탭을 선택하여 변경한 다음 변경 내용을 저장합니다. 포팅된 앱 매니페스트의 **pm:PhoneIdentity** 요소가 포팅할 앱의 앱 매니페스트에 있는 요소와 일치하는지 확인할 수 있습니다(자세한 내용은 [**pm:PhoneIdentity**](https://msdn.microsoft.com/library/windows/apps/dn934763) 항목 참조).

[Windows 10 용 패키지 매니페스트 스키마](https://msdn.microsoft.com/library/windows/apps/dn934820)를 참조하세요.

다음 항목은 [문제 해결](wpsl-to-uwp-troubleshooting.md)입니다.



<!--HONumber=Mar16_HO1-->


