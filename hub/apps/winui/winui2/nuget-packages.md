---
title: Windows UI 라이브러리 NuGet 패키지 목록
description: Windows UI 라이브러리용 NuGet 패키지를 나열합니다.
ms.topic: article
ms.date: 07/15/2020
keywords: Windows 10, UWP, 도구 키트 SDK
ms.openlocfilehash: ca3f2915d77bb83f45744e90bd86e82edba013b8
ms.sourcegitcommit: c1226b6b9ec5ed008a75a3d92abb0e50471bb988
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86492888"
---
# <a name="windows-ui-library-nuget-packages"></a>Windows UI 라이브러리 NuGet 패키지

NuGet은 Visual Studio에 기본적으로 제공되는 .Net 애플리케이션의 표준 패키지 관리자입니다. 열려 있는 솔루션에서 *도구* 메뉴, *NuGet 패키지 관리자*, *솔루션용 NuGet 패키지 관리...* 를 차례로 선택하여 UI를 엽니다.  온라인에서 검색하려면 아래 패키지 이름 중 하나를 입력하세요.

| NuGet 패키지 이름 | 설명 |
| --- | --- |
| [Microsoft.UI.Xaml](https://www.nuget.org/packages/Microsoft.UI.Xaml/) | UWP 앱용 컨트롤입니다. 다음 네임스페이스의 API를 포함하고 있습니다. [Microsoft.UI.Xaml](/uwp/api/microsoft.ui.xaml), [Microsoft.UI.Xaml.Automation.Peers](/uwp/api/microsoft.ui.xaml.automation.peers), [Microsoft.Ui.Xaml.Controls](/uwp/api/microsoft.ui.xaml.controls), [Microsoft.UI.Xaml.Controls.Primitives](/uwp/api/microsoft.ui.xaml.controls.primitives), [Microsoft.UI.Xaml.CustomAttributes](/uwp/api/microsoft.ui.xaml.customattributes), [Microsoft.UI.Xaml.Media](/uwp/api/microsoft.ui.xaml.media), [Microsoft.Ui.Xaml.XamlTypeInfo](/uwp/api/microsoft.ui.xaml.xamltypeinfo) |
| [Microsoft.UI.Xaml.Core.Direct](https://www.nuget.org/packages/Microsoft.UI.Xaml.Core.Direct) | 이를 사용하도록 설정하면 여러 대상 Windows 10 버전을 처리하는 특수 코드를 작성할 필요 없이 이전 버전의 Windows 10에서 [XamlDirect](/uwp/api/microsoft.ui.xaml.core.direct.xamldirect) API를 사용할 수 있습니다. |


## <a name="search-in-visual-studio"></a>Visual Studio에서 검색

Visual Studio 패키지 관리자에서 검색하면 다음과 비슷한 목록이 표시됩니다(버전 번호는 다를 수 있지만 이름은 동일해야 함).

![NuGet 패키지 관리자](images/NugetPackages.png)

## <a name="update-nuget-packages"></a>NuGet 패키지 업데이트

정기적으로 Windows UI 라이브러리를 새 컨트롤, 서비스, API 및 특히 버그 수정으로 업데이트합니다. 최신 버전인지 확인하려면 Visual Studio에서 프로젝트를 열고, **도구** 메뉴를 선택하고, **NuGet 패키지 관리자** -> **솔루션용 NuGet 패키지 관리...** 를 차례로 선택하고, *업데이트* 탭을 선택합니다. 업데이트하려는 패키지를 선택하고, [설치]를 클릭하여 최신 버전으로 업데이트합니다.

## <a name="getting-started"></a>시작하기

사용자 고유의 프로젝트에서 이러한 NuGet 패키지를 사용하는 방법에 대한 자세한 지침은 [Windows UI 라이브러리 시작](getting-started.md)을 참조하세요.
