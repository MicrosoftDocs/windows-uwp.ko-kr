---
description: 새 앱에 적합한 앱 플랫폼을 선택하는 방법 및 Windows 10용 기존 앱을 현대화하는 방법을 비롯하여 Windows PC용 데스크톱 앱을 빌드하는 방법을 알아봅니다.
title: Windows PC용 데스크톱 앱 빌드
ms.topic: article
ms.date: 9/10/2020
keywords: windows win32, 데스크톱 개발
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.openlocfilehash: 554869625c0db7a2a19dc7c01837e47029bd28e6
ms.sourcegitcommit: cddc595969c658ce30fbc94ded92db4a8ad1bf66
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/11/2020
ms.locfileid: "97214270"
---
# <a name="build-desktop-apps-for-windows-pcs"></a>Windows PC용 데스크톱 앱 빌드

이 문서에서는 Windows용 데스크톱 앱 빌드 또는 Windows 10 최신 환경 도입을 위한 기존 데스크톱 앱 업데이트를 시작하는 데 필요한 정보를 제공합니다.

## <a name="platforms-for-desktop-apps"></a>데스크톱 앱용 플랫폼

Windows PC용 데스크톱 앱 빌드를 위한 주요 플랫폼에는 4가지가 있습니다. 각 플랫폼은 앱의 수명 주기, 완전한 UI 프레임워크 및 UI 컨트롤 세트를 정의하는 앱 모델을 제공합니다. 이를 통해 Word, Excel 및 Photoshop과 같은 데스크톱 앱을 만들고 Windows 기능을 사용하기 위한 포괄적인 관리형 또는 네이티브 API 세트에 액세스할 수 있습니다. 

각 플랫폼에 대한 추가 리소스를 비롯한 이 플랫폼의 상세 비교는 [앱 플랫폼 선택](choose-your-platform.md)을 참조하세요.

<br/>
<table>
<colgroup>
<col width="20%" />
<col width="60%" />
<col width="20%" />
</colgroup>
<thead>
<tr class="header">
<th>플랫폼</th>
<th>설명</th>
<th>문서 및 리소스</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><a href="/windows/uwp/">UWP(유니버설 Windows 플랫폼)</a></td>
<td><p>Windows 10 앱 및 게임을 위한 최신 플랫폼입니다. UWP 컨트롤 및 API를 독점적으로 사용하는 UWP 앱을 빌드하거나, 다른 플랫폼 중 하나를 사용하여 빌드된 데스크톱 앱에서 UWP 컨트롤 및 API를 사용할 수 있습니다.</p></td>
<td><a href="/windows/uwp/get-started/">시작</a><br/><a href="/uwp/">API 참조</a><br/><a href="https://github.com/Microsoft/Windows-universal-samples">샘플</a></td>
</tr>
<tr class="even">
<td><a href="/windows/win32/">C++/Win32</a></td>
<td><p>Windows 및 하드웨어에 직접 액세스해야 하는 네이티브 Windows 앱에 적합한 플랫폼입니다.</p></td>
<td><a href="/windows/win32/desktop-programming/">시작</a><br/><a href="/windows/win32/apiindex/windows-api-list/">API 참조</a><br/><a href="https://github.com/Microsoft/Windows-classic-samples">샘플</a></td>
</tr>
<tr class="odd">
<td><a href="/dotnet/framework/wpf/">WPF</a></td>
<td><p>XAML UI 모델을 바탕으로 하고 그래픽이 많은 관리형 Windows 앱을 위한 설정된 NET 기반 플랫폼입니다. 이러한 앱은 <a href="/dotnet/core/whats-new/dotnet-core-3-0">.NET Core 3</a> 또는 전체 .NET Framework를 대상으로 지정할 수 있습니다.</p></td>
<td><a href="/dotnet/framework/wpf/getting-started/">시작</a><br/><a href="/dotnet/api/index">API 참조(.NET)</a><br/><a href="https://github.com/Microsoft/WPF-Samples">샘플</a></td>
</tr>
<tr class="even">
<td><a href="/dotnet/framework/winforms/">Windows Forms</a></td>
<td><p>경량 UI 모델을 사용하는 관리형 LOB(기간 업무) 앱용으로 설계된 .NET 기반 플랫폼입니다. 이러한 앱은 <a href="/dotnet/core/whats-new/dotnet-core-3-0">.NET Core 3</a> 또는 전체 .NET Framework를 대상으로 지정할 수 있습니다.</p></td>
<td><a href="/dotnet/framework/winforms/getting-started-with-windows-forms">시작</a><br/><a href="/dotnet/api/index">API 참조(.NET)</a></td>
</tr>
</tbody>
</table>

### <a name="future-roadmap"></a>향후 로드맵

앞으로 Windows UI(WinUI) 라이브러리 및 프로젝트 리뉴니언을 통해 Windows 앱 개발 플랫폼을 발전시킬 것입니다.

* **WinUI** 는 Windows 10 앱에 대한 기본 UX(사용자 환경) 프레임워크입니다. WinUI는 하위 버전의 Windows 10을 대상으로 하는 UWP 앱을 위한 최신 버전 및 업데이트된 버전의 UWP 컨트롤을 제공하는 도구 키트로 시작되었습니다. WinUI는 대상 범위를 늘려 왔으며, WinUI 3(현재 미리 보기)부터 UWP, .NET 및 Win32에서 Windows 10 앱을 위한 최신 기본 UI(사용자 인터페이스) 플랫폼이 되었습니다. 

    자세한 내용은 [Windows UI(WinUI) 라이브러리](../winui/index.md)를 참조하세요.

* **프로젝트 리뉴니언**(현재 미리 보기)은 Windows 앱 개발 플랫폼의 한 단계 발전을 나타내는 광범위한 새 개발자 구성 요소 및 도구 집합의 코드 이름입니다. 프로젝트 리유니언은 광범위한 대상 Windows 10 OS 버전 세트의 모든 앱에서 일관된 방식으로 사용할 수 있는 통합 API 및 도구 집합을 제공합니다. 프로젝트 리유니언은 기존 Windows 앱 플랫폼과 UWP, 네이티브 Win32, .NET과 같은 프레임워크를 개발자가 이러한 플랫폼에서 사용할 수 있는 공통 API 및 도구 집합으로 보완합니다. 

    자세한 내용은 [프로젝트 리유니언](../project-reunion/index.md)을 참조하세요.

## <a name="update-existing-desktop-apps-for-windows-10"></a>Windows 10용 기존 데스크톱 앱 업데이트

기존 WPF, Windows Forms 또는 네이티브 Win32 데스크톱 앱이 있는 경우 Windows 10 및 UWP(유니버설 Windows 플랫폼)는 앱에서 최신 환경을 제공하는 데 사용할 수 있는 많은 기능을 제공합니다. 이러한 기능 대부분은 다른 플랫폼용 앱을 다시 작성하지 않고도 자신의 업무 속도에 맞춰 앱에 채택할 수 있는 모듈식 구성 요소로 사용할 수 있습니다.

이 내용은 기존 데스크톱 앱 향상에 사용할 수 있는 기능 중 일부에 불과합니다.

* [MSIX](/windows/msix/)를 사용하여 데스크톱 앱을 패키징 및 배포합니다. MSIX는 모든 Windows 앱에 유니버설 패키징 환경을 제공하는 최신 Windows 앱 패키지 형식입니다. MSIX는 MSI, .appx, App-V 및 ClickOnce 설치 기술의 장점을 결합하며 안전하고, 안정적이며, 신뢰할 수 있도록 빌드되었습니다.
* [패키지 확장](./modernize/desktop-to-uwp-extensions.md)을 사용하여 데스크톱 앱을 Windows 10 환경과 통합합니다. 예를 들어 앱의 시작 타일을 가리키거나, 앱을 공유 대상으로 지정하거나, 앱에서 알림 메시지를 보냅니다.
* [XAML Islands](./modernize/xaml-islands.md)를 사용하여 데스크톱 앱에서 UWP XAML 컨트롤을 호스팅합니다. 최신 Windows 10 UI 기능은 대부분 UWP XAML 컨트롤에서만 사용할 수 있습니다.

자세한 내용은 다음 문서를 참조하세요.

<br/>

| 문서 | 설명 |
|---------|-------------|
| [데스크톱 앱 현대화](./modernize/index.md) | WPF, Windows Forms 및 C++ Win32 앱을 비롯한 모든 데스크톱 앱에서 사용할 수 있는 최신 Windows 10 및 UWP 개발 기능을 설명합니다. |
| [자습서: WPF 앱 현대화](./modernize/modernize-wpf-tutorial.md) | 단계별 지침에 따라 앱에 UWP 잉크 및 일정 컨트롤을 추가하고 MSIX 패키지에서 패키징하여 기존 WPF LOB(기간 업무) 샘플 앱을 현대화합니다.  |

## <a name="create-new-desktop-apps"></a>새 데스크톱 앱 만들기

Windows용 새 데스크톱 앱을 만드는 경우 시작에 도움이 되는 몇 가지 리소스는 다음과 같습니다.

<br/>

| 문서 | 설명 |
|---------|-------------|
| [앱 플랫폼 선택](choose-your-platform.md) | 주요 데스크톱 앱 플랫폼을 상세히 비교하며, 요구 사항에 적합한 플랫폼을 선택하는 데 도움이 될 수 있습니다. 이 문서에서는 각 플랫폼 관련 문서에 대한 유용한 링크도 제공합니다. |
| [Windows 앱용 Visual Studio 프로젝트 템플릿](visual-studio-templates.md) | C\# 또는 C++를 사용하여 Windows 10 디바이스용 앱을 빌드하는 데 도움이 되도록 Visual Studio에서 제공하는 프로젝트 및 항목 템플릿에 대해 설명합니다. |
| [데스크톱 앱 현대화](./modernize/index.md) | WPF, Windows Forms 및 C++ Win32 앱을 비롯한 모든 데스크톱 앱에서 사용할 수 있는 최신 Windows 10 및 UWP 개발 기능을 설명합니다. |
| [기능 및 기술](../features-and-technologies.md) | 각각의 주요 데스크톱 앱 플랫폼을 통해 액세스할 수 있는 Windows 기능 개요와 관련 문서 링크를 제공합니다. |

## <a name="related-documentation-and-technologies"></a>관련 설명서 및 기술

| 리소스 | Description |
|---------|-------------|
| [.NET Core 3.1](/dotnet/core/whats-new/dotnet-core-3-1) | WPF 및 Windows Forms 앱에 대한 향상된 기능을 비롯하여 .NET Core 3.1의 최신 기능에 대해 알아봅니다. |
| [.NET 5](/dotnet/core/dotnet-five) | 이 문서에서는 3.1 이후 .NET Core의 다음 릴리스인 .NET 5에 포함된 내용을 자세히 설명합니다. |
| [WPF 및 .NET Core 데스크톱 가이드](/dotnet/desktop-wpf/overview/index) | 전체 .NET Framework 대신 .NET Core를 대상으로 하는 WPF 앱을 개발합니다.  |
| [Azure](/azure/) | Azure Cloud Services를 사용하여 앱의 범위를 확장합니다. |
| [Visual Studio](/visualstudio/) | Visual Studio를 사용하여 앱 및 서비스를 개발하는 방법을 알아봅니다. |
| [MSIX](/windows/msix/) | 모든 Windows 앱을 최신 및 범용 패키징 형식으로 패키징 및 배포합니다. |
| [Windows AI](/windows/ai/) | Windows AI를 사용하여 앱의 복잡한 문제에 대한 인텔리전트 솔루션을 빌드합니다. |
| [Windows 컨테이너](/virtualization/windowscontainers/) | 애플리케이션을 신속하고 완전히 격리된 Windows 환경에서 종속성과 함께 패키지합니다. |
| [프로그레시브 웹앱](/microsoft-edge/progressive-web-apps) | 웹앱을, Windows 10에서 UWP 앱으로 배포 및 실행할 수 있는 프로그레시브 웹앱으로 변환합니다. |
| [Xamarin](/xamarin/) | .NET 코드 및 플랫폼별 사용자 인터페이스를 사용하여 Windows, Android, iOS 및 macOS용 플랫폼 간 앱을 빌드합니다. |
| [Windows 8.x 및 이전 버전의 문서 보관](/previous-versions/windows/) | Windows 8.x 및 이전 버전용 앱 빌드에 대한 보관된 설명서에 액세스합니다. |