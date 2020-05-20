---
Description: 새 앱에 적합한 앱 플랫폼을 선택하는 방법 및 Windows 10용 기존 앱을 현대화하는 방법을 비롯하여 Windows PC용 데스크톱 앱을 빌드하는 방법을 알아봅니다.
title: Windows PC용 데스크톱 앱 빌드
ms.topic: article
ms.date: 11/04/2019
keywords: windows win32, 데스크톱 개발
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.openlocfilehash: 5a55a1578fba7052396ecec725db2678ea8bd6ff
ms.sourcegitcommit: d0f479f1955881afb62c2af249db5d0b053b63e5
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83580020"
---
# <a name="build-desktop-apps-for-windows-pcs"></a>Windows PC용 데스크톱 앱 빌드

이 문서에서는 Windows용 데스크톱 앱 빌드 또는 Windows 10 최신 환경 도입을 위한 기존 데스크톱 앱 업데이트를 시작하는 데 필요한 정보를 제공합니다.

## <a name="platforms-for-desktop-apps"></a>데스크톱 앱용 플랫폼

Windows PC용 데스크톱 앱 빌드를 위한 주요 플랫폼에는 4가지가 있습니다. 각 플랫폼은 앱의 수명 주기를 정의하는 앱 모델과 전체 UI 컨트롤 집합, Windows 기능 사용을 위한 포괄적인 관리형 또는 네이티브 API 집합에 대한 액세스를 제공합니다.

다음 표에서는 플랫폼에 대해 소개합니다. 각 플랫폼에 대한 추가 리소스를 비롯한 이 플랫폼의 상세 비교는 [앱 플랫폼 선택](choose-your-platform.md)을 참조하세요.

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
<td><a href="https://docs.microsoft.com/windows/uwp/">UWP(유니버설 Windows 플랫폼)</a></td>
<td><p>Windows 10 앱 및 게임을 위한 최신 플랫폼입니다. UWP 컨트롤 및 API를 독점적으로 사용하는 UWP 앱을 빌드하거나, 다른 플랫폼 중 하나를 사용하여 빌드된 데스크톱 앱에서 UWP 컨트롤 및 API를 사용할 수 있습니다.</p></td>
<td><a href="/windows/uwp/get-started/">시작</a><br/><a href="/uwp/">API 참조</a><br/><a href="https://github.com/Microsoft/Windows-universal-samples">샘플</a></td>
</tr>
<tr class="even">
<td><a href="https://docs.microsoft.com/windows/win32/">Win32</a></td>
<td><p>Windows 및 하드웨어에 직접 액세스해야 하는 네이티브 C/C++ Windows 앱을 위한 플랫폼입니다.</p></td>
<td><a href="/windows/win32/desktop-programming/">시작</a><br/><a href="/windows/win32/apiindex/windows-api-list/">API 참조</a><br/><a href="https://github.com/Microsoft/Windows-classic-samples">샘플</a></td>
</tr>
<tr class="odd">
<td><a href="https://docs.microsoft.com/dotnet/framework/wpf/">WPF</a></td>
<td><p>XAML UI 모델을 바탕으로 하고 그래픽이 많은 관리형 Windows 앱을 위한 설정된 NET 기반 플랫폼입니다. 이러한 앱은 <a href="https://docs.microsoft.com/dotnet/core/whats-new/dotnet-core-3-0">.NET Core 3</a> 또는 전체 .NET Framework를 대상으로 지정할 수 있습니다.</p></td>
<td><a href="/dotnet/framework/wpf/getting-started/">시작</a><br/><a href="https://docs.microsoft.com/dotnet/api/index">API 참조(.NET)</a><br/><a href="https://github.com/Microsoft/WPF-Samples">샘플</a></td>
</tr>
<tr class="even">
<td><a href="https://docs.microsoft.com/dotnet/framework/winforms/">Windows Forms</a></td>
<td><p>경량 UI 모델을 사용하는 관리형 LOB(기간 업무) 앱용으로 설계된 .NET 기반 플랫폼입니다. 이러한 앱은 <a href="https://docs.microsoft.com/dotnet/core/whats-new/dotnet-core-3-0">.NET Core 3</a> 또는 전체 .NET Framework를 대상으로 지정할 수 있습니다.</p></td>
<td><a href="/dotnet/framework/winforms/getting-started-with-windows-forms">시작</a><br/><a href="https://docs.microsoft.com/dotnet/api/index">API 참조(.NET)</a><br/><a href="https://code.msdn.microsoft.com/windowsdesktop/site/search?f%5B0%5D.Type=Technology&f%5B0%5D.Value=Windows%20Forms">샘플</a></td>
</tr>
</tbody>
</table>

> [!NOTE]
> 이러한 모든 애플리케이션 플랫폼은 클래식 Windows 데스크톱에서 실행되고 해당 환경의 기능을 최대한 활용하는 Word, Excel, Photoshop 등의 데스크톱 앱을 만들 수 있는 완전한 UI 프레임워크 및 UI 컨트롤 세트를 제공합니다. Windows 10에서 각 플랫폼은 Windows UI(WinUI) 라이브러리를 사용하여 사용자 인터페이스를 만들 수 있습니다. 데스크톱 앱용 WinUI에 대한 자세한 내용은 [이 섹션](choose-your-platform.md#windows-ui-library)을 참조하세요.

## <a name="update-existing-desktop-apps-for-windows-10"></a>Windows 10용 기존 데스크톱 앱 업데이트

기존 WPF, Windows Forms 또는 네이티브 Win32 데스크톱 앱이 있는 경우 Windows 10 및 UWP(유니버설 Windows 플랫폼)는 앱에서 최신 환경을 제공하는 데 사용할 수 있는 많은 기능을 제공합니다. 이러한 기능 대부분은 다른 플랫폼용 앱을 다시 작성하지 않고도 자신의 업무 속도에 맞춰 앱에 채택할 수 있는 모듈식 구성 요소로 사용할 수 있습니다.

이 내용은 기존 데스크톱 앱 향상에 사용할 수 있는 기능 중 일부에 불과합니다.

* [MSIX](/windows/msix/)를 사용하여 데스크톱 앱을 패키징 및 배포합니다. MSIX는 모든 Windows 앱에 유니버설 패키징 환경을 제공하는 최신 Windows 앱 패키지 형식입니다. MSIX는 MSI, .appx, App-V 및 ClickOnce 설치 기술의 장점을 결합하며 안전하고, 안정적이며, 신뢰할 수 있도록 빌드되었습니다.
* [패키지 확장](/windows/apps/desktop/modernize/desktop-to-uwp-extensions)을 사용하여 데스크톱 앱을 Windows 10 환경과 통합합니다. 예를 들어 앱의 시작 타일을 가리키거나, 앱을 공유 대상으로 지정하거나, 앱에서 알림 메시지를 보냅니다.
* [XAML Islands](/windows/apps/desktop/modernize/xaml-islands)를 사용하여 데스크톱 앱에서 UWP XAML 컨트롤을 호스팅합니다. 최신 Windows 10 UI 기능은 대부분 UWP XAML 컨트롤에서만 사용할 수 있습니다.

자세한 내용은 다음 문서를 참조하세요.

<br/>

| 문서 | 설명 |
|---------|-------------|
| [데스크톱 앱 현대화](/windows/apps/desktop/modernize) | WPF, Windows Forms 및 C++ Win32 앱을 비롯한 모든 데스크톱 앱에서 사용할 수 있는 최신 Windows 10 및 UWP 개발 기능을 설명합니다. |
| [자습서: WPF 앱 현대화](/windows/apps/desktop/modernize/modernize-wpf-tutorial) | 단계별 지침에 따라 앱에 UWP 잉크 및 일정 컨트롤을 추가하고 MSIX 패키지에서 패키징하여 기존 WPF LOB(기간 업무) 샘플 앱을 현대화합니다.  |

## <a name="create-new-desktop-apps"></a>새 데스크톱 앱 만들기

Windows용 새 데스크톱 앱을 만드는 경우 시작에 도움이 되는 몇 가지 리소스는 다음과 같습니다.

<br/>

| 문서 | 설명 |
|---------|-------------|
| [앱 플랫폼 선택](choose-your-platform.md) | 주요 데스크톱 앱 플랫폼을 상세히 비교하며, 요구 사항에 적합한 플랫폼을 선택하는 데 도움이 될 수 있습니다. 이 문서에서는 각 플랫폼 관련 문서에 대한 유용한 링크도 제공합니다. |
| [데스크톱 앱 현대화](/windows/apps/desktop/modernize) | WPF, Windows Forms 및 C++ Win32 앱을 비롯한 모든 데스크톱 앱에서 사용할 수 있는 최신 Windows 10 및 UWP 개발 기능을 설명합니다. |
| [기능 및 기술](/windows/apps/features-and-technologies) | 각각의 주요 데스크톱 앱 플랫폼을 통해 액세스할 수 있는 Windows 기능 개요와 관련 문서 링크를 제공합니다. |

## <a name="related-documentation-and-technologies"></a>관련 설명서 및 기술

| 리소스 | 설명 |
|---------|-------------|
| [.NET Core 3.0](https://docs.microsoft.com/dotnet/core/whats-new/dotnet-core-3-0) | WPF 및 Windows Forms 앱에 대한 향상된 기능을 비롯하여 .NET Core 3.0의 최신 기능에 대해 알아봅니다. |
| [WPF 및 .NET Core 3.0 데스크톱 가이드](https://docs.microsoft.com/dotnet/desktop-wpf/overview/index) | 전체 .NET Framework 대신 .NET Core 3.0을 대상으로 하는 WPF 앱을 개발합니다.  |
| [Azure](https://docs.microsoft.com/azure/) | Azure Cloud Services를 사용하여 앱의 범위를 확장합니다. |
| [Visual Studio](https://docs.microsoft.com/visualstudio/) | Visual Studio를 사용하여 앱 및 서비스를 개발하는 방법을 알아봅니다. |
| [MSIX](https://docs.microsoft.com/windows/msix/) | 모든 Windows 앱을 최신 및 범용 패키징 형식으로 패키징 및 배포합니다. |
| [Windows AI](https://docs.microsoft.com/windows/ai/) | Windows AI를 사용하여 앱의 복잡한 문제에 대한 인텔리전트 솔루션을 빌드합니다. |
| [Windows 컨테이너](https://docs.microsoft.com/virtualization/windowscontainers/) | 애플리케이션을 신속하고 완전히 격리된 Windows 환경에서 종속성과 함께 패키지합니다. |
| [프로그레시브 웹앱](https://docs.microsoft.com/microsoft-edge/progressive-web-apps) | 웹앱을, Windows 10에서 UWP 앱으로 배포 및 실행할 수 있는 프로그레시브 웹앱으로 변환합니다. |
| [Xamarin](https://docs.microsoft.com/xamarin/) | .NET 코드 및 플랫폼별 사용자 인터페이스를 사용하여 Windows, Android, iOS 및 macOS용 플랫폼 간 앱을 빌드합니다. |
| [Windows 8.x 및 이전 버전의 문서 보관](https://docs.microsoft.com/previous-versions/windows/) | Windows 8.x 및 이전 버전용 앱 빌드에 대한 보관된 설명서에 액세스합니다. |
