---
description: Windows Phone Silverlight 앱을 사용 하는 개발자 인 경우 Windows 10으로 이동에서 기술 집합과 소스 코드를 효율적으로 사용할 수 있습니다.
title: Windows Phone Silverlight에서 UWP로 이동
ms.assetid: 9E0C0315-6097-488B-A3AF-7120CCED651A
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: d37e40d58d8825d4361efbd10108c1169b8df87f
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74260085"
---
#  <a name="move-from-windowsphone-silverlight-to-uwp"></a>Windows Phone Silverlight에서 UWP로 이동


Windows Phone Silverlight 앱을 사용 하는 개발자 인 경우 Windows 10으로 이동에서 기술 집합과 소스 코드를 효율적으로 사용할 수 있습니다. Windows 10을 사용 하면 고객이 모든 종류의 장치에 설치할 수 있는 단일 앱 패키지인 UWP (유니버설 Windows 플랫폼) 앱을 만들 수 있습니다. Windows 10, UWP 앱 및이 포팅 가이드에서 설명 하는 적응 코드 및 적응 UI의 개념에 대 한 자세한 배경 정보는 [UWP (유니버설 Windows 플랫폼) 앱을](https://docs.microsoft.com/windows/uwp/get-started/universal-application-platform-guide)참조 하세요.

Windows Phone Silverlight 앱을 Windows 10 앱으로 이식 하는 경우 [Windows Phone 8.1에 도입](https://docs.microsoft.com/previous-versions/windows/apps/ff402535(v=vs.105))된 모바일 기능을 파악 하 여 앱 모델 및 UI 프레임 워크가 모든 Windows 10 장치에서 Universal 유니버설 WINDOWS 플랫폼 (UWP)를 사용할 수 있습니다. PC, 태블릿, 휴대폰 및 기타 여러 종류의 장치를 하나의 코드 베이스에서 그리고 하나의 앱 패키지를 통해 지원할 수 있게 됩니다. 또한 앱의 잠재 사용자 수가 배로 증가하고 공유 데이터, 구매한 소모성 항목 등을 통해 새로운 가능성이 열립니다. 새 기능에 대 한 자세한 내용은 [Windows 10의 개발자를 위한 새로운](https://docs.microsoft.com/windows/uwp/whats-new/windows-10-version-latest)기능을 참조 하세요.

을 선택 하는 경우 Windows Phone Silverlight 버전의 앱과 Windows 10 버전을 동시에 고객에 게 제공할 수 있습니다.

**참고**  이 가이드는 Windows Phone Silverlight 앱을 Windows 10으로 수동으로 이식할 수 있도록 설계 되었습니다. 이 가이드의 정보를 사용하여 앱을 포팅하는 것 외에도 **Mobilize.NET의 Silverlight 브리지**의 개발자 미리 보기를 시도하여 포팅 프로세스를 자동화할 수 있습니다. 이 도구는 앱의 소스 코드를 분석 하 고 Silverlight 컨트롤 및 Api Windows Phone에 대 한 참조를 UWP 대응 항목으로 변환 합니다. 이 도구는 여전히 개발자 미리 보기 상태이므로 아직 모든 변환 시나리오를 처리하지는 않습니다. 그러나 이 도구로 시작하면 대부분의 개발자가 일부 시간과 노력을 줄일 수 있습니다. 개발자 미리 보기를 시도하려면 [Mobilize.NET 웹 사이트](https://www.mobilize.net/uwp-bridge)를 방문하세요.

## <a name="xaml-and-net-or-html"></a>XAML 및 .NET 또는 HTML?

Windows Phone Silverlight에는 Silverlight 4.0을 기반으로 하는 XAML UI 프레임 워크가 있으며 .NET Framework 버전 및 UWP Api의 작은 하위 집합에 대해 프로그래밍할 수 있습니다. Windows Phone Silverlight 앱에서 Extensible Application Markup Language (XAML)을 사용 했으므로 대부분의 지식과 환경이 전송 되는 대부분의 소스 코드와 마찬가지로 사용자의 Windows 10 버전용으로 XAML을 선택할 수 있습니다. 사용 하는 소프트웨어 패턴. UI 태그와 디자인도 쉽게 포팅될 수 있습니다. Managed API, XAML 태그, UI 프레임워크 및 도구가 매우 친숙함을 알 수 있으며, UWP 앱에서 XAML과 함께 C++, C# 또는 Visual Basic을 사용할 수 있습니다. 처리하는 과정에서 한 두 가지 문제가 있긴 하지만 프로세스는 상대적으로 쉽습니다.

[C# 또는 Visual Basic을 사용한 UWP(유니버설 Windows 플랫폼) 앱용 로드맵](https://docs.microsoft.com/previous-versions/windows/apps/br229583(v=win.10))을 참조하세요.

**참고**  Windows 10은 Windows Phone 스토어 앱이 수행 하는 것 보다 훨씬 더 많은 .NET Framework를 지원 합니다. 예를 들어 Windows 10에는 여러 System.servicemodel이 있습니다. System.Net, System.net.networkinformation 및 시스템 .Net. n e t.\* 따라서 이제 Windows Phone Silverlight를 이식 하 고 .NET 코드를 새 플랫폼에서 컴파일 및 작동 하 게 하는 것이 좋습니다. [네임스페이스 및 클래스 매핑](wpsl-to-uwp-namespace-and-class-mappings.md)을 참조하세요.
기존 .NET 소스 코드를 Windows 10 앱에 다시 컴파일하는 또 다른 좋은 이유는 MSIL을 네이티브 실행 가능 기계어 코드로 변환 하는 미리 컴파일 기술인 .NET 네이티브를 활용 하는 것입니다. .NET 네이티브 앱은 해당 MSIL 앱보다 더 빠르게 시작되고 메모리와 배터리를 더 적게 사용합니다.

이 포팅 가이드에서는 XAML에 집중하지만, JavaScript, CSS 스타일시트 및 HTML5를 JavaScript용 Windows 라이브러리와 함께 사용하여 동일한 UWP API 중 다수를 많이 호출하는 기능적으로 동일한 앱을 빌드할 수 있습니다. XAML과 HTML의 Windows 런타임 UI 프레임워크는 서로 다르지만 어느 경우든 모든 Windows 디바이스에서 보편적으로 작동합니다.

## <a name="targeting-the-universal-or-the-mobile-device-family"></a>범용 또는 모바일 디바이스 패밀리를 대상으로 지정

선택할 수 있는 한 가지 옵션은 범용 디바이스 패밀리를 대상으로 하는 앱으로 앱을 포팅하는 것입니다. 이 경우 앱을 광범위한 장치에 설치할 수 있습니다. 앱이 모바일 디바이스 패밀리에서만 구현되는 API를 호출하는 경우 적응 코드로 이러한 호출을 지원할 수 있습니다. 또는 적응 코드를 작성할 필요가 없는 경우에는 모바일 디바이스 패밀리를 대상으로 하는 앱으로 앱을 포팅하도록 선택할 수 있습니다.

## <a name="adapting-your-app-to-multiple-form-factors"></a>여러 양식 요소에 맞게 앱 조정

이전 섹션에서 선택한 옵션에 따라 앱이 실행되는 장치 범위가 결정되며, 이러한 장치는 다양할 수 있습니다. 앱을 모바일 디바이스 패밀리로 제한해도 지원할 화면 크기는 무척 다양합니다. 따라서 이전에는 지원하지 않았던 폼 팩터에서 앱을 실행할 계획이므로 해당 폼 팩터에서 UI를 테스트하고 UI가 각 폼 팩터에 잘 맞도록 필요한 사항을 변경하도록 합니다. 이것을 포팅 후 작업 또는 포팅 추가 목표로 생각해도 되며 [Bookstore2](wpsl-to-uwp-case-study-bookstore2.md) 사례 연구의 연습에 예제가 있습니다.

## <a name="approaching-porting-layer-by-layer"></a>계층별 포팅에 대한 접근법

-   **보기**. 보기는 보기 모델과 함께 앱의 UI를 구성합니다. 보기는 보기 모델의 관찰 가능한 속성에 바인딩되는 태그로 구성되는 것이 좋습니다. 다른 패턴(일반적이고 편리하지만 단기적으로만 사용)은 코드 숨김 파일의 명령적 코드에서 UI 요소를 직접 조작하는 데 사용됩니다. 어느 경우든 대부분의 UI 태그와 디자인 및 UI 요소를 조작하는 명령적 코드를 포팅하는 것은 간단합니다.
-   **보기 모델 및 데이터 모델**. 관심사 분리 패턴(예제: MVVM)을 공식적으로 따르지 않더라도 앱에는 보기 모델과 데이터 모델의 기능을 수행하는 코드가 있기 마련입니다. 보기 모델 코드에서는 UI 프레임워크 네임스페이스의 형식을 활용합니다. 또한 보기 모델 및 데이터 모델 코드는 모두 비시각적 운영 체제와 .NET API(데이터 액세스를 위한 API 포함)를 사용합니다. 대부분은 [UWP 앱](https://docs.microsoft.com/previous-versions/windows/br211369(v=win.10))에서 사용할 수 있으므로, 이 코드의 대부분을 변경하지 않고 포팅할 수 있습니다. 보기 모델은 보기의 모델 또는 *추상화*입니다. 보기 모델은 UI의 상태와 동작을 제공하지만, 보기 자체는 시각적 기능을 제공합니다. 따라서 UWP를 통해 실행할 수 있도록 다른 폼 팩터에 맞게 UI를 조정할 경우 해당 보기 모델을 변경해야 합니다. 클라우드 서비스 네트워킹 및 호출의 경우 일반적으로 .NET 또는 UWP API 사용에 선택할 수 있는 옵션이 있습니다. 해당 의사 결정과 관련한 요소에 대해서는 [클라우드 서비스, 네트워킹 및 데이터베이스](wpsl-to-uwp-business-and-data.md)를 참조하세요.
-   **클라우드 서비스**. 앱의 상당 부분이 클라우드에서 서비스의 형태로 실행될 수 있습니다. 클라이언트 장치에서 실행 중인 앱 부분이 해당 부분에 연결됩니다. 이 부분은 클라이언트 부분을 포팅할 때 변경되지 않고 유지되는 분산 앱 부분입니다. UWP 앱에 대한 클라우드 서비스 옵션이 없을 경우, 유니버설 Windows 앱에서 간단한 라이브 타일 업데이트 알림부터 서버 팜에서 제공 가능한 복잡한 확장성까지 다양한 서비스를 호출할 수 있는 강력한 백 엔드 구성 요소를 제공하는 [Microsoft Azure 모바일 서비스](https://azure.microsoft.com/services/mobile-services/)를 선택하는 것이 좋습니다.

포팅 전이나 포팅 중에 비슷한 용도를 가진 코드가 임의적으로 분산되지 않고 계층으로 함께 수집되도록 앱을 리펙터링하여 개선할 수 있는지 여부를 고려하세요. 위에서 설명한 대로 UWP 앱을 계층으로 팩터링하면 앱을 수정하고 테스트한 다음 지속적으로 읽고 유지 관리하기가 쉬워집니다. [MVVM](https://msdn.microsoft.com/magazine/dd419663.aspx)(Model-View-ViewModel) 패턴을 따르면 기능을 쉽게 재사용하고 플랫폼 간의 UI API 차이로 인한 일부 문제를 방지할 수 있습니다. 이 패턴은 앱의 데이터, 비즈니스 및 UI 부분을 서로 별도로 유지합니다. UI 내에서도 상태와 동작을 시각적으로 분리하고 별도로 테스트할 수 있습니다. MVVM을 사용하면 데이터와 비즈니스 논리를 한 번 작성하여 UI에 관계없이 모든 장치에서 사용할 수 있습니다. 또한 장치 간에 많은 보기 모델 및 보기 부분이 다시 사용될 수 있습니다.

## <a name="one-or-two-exceptions-to-the-rule"></a>규칙의 한두 가지 예외

이 포팅 가이드를 읽었으므로 [네임스페이스 및 클래스 매핑](wpsl-to-uwp-namespace-and-class-mappings.md)을 참조할 수 있습니다. 상당히 간단한 매핑은 일반 규칙이며, 네임스페이스 및 클래스 매핑 테이블은 예외를 설명합니다.

다행히도 UWP의 기능 수준에서 지원되지 않는 경우는 거의 없습니다. 이 포팅 가이드의 나머지 부분에서 알 수 있듯이 대부분의 기술과 소스 코드는 UWP 앱으로 매우 잘 변환됩니다. 하지만 다음과 같은 몇 가지 Windows Phone Silverlight 기능을 사용 했을 수 있습니다 .이 기능은 UWP와 동일 하지 않습니다.

| 해당하는 UWP 항목이 없는 기능 | 기능에 대 한 Windows Phone Silverlight 설명서 |
|----------------------------------------------|---------------------------------------------------------|
| Microsoft XNA. 일반적으로 C++로 작성한 [Microsoft DirectX](https://docs.microsoft.com/windows/desktop/directx)로 대체됩니다. [게임 개발](https://docs.microsoft.com/previous-versions/windows/apps/hh452744(v=win.10)) 및 [DirectX 및 XAML interop](https://docs.microsoft.com/previous-versions/windows/apps/hh825871(v=win.10))을 참조하세요. | [XNA Framework 클래스 라이브러리](https://docs.microsoft.com/previous-versions/windows/xna/bb200104(v=xnagamestudio.41)) | 
|필터 앱 | [Windows Phone 8의 Lenses](https://docs.microsoft.com/previous-versions/windows/apps/jj206990(v=vs.105)) |

&nbsp;

| 항목| 설명|
|------|------------| 
| [네임 스페이스 및 클래스 매핑](wpsl-to-uwp-namespace-and-class-mappings.md) | 이 항목에서는 Windows Phone Silverlight Api를 해당 UWP에 대 한 포괄적인 매핑을 제공 합니다. |
| [프로젝트 포팅](wpsl-to-uwp-porting-to-a-uwp-project.md) | Visual Studio에서 새 Windows 10 프로젝트를 만들고 파일을 복사 하 여 포팅 프로세스를 시작 합니다. |
| [문제 해결](wpsl-to-uwp-troubleshooting.md) | 이 포팅 가이드를 끝까지 읽어 보시고 프로젝트를 빌드하여 실행하는 단계를 신속하게 진행해 보세요. 그렇게 하려면 필수가 아닌 코드에 주석이나 설명을 추가하고 나중에 돌아와서 해당 부채를 청산하여 일시적으로 진행할 수 있습니다. 이 항목의 문제 해결 증상 및 해결 방법 표는 다음 몇 개 항목을 대체하지는 않지만 이 단계에 유용한 정보를 제공합니다. 이후 항목을 진행하는 중에 언제든지 이 표를 다시 참조할 수 있습니다. |
| [XAML 및 UI 포팅](wpsl-to-uwp-porting-xaml-and-ui.md) | 선언적 XAML 태그의 형태로 UI를 정의 하는 방법은 Silverlight에서 UWP 앱으로 Windows Phone 매우 효율적으로 변환 합니다. 시스템 리소스 키 참조를 업데이트하고 일부 요소 형식 이름을 변경하고 "clr-namespace"를 "using"으로 변경한 경우 태그의 많은 부분이 호환됩니다. |
| [I/o, 장치 및 앱 모델용 포팅](wpsl-to-uwp-input-and-sensors.md) | 디바이스 및 센서와 통합되는 코드는 사용자의 입력과 사용자에 대한 출력을 포함합니다. 데이터 처리를 포함할 수도 있습니다. 하지만 이 코드는 UI 계층 또는 데이터 계층으로 간주되지 않습니다. 이 코드는 진동 컨트롤러, 가속도계, 자이로스코프, 마이크와 스피커(음성 인식 및 음성 합성과 교차), 지리적 위치, 입력 형식(예제: 터치, 마우스, 키보드, 펜) 등과의 통합을 포함합니다. |
| [비즈니스 및 데이터 계층 포팅](wpsl-to-uwp-business-and-data.md) | UI의 뒤에는 비즈니스 및 데이터 계층이 있습니다. 이러한 계층의 코드는 운영 체제 및 .NET Framework API를 호출합니다(예제: 후순위 처리, 위치, 카메라, 파일 시스템, 네트워크 및 기타 데이터 액세스). 대부분은 [UWP 앱에서 사용할 수 있으므로](https://docs.microsoft.com/previous-versions/windows/br211369(v=win.10)), 이 코드의 대부분을 변경하지 않고 포팅할 수 있습니다. |
| [폼 팩터 및 UX를 위한 포팅](wpsl-to-uwp-form-factors-and-ux.md) | Windows 앱은 PC, 모바일 디바이스 및 기타 여러 종류의 디바이스에서 일반적인 모양과 느낌을 공유합니다. 사용자 인터페이스, 입력 및 조작 패턴이 매우 비슷하고, 디바이스 간에 이동하면 친숙한 환경이 제공됩니다.|
|[사례 연구: Bookstore1](wpsl-to-uwp-case-study-bookstore1.md) | 이 항목에서는 매우 간단한 Windows Phone Silverlight 앱을 Windows 10 UWP 앱으로 이식 하는 사례 연구를 제공 합니다. Windows 10에서는 고객이 다양 한 장치에 설치할 수 있는 단일 앱 패키지를 만들 수 있으며,이 사례 연구에서이 작업을 수행할 수 있습니다. |
| [사례 연구: Bookstore2](wpsl-to-uwp-case-study-bookstore2.md) | [Bookstore1](wpsl-to-uwp-case-study-bookstore1.md)에 제공 된 정보를 기반으로 하는이 사례 연구는 데이터를 작성 하는 데 **Windows Phone 사용 됩니다.** 보기 모델에서 **Author** 클래스의 각 인스턴스는 해당 저자가 쓴 책의 그룹을 나타내며, **LongListSelector**에서 저자가 그룹화한 책 목록을 보거나 저자의 점프 목록을 축소할 수 있습니다. |

## <a name="related-topics"></a>관련 항목

**설명서**
* [Windows 10의 개발자를 위한 새로운 기능](https://docs.microsoft.com/windows/uwp/whats-new/windows-10-version-latest)
* [UWP(유니버설 Windows 플랫폼) 앱 가이드](https://docs.microsoft.com/windows/uwp/get-started/universal-application-platform-guide)
* [또는 Visual Basic를 사용 하 C# 는 UWP (유니버설 Windows 플랫폼) 앱에 대 한 로드맵](https://docs.microsoft.com/previous-versions/windows/apps/br229583(v=win.10))
* [Windows Phone 8 개발자를 위한 다음 단계](https://docs.microsoft.com/previous-versions/windows/apps/dn655121(v=vs.105))

**잡지 문서**
* [Visual Studio Magazine: Windows Phone 8.1: 수렴을 위한 자이언트 도약](https://visualstudiomagazine.com/articles/2014/05/01/whats-new-for-developers-with-windows-phone-8_1.aspx)

**프레젠테이션에**
* [Windows Phone에서 Windows 8로 Nokia 음악을 가져오는 스토리](https://channel9.msdn.com/Events/Build/2013/2-219)
 

