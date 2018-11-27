---
title: UWP(유니버설 Windows 플랫폼) 앱용 게임 기술
description: 이 가이드에서는 UWP(유니버설 Windows 플랫폼) 게임을 개발하는 데 사용할 수 있는 기술에 대해 알아봅니다.
ms.assetid: bc4d4648-0d6e-efbb-7608-80bd09decd6e
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 게임, 기술, directx
ms.localizationpriority: medium
ms.openlocfilehash: c6d2ebad640849cd81d6a2704f89ca1f05cc1b27
ms.sourcegitcommit: 681c70f964210ab49ac5d06357ae96505bb78741
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/26/2018
ms.locfileid: "7707676"
---
# <a name="game-technologies-for-uwp-apps"></a>UWP 앱용 게임 기술



이 가이드에서는 UWP(유니버설 Windows 플랫폼) 게임을 개발하는 데 사용할 수 있는 기술에 대해 알아봅니다.

##  <a name="benefits-of-windows10-for-game-development"></a>게임 개발에 대 한 Windows10의 이점


Windows10에 UWP 도입 되면서 Windows10 타이틀이 모든 Microsoft 플랫폼을 확장할 수 있습니다 됩니다. 이전 버전의 Windows에서 무료 마이그레이션 Windows10 클라이언트의 증가 마이그레이션할 수 있기 때문입니다. 이 두 가지의 조합은 Windows10 제목 아주 많은 고객이 Microsoft Store를 통해 연결할 수 있는 것을 의미 합니다.

또한 Windows10 게임에 특히 유용한 많은 새로운 기능을 제공 합니다.

-   메모리 페이징 및 전체 메모리 시스템 크기 감소
-   향상된 그래픽 메모리 관리를 통해 포그라운드 게임에 대한 추가 메모리 할당 및 보호

## <a name="uwp-games-with-c-and-directx"></a>C++ 및 DirectX로 작성된 UWP 게임


고성능을 요구하는 실시간 게임에는 DirectX API를 사용해야 합니다. DirectX는 3D 게임과 같은 고성능을 필요로 하는 게임 및 멀티미디어 응용 프로그램을 만들기 위한 네이티브 API의 컬렉션입니다.

## <a name="development-environment"></a>개발 환경


UWP 용 게임을 만들려면 Visual Studio 2015를 설치 하 여 개발 환경을 설정 해야 합니다. 권장 Visual Studio의 최신 버전을 설치 하는 최신 개발 및 보안 업데이트에 대 한 액세스를 제공 합니다. Visual Studio에서는 UWP 앱을 만드는 수 있으며 게임 개발용 도구를 제공 합니다.

-   DX 게임 프로그래밍용 Visual Studio 도구 - Visual Studio에서는 이미지, 모델 및 셰이더 리소스를 만들고, 편집하고, 미리 보고, 내보낼 수 있는 도구를 제공합니다. 빌드 시 리소스를 변환하고 DirectX 그래픽 코드를 디버그하는 데 사용할 수 있는 도구도 있습니다. 자세한 내용은 [게임 프로그래밍용 Visual Studio 도구 사용](set-up-visual-studio-for-game-development.md)을 참조하세요.
-   Visual Studio 그래픽 진단 기능 - 이제 그래픽 진단 도구를 Windows 내에서 옵션 기능으로 사용할 수 있습니다. 진단 도구를 사용하여 그래픽 디버깅 및 그래픽 프레임 분석을 수행하고 GPU 사용을 실시간으로 모니터링할 수 있습니다. 자세한 내용은 [DirectX 런타임 및 Visual Studio 그래픽 진단 기능 사용](use-the-directx-runtime-and-visual-studio-graphics-diagnostic-features.md)을 참조하세요.

자세한 내용은 유니버설 Windows 플랫폼 및 [DirectX 프로그래밍](directx-programming.md) 준비를 참조하세요.

## <a name="getting-started-with-directx-game-project-templates"></a>DirectX 게임 프로젝트 템플릿 시작


개발 환경을 설정한 후에는 DirectX 관련 프로젝트 템플릿 중 하나를 사용하여 UWP DirectX 게임을 만들 수 있습니다. Visual Studio 2015에는 새 UWP DirectX 프로젝트를 만들 수 있는 세 가지 템플릿이 있습니다(**DirectX 11 앱(유니버설 Windows)**, **DirectX 12 앱(유니버설 Windows)**, **DirectX 11 및 XAML 앱(유니버설 Windows)**). 자세한 내용은 [템플릿에서 유니버설 Windows 플랫폼 및 DirectX 게임 프로젝트 만들기](user-interface.md)를 참조하세요.

## <a name="windows-10-apis"></a>Windows 10 API


Windows 10에서는 게임 개발에 유용한 광범위한 API 컬렉션을 제공합니다. 3D 그래픽, 2D 그래픽, 오디오, 입력, 텍스트 리소스, 사용자 인터페이스, 네트워킹 등 거의 모든 게임 측면에 사용되는 API가 있습니다.

게임 개발과 관련된 많은 API가 있지만 일부 게임에서는 모든 API를 사용할 필요가 없습니다. 예를 들어 일부 게임에서는 3D 그래픽과 Direct3D만 사용하며, 일부 게임에서는 2D 그래픽과 Direct2D만 사용합니다. 또한 이 두 가지를 모두 사용하는 게임도 있습니다. 다음 다이어그램에서는 게임 개발 관련 API를 기능 유형별로 그룹화하여 보여 줍니다.

![게임 플랫폼 기술](images/gameplatformtechnologies.png)

-   3D 그래픽 - Windows 10에서는 두 가지 3D 그래픽 API 집합, 즉 Direct3D 11과 [Direct3D 12](https://msdn.microsoft.com/library/windows/desktop/dn899121)를 지원합니다. 이러한 API는 둘 다 3D 및 2D 그래픽을 만들 수 있는 기능을 제공합니다. Direct3D 11과 Direct3D 12는 함께 사용되지 않지만 2D 그래픽 및 UI 그룹의 모든 API와 함께 사용될 수 있습니다. 게임에서 그래픽 API를 사용하는 방법에 대한 자세한 내용은 [DirectX 게임의 기본 3D 그래픽](an-introduction-to-3d-graphics-with-directx.md)을 참조하세요.

    <table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th align="left">API</th>
    <th align="left">설명</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td align="left">Direct3D 12</td>
    <td align="left"><p>Direct3D 12에서는 DirectX의 핵심에 해당하는 Direct3D 그래픽 API인 Direct3D의 새로운 버전을 제공합니다. 이 버전의 Direct3D는 이전 버전의 Direct3D보다 빠르고 효율적이도록 설계되었습니다. Direct3D 12는 속도가 증한 반면, 수준이 낮으며 그래픽 리소스를 사용자가 직접 관리해야 합니다. 또한 증가된 속도를 실현할 수 있는 보다 강력한 그래픽 프로그래밍 환경을 제공합니다.</p>
    <p><strong>사용해야 하는 경우</strong></p>
    <p>게임의 성능을 최대화해야 하고 게임이 CPU에 바인딩된 경우 Direct3D 12를 사용합니다.</p>
    <p><strong>자세한 정보</strong></p>
    <p><a href="https://msdn.microsoft.com/library/windows/desktop/dn899121">Direct3d 12</a> 설명서를 참조하세요.</p></td>
    </tr>
    <tr class="even">
    <td align="left">Direct3D 11</td>
    <td align="left"><p>Direct3D 11은 Direct3D의 이전 버전으로서, D3D 12보다 높은 수준의 하드웨어 추상화를 사용하여 3D 그래픽을 만들 수 있습니다.</p>
    <p><strong>사용해야 하는 경우</strong></p>
    <p>기존 Direct3D 11 코드가 있거나, 게임이 CPU에 바인딩되지 않았거나, 리소스를 자동으로 관리하려는 경우에 Direct3D 11을 사용합니다.</p>
    <p><strong>자세한 정보</strong></p>
    <p><a href="https://msdn.microsoft.com/library/windows/desktop/ff476080">Direct3D 11</a> 설명서를 참조하세요.</p></td>
    </tr>
    </tbody>
    </table>

     

-   2D 그래픽 및 UI - 텍스트 및 사용자 인터페이스와 같은 2D 그래픽 관련 API입니다. 모든 2D 그래픽 및 UI API는 옵션입니다.

    <table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th align="left">API</th>
    <th align="left">설명</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td align="left">Direct2D</td>
    <td align="left"><p>Direct2D는 2D 기하 도형, 비트맵 및 텍스트에 대한 고성능 및 고품질 렌더링을 제공하는 하드웨어 가속화된 직접 실행 모드 2D 그래픽 API입니다. Direct2D API는 Direct3D에 기본 제공되며, GDI, GDI+ 및 Direct3D와 원활하게 상호 운용되도록 설계되었습니다.</p>
    <p><strong>사용 시기</strong></p>
    <p>Direct2D 횡스크롤 또는 보드 게임과 같은 순수 2D 게임용 그래픽을 제공하려는 경우 Direct3D 대신 사용할 수 있으며, 3D 게임에서 사용자 인터페이스 또는 HUD(헤드업 디스플레이)와 같은 2D 그래픽 만들기를 간소화하려는 경우 Direct3D와 함께 사용할 수 있습니다.</p>
    <p><strong>자세한 정보</strong></p>
    <p><a href="https://msdn.microsoft.com/library/windows/desktop/dd370990">Direct2D</a> 설명서를 참조하세요.</p></td>
    </tr>
    <tr class="even">
    <td align="left">DirectWrite</td>
    <td align="left"><p>DirectWrite는 텍스트 작업을 위한 추가 기능을 제공하며, Direct3D 또는 Direct2D와 사용할 경우 사용자 인터페이스 또는 텍스트가 필요한 다른 영역에 대한 텍스트 출력을 제공할 수 있습니다. DirectWrite는 다중 형식 텍스트의 측정, 그리기 및 힌트 테스트를 지원합니다. DirectWrite는 글로벌 및 지역화된 응용 프로그램에 대해 지원되는 모든 언어로 텍스트를 처리합니다. 또한 DirectWrite는 고유한 레이아웃 및 유니코드-문자 모양 처리를 수행하려는 개발자를 위해 하위 수준의 문자 모양 렌더링 API를 제공합니다.</p>
    <p><strong>사용해야 하는 경우</strong></p>
    <p></p>
    <p><strong>자세한 정보</strong></p>
    <p><a href="https://msdn.microsoft.com/library/windows/desktop/dd368038">DirectWrite</a> 설명서를 참조하세요.</p></td>
    </tr>
    <tr class="odd">
    <td align="left">DirectComposition</td>
    <td align="left"><p>DirectComposition은 변형, 효과 및 애니메이션을 사용하여 고성능 비트맵 컴퍼지션을 지원하는 Windows 구성 요소입니다. 응용 프로그램 개발자는 DirectComposition API를 사용하여 시각 요소 간에 풍부하고 유연한 애니메이션 전환을 지원하는 시각적으로 매력적인 사용자 인터페이스를 만들 수 있습니다.</p>
    <p><strong>사용해야 하는 경우</strong></p>
    <p>DirectComposition은 시각 효과 컴퍼지션 및 애니메이션 전환 생성 프로세스를 간소화하도록 설계되었습니다. 게임에 복잡한 사용자 인터페이스가 필요한 경우 DirectComposition를 사용하여 UI 만들기 및 관리를 간소화할 수 있습니다.</p>
    <p><strong>자세한 정보</strong></p>
    <p><a href="https://msdn.microsoft.com/library/windows/desktop/hh437371">DirectComposition</a> 설명서를 참조하세요.</p></td>
    </tr>
    </tbody>
    </table>

     

-   오디오 - 오디오 재생 및 오디오 효과 적용과 관련된 API입니다. 게임에서 오디오 API를 사용하는 방법에 대한 자세한 내용은 [게임의 오디오](working-with-audio-in-your-directx-game.md)를 참조하세요.

    <table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th align="left">API</th>
    <th align="left">설명</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td align="left">XAudio2</td>
    <td align="left"><p>XAudio2는 신호 처리 및 믹싱에 대한 기초를 제공하는 하위 수준의 오디오 API입니다. XAudio는 사용자 지정 오디오 효과 및 오디오 효과와 필터의 복잡한 체인을 만들 수 있는 기능을 유지하면서 게임 오디오 엔진에 대한 응답성이 매우 뛰어나도록 설계되었습니다.</p>
    <p><strong>사용해야 하는 경우</strong></p>
    <p>게임에서 최소한의 오버헤드 및 지연으로 소리를 재생해야 하는 경우 XAudio2를 사용합니다.</p>
    <p><strong>자세한 정보</strong></p>
    <p><a href="https://msdn.microsoft.com/library/windows/desktop/hh405049">XAudio2</a> 설명서를 참조하세요.</p></td>
    </tr>
    <tr class="even">
    <td align="left">미디어 파운데이션</td>
    <td align="left"><p>Microsoft 미디어 파운데이션은 미디어 파일과 스트림의 오디오 및 비디오 재생을 위해 설계되었지만 XAudio2보다 높은 수준의 기능이 필요하고 일부 추가 오버헤드가 허용되는 게임에서 사용할 수도 있습니다.</p>
    <p><strong>사용해야 하는 경우</strong></p>
    <p>미디어 파운데이션은 영화 장면이나 게임의 대화형이 아닌 구성 요소에 특히 유용합니다. 또한 XAudio2를 사용한 재생을 위해 오디오 파일을 디코딩하는 데에도 유용합니다.</p>
    <p><strong>자세한 정보</strong></p>
    <p><a href="https://msdn.microsoft.com/library/windows/desktop/ms694197">Microsoft 미디어 파운데이션</a> 개요를 참조하세요.</p></td>
    </tr>
    </tbody>
    </table>

     

-   입력 - 키보드, 마우스, 게임 패드 및 기타 사용자 입력 소스에서의 입력과 관련된 API입니다.

    <table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th align="left">API</th>
    <th align="left">설명</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td align="left">XInput</td>
    <td align="left"><p>XInput 게임 컨트롤러 API는 응용 프로그램에서 게임 컨트롤러의 입력을 수신하도록 해줍니다.</p>
    <p><strong>사용해야 하는 경우</strong></p>
    <p>게임에서 게임 패드 입력을 지원해야 하는 경우 기존 XInput 코드가 있으면 XInput을 계속 사용할 수 있습니다. XInput은 UWP용 Windows.Gaming.Input으로 대체되었으므로 새 입력 코드를 작성하는 경우 XInput 대신 Windows.Gaming.Input을 사용해야 합니다.</p>
    <p><strong>자세한 정보</strong></p>
    <p><a href="https://msdn.microsoft.com/library/windows/desktop/hh405053">XInput</a> 설명서를 참조하세요.</p></td>
    </tr>
    <tr class="even">
    <td align="left">Windows.Gaming.Input</td>
    <td align="left"><p>Windows.Gaming.Input API는 XInput을 대체하며, Xinput과 동일한 기능 외에 다음과 같은 이점을 제공합니다.</p>
    <ul>
    <li>리소스 사용 감소</li>
    <li>입력을 검색하기 위한 API 호출 대기 시간 단축</li>
    <li>한 번에 5개 이상의 게임 패드로 작업 가능</li>
    <li>트리거 진동 모터와 같은 추가 Xbox One 게임 패드 기능에 액세스하는 것이 가능</li>
    <li>컨트롤러가 연결되거나 연결이 끊어진 경우 폴링 대신 이벤트를 통해 알림을 제공하는 기능</li>
    <li>입력을 특정 사용자에게 귀속하는 기능(Windows.System.User)</li>
    </ul>
    <p><strong>사용해야 하는 경우</strong></p>
    <p>게임에서 게임 패드 입력을 지원해야 하지만 기존 XInput 코드가 없거나 위에 나열된 이점 중 하나가 필요한 경우 Windows.Gaming.Input을 사용해야 합니다.</p>
    <p><strong>자세한 정보</strong></p>
    <p><a href="https://msdn.microsoft.com/library/windows/apps/dn707817">Windows.Gaming.Input</a> 설명서를 참조하세요.</p></td>
    </tr>
    <tr class="odd">
    <td align="left">Windows.UI.Core.CoreWindow</td>
    <td align="left"><p>Windows.UI.Core.CoreWindow 클래스는 포인터 누르기 및 이동을 추적하는 이벤트와 키 위 및 아래로 이동 이벤트를 제공합니다.</p>
    <p><strong>사용 시기</strong></p>
    <p>게임에서 마우스 또는 키 누르기를 추적해야 하는 경우 Windows.UI.Core.CoreWindows 이벤트를 사용합니다.</p>
    <p><strong>자세한 정보</strong></p>
    <p>게임에서 마우스 또는 키보드를 사용하는 방법에 대한 자세한 내용은 <a href="https://docs.microsoft.com/windows/uwp/gaming/tutorial--adding-move-look-controls-to-your-directx-game">게임용 이동-보기 컨트롤</a>을 참조하세요.</p></td>
    </tr>
    </tbody>
    </table>

     

-   수학 - 자주 사용되는 수학 연산을 간소화하는 API입니다.

    <table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th align="left">API</th>
    <th align="left">설명</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td align="left">DirectXMath</td>
    <td align="left"><p>DirectXMath API는 일반적인 선형 대수 및 게임에 일반적인 그래픽 수학 연산을 위한 SIMD 친화적인 C++ 형식과 함수를 제공합니다.</p>
    <p><strong>사용 시기</strong></p>
    <p>DirectXMath의 사용은 옵션이며, 일반적인 수학 연산을 간소화합니다.</p>
    <p><strong>자세한 정보</strong></p>
    <p><a href="https://msdn.microsoft.com/library/windows/desktop/hh437833">DirectXMath</a> 설명서를 참조하세요.</p></td>
    </tr>
    </tbody>
    </table>

     

-   네트워킹 - 인터넷이나 개인 네트워크를 통해 다른 컴퓨터 및 장치와 통신하도록 해주는 API입니다.

    <table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th align="left">API</th>
    <th align="left">설명</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td align="left">Windows.Networking.Sockets</td>
    <td align="left"><p>Windows.Networking.Sockets 네임스페이스는 안정적이거나 불안정한 네트워크 통신을 허용하는 TCP 및 UDP 소켓을 제공합니다.</p>
    <p><strong>사용해야 하는 경우</strong></p>
    <p>게임에서 네트워크를 통해 다른 컴퓨터 또는 장치와 통신해야 하는 경우 Windows.Networking.Sockets를 사용합니다.</p>
    <p><strong>자세한 정보</strong></p>
    <p><a href="https://docs.microsoft.com/windows/uwp/gaming/work-with-networking-in-your-directx-game">게임의 네트워킹 작업</a>을 참조하세요.</p></td>
    </tr>
    <tr class="even">
    <td align="left">Windows.Web.HTTP</td>
    <td align="left"><p>Windows.Web.HTTP 네임스페이스는 웹 사이트에 액세스하는 데 사용할 수 있는 HTTP 서버에 대한 안정적인 연결을 제공합니다.</p>
    <p><strong>사용 시기</strong></p>
    <p>게임에서 정보를 검색하거나 저장하기 위해 웹 사이트에 액세스해야 하는 경우 Windows.Web.HTTP를 사용합니다.</p>
    <p><strong>자세한 정보</strong></p>
    <p><a href="https://docs.microsoft.com/windows/uwp/gaming/work-with-networking-in-your-directx-game">게임의 네트워킹 작업</a>을 참조하세요.</p></td>
    </tr>
    </tbody>
    </table>

     

-   지원 유틸리티 - Windows 10 API를 기반으로 하는 라이브러리입니다.

    <table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th align="left">라이브러리</th>
    <th align="left">설명</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td align="left">DirectX 도구 키트</td>
    <td align="left"><p>DirectXTK(DirectX 도구 키트)는 C++로 DirectX 11.x 코드를 작성하기 위한 도우미 클래스 컬렉션입니다.</p>
    <p><strong>사용 시기</strong></p>
    <p>레거시 D3DX 유틸리티 코드를 최신 코드로 대체하려는 C++ 개발자이거나 네이티브 C++로 전환하려는 XNA Game Studio 개발자인 경우 DirectX 도구 키트를 사용합니다.</p>
    <p><strong>자세한 정보</strong></p>
    <p>DirectX 도구 키트 프로젝트 페이지 <a href="https://github.com/Microsoft/DirectXTK">https://github.com/Microsoft/DirectXTK</a>를 참조하세요.</p></td>
    </tr>
    <tr class="even">
    <td align="left">Win2D</td>
    <td align="left"><p>Win2D는 직접 실행 모드 2D 그래픽에 사용하기 쉬운 Windows 런타임 API입니다.</p>
    <p><strong>사용해야 하는 경우</strong></p>
    <p>Direct2D 및 DirectWrite용 WinRT 래퍼를 보다 쉽게 사용하기를 원하는 C++ 개발자이거나, Direct2D 및 DirectWrite를 사용하려는 C# 개발자인 경우 Win2D를 사용합니다.</p>
    <p><strong>자세한 정보</strong></p>
    <p>Win2D 프로젝트 페이지 <a href="https://github.com/Microsoft/Win2D">https://github.com/Microsoft/Win2D</a>를 참조하세요.</p></td>
    </tr>
    </tbody>
    </table>

     

## <a name="xbox-live-services"></a>Xbox Live 서비스

[Xbox Live 크리에이터 스 프로그램](https://developer.microsoft.com/games/xbox/xboxlive/creator) 개발자를 UWP 게임에 Xbox Live를 통합 하 고 Xbox One 및 Windows10을 게시할 수 있습니다. 최소한의 개발 시간을 투자하여 로그인, 상태, 순위표 등의 Xbox Live 소셜 환경을 타이틀과 통합하세요. Xbox Live 소셜 기능은 5500만이 넘는 활성 사용자에게 인지도를 확산시켜 고객을 조직적으로 유치할 수 있도록 설계되었습니다.

훨씬 다양한 Xbox Live 기능을 사용하고 전용 마케팅 및 개발 지원을 이용하고 메인 Xbox One 스토어에 추천 앱으로 등록되고 싶다면 [ID@Xbox](http://www.xbox.com/developers/id) 프로그램에 지원하세요. Xbox Live 크리에이터스 프로그램 및 ID@Xbox프로그램에 제공되는 기능을 살펴보려면 [기능표](../xbox-live/developer-program-overview.md#feature-table)를 참조하세요.

자세한 내용을 보려면 [게임에 Xbox Live 추가](e2e.md#adding-xbox-live-to-your-game)로 이동하세요.

##  <a name="alternatives-to-writing-games-with-directx-and-uwp"></a>DirectX와 UWP를 사용한 게임 작성의 대안


### <a name="uwp-games-without-directx"></a>DirectX를 사용하지 않는 UWP 게임

카드 게임 또는 보드 게임과 같은 최소한의 성능이 필요한 간단한 게임은 DirectX 없이 작성할 수 있으며 C++로 작성하지 않아도 됩니다. 이러한 종류의 게임에는 C#, Visual Basic, C++, HTML/JavaScript 등 UWP에서 지원하는 언어를 사용할 수 있습니다. 게임에서 성능이 중요하지 않거나 그래픽이 많이 사용되지 않는 경우 [JavaScript 및 HTML5 터치 게임 샘플](http://code.msdn.microsoft.com/windowsapps/JavaScript-and-HTML5-touch-d96f6031)을 예제로 확인하세요.

### <a name="game-engines"></a>게임 엔진

Windows 게임 개발 API를 사용하여 게임 엔진을 작성하는 대신 Windows 게임 개발 API를 기반으로 하는 뛰어난 품질의 다양한 게임 엔진을 Windows 플랫폼에서 게임 개발에 사용할 수 있습니다. 게임 엔진 또는 라이브러리와 관련된 여러 옵션이 있습니다.

-   전체 게임 엔진 - 전체 게임 엔진은 그래픽, 오디오, 입력, 네트워킹 등 게임 엔진을 처음부터 작성할 때 사용하는 거의 모든 Windows 10 API를 캡슐화합니다. 또한 전체 게임 엔진은 인공 지능 및 경로 찾기와 같은 게임 논리 기능을 제공할 수도 있습니다.
-   그래픽 엔진 - 그래픽 엔진은 Windows 10 그래픽 API를 캡슐화하고, 그래픽 리소스를 관리하며, 다양한 모델 및 세계 형식을 지원합니다.
-   오디오 엔진 - 오디오 엔진은 Windows 10 오디오 API를 캡슐화하고, 오디오 리소스를 관리하며, 고급 오디오 처리 및 효과를 제공합니다.
-   네트워크 엔진 - 네트워크 엔진은 피어 투 피어 또는 서버 기반 다중 플레이어 지원을 게임에 추가하는 Windows 10 네트워킹 API를 캡슐화하며, 대부분 많은 플레이어를 지원하기 위해 고급 네트워킹 기능을 포함합니다.
-   인공 지능 및 경로 찾기 엔진 - AI 및 경로 찾기 엔진은 게임에서 에이전트의 동작을 제어하기 위한 프레임워크를 제공합니다.
-   특수 목적 엔진 - 인벤토리 시스템 및 대화 트리 만들기 등 실행할 수 있는 거의 모든 게임 개발 관련 작업을 처리하는 다양한 추가 엔진이 있습니다.

## <a name="submitting-a-game-to-the-store"></a>스토어에 게임 제출


게임을 게시할 준비가 되면 개발자 계정을 만들고 Microsoft Store에 게임을 제출해야 합니다.

Microsoft Store에 게임을 제출하는 방법은 [게임 제출 및 게시](e2e.md#submitting-and-publishing-your-game)를 참조하세요.

 

 




