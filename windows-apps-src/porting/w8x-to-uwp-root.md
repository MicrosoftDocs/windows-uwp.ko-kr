---
description: If you have a Universal 8.1 app&\#8212;whether it's targeting Windows 8.1, Windows Phone 8.1, or both&\#8212;then you'll find that your source code and skills will port smoothly to Windows 10.
title: Windows 런타임 8.x에서 UWP로 이동'
ms.assetid: ac163b57-dee0-43fa-bab9-8c37fbee3913
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: dafa2b5820c0c93f5b7ff5fbefc2b7d4db6d3018
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259095"
---
# <a name="move-from-windows-runtime-8x-to-uwp"></a>Windows 런타임 8.x에서 UWP로 이동


If you have a Universal 8.1 app—whether it's targeting Windows 8.1, Windows Phone 8.1, or both—then you'll find that your source code and skills will port smoothly to Windows 10. With Windows 10, you can create a Universal Windows Platform (UWP) app, which is a single app package that your customers can install onto every kind of device. For more background on Windows 10, UWP apps, and the concepts of adaptive code and adaptive UI that we'll mention in this porting guide, see [Guide to UWP apps](https://docs.microsoft.com/windows/uwp/get-started/universal-application-platform-guide).

While porting, you'll find that Windows 10 shares the majority of APIs with the previous platforms, as well as XAML markup, UI framework, and tooling, and you'll find it all reassuringly familiar. 이전과 마찬가지로 C++, C# 및 Visual Basic 중에서 XAML UI 프레임워크와 함께 사용할 프로그래밍 언어를 선택할 수 있습니다. 처음에 현재 앱을 사용해서 수행할 작업을 정확히 계획할 때는 앱 및 프로젝트 종류를 고려해야 합니다. 이 내용은 다음 섹션에서 설명합니다.

## <a name="if-you-have-a-universal-81-app"></a>유니버설 8.1 앱이 있는 경우

유니버설 8.1 앱은 8.1 유니버설 앱 프로젝트에서 빌드합니다. Let's say the project's name is AppName\_81. 여기에는 다음과 같은 하위 프로젝트가 포함되어 있습니다.

-   AppName\_81.Windows. This is the project that builds the app package for Windows 8.1.
-   AppName\_81.WindowsPhone. Windows Phone 8.1용 앱 패키지를 빌드하는 프로젝트입니다.
-   AppName\_81.Shared. 두 프로젝트 모두에서 사용되는 소스 코드, 태그 파일, 기타 자산 및 리소스가 포함된 프로젝트입니다.

Often, an 8.1 Universal Windows app offers the same features—and does so using the same code and markup—in both its Windows 8.1 and Windows Phone 8.1 forms. An app like that is an ideal candidate for porting to a single Windows 10 app that targets the Universal device family (and that you can install onto the widest range of devices). 공유 프로젝트의 콘텐츠를 기본적으로 포팅하게 되며, 다른 두 프로젝트에는 콘텐츠가 거의 없거나 전혀 없으므로 포팅할 필요가 없습니다.

Other times, the Windows 8.1 and/or the Windows Phone 8.1 form of the app contain unique features. 또는 동일한 기능을 포함하지만 다른 기법이나 기술을 사용하여 이러한 기능을 구현하기도 합니다. 유니버설 디바이스 패밀리를 대상으로 하는 단일 앱으로 포팅하도록 선택할 수 있는 앱(앱 자체를 다른 장치에 맞게 조정하려는 경우) 또는 둘 이상의 앱으로 포팅하도록 선택할 수 있는 앱(예: 데스크톱 디바이스 패밀리 대상 앱 및 모바일 디바이스 패밀리 대상 앱). 유니버설 8.1 앱의 특성에 따라 어떤 옵션이 가장 적합한지 결정합니다.

1.  유니버설 디바이스 패밀리를 대상으로 하는 앱에 공유 프로젝트의 콘텐츠를 포팅합니다. 해당되는 경우 Windows 및 WindowsPhone 프로젝트의 다른 콘텐츠를 복구하여 앱에서 조건 없이 사용하거나 앱이 실행될 수 있는 장치에서 조건부로 사용합니다(후자의 경우를 *적응* 방식이라고 함).
2.  범용 디바이스 패밀리를 대상으로 하는 앱에 WindowsPhone 프로젝트의 콘텐츠를 포팅합니다. 해당되는 경우 무조건 또는 적응 방식으로 Windows 프로젝트의 다른 콘텐츠를 복구합니다.
3.  범용 디바이스 패밀리를 대상으로 하는 앱에 Windows 프로젝트의 콘텐츠를 포팅합니다. 해당되는 경우 무조건 또는 적응 방식으로 WindowsPhone 프로젝트의 다른 콘텐츠를 복구합니다.
4.  Windows 프로젝트의 내용을 범용 또는 데스크톱 디바이스 패밀리를 대상으로 하는 앱으로 포팅하고, WindowsPhone 프로젝트의 내용을 범용 또는 모바일 디바이스 패밀리를 대상으로 하는 앱으로 포팅합니다. 공유 프로젝트를 사용하여 솔루션을 만들 수 있으며 두 프로젝트 간에 소스 코드, 태그 파일 및 기타 자산과 리소스를 계속 공유할 수 있습니다. 또는 다른 솔루션을 만들고 링크를 사용하여 동일한 항목을 계속 공유할 수 있습니다.

## <a name="if-you-have-a-windows-81-app"></a>Windows 8.1 앱이 있는 경우

유니버설 또는 데스크톱 디바이스 패밀리를 대상으로 하는 앱에 프로젝트를 포팅합니다. 유니버설 디바이스 패밀리를 선택하며 앱이 데스크톱 디바이스 패밀리에서만 구현되는 API를 호출하는 경우 적응 코드로 이러한 호출을 지원할 수 있습니다.

## <a name="if-you-have-a-windows-phone-81-app"></a>Windows Phone 8.1 앱이 있는 경우

유니버설 또는 모바일 디바이스 패밀리를 대상으로 하는 앱에 프로젝트를 포팅합니다. 유니버설 디바이스 패밀리를 선택하며 앱이 모바일 디바이스 패밀리에서만 구현되는 API를 호출하는 경우 적응 코드로 이러한 호출을 지원할 수 있습니다.

## <a name="adapting-your-app-to-multiple-form-factors"></a>여러 양식 요소에 맞게 앱 조정

이전 섹션에서 선택한 옵션에 따라 앱이 실행되는 장치 범위가 결정되며, 이러한 장치는 다양할 수 있습니다. 앱을 모바일 디바이스 패밀리로 제한해도 지원할 화면 크기는 무척 다양합니다. 따라서 앱을 이전에는 지원하지 않았던 양식 요소에서 실행하려는 경우 해당 양식 요소에서 UI를 테스트하고 UI가 각 양식 요소에 잘 맞도록 필요한 사항을 변경하도록 합니다. 이것을 포팅 후 작업 또는 포팅 추가 목표로 생각해도 되며 [Bookstore2](w8x-to-uwp-case-study-bookstore2.md) 및 [QuizGame](w8x-to-uwp-case-study-quizgame.md) 사례 연구의 연습에 몇 가지 예가 있습니다.

## <a name="approaching-porting-layer-by-layer"></a>계층별 포팅에 대한 접근법

유니버설 8.1 앱을 UWP 앱 모델로 포팅할 경우 대부분의 지식과 경험은 물론이고, 소스 코드와 태그 및 사용 중인 소프트웨어 패턴도 이전됩니다.

-   **보기**. 보기는 보기 모델과 함께 앱의 UI를 구성합니다. 보기는 보기 모델의 관찰 가능한 속성에 바인딩되는 태그로 구성되는 것이 좋습니다. 다른 패턴(일반적이고 편리하지만 단기적으로만 사용)은 코드 숨김 파일의 명령적 코드에서 UI 요소를 직접 조작하는 데 사용됩니다. 어느 경우든 UI 태그와 디자인 및 UI 요소를 조작하는 명령적 코드를 포팅하는 것은 간단합니다.
-   **보기 모델 및 데이터 모델**. 관심사 분리 패턴(예제: MVVM)을 공식적으로 따르지 않더라도 앱에는 보기 모델과 데이터 모델의 기능을 수행하는 코드가 있기 마련입니다. 보기 모델 코드에서는 UI 프레임워크 네임스페이스의 형식을 활용합니다. 또한 보기 모델 및 데이터 모델 코드는 모두 비시각적 운영 체제와 .NET Framework API(데이터 액세스를 위한 API 포함)를 사용합니다. 또한 이러한 API는 [UWP 앱에서도 사용할 수 있으므로](https://docs.microsoft.com/previous-versions/windows/br211369(v=win.10)) 전체는 아니더라도 이 코드 대부분이 변경 없이 포팅됩니다.
-   **클라우드 서비스**. 앱의 상당 부분이 클라우드에서 서비스의 형태로 실행될 수 있습니다. 클라이언트 장치에서 실행 중인 앱 부분이 해당 부분에 연결됩니다. 이 부분은 클라이언트 부분을 포팅할 때 변경되지 않고 유지되는 분산 앱 부분입니다. UWP 앱에 대한 클라우드 서비스 옵션이 없을 경우, 유니버설 Windows 앱에서 간단한 라이브 타일 업데이트 알림부터 서버 팜에서 제공 가능한 복잡한 확장성까지 다양한 서비스를 호출할 수 있는 강력한 백 엔드 구성 요소를 제공하는 [Microsoft Azure 모바일 서비스](https://azure.microsoft.com/services/mobile-services/)를 선택하는 것이 좋습니다.

포팅 전이나 포팅 중에 비슷한 용도를 가진 코드가 임의적으로 분산되지 않고 계층으로 함께 수집되도록 앱을 리펙터링하여 개선할 수 있는지 여부를 고려하세요. 위에서 설명한 것처럼 앱을 계층으로 팩터링하면 앱을 수정하고 테스트한 다음 지속적으로 읽고 유지 관리하기가 쉬워집니다. Model-View-ViewModel([MVVM](https://msdn.microsoft.com/magazine/dd419663.aspx)) 패턴에 따라 기능을 재사용하기 쉽게 만들 수 있습니다. 이 패턴은 앱의 데이터, 비즈니스 및 UI 부분을 서로 별도로 유지합니다. UI 내에서도 상태와 동작을 시각적으로 분리하고 별도로 테스트할 수 있습니다. MVVM을 사용하면 데이터와 비즈니스 논리를 한 번 작성하여 UI에 관계없이 모든 장치에서 사용할 수 있습니다. 또한 장치 간에 많은 보기 모델 및 보기 부분이 다시 사용될 수 있습니다.

| 항목 | 설명 |
|-------|-------------|
| [Porting the project](w8x-to-uwp-porting-to-a-uwp-project.md) | 포팅 프로세스를 시작할 경우 두 가지 옵션이 있습니다. 하나는 앱 패키지 매니페스트를 비롯하여 기존 프로젝트 파일의 복사본을 편집하는 옵션입니다(해당 옵션은 [UWP(유니버설 Windows 플랫폼)으로 앱 마이그레이션](https://docs.microsoft.com/visualstudio/misc/migrate-apps-to-the-universal-windows-platform-uwp?view=vs-2015)에서 프로젝트 파일을 업데이트하는 방법에 대한 정보 참조). 다른 하나는 Visual Studio에서 새 Windows 10 프로젝트를 만들고 해당 프로젝트에 파일을 복사하는 옵션입니다. |
| [문제 해결](w8x-to-uwp-troubleshooting.md) | 이 포팅 가이드를 끝까지 읽어 보시고 프로젝트를 빌드하여 실행하는 단계를 신속하게 진행해 보세요. 그렇게 하려면 필수가 아닌 코드에 주석이나 설명을 추가하고 나중에 돌아와서 해당 부채를 청산하여 일시적으로 진행할 수 있습니다. 이 항목의 문제 해결 증상 및 해결 방법 표는 다음 몇 개 항목을 대체하지는 않지만 이 단계에 유용한 정보를 제공합니다. 이후 항목을 진행하는 중에 언제든지 이 표를 다시 참조할 수 있습니다. |
| [Porting XAML and UI](w8x-to-uwp-porting-xaml-and-ui.md) | 선언적 XAML 태그 형식으로 UI를 정의하는 방법을 사용하면 유니버설 8.1 앱에서 UWP 앱으로 매우 원활하게 변환됩니다. 사용 중인 시스템 리소스 키 또는 사용자 지정 템플릿을 약간 조정해야 할 수도 있지만 대부분의 태그는 호환됩니다. |
| [Porting for I/O, device, and app model](w8x-to-uwp-input-and-sensors.md) | 디바이스 및 센서와 통합되는 코드는 사용자의 입력과 사용자에 대한 출력을 포함합니다. 데이터 처리를 포함할 수도 있습니다. 하지만 이 코드는 UI 계층 또는 데이터 계층으로 간주되지 않습니다. 이 코드는 진동 컨트롤러, 가속도계, 자이로스코프, 마이크와 스피커(음성 인식 및 음성 합성과 교차), 지리적 위치, 입력 형식(예제: 터치, 마우스, 키보드, 펜) 등과의 통합을 포함합니다. |
| [Case study: Bookstore1](w8x-to-uwp-case-study-bookstore1.md) | 이 항목에서는 매우 간단한 유니버설 8.1 앱을 Windows 10 UWP 앱으로 포팅하는 사례 연구를 제공합니다. 유니버설 8.1 앱은 Windows 8.1용 앱 패키지와 Windows Phone 8.1용 앱 패키지를 각각 빌드한 앱입니다. Windows 10을 사용하면 고객이 다양한 디바이스에 설치할 수 있는 단일 앱 패키지를 만들 수 있습니다. 이 사례 연구에서는 이 작업을 수행합니다. [UWP 앱 지침](https://docs.microsoft.com/windows/uwp/get-started/universal-application-platform-guide)을 참조하세요. |
| [Case study: Bookstore2](w8x-to-uwp-case-study-bookstore2.md) | [SemanticZoom](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SemanticZoom) 컨트롤에 제공된 정보를 기반으로 하는 이 사례 연구입니다. 보기 모델에서 Author 클래스의 각 인스턴스는 해당 저자가 쓴 책의 그룹을 나타내며, SemanticZoom에서 저자가 그룹화한 책 목록을 보거나 저자의 점프 목록을 축소할 수 있습니다. |
| [Case study: QuizGame](w8x-to-uwp-case-study-quizgame.md) | 이 항목에서는 작동하는 피어 투 피어 퀴즈 게임 WinRT 8.1 샘플 앱을 Windows 10 UWP 앱에 포팅하는 사례 연구를 제공합니다. |

## <a name="related-topics"></a>관련 항목

**설명서**
* [Windows Runtime reference](https://docs.microsoft.com/uwp/api/)
* [Building Universal Windows apps for all Windows devices](https://docs.microsoft.com/windows/uwp/get-started/universal-application-platform-guide)
* [Designing UX for apps](https://docs.microsoft.com/previous-versions/windows/hh767284(v=win.10))
