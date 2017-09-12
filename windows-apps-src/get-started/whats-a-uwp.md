---
author: GrantMeStrength
ms.assetid: C9787269-B54F-4FFA-A884-D4A3BF28F80D
title: "UWP(유니버설 Windows 플랫폼) 앱이란?"
description: "다양한 유형의 유니버설 Windows 앱(Windows 스토어 앱, Windows Phone 스토어 앱 및 Windows 런타임 앱)에 대해 알아보세요."
ms.author: jken
ms.date: 03/22/2017
ms.topic: article
pms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: 3bbced2db33210952b6c8a45f98e36582330d7d9
ms.sourcegitcommit: 214a1dcb24e0811811bd7a4a07bfe707ecd93b18
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/15/2017
---
# <a name="whats-a-universal-windows-platform-uwp-app"></a>UWP(유니버설 Windows 플랫폼) 앱이란?

UWP(유니버설 Windows 플랫폼)는 Windows 10용 앱 플랫폼입니다. 하나의 API 집합과 하나의 앱 패키지, 그리고 하나의 스토어만을 사용해 UWP용 앱을 개발하여 PC, 태블릿, 휴대폰, Xbox, HoloLens, Surface Hub 등의 모든 Windows 10 디바이스에서 사용할 수 있습니다. 다양한 화면 크기뿐만 아니라 터치, 마우스 및 키보드, 게임 컨트롤러, 펜 등 다양한 조작 모델을 손쉽게 지원합니다. UWP 앱의 핵심은 사용자가 자신의 모든 장치에서 모바일 *환경*을 원하고 작업에 가장 편리하거나 생산적인 장치를 사용하기를 원한다는 개념입니다.

또한 UWP는 유연합니다. 원치 않을 경우 C# 및 XAML을 사용할 필요가 없습니다. Unity 또는 MonoGame으로 개발해도 됩니다. JavaScript로 개발해도 됩니다. 원하는 것을 사용하면 됩니다. UWP 기능으로 확장하고 스토어에서 판매하려는 C++ 데스크톱 앱이 있나요? 그것도 가능합니다. 

결론: 하나의 프로젝트에서 친숙한 프로그래밍 언어, 프레임워크 및 API를 모두 사용하여 작업을 수행하고, 현존하는 다양한 Windows 하드웨어에서 동일한 코드를 실행할 수 있습니다. UWP 앱을 작성한 후에는 전 세계의 사람들이 볼 수 있도록 스토어에 게시할 수 있습니다.

![Windows 기반 디바이스](images/1894834-hig-device-primer-01-500.png)
 
##<a name="so-what-exactly-is-a-uwp-app"></a>UWP 앱은 *정확히* 무엇인가요?

무엇이 UWP 앱을 특별하게 만드나요? 다음은 Windows 10의 UWP 앱을 차별화하는 몇 가지 특성입니다.

-   **모든 디바이스 간의 공통 API 표면이 있습니다.**

    UWP(유니버설 Windows 플랫폼) 핵심 API가 모든 Windows 장치 클래스에 대해 동일합니다. 앱이 핵심 API만 사용하는 경우 데스크톱 PC, Xbox 또는 혼합 현실 헤드셋 중 무엇을 대상으로 하든 모든 Windows 10 장치에서 실행됩니다.

-   **확장 SDK를 사용하면 앱이 특정 디바이스 유형에서 유용한 작용을 합니다.**

    확장 SDK는 각 디바이스 클래스에 특화된 API를 추가합니다. 예를 들어 UWP 앱이 HoloLens를 대상으로 하는 경우 일반적인 UWP 핵심 API 외에 HoloLens 기능도 추가할 수 있습니다.
    범용 API를 대상으로 하는 경우 Windows 10을 실행하는 모든 디바이스에서 앱 패키지가 실행됩니다. 하지만 UWP 앱이 특정 디바이스 클래스에서 실행 중인 디바이스에 특화된 API를 활용하도록 하려면 API를 호출하기 전에 런타임 시 API가 있는지 확인하면 됩니다. 

-   **앱은 .AppX 패키지 형식을 사용하여 패키징되고 스토어에서 배포됩니다.**

    모든 UWP 앱은 AppX 패키지로 배포됩니다. 이는 신뢰할 수 있는 설치 메커니즘을 제공하며 앱을 원활하게 배포 및 업데이트할 수 있도록 해줍니다.

-   **모든 장치를 위한 하나의 스토어가 있습니다.**

    앱 개발자로 등록한 후 스토어에 앱을 제출하고 모든 디바이스 유형에서 사용하도록 하거나 선택한 디바이스 유형에서만 사용하도록 할 수 있습니다. 한 곳에서 모든 Windows 장치용 앱을 제출하고 관리합니다.

-   **앱에서 적응형 컨트롤 및 입력 지원**

    UI 요소에서 *유효 픽셀*([UWP 앱용 반응형 디자인 101](https://msdn.microsoft.com/library/windows/apps/Dn958435) 참조)을 사용하므로 디바이스에서 사용할 수 있는 화면 픽셀 수에 따라 효과적인 레이아웃으로 응답할 수 있습니다. 또한 키보드, 마우스, 터치, 펜, Xbox One 컨트롤러 등 여러 입력 유형에서 원활하게 작동합니다. 특정 화면 크기나 장치에 맞게 UI를 추가로 조정해야 하는 경우 새로운 레이아웃 패널 및 도구를 통해 앱이 실행될 수 있는 장치에 UI를 맞출 수 있습니다.



## <a name="use-a-language-you-already-know"></a>이미 알고 있는 언어 사용


UWP 앱은 운영 체제에서 기본 제공되는 API인 Windows 런타임을 사용합니다. 이 API는 C++에서 구현되고 C#, Visual Basic, C++ 및 JavaScript에서 지원됩니다. UWP에서 앱을 작성하기 위한 몇 가지 옵션은 다음과 같습니다.
-   XAML UI 및 C#, VB 또는 C++ 백 엔드
-   DirectX UI 및 C++ 백 엔드
-   JavaScript 및 HTML

Microsoft Visual Studio 2017에서는 각 언어에 대해 모든 디바이스의 단일 프로젝트를 만들 수 있는 UWP 앱 템플릿을 제공합니다. 작업이 완료되면 Visual Studio 내에서 앱 패키지를 생성하고 Windows 스토어에 제출하여 모든 Windows 10 장치의 고객에게 앱을 제공할 수 있습니다.

## <a name="uwp-apps-come-to-life-on-windows"></a>Windows에서 UWP 앱의 가치 발휘


Windows에서 앱은 사용자에게 관련 정보를 실시간으로 제공하고 사용자가 더 많은 정보를 위해 계속 이용하게 만들 수 있습니다. 현대적인 앱 경제학에서는 앱이 사용자의 생활 전반에서 활용되려면 매력적이어야 합니다. Windows는 사용자가 앱을 계속 이용하게 하는 다양한 리소스를 제공합니다.

-   라이브 타일과 잠금 화면은 상황에 맞고 시의 적절한 정보를 한눈에 볼 수 있도록 표시합니다.

-   푸시 알림은 사용자가 필요할 때 최신 경고를 실시간으로 표시합니다.

-   관리 센터를 통해 사용자가 작업을 수행해야 하는 알림 및 콘텐츠를 구성하고 표시할 수 있습니다.

-   백그라운드 실행 및 트리거는 사용자가 필요할 때만 앱에 생명을 불어 넣습니다.

-   사용자가 전 세계와 상호 작용하는 데 도움이 되도록 앱에서 음성 및 Bluetooth LE 디바이스를 사용할 수 있습니다.

-   풍부한 디지털 잉크와 혁신적인 다이얼을 지원합니다.

-   Cortana는 소프트웨어에 퍼스낼리티를 더합니다.

-   XAML은 부드럽고 애니메이션 효과를 내는 사용자 인터페이스를 만드는 도구를 제공합니다.

마지막으로, 로밍 데이터와 Windows 자격 증명 보관을 통해 사용자가 앱을 실행하는 모든 Windows 화면에서 일관된 로밍 환경을 사용할 수 있습니다. 로밍 데이터를 사용하면 고유한 동기화 인프라를 구축하지 않고도 사용자 기본 설정을 클라우드에 쉽게 저장할 수 있습니다. 또한 보안과 안정성이 최고 우선 순위인 자격 증명 보관에 사용자 자격 증명을 저장할 수 있습니다.

##  <a name="monetize-your-app"></a>앱으로 수익 창출


Windows에서 휴대폰, 태블릿, PC 및 기타 디바이스를 통해 앱으로 수익을 창출하는 방법을 선택할 수 있습니다. 앱과 앱에서 제공하는 서비스로 돈을 버는 다양한 방법이 제공됩니다. 가장 적합한 방법을 선택하기만 하면 됩니다.

-   가장 간단한 옵션은 유료 다운로드입니다. 가격만 정하세요.
-   평가판은 사용자가 앱을 구입하기 전에 시험 삼아 사용하여 기존의 "프리미엄(Freemium)" 옵션보다 앱 검색률과 구매전환율을 더욱 쉽게 높일 수 있게 해줍니다.
-   앱 및 추가 기능에 할인 가격을 적용하세요.
-   앱에서 바로 구매 기능과 광고도 사용할 수 있습니다.

## <a name="lets-get-started"></a>시작하기


UWP에 대한 자세한 내용은 [유니버설 Windows 플랫폼 앱 가이드](universal-application-platform-guide.md)를 참조하세요. 그런 다음 [설정 방법](get-set-up.md)을 확인하여 앱 만들기를 시작한 후 [첫 번째 앱](your-first-app.md)을 작성하는 데 필요한 도구를 다운로드하세요.


## <a name="more-advanced-topics"></a>고급 항목

* [.NET 네이티브 - UWP(유니버설 Windows 플랫폼) 개발자를 위한 의미](https://blogs.windows.com/buildingapps/2015/08/20/net-native-what-it-means-for-universal-windows-platform-uwp-developers/#TYsD3tJuBJpK3Hc7.97)
* [.NET의 유니버설 Windows 앱](https://blogs.msdn.microsoft.com/dotnet/2015/07/30/universal-windows-apps-in-net)
* [UWP 앱용 .NET](https://msdn.microsoft.com/library/mt185501.aspx)
