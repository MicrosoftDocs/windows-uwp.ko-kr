---
description: Windows Phone Silverlight 앱을 사용 하는 개발자 인 경우 Windows 10으로 이동에서 기술 집합과 소스 코드를 효율적으로 사용할 수 있습니다.
title: Windows Phone Silverlight에서 UWP로 이동
ms.assetid: 9E0C0315-6097-488B-A3AF-7120CCED651A
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 1858600b1d21c2462404a45620584735064176e0
ms.sourcegitcommit: ef723e3d6b1b67213c78da696838a920c66d5d30
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/02/2020
ms.locfileid: "82730334"
---
#  <a name="move-from-windowsphone-silverlight-to-uwp"></a>Windows Phone Silverlight에서 UWP로 이동


Windows Phone Silverlight 앱을 사용 하는 개발자 인 경우 Windows 10으로 이동에서 기술 집합과 소스 코드를 효율적으로 사용할 수 있습니다. Windows 10을 사용 하면 고객이 모든 종류의 장치에 설치할 수 있는 단일 앱 패키지인 UWP (유니버설 Windows 플랫폼) 앱을 만들 수 있습니다. Windows 10, UWP 앱, 이 포팅 가이드에서 언급할 적응 코드 및 적응 UI 개념에 대한 추가적인 배경 정보에 대해서는 [UWP(유니버설 Windows 플랫폼) 앱에 대한 가이드](https://docs.microsoft.com/windows/uwp/get-started/universal-application-platform-guide)를 참조하세요.

Windows Phone Silverlight 앱을 Windows 10 앱으로 이식 하는 경우 [Windows Phone 8.1에 도입](https://docs.microsoft.com/previous-versions/windows/apps/ff402535(v=vs.105))된 모바일 기능을 파악 하 여 앱 모델 및 UI 프레임 워크가 모든 Windows 10 장치에서 Universal 유니버설 WINDOWS 플랫폼 (UWP)를 사용할 수 있습니다. 따라서 한 코드 베이스와 하나의 앱 패키지를 통해 Pc, 태블릿, 휴대폰 및 다 수의 다른 종류의 장치를 지원할 수 있습니다. 그리고 앱의 잠재적 대상을 곱하여 공유 데이터, 구매한 소모품 등을 사용 하 여 새로운 가능성을 만듭니다. 새 기능에 대 한 자세한 내용은 [Windows 10의 개발자를 위한 새로운](https://docs.microsoft.com/windows/uwp/whats-new/windows-10-version-latest)기능을 참조 하세요.

을 선택 하는 경우 Windows Phone Silverlight 버전의 앱과 Windows 10 버전을 동시에 고객에 게 제공할 수 있습니다.

**참고**  이 가이드는 Windows Phone Silverlight 앱을 Windows 10으로 수동으로 이식할 수 있도록 설계 되었습니다. 이 가이드의 정보를 사용 하 여 앱을 이식 하는 것 외에도 이식 프로세스를 자동화 하는 데 도움이 되는 **모바일의 Silverlight 브리지** 의 개발자 미리 보기를 사용해 볼 수 있습니다. 이 도구는 앱의 소스 코드를 분석 하 고 Silverlight 컨트롤 및 Api Windows Phone에 대 한 참조를 UWP 대응 항목으로 변환 합니다. 이 도구는 아직 개발자 미리 보기 상태 이므로 모든 변환 시나리오를 처리 하지는 않습니다. 그러나 대부분의 개발자는이 도구로 시작 하 여 시간과 노력을 절약할 수 있습니다. 개발자 미리 보기를 시도 하려면 [모바일의 웹 사이트](https://www.mobilize.net/uwp-bridge)를 방문 하세요.

## <a name="xaml-and-net-or-html"></a>XAML, .NET 또는 HTML?

Windows Phone Silverlight에는 Silverlight 4.0을 기반으로 하는 XAML UI 프레임 워크가 있으며 .NET Framework 버전 및 Windows 런타임 Api의 작은 하위 집합에 대해 프로그래밍할 수 있습니다. Windows Phone Silverlight 앱에서 Extensible Application Markup Language (XAML)을 사용 했으므로 대부분의 지식과 경험이 사용자의 소스 코드와 사용 하는 소프트웨어 패턴에 따라 전송 되기 때문에 Windows 10 버전용으로 XAML을 선택할 수 있습니다. UI 태그와 디자인도 쉽게 이식할 수 있습니다. 관리 되는 Api, XAML 태그, UI 프레임 워크 및 도구를 모두 reassuringly, UWP 앱에서 c + +, c # 또는 Visual Basic를 XAML과 함께 사용할 수 있습니다. 작업이 나 두 가지 방법이 있는 경우에도 프로세스를 비교적 쉽게 수행할 수 있습니다.

[C# 또는 Visual Basic을 사용한 UWP(유니버설 Windows 플랫폼) 앱용 로드맵](https://docs.microsoft.com/previous-versions/windows/apps/br229583(v=win.10))을 참조하세요.

**참고**  Windows 10은 Windows Phone 스토어 앱에서 수행 하는 것 보다 훨씬 많은 .NET Framework 지원 합니다. 예를 들어 Windows 10에는 여러 System.servicemodel이 있습니다. \* 네임 스페이스 뿐만 아니라 System.Net, System.net.networkinformation 및 시스템 .net. n e t. 따라서 이제 Windows Phone Silverlight를 이식 하 고 .NET 코드를 새 플랫폼에서 컴파일 및 작동 하 게 하는 것이 좋습니다. [네임 스페이스 및 클래스 매핑을](wpsl-to-uwp-namespace-and-class-mappings.md)참조 하세요.
기존 .NET 소스 코드를 Windows 10 앱에 다시 컴파일하는 또 다른 좋은 이유는 MSIL을 네이티브 실행 가능 기계어 코드로 변환 하는 미리 컴파일 기술인 .NET 네이티브를 활용 하는 것입니다. .NET 네이티브 앱은 해당 MSIL 앱보다 더 빠르게 시작되고 메모리와 배터리를 더 적게 사용합니다.

이 포팅 가이드는 XAML에 중점을 두고 있지만 javascript 용 Windows 라이브러리와 함께 JavaScript, CSS 스타일시트 (CSS) 및 HTML5를 사용 하 여 동일한 Windows 런타임 Api를 여러 개 호출 하는 기능적으로 동일한 앱을 빌드할 수 있습니다. XAML 및 HTML의 Windows 런타임 UI 프레임 워크는 서로 다르며, 선택 하는 UI 프레임 워크는 전체 범위의 Windows 장치에서 전체적으로 작동 합니다.

## <a name="targeting-the-universal-or-the-mobile-device-family"></a>유니버설 또는 모바일 장치 제품군을 대상으로 지정

한 가지 옵션은 유니버설 장치 제품군을 대상으로 하는 앱으로 앱을 이식 하는 것입니다. 이 경우 가장 넓은 범위의 장치에 앱을 설치할 수 있습니다. 앱이 모바일 장치 제품군 에서만 구현 된 Api를 호출 하는 경우 적응 코드를 사용 하 여 이러한 호출을 보호할 수 있습니다. 또는 모바일 장치 제품군을 대상으로 하는 앱으로 앱을 이식 하도록 선택할 수 있습니다 .이 경우에는 적응 코드를 작성할 필요가 없습니다.

## <a name="adapting-your-app-to-multiple-form-factors"></a>여러 폼 팩터를 위한 앱 적응

이전 섹션에서 선택 하는 옵션은 앱 또는 앱이 실행 되는 장치 범위를 결정 하 고 매우 광범위 한 장치를 사용할 수 있습니다. 앱을 모바일 장치 제품군으로 제한 하더라도 지원 하기 위한 다양 한 화면 크기를 그대로 유지 합니다. 따라서 앱이 이전에는 지원 하지 않았던 폼 팩터를 실행 하 고 있으므로 해당 폼 팩터에서 UI를 테스트 하 고 필요한 사항을 변경 하 여 UI가 각에서 적절 하 게 적응 하도록 해야 합니다. 이 작업은 포팅 후 작업 또는 포팅 스트레치 목표 이며 [Bookstore2](wpsl-to-uwp-case-study-bookstore2.md) 사례 연구에서 실제 사례에 대 한 예제가 있습니다.

## <a name="approaching-porting-layer-by-layer"></a>계층 별로 포팅 계층에 근접

-   **보기**. 보기 (뷰 모델과 함께)를 사용 하 여 앱의 UI를 구성 합니다. 보기는 보기 모델의 관찰 가능한 속성에 바인딩된 태그로 구성 하는 것이 가장 좋습니다. 다른 패턴 (일반적이 고 편리 하지만 단기간에만 해당)은 코드를 통해 UI 요소를 직접 조작 하는 코드를 사용할 수 있는 파일의 명령적 코드입니다. 두 경우 모두 UI 태그와 디자인 및 UI 요소를 조작 하는 명령적 코드는 포트에 따라 간단 합니다.
-   **모델 및 데이터 모델 보기** 문제 분리 패턴 (예: MVVM)을 공식적으로 도입 하지 않더라도 응용 프로그램에 뷰 모델 및 데이터 모델의 기능을 수행 하는 코드가 있습니다. 모델 코드 보기에서는 UI 프레임 워크 네임 스페이스의 형식을 사용 합니다. 뷰 모델 및 데이터 모델 코드는 모두 비시각적 운영 체제 및 .NET Api (데이터 액세스용 Api 포함)도 사용 합니다. 그리고 대부분의 [응용 프로그램은 UWP 앱에서 사용할 수](https://docs.microsoft.com/previous-versions/windows/br211369(v=win.10))있으므로이 코드의 대부분을 변경 하지 않고 이식할 수 있는 것으로 예측할 수 있습니다. 그러나 뷰 모델은 뷰의 모델 또는 *추상적*입니다. 뷰 모델은 UI의 상태와 동작을 제공 하는 반면 보기 자체는 시각적 개체를 제공 합니다. 이러한 이유로 UWP에서 실행할 수 있는 다양 한 폼 팩터를 조정 하는 모든 UI는 해당 뷰 모델 변경 내용이 필요할 수 있습니다. 네트워킹 및 호출 클라우드 서비스의 경우 일반적으로 .NET 또는 Windows 런타임 Api를 사용 하는 옵션이 있습니다. 이러한 결정을 내리는 데 관련 된 요소에 대 한 자세한 내용은 [Cloud services, 네트워킹 및 데이터베이스](wpsl-to-uwp-business-and-data.md)를 참조 하세요.
-   **클라우드 서비스**. 앱의 상당 부분이 클라우드에서 서비스의 형태로 실행될 수 있습니다. 클라이언트 장치에서 실행 되는 앱의 일부가 해당 앱에 연결 됩니다. 이는 클라이언트 파트를 이식할 때 변경 되지 않은 상태로 유지 될 가능성이 높은 분산 앱의 일부입니다. 아직 없는 경우 UWP 앱에 대 한 좋은 클라우드 서비스 옵션은 [Microsoft Azure Mobile Services](https://azure.microsoft.com/services/mobile-services/)입니다 .이를 통해 유니버설 Windows 앱에서 라이브 타일에 대 한 간단한 알림부터 서버 팜이 제공할 수 있는 다양 한 수준의 확장성까지 다양 한 서비스에 대해 호출할 수 있는 강력한 백 엔드 구성 요소를 제공 합니다.

포팅 전후에는 유사한 용도의 코드가 계층에서 함께 수집 되 고 임의로 분산 되지 않도록 하 여 앱을 향상 시킬 수 있는지 여부를 고려 합니다. 위에 설명 된 것과 같은 계층으로 UWP 앱을 팩터링 하면 앱을 정확 하 게 만들고, 테스트 하 고, 이후에 읽고 유지 관리할 수 있습니다. 기능을 더 쉽게 재사용할 수 있으며,[MVVM](https://msdn.microsoft.com/magazine/dd419663.aspx)(모델-뷰-ViewModel) 패턴에 따라 플랫폼 간 UI API의 차이점에 대 한 몇 가지 문제를 방지할 수 있습니다. 이 패턴은 응용 프로그램의 데이터, 비즈니스 및 UI 부분을 서로 분리 된 상태로 유지 합니다. UI 내 에서도 시각적 개체에서 상태와 동작을 별도로 테스트할 수 있으며 별도로 테스트할 수 있습니다. MVVM를 사용 하면 데이터 및 비즈니스 논리를 한 번 작성 하 고 UI에 관계 없이 모든 장치에서 사용할 수 있습니다. 대부분의 보기 모델을 다시 사용 하 고 장치에서 파트를 볼 수도 있습니다.

## <a name="one-or-two-exceptions-to-the-rule"></a>규칙에 대 한 하나 또는 두 개의 예외

이 포팅 가이드를 읽을 때 [네임 스페이스 및 클래스 매핑을](wpsl-to-uwp-namespace-and-class-mappings.md)참조할 수 있습니다. 일반적인 규칙은 매우 간단 하며, 네임 스페이스 및 클래스 매핑 테이블은 모든 예외를 설명 합니다.

기능 수준에서 가장 좋은 소식은 UWP에서 지원 되지 않는 것입니다. 이 포팅 가이드의 나머지 부분에서 읽어 볼 수 있듯이 대부분의 기술 집합 및 소스 코드는 UWP 앱에 매우 잘 번역 됩니다. 하지만 다음과 같은 몇 가지 Windows Phone Silverlight 기능을 사용 했을 수 있습니다 .이 기능은 UWP와 동일 하지 않습니다.

| UWP에 해당 하지 않는 기능 | 기능에 대 한 Windows Phone Silverlight 설명서 |
|----------------------------------------------|---------------------------------------------------------|
| Microsoft XNA. 일반적으로 c + +를 사용 하는 [Microsoft DirectX](https://docs.microsoft.com/windows/desktop/directx) 는 대체입니다. [게임 개발](https://docs.microsoft.com/previous-versions/windows/apps/hh452744(v=win.10)) 및 [DirectX 및 XAML interop](https://docs.microsoft.com/previous-versions/windows/apps/hh825871(v=win.10))을 참조하세요. | [XNA Framework 클래스 라이브러리](https://docs.microsoft.com/previous-versions/windows/xna/bb200104(v=xnagamestudio.41)) | 
|렌즈 앱 | [Windows Phone 8의 Lenses](https://docs.microsoft.com/previous-versions/windows/apps/jj206990(v=vs.105)) |

&nbsp;

| 항목| Description|
|------|------------| 
| [네임 스페이스 및 클래스 매핑](wpsl-to-uwp-namespace-and-class-mappings.md) | 이 항목에서는 Windows Phone Silverlight Api를 해당 UWP에 대 한 포괄적인 매핑을 제공 합니다. |
| [프로젝트 포팅](wpsl-to-uwp-porting-to-a-uwp-project.md) | Visual Studio에서 새 Windows 10 프로젝트를 만들고 파일을 복사 하 여 포팅 프로세스를 시작 합니다. |
| [문제 해결](wpsl-to-uwp-troubleshooting.md) | 이 포팅 가이드의 끝 부분에 대 한 읽기를 권장 하지만, 프로젝트를 빌드하고 실행 하는 단계를 계속 해 서 확인 하는 것도 이해 하 고 있습니다. 이렇게 하려면 중요 하지 않은 코드를 주석 처리 하거나 구체적인 다음을 반환 하 여 나중에 해당 부채를 지불 하는 방식으로 임시 진행률을 만들 수 있습니다. 이 항목의 문제 해결 및 해결 방법 표는이 단계에서 도움이 될 수 있지만 다음 몇 가지 항목을 읽는 것은 아닙니다. 이후 항목을 진행 하면서 언제 든 지 테이블을 다시 참조할 수 있습니다. |
| [XAML 및 UI 포팅](wpsl-to-uwp-porting-xaml-and-ui.md) | 선언적 XAML 태그의 형태로 UI를 정의 하는 방법은 Silverlight에서 UWP 앱으로 Windows Phone 매우 효율적으로 변환 합니다. 시스템 리소스 키 참조를 업데이트 하 고, 일부 요소 형식 이름을 변경 하 고, "clr-namespace"를 "using"로 변경한 후에는 태그의 많은 섹션이 호환 됩니다. |
| [I/o, 장치 및 앱 모델용 포팅](wpsl-to-uwp-input-and-sensors.md) | 장치 자체 및 센서와 통합 되는 코드는 사용자의 입력 및 출력을 포함 합니다. 데이터 처리도 포함 될 수 있습니다. 그러나이 코드는 일반적으로 UI 계층 또는 데이터 계층으로 간주 되지 않습니다. 이 코드에는 진동 컨트롤러,가 속도계, 자이로스코프가, 마이크 및 스피커 (음성 인식 및 합성과 교차), (지역) 위치, 터치, 마우스, 키보드, 펜 등의 입력 소프트웨어나와의 통합이 포함 됩니다. |
| [비즈니스 및 데이터 계층 포팅](wpsl-to-uwp-business-and-data.md) | UI 뒤에는 비즈니스 및 데이터 계층이 있습니다. 이러한 계층의 코드는 운영 체제 및 .NET Framework Api (예: 백그라운드 처리, 위치, 카메라, 파일 시스템, 네트워크 및 기타 데이터 액세스)를 호출 합니다. 이러한 대부분은 [UWP 앱에서 사용할 수](https://docs.microsoft.com/previous-versions/windows/br211369(v=win.10))있으므로이 코드를 변경 하지 않고 이식할 수 있는 것으로 예측할 수 있습니다. |
| [폼 팩터 및 UX를 위한 포팅](wpsl-to-uwp-form-factors-and-ux.md) | Windows 앱은 Pc, 모바일 장치 및 기타 여러 종류의 장치에서 공통적인 모양과 느낌을 공유 합니다. 사용자 인터페이스, 입력 및 상호 작용 패턴은 매우 비슷하며 장치 간에 이동 하는 사용자는 친숙 한 환경을 시작 합니다.|
|[사례 연구: Bookstore1](wpsl-to-uwp-case-study-bookstore1.md) | 이 항목에서는 매우 간단한 Windows Phone Silverlight 앱을 Windows 10 UWP 앱으로 이식 하는 사례 연구를 제공 합니다. Windows 10에서는 고객이 다양 한 장치에 설치할 수 있는 단일 앱 패키지를 만들 수 있으며,이 사례 연구에서이 작업을 수행할 수 있습니다. |
| [사례 연구: Bookstore2](wpsl-to-uwp-case-study-bookstore2.md) | [Bookstore1](wpsl-to-uwp-case-study-bookstore1.md)에 제공 된 정보를 기반으로 하는이 사례 연구는 데이터를 작성 하는 데 **Windows Phone 사용 됩니다.** 뷰 모델에서 클래스 **작성자** 의 각 인스턴스는 해당 저자가 작성 한 책의 그룹을 나타내고,이를 **선택**하면 author 별로 그룹화 된 책 목록을 볼 수 있습니다. 또는 축소 하 여 저자의 점프 목록을 볼 수 있습니다. |

## <a name="related-topics"></a>관련 항목

**설명서**
* [Windows 10의 개발자를 위한 새로운 기능](https://docs.microsoft.com/windows/uwp/whats-new/windows-10-version-latest)
* [UWP (유니버설 Windows 플랫폼) 앱에 대 한 가이드](https://docs.microsoft.com/windows/uwp/get-started/universal-application-platform-guide)
* [C # 또는 Visual Basic를 사용 하는 UWP (유니버설 Windows 플랫폼) 앱에 대 한 로드맵](https://docs.microsoft.com/previous-versions/windows/apps/br229583(v=win.10))
* [Windows Phone 8 개발자를 위한 다음 단계](https://docs.microsoft.com/previous-versions/windows/apps/dn655121(v=vs.105))

**잡지 문서**
* [Visual Studio Magazine: Windows Phone 8.1: 수렴을 위한 자이언트 도약](https://visualstudiomagazine.com/articles/2014/05/01/whats-new-for-developers-with-windows-phone-8_1.aspx)

**프레젠테이션**
* [Windows Phone에서 Windows 8로 Nokia 음악을 가져오는 스토리](https://channel9.msdn.com/Events/Build/2013/2-219)
 

