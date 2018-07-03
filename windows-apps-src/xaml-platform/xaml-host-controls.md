---
author: normesta
description: 이 가이드에서는 WPF 및 Windows Forms 응용 프로그램에서 직접 Fluent 기반 UWP UI를 만들 수 있습니다.
title: WPF 및 Windows Forms 응용 프로그램의 호스트 UWP 컨트롤
ms.author: normesta
ms.date: 05/03/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp, windows forms, wpf
keywords: Windows 10, uwp, windows 양식, wpf
ms.localizationpriority: medium
ms.openlocfilehash: 4823654bce3373ace5b04ced8ec14c4b6c1b6f1d
ms.sourcegitcommit: 3500825bc2e5698394a8b1d2efece7f071f296c1
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/09/2018
ms.locfileid: "1862072"
---
# <a name="host-uwp-controls-in-wpf-and-windows-forms-applications"></a>WPF 및 Windows Forms 응용 프로그램의 호스트 UWP 컨트롤

> [!NOTE]
> 일부 정보는 상업용으로 출시되기 전에 상당 부분 수정될 수 있는 시험판 제품과 관련이 있습니다. Microsoft는 여기에 제공된 정보에 대해 명시적 또는 묵시적 보증을 하지 않습니다.

기존 WPF 또는 흐름 디자인 기능이 있는 Windows 응용 프로그램의 모양과 느낌, 기능을 향상시킬 수 있도록 데스크톱에 UWP 컨트롤을 포함했습니다. 이를 위한 두 가지 방법은 다음과 같습니다.

첫째, WPF 또는 Windows Forms 프로젝트의 디자인 화면에 바로 컨트롤을 추가하고 디자이너에서 다른 컨트롤처럼 사용할 수 있습니다.  지금 새 **WebView**컨트롤을 사용해 보세요. 이 컨트롤은 Microsoft Edge 렌더링 엔진을 사용하며 지금까지 이 컨트롤은 UWP 응용 프로그램에만 사용할 수 있었습니다. [Windows 커뮤니티 도구 키트](https://docs.microsoft.com/windows/uwpcommunitytoolkit/)의 최신 버전에서 **WebView**를 찾을 수 있습니다.

곧 더 많은 흐름 디자인 기능에도 액세스할 수 있습니다. 다양한 UWP 컨트롤을 호스트할 수 있는 컨트롤이 제공될 예정입니다. 향후 Windows 커뮤니티 도구 키트의 릴리스에서 이 컨트롤과 여러 기타 컨트롤을 확인하세요.

여기에서 이러한 컨트롤이 아키텍처 측면에서 구성되는 방법을 빠르게 확인할 수 있습니다. 이 다이어그램에 사용되는 이름은 변경될 수 있습니다.  

![호스트 컨트롤 아키텍처](images/host-controls.png)

이 다이어그램 맨 아래에 표시되는 API는 Windows SDK와 함께 제공됩니다.  디자이너에 추가할 컨트롤은 Windows 커뮤니티 도구 키트에 NuGet 패키지로 제공됩니다.

이러한 새 컨트롤은 제한 사항이 있으므로 사용하기 전에 잠시 지원되지 않는 사항과 임시 방편으로만 사용할 수 있는 기능을 살펴보세요.

### <a name="whats-supported"></a>지원되는 사항

대부분의 경우 아래 목록에서 명시적으로 언급되지 않으면 모든 사항이 지원됩니다.

### <a name="whats-supported-only-with-workarounds"></a>임시 방편으로만 지원되는 사항

:heavy_check_mark: 다중 창 내의 여러 받은 편지함 컨트롤을 호스팅. 자체 스레드에 각 창을 배치해야 합니다.

:heavy_check_mark: 호스트된 컨트롤로 ``x:Bind`` 사용. .NET 표준 라이브러리에서 데이터 모델을 선언해야 합니다.

:heavy_check_mark: C# 기반 타사 컨트롤. 타사 컨트롤에 대한 소스 코드가 있는 경우 이에 대해 컴파일할 수 있습니다.

### <a name="whats-not-yet-supported"></a>아직 지원되지 않는 사항

:no_entry_sign: 응용 프로그램 및 호스트된 컨트롤에서 원활하게 작동하는 접근성 도구.

:no_entry_sign: Windows 앱 패키지를 포함하지 않는 응용 프로그램에 추가하는 컨트롤의 지역화된 콘텐츠.

:no_entry_sign: Windows 앱 패키지를 포함하지 않는 응용 프로그램 내의 XAML에서 만든 자산 참조.

:no_entry_sign: DPI 및 규모의 변경에 제대로 응답하는 컨트롤.

:no_entry_sign: 사용자 지정 사용자 컨트롤(스레드에서, 스레드 밖에서 또는 프로세서 밖에서)에 **웹 보기** 컨트롤 추가.

:no_entry_sign: [강조 표시](https://docs.microsoft.com/windows/uwp/design/style/reveal) Fluent 효과.

:no_entry_sign: 입력 컨트롤에 대한 수동 입력, @Places 및 @People.

:no_entry_sign: 바로 가기를 키 할당.

:no_entry_sign: C++ 기반 타사 컨트롤.

:no_entry_sign: 사용자 지정 사용자 컨트롤 호스팅.

바탕 화면에 Fluent를 가져오는 환경을 개선하기 위해 이 목록의 항목은 계속해서 변경될 수 있습니다.  
