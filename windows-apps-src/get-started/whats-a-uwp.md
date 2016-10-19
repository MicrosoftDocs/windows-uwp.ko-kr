---
author: GrantMeStrength
ms.assetid: C9787269-B54F-4FFA-A884-D4A3BF28F80D
title: "UWP(유니버설 Windows 플랫폼) 앱이란?"
description: "유니버설 Windows 앱이라 불리는 다양한 유형의 앱(Windows 스토어 앱, Windows Phone 스토어 앱 및 Windows 런타임 앱)에 대해 알아보세요."
translationtype: Human Translation
ms.sourcegitcommit: 6be8bb0a78b614e160fb40629601bdab2d7fc71a
ms.openlocfilehash: 3fd9f4d539977c4b531e08efe0719328365c757c

---

# UWP(유니버설 Windows 플랫폼) 앱이란?

Windows 플랫폼을 처음 사용하거나 .NET, Windows Forms 또는 Silverlight에서 전환한 경우 UWP 앱에 대해 *궁금할 수 있습니다*. 

당황하지 마세요. 금방 익숙해지실 것입니다. 

UWP(유니버설 Windows 플랫폼) 앱은 Windows 8에서 Windows 런타임으로 처음 도입된 UWP(유니버설 Windows 플랫폼)를 기반으로 하는 Windows 환경입니다. UWP 앱의 핵심은 사용자가 자신의 모든 장치에서 모바일 *환경*을 원하고 작업에 가장 편리하거나 생산적인 장치를 사용하기를 원한다는 개념입니다.

Windows 10에서는 하나의 API 집합과 하나의 앱 패키지, 그리고 하나의 스토어만을 사용해 UWP용 앱을 이전보다 더욱 쉽게 개발하여 PC, 태블릿, 휴대폰, Xbox, HoloLens, Surface Hub 등의 모든 Windows 10 디바이스에서 사용할 수 있습니다. 다양한 화면 크기뿐만 아니라 터치, 마우스 및 키보드, 게임 컨트롤러, 펜 등 다양한 조작 모델을 손쉽게 지원합니다.

결론: 친숙한 프로그래밍 언어 및 API를 사용하여 하나의 프로젝트에서 작업을 수행하고 현존하는 다양한 Windows 하드웨어에서 동일한 코드를 실행할 수 있습니다.

![Windows 기반 장치](images/1894834-hig-device-primer-01-500.png)

##UWP 앱은 *정확히* 무엇인가요?


무엇이 UWP 앱을 특별하게 만드나요? 다음은 Windows 10의 UWP 앱을 차별화하는 몇 가지 특성입니다.

-   OS가 아니라 디바이스 패밀리를 대상으로 합니다.

    디바이스 패밀리는 디바이스 패밀리 내의 장치에서 기대할 수 있는 API, 시스템 특성 및 동작을 식별합니다. 또한 스토어에서 앱을 설치할 수 있는 장치 집합을 결정합니다.

-   .AppX 패키지 형식을 사용하여 앱이 패키지되고 배포됩니다.

    모든 UWP 앱은 AppX 패키지로 배포됩니다. 이는 신뢰할 수 있는 설치 메커니즘을 제공하며 앱을 원활하게 배포 및 업데이트할 수 있도록 해줍니다.

-   모든 장치를 위한 하나의 스토어가 있습니다.

    앱 개발자로 등록한 후 스토어에 앱을 제출하고 모든 디바이스 패밀리에서 사용할 수 있도록 하거나 선택한 디바이스 패밀리에서만 사용할 수 있도록 할 수 있습니다. 한 곳에서 모든 Windows 장치용 앱을 제출하고 관리합니다.

-   디바이스 패밀리 간의 공통 API 표면이 있습니다.

    UWP(유니버설 Windows 플랫폼) 핵심 API가 모든 Windows 디바이스 패밀리에 대해 동일합니다. 앱에서 핵심 API만 사용하는 경우 이 앱은 모든 Windows 10 장치에서 실행됩니다.

-   확장 SDK를 통해 특수 장치에서 앱을 실행할 수 있습니다.

    확장 SDK는 각 디바이스 패밀리용 특수 API를 추가합니다. 앱은 특정 디바이스 패밀리용으로 만들어진 경우 이러한 API를 사용하여 실행할 수 있습니다. 뿐만 아니라 확장 API를 호출하기 전에 앱이 실행 중인 디바이스 패밀리를 확인하여 모든 장치에서 실행되는 하나의 앱 패키지를 만들 수 있습니다.

-   적응형 컨트롤 및 입력

    UI 요소에서 *유효 픽셀*([UWP 앱용 반응형 디자인 101](https://msdn.microsoft.com/library/windows/apps/Dn958435) 참조)을 사용하므로 장치에서 사용할 수 있는 화면 픽셀 수에 따라 자동으로 적응합니다. 또한 키보드, 마우스, 터치, 펜, Xbox One 컨트롤러 등 여러 입력 유형에서 원활하게 작동합니다. 특정 화면 크기나 장치에 맞게 UI를 추가로 조정해야 하는 경우 새로운 레이아웃 패널 및 도구를 통해 앱이 실행될 수 있는 장치에 UI를 맞출 수 있습니다.

UWP에 대한 자세한 내용은 [유니버설 Windows 플랫폼 앱 가이드](universal-application-platform-guide.md)를 참조하세요.

## 이미 알고 있는 언어 사용


C# 또는 Visual Basic과 XAML, JavaScript와 HTML, C++와 DirectX 및/또는 XAML(Extensible Application Markup Language) 등 가장 친숙한 프로그래밍 언어를 사용하여 UWP 앱을 만들 수 있습니다. 한 언어로 구성 요소를 작성한 후 다른 언어로 작성한 앱에서 사용할 수도 있습니다.

UWP 앱은 운영 체제에서 기본 제공되는 API인 Windows 런타임을 사용합니다. 이 API는 C++로 구현되며 C#, Visual Basic, C++ 및 JavaScript 언어에서도 각각 적합한 방식으로 지원됩니다.

Microsoft Visual Studio 2015에서는 각 언어에 대해 모든 장치의 단일 프로젝트를 만들 수 있는 UWP 앱 템플릿을 제공합니다. 작업이 완료되면 Visual Studio 내에서 앱 패키지를 생성하고 Windows 스토어에 제출하여 모든 Windows 10 장치의 고객에게 앱을 제공할 수 있습니다.

## Windows에서 UWP 앱의 가치 발휘


Windows에서 앱은 사용자에게 관련 정보를 실시간으로 제공하고 사용자가 더 많은 정보를 위해 계속 이용하게 만들 수 있습니다. 현대적인 앱 경제학에서는 앱이 사용자의 생활 전반에서 활용되려면 매력적이어야 합니다. Windows는 사용자가 앱을 계속 이용하게 하는 다양한 리소스를 제공합니다.

-   라이브 타일과 잠금 화면은 상황에 맞고 시의 적절한 정보를 한눈에 볼 수 있도록 표시합니다.
-   푸시 알림은 사용자가 필요할 때 최신 경고를 실시간으로 표시합니다.

-   관리 센터를 통해 사용자가 작업을 수행해야 하는 알림 및 콘텐츠를 구성하고 표시할 수 있습니다.

-   백그라운드 실행 및 트리거는 사용자가 필요할 때만 앱에 생명을 불어 넣습니다.

-   사용자가 전 세계와 상호 작용하는 데 도움이 되도록 앱에서 음성 및 Bluetooth LE 장치를 사용할 수 있습니다.

마지막으로, 로밍 데이터와 Windows 자격 증명 보관을 통해 사용자가 앱을 실행하는 모든 Windows 화면에서 일관된 로밍 환경을 사용할 수 있습니다. 로밍 데이터를 사용하면 고유한 동기화 인프라를 구축하지 않고도 사용자 기본 설정을 클라우드에 쉽게 저장할 수 있습니다. 또한 보안과 안정성이 최고 우선 순위인 자격 증명 보관에 사용자 자격 증명을 저장할 수 있습니다.

##  앱으로 수익 창출


Windows에서 휴대폰, 태블릿, PC 및 기타 디바이스를 통해 앱으로 수익을 창출하는 방법을 선택할 수 있습니다. 앱과 앱에서 제공하는 서비스로 돈을 버는 다양한 방법이 제공됩니다. 가장 적합한 방법을 선택하기만 하면 됩니다.

-   가장 간단한 옵션은 유료 다운로드입니다. 가격만 정하세요.
-   평가판은 사용자가 앱을 구입하기 전에 시험 삼아 사용하여 기존의 "프리미엄(Freemium)" 옵션보다 앱 검색율과 구매전환율을 더욱 쉽게 높일 수 있는 유용한 방법을 제공합니다.
-   앱에서 바로 구매는 앱으로 수익을 창출하는 최대 유연성을 제공합니다.

## 시작하기


UWP에 대한 자세한 내용은 [유니버설 Windows 플랫폼 앱 가이드](universal-application-platform-guide.md)를 참조하세요. 그런 다음 [설정 방법](get-set-up.md)을 확인하여 앱을 만드는 데 필요한 도구를 다운로드하세요.

## 관련 항목


* [유니버설 Windows 플랫폼 앱에 대한 가이드](universal-application-platform-guide.md)
* [설정 방법](get-set-up.md)

## 고급 항목

* [.NET 네이티브 - UWP(유니버설 Windows 플랫폼) 개발자를 위한 의미](https://blogs.windows.com/buildingapps/2015/08/20/net-native-what-it-means-for-universal-windows-platform-uwp-developers/#TYsD3tJuBJpK3Hc7.97)
* [.NET의 유니버설 Windows 앱](https://blogs.msdn.microsoft.com/dotnet/2015/07/30/universal-windows-apps-in-net)
* [UWP 앱용 .NET](https://msdn.microsoft.com/en-us/library/mt185501.aspx)



<!--HONumber=Sep16_HO3-->


