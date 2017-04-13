---
author: mcleblanc
description: "Windows Phone Silverlight 앱을 사용하는 개발자는 Windows 10으로 이동하는 과정에서 자신의 기술과 소스 코드를 최대한 활용할 수 있습니다."
title: "Windows Phone Silverlight에서 UWP로 이동"
ms.assetid: 9E0C0315-6097-488B-A3AF-7120CCED651A
ms.author: markl
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: d055f576cfa56502da845e849c100f66dd0c7ccf
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
#  <a name="move-from-windows-phone-silverlight-to-uwp"></a>Windows Phone Silverlight에서 UWP로 이동

\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]

Windows Phone Silverlight 앱을 사용하는 개발자는 Windows 10으로 이동하는 과정에서 자신의 기술과 소스 코드를 최대한 활용할 수 있습니다. Windows 10을 사용하면 모든 종류의 장치에 설치할 수 있는 단일 앱 패키지인 UWP(유니버설 Windows 플랫폼) 앱을 만들 수 있습니다. Windows 10, UWP 앱, 이 포팅 가이드에서 언급할 적응 코드 및 적응 UI 개념에 대한 추가적인 배경 정보에 대해서는 [UWP(유니버설 Windows 플랫폼) 앱에 대한 가이드](https://msdn.microsoft.com/library/windows/apps/dn894631)를 참조하세요.

Windows Phone Silverlight 앱을 Windows 10 앱으로 포팅할 경우 [Windows Phone 8.1에서 도입한](https://msdn.microsoft.com/library/windows/apps/dn632424) 모바일 기능을 사용할 수 있으며 더 나아가 Windows 10의 모든 장치에서 앱 모델 및 UI 프레임워크가 범용인 UWP(유니버설 Windows 플랫폼)를 최대한 활용할 수 있습니다. PC, 태블릿, 휴대폰 및 기타 여러 종류의 장치를 하나의 코드 베이스에서 그리고 하나의 앱 패키지를 통해 지원할 수 있게 됩니다. 또한 앱의 잠재 사용자 수가 배로 증가하고 공유 데이터, 구매한 소모성 항목 등을 통해 새로운 가능성이 열립니다. 새로운 기능에 대한 자세한 내용은 [Windows 10 개발자를 위한 새로운 기능](https://dev.windows.com/getstarted/whats-new-windows-10)을 참조하세요.

고객이 원하는 경우 Windows Phone Silverlight 버전 앱과 Windows 10 버전 앱을 함께 사용할 수 있습니다.

**참고** 이 가이드는 Windows Phone Silverlight 앱을 Windows 10으로 수동으로 포팅하는 데 도움이 되도록 설계되었습니다. 이 가이드의 정보를 사용하여 앱을 포팅하는 것 외에도 **Mobilize.NET의 Silverlight 브리지**의 개발자 미리 보기를 시도하여 포팅 프로세스를 자동화할 수 있습니다. 이 도구는 앱의 소스 코드를 분석하고 Windows Phone Silverlight 컨트롤 및 API에 대한 참조를 해당 UWP 항목으로 변환합니다. 이 도구는 여전히 개발자 미리 보기 상태이므로 아직 모든 변환 시나리오를 처리하지는 않습니다. 그러나 이 도구로 시작하면 대부분의 개발자가 일부 시간과 노력을 줄일 수 있습니다. 개발자 미리 보기를 시도하려면 [Mobilize.NET 웹 사이트](http://go.microsoft.com/fwlink/p/?LinkId=624546)를 방문하세요.

## <a name="xaml-and-net-or-html"></a>XAML 및 .NET 또는 HTML?

Windows Phone Silverlight의 XAML UI 프레임워크는 Silverlight 4.0 기반으로 하고 개발자는 .NET Framework 버전과 UWP API의 일부 하위 집합에 대해 프로그래밍합니다. Windows Phone Silverlight 앱에서 XAML(EXtensible Application Markup Language)을 사용했으므로 Windows 10 버전에서도 XAML을 선택할 가능성이 있습니다. 대부분의 지식 및 경험과 마찬가지로 사용한 소스 코드 및 소프트웨어 패턴의 상당 부분을 그대로 사용할 수 있기 때문입니다. UI 태그와 디자인도 쉽게 포팅될 수 있습니다. Managed API, XAML 태그, UI 프레임워크 및 도구가 매우 친숙함을 알 수 있으며, UWP 앱에서 XAML과 함께 C++, C# 또는 Visual Basic을 사용할 수 있습니다. 처리하는 과정에서 한 두 가지 문제가 있긴 하지만 프로세스는 상대적으로 쉽습니다.

[C# 또는 Visual Basic을 사용한 UWP(유니버설 Windows 플랫폼) 앱용 로드맵](https://msdn.microsoft.com/library/windows/apps/br229583)을 참조하세요.

**참고** Windows 10은 .NET Framework에 대해 Windows Phone 스토어 앱보다 훨씬 더 많이 지원합니다. 예를 들어 Windows 10에는 System.Net, System.Net.NetworkInformation 및 System.Net.Sockets뿐만 아니라 여러 System.ServiceModel.\* 네임스페이스도 있습니다. 따라서 이제 Windows Phone Silverlight로 포팅하고 .NET 코드를 바로 컴파일하고 새 플랫폼에서 작업하는 것이 좋습니다. [네임스페이스 및 클래스 매핑](wpsl-to-uwp-namespace-and-class-mappings.md)을 참조하세요.
기존 .NET 소스 코드를 Windows 10 앱으로 다시 컴파일해야 할 또 다른 이유는 MSIL을 기본적으로 실행 가능 컴퓨터 코드로 변환하는 미리 컴파일 기술인 .NET 네이티브의 이점을 활용할 수 있기 때문입니다. .NET 네이티브 앱은 해당 MSIL 앱보다 더 빠르게 시작되고 메모리와 배터리를 더 적게 사용합니다.

이 포팅 가이드에서는 XAML에 집중하지만, JavaScript, CSS 스타일시트 및 HTML5를 JavaScript용 Windows 라이브러리와 함께 사용하여 동일한 UWP API 중 다수를 많이 호출하는 기능적으로 동일한 앱을 빌드할 수 있습니다. XAML과 HTML의 Windows 런타임 UI 프레임워크는 서로 다르지만 어느 경우든 모든 Windows 디바이스에서 보편적으로 작동합니다.

## <a name="targeting-the-universal-or-the-mobile-device-family"></a>범용 또는 모바일 디바이스 패밀리를 대상으로 지정

선택할 수 있는 한 가지 옵션은 범용 디바이스 패밀리를 대상으로 하는 앱으로 앱을 포팅하는 것입니다. 이 경우 앱을 광범위한 장치에 설치할 수 있습니다. 앱이 모바일 디바이스 패밀리에서만 구현되는 API를 호출하는 경우 적응 코드로 이러한 호출을 지원할 수 있습니다. 또는 적응 코드를 작성할 필요가 없는 경우에는 모바일 디바이스 패밀리를 대상으로 하는 앱으로 앱을 포팅하도록 선택할 수 있습니다.

## <a name="adapting-your-app-to-multiple-form-factors"></a>여러 양식 요소에 맞게 앱 조정

이전 섹션에서 선택한 옵션에 따라 앱이 실행되는 장치 범위가 결정되며, 이러한 장치는 다양할 수 있습니다. 앱을 모바일 디바이스 패밀리로 제한해도 지원할 화면 크기는 무척 다양합니다. 따라서 이전에는 지원하지 않았던 폼 팩터에서 앱을 실행할 계획이므로 해당 폼 팩터에서 UI를 테스트하고 UI가 각 폼 팩터에 잘 맞도록 필요한 사항을 변경하도록 합니다. 이것을 포팅 후 작업 또는 포팅 추가 목표로 생각해도 되며 [Bookstore2](wpsl-to-uwp-case-study-bookstore2.md) 사례 연구의 연습에 예제가 있습니다.

## <a name="approaching-porting-layer-by-layer"></a>계층별 포팅에 대한 접근법

-   **보기**. 보기는 보기 모델과 함께 앱의 UI를 구성합니다. 보기는 보기 모델의 관찰 가능한 속성에 바인딩되는 태그로 구성되는 것이 좋습니다. 다른 패턴(일반적이고 편리하지만 단기적으로만 사용)은 코드 숨김 파일의 명령적 코드에서 UI 요소를 직접 조작하는 데 사용됩니다. 어느 경우든 대부분의 UI 태그와 디자인 및 UI 요소를 조작하는 명령적 코드를 포팅하는 것은 간단합니다.
-   **보기 모델 및 데이터 모델**. 관심사 분리 패턴(예제: MVVM)을 공식적으로 따르지 않더라도 앱에는 보기 모델과 데이터 모델의 기능을 수행하는 코드가 있기 마련입니다. 보기 모델 코드에서는 UI 프레임워크 네임스페이스의 형식을 활용합니다. 또한 보기 모델 및 데이터 모델 코드는 모두 비시각적 운영 체제와 .NET API(데이터 액세스를 위한 API 포함)를 사용합니다. 대부분은 [UWP 앱](https://msdn.microsoft.com/library/windows/apps/br211369)에서 사용할 수 있으므로, 이 코드의 대부분을 변경하지 않고 포팅할 수 있습니다. 보기 모델은 보기의 모델 또는 *추상화*입니다. 보기 모델은 UI의 상태와 동작을 제공하지만, 보기 자체는 시각적 기능을 제공합니다. 따라서 UWP를 통해 실행할 수 있도록 다른 폼 팩터에 맞게 UI를 조정할 경우 해당 보기 모델을 변경해야 합니다. 클라우드 서비스 네트워킹 및 호출의 경우 일반적으로 .NET 또는 UWP API 사용에 선택할 수 있는 옵션이 있습니다. 해당 의사 결정과 관련한 요소에 대해서는 [클라우드 서비스, 네트워킹 및 데이터베이스](wpsl-to-uwp-business-and-data.md)를 참조하세요.
-   **클라우드 서비스**. 앱의 상당 부분이 클라우드에서 서비스의 형태로 실행될 수 있습니다. 클라이언트 장치에서 실행 중인 앱 부분이 해당 부분에 연결됩니다. 이 부분은 클라이언트 부분을 포팅할 때 변경되지 않고 유지되는 분산 앱 부분입니다. UWP 앱에 대한 클라우드 서비스 옵션이 없을 경우, 유니버설 Windows 앱에서 간단한 라이브 타일 업데이트 알림부터 서버 팜에서 제공 가능한 복잡한 확장성까지 다양한 서비스를 호출할 수 있는 강력한 백 엔드 구성 요소를 제공하는 [Microsoft Azure 모바일 서비스](http://azure.microsoft.com/services/mobile-services/)를 선택하는 것이 좋습니다.

포팅 전이나 포팅 중에 비슷한 용도를 가진 코드가 임의적으로 분산되지 않고 계층으로 함께 수집되도록 앱을 리펙터링하여 개선할 수 있는지 여부를 고려하세요. 위에서 설명한 대로 UWP 앱을 계층으로 팩터링하면 앱을 수정하고 테스트한 다음 지속적으로 읽고 유지 관리하기가 쉬워집니다. [MVVM](http://msdn.microsoft.com/magazine/dd419663.aspx)(Model-View-ViewModel) 패턴을 따르면 기능을 쉽게 재사용하고 플랫폼 간의 UI API 차이로 인한 일부 문제를 방지할 수 있습니다. 이 패턴은 앱의 데이터, 비즈니스 및 UI 부분을 서로 별도로 유지합니다. UI 내에서도 상태와 동작을 시각적으로 분리하고 별도로 테스트할 수 있습니다. MVVM을 사용하면 데이터와 비즈니스 논리를 한 번 작성하여 UI에 관계없이 모든 장치에서 사용할 수 있습니다. 또한 장치 간에 많은 보기 모델 및 보기 부분이 다시 사용될 수 있습니다.

## <a name="one-or-two-exceptions-to-the-rule"></a>규칙의 한두 가지 예외

이 포팅 가이드를 읽었으므로 [네임스페이스 및 클래스 매핑](wpsl-to-uwp-namespace-and-class-mappings.md)을 참조할 수 있습니다. 상당히 간단한 매핑은 일반 규칙이며, 네임스페이스 및 클래스 매핑 테이블은 예외를 설명합니다.

다행히도 UWP의 기능 수준에서 지원되지 않는 경우는 거의 없습니다. 이 포팅 가이드의 나머지 부분에서 알 수 있듯이 대부분의 기술과 소스 코드는 UWP 앱으로 매우 잘 변환됩니다. 하지만 사용한 Windows Phone Silverlight 기능 중에 해당하는 UWP 항목이 없는 몇 가지 기능이 있습니다.

| 해당하는 UWP 항목이 없는 기능 | 기능에 대한 Windows Phone Silverlight 설명서 |
|----------------------------------------------|---------------------------------------------------------|
| Microsoft XNA. 일반적으로 C++로 작성한 [Microsoft DirectX](https://msdn.microsoft.com/library/windows/desktop/ee663274)로 대체됩니다. [게임 개발](https://msdn.microsoft.com/library/windows/apps/hh452744) 및 [DirectX 및 XAML interop](https://msdn.microsoft.com/library/windows/apps/hh825871)을 참조하세요. | [XNA Framework 클래스 라이브러리](http://msdn.microsoft.com/library/bb203940.aspx) | 
|필터 앱 | [Windows Phone 8의 필터](http://msdn.microsoft.com/library/windows/apps/jj206990.aspx) |

&nbsp;

| 항목| 설명|
|------|------------| 
| [네임스페이스 및 클래스 매핑](wpsl-to-uwp-namespace-and-class-mappings.md) | 이 항목에서는 Windows Phone Silverlight API를 해당 UWP API에 포괄적으로 매핑합니다. |
| [프로젝트 포팅](wpsl-to-uwp-porting-to-a-uwp-project.md) | Visual Studio에서 새 Windows 10 프로젝트를 만든 다음 파일을 프로젝트로 복사하여 포팅 프로세스를 시작합니다. |
| [문제 해결](wpsl-to-uwp-troubleshooting.md) | 이 포팅 가이드를 끝까지 읽어 보시고 프로젝트를 빌드하여 실행하는 단계를 신속하게 진행해 보세요. 그렇게 하려면 필수가 아닌 코드에 주석이나 설명을 추가하고 나중에 돌아와서 해당 부채를 청산하여 일시적으로 진행할 수 있습니다. 이 항목의 문제 해결 증상 및 해결 방법 표는 다음 몇 개 항목을 대체하지는 않지만 이 단계에 유용한 정보를 제공합니다. 이후 항목을 진행하는 중에 언제든지 이 표를 다시 참조할 수 있습니다. |
| [XAML 및 UI 포팅](wpsl-to-uwp-porting-xaml-and-ui.md) | 선언적 XAML 태그 형식으로 UI를 정의하는 방법을 사용하면 Windows Phone Silverlight에서 UWP 앱으로 매우 잘 변환됩니다. 시스템 리소스 키 참조를 업데이트하고 일부 요소 형식 이름을 변경하고 "clr-namespace"를 "using"으로 변경한 경우 태그의 많은 부분이 호환됩니다. |
| [I/O, 디바이스 및 앱 모델에 대한 포팅](wpsl-to-uwp-input-and-sensors.md) | 디바이스 및 센서와 통합되는 코드는 사용자의 입력과 사용자에 대한 출력을 포함합니다. 데이터 처리를 포함할 수도 있습니다. 하지만 이 코드는 UI 계층 또는 데이터 계층으로 간주되지 않습니다. 이 코드는 진동 컨트롤러, 가속도계, 자이로스코프, 마이크와 스피커(음성 인식 및 음성 합성과 교차), 지리적 위치, 입력 형식(예제: 터치, 마우스, 키보드, 펜) 등과의 통합을 포함합니다. |
| [비즈니스 및 데이터 계층 포팅](wpsl-to-uwp-business-and-data.md) | UI의 뒤에는 비즈니스 및 데이터 계층이 있습니다. 이러한 계층의 코드는 운영 체제 및 .NET Framework API를 호출합니다(예제: 후순위 처리, 위치, 카메라, 파일 시스템, 네트워크 및 기타 데이터 액세스). 대부분은 [UWP 앱에서 사용할 수 있으므로](https://msdn.microsoft.com/library/windows/apps/br211369), 이 코드의 대부분을 변경하지 않고 포팅할 수 있습니다. |
| [폼 팩터 및 UX에 대한 포팅](wpsl-to-uwp-form-factors-and-ux.md) | Windows 앱은 PC, 모바일 디바이스 및 기타 여러 종류의 디바이스에서 일반적인 모양과 느낌을 공유합니다. 사용자 인터페이스, 입력 및 조작 패턴이 매우 비슷하고, 디바이스 간에 이동하면 친숙한 환경이 제공됩니다.|
|[사례 연구: Bookstore1](wpsl-to-uwp-case-study-bookstore1.md) | 이 항목에서는 매우 간단한 Windows Phone Silverlight 앱을 Windows 10 UWP 앱으로 포팅하는 사례 연구를 제공합니다. Windows 10을 사용하면 고객이 다양한 디바이스에 설치할 수 있는 단일 앱 패키지를 만들 수 있습니다. 이 사례 연구에서는 이 작업을 수행합니다. |
| [사례 연구: Bookstore2](wpsl-to-uwp-case-study-bookstore2.md) | [Bookstore1](wpsl-to-uwp-case-study-bookstore1.md)에 제공된 정보를 기반으로 하는 이 사례 연구는 **LongListSelector**에서 그룹화된 데이터를 표시하는 Windows Phone Silverlight 앱으로 시작합니다. 보기 모델에서 **Author** 클래스의 각 인스턴스는 해당 저자가 쓴 책의 그룹을 나타내며, **LongListSelector**에서 저자가 그룹화한 책 목록을 보거나 저자의 점프 목록을 축소할 수 있습니다. |

## <a name="related-topics"></a>관련 항목

**설명서**
* [Windows 10 개발자를 위한 새로운 기능](https://dev.windows.com/getstarted/whats-new-windows-10)
* [UWP(유니버설 Windows 플랫폼) 앱 지침](https://msdn.microsoft.com/library/windows/apps/dn894631)
* [C# 또는 Visual Basic을 사용한 UWP(유니버설 Windows 플랫폼) 앱용 로드맵](https://msdn.microsoft.com/library/windows/apps/br229583)
* [Windows Phone 8 개발자용 추가 사항](https://msdn.microsoft.com/library/windows/apps/xaml/dn655121.aspx)

**잡지 기사**
* [Visual Studio Magazine: Windows Phone 8.1: 수렴을 위한 큰 도약](http://go.microsoft.com/fwlink/p/?LinkID=398541)

**프레젠테이션**
* [Windows Phone에서 Windows 8으로 Nokia Music 가져오기](http://go.microsoft.com/fwlink/p/?LinkId=321521)
 

