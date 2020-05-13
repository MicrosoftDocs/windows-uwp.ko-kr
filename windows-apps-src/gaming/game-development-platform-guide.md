---
title: 유니버설 Windows 플랫폼 (UWP) 앱에 대 한 게임 기술
description: 이 가이드에서는 유니버설 Windows 플랫폼 (UWP) 게임을 개발 하는 데 사용할 수 있는 기술에 대해 알아봅니다.
ms.assetid: bc4d4648-0d6e-efbb-7608-80bd09decd6e
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 게임, 기술, directx
ms.localizationpriority: medium
ms.openlocfilehash: a8fcf2b7a621b7d76411834826bf5db43ddbcfb7
ms.sourcegitcommit: 0f2ae8f97daac440c8e86dc07d11d356de29515c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/13/2020
ms.locfileid: "83280213"
---
# <a name="game-technologies-for-uwp-apps"></a>UWP 앱에 대 한 게임 기술

이 가이드에서는 유니버설 Windows 플랫폼 (UWP) 게임을 개발 하는 데 사용할 수 있는 기술에 대해 알아봅니다.

##  <a name="benefits-of-windows10-for-game-development"></a>게임 개발을 위한 Windows 10의 이점

Windows 10에서 UWP가 도입 됨에 따라 Windows 10 제목은 모든 Microsoft 플랫폼에 걸쳐 있을 수 있습니다. 이전 버전의 Windows에서 무료 마이그레이션을 사용 하면 Windows 10 클라이언트 수가 꾸준히 늘어납니다. 이러한 두 가지 항목의 조합은 Windows 10 제목이 Microsoft Store을 통해 많은 수의 고객에 게 도달할 수 있음을 의미 합니다.

또한 Windows 10은 게임에 특히 도움이 되는 다양 한 새로운 기능을 제공 합니다.

-   메모리 페이징이 줄어들고 전체 메모리 시스템 크기가 줄었습니다.
-   향상 된 그래픽 메모리 관리는 포그라운드 게임에 더 많은 메모리를 할당 하 고 보호 합니다.

## <a name="uwp-games-with-c-and-directx"></a>C + + 및 DirectX를 사용 하는 UWP 게임

고성능 게임을 사용 하려면 DirectX Api를 사용 해야 합니다. DirectX는 3D 게임과 같이 고성능을 필요로 하는 게임 및 멀티미디어 응용 프로그램을 만들기 위한 기본 Api 컬렉션입니다.

## <a name="development-environment"></a>개발 환경

UWP 용 게임을 만들려면 Visual Studio 2015 이상을 설치 하 여 개발 환경을 설정 해야 합니다. 최신 개발 및 보안 업데이트에 대 한 액세스를 제공 하 여 최신 버전의 Visual Studio를 설치 하는 것이 좋습니다. Visual Studio를 사용 하 여 UWP 앱을 만들고 게임 개발을 위한 도구를 제공 합니다.

-   Visual Studio tools for DX 게임 프로그래밍-Visual Studio에서는 이미지, 모델 및 셰이더 리소스를 생성, 편집, 미리 보기 및 내보낼 도구를 제공 합니다. 빌드 시 리소스를 변환 하 고 DirectX 그래픽 코드를 디버그 하는 데 사용할 수 있는 도구도 있습니다. 자세한 내용은 [Visual Studio tools for game 프로그래밍 사용](set-up-visual-studio-for-game-development.md)을 참조 하세요.
-   Visual Studio 그래픽 진단 기능 - 이제 그래픽 진단 도구를 Windows 내에서 옵션 기능으로 사용할 수 있습니다. 진단 도구를 사용 하면 그래픽 디버깅, 그래픽 프레임 분석 및 실시간으로 GPU 사용량을 모니터링할 수 있습니다. 자세한 내용은 [DirectX 런타임 및 Visual Studio 그래픽 진단 기능 사용](use-the-directx-runtime-and-visual-studio-graphics-diagnostic-features.md)을 참조 하세요.

자세한 내용은 유니버설 Windows 플랫폼 준비 및 [DirectX 프로그래밍](directx-programming.md)을 참조 하세요.

## <a name="getting-started-with-directx-game-project-templates"></a>DirectX 게임 프로젝트 템플릿 시작

개발 환경을 설정한 후에는 DirectX 관련 프로젝트 템플릿 중 하나를 사용 하 여 UWP DirectX 게임을 만들 수 있습니다. Visual Studio 2015에는 새 UWP DirectX 프로젝트, **directx 11 앱 (** 유니버설 windows), **directx 12 앱 (유니버설 Windows**) 및 **directx 11 및 XAML 앱 (유니버설 windows)** 을 만드는 데 사용할 수 있는 세 가지 템플릿이 있습니다. 자세한 내용은 [템플릿에서 유니버설 Windows 플랫폼 및 DirectX 게임 프로젝트 만들기](user-interface.md)를 참조 하세요.

## <a name="windows-10-apis"></a>Windows 10 Api

Windows 10은 게임 개발에 유용한 다양 한 Api 컬렉션을 제공 합니다. 3D 그래픽, 2D 그래픽, 오디오, 입력, 텍스트 리소스, 사용자 인터페이스 및 네트워킹을 비롯 한 게임의 거의 모든 측면에 대 한 Api가 있습니다.

게임 개발과 관련 된 많은 Api가 있지만 모든 게임에서 모든 Api를 사용 해야 하는 것은 아닙니다. 예를 들어 일부 게임은 3D 그래픽만 사용 하 고 Direct3D를 사용 하는 경우에만 2D 그래픽을 사용 하 고 Direct2D만 사용할 수 있으며, 다른 게임에서는이 두 가지를 모두 사용할 수 있습니다. 다음 다이어그램은 기능 유형별로 그룹화 된 게임 개발 관련 Api를 보여 줍니다.

![게임 플랫폼 기술](images/gameplatformtechnologies.png)

-   3D 그래픽-Windows 10은 두 가지 3D 그래픽 API 집합, Direct3D 11 및 [direct3d 12](/windows/win32/direct3d12/directx-12-programming-guide)를 지원 합니다. 이러한 Api는 모두 3D 및 2D 그래픽을 만드는 기능을 제공 합니다. Direct3D 11 및 Direct3D 12는 함께 사용 되지 않지만 2D 그래픽 및 UI 그룹의 모든 Api와 함께 사용할 수 있습니다. 게임에서 그래픽 Api를 사용 하는 방법에 대 한 자세한 내용은 [DirectX 게임의 기본 3d 그래픽](an-introduction-to-3d-graphics-with-directx.md)을 참조 하세요.

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
    <tr>
    <td align="left">Direct3D 12</td>
    <td align="left"><p>Direct3D 12에는 DirectX의 핵심에 있는 3D 그래픽 API 인 다음 버전의 Direct3D가 도입 되었습니다. 이 버전의 Direct3D는 이전 버전의 Direct3D 보다 빠르고 효율적으로 설계 되었습니다. Direct3D 12의 향상 된 속도는 더 낮은 수준 이므로 그래픽 리소스를 직접 관리 하 고 향상 된 속도를 실현 하는 데 더 광범위 한 그래픽 프로그래밍 환경을 제공 해야 한다는 것입니다.</p>
    <p><strong>사용 시기</strong></p>
    <p>게임의 성능을 최대화 해야 하 고 게임의 CPU가 바인딩되면 Direct3D 12를 사용 합니다.</p>
    <p><strong>자세한 내용</strong></p>
    <p><a href="/windows/win32/direct3d12/directx-12-programming-guide">Direct3d 12</a> 설명서를 참조 하세요.</p></td>
    </tr>
    <tr>
    <td align="left">Direct3D 11</td>
    <td align="left"><p>Direct3D 11은 이전 버전의 Direct3D 이며, D3D 12 보다 높은 수준의 하드웨어 추상화를 사용 하 여 3D 그래픽을 만들 수 있습니다.</p>
    <p><strong>사용 시기</strong></p>
    <p>Direct3d 11을 사용 하 여 기존 Direct3D 11 코드가 있거나, 게임이 CPU에 바인딩되어 있지 않거나, 리소스를 관리 하는 이점을 원합니다.</p>
    <p><strong>자세한 내용</strong></p>
    <p><a href="/windows/win32/direct3d11/atoc-dx-graphics-direct3d-11">Direct3D 11</a> 설명서를 참조 하세요.</p></td>
    </tr>
    </tbody>
    </table>

     

-   2d 그래픽 및 UI-텍스트 및 사용자 인터페이스와 같은 2D 그래픽과 관련 된 Api입니다. 모든 2D 그래픽 및 UI Api는 선택 사항입니다.

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
    <tr>
    <td align="left">Direct2D</td>
    <td align="left"><p>Direct2D는 2 차원 기 하 도형, 비트맵 및 텍스트에 대 한 고성능 및 고품질 렌더링을 제공 하는 하드웨어 가속, 직접 모드, 2 차원 그래픽 API입니다. Direct2D API는 Direct3D를 기반으로 하며 GDI, GDI + 및 Direct3D와 원활 하 게 상호 운용 하도록 설계 되었습니다.</p>
    <p><strong>사용 시기</strong></p>
    <p>Direct2D는 Direct3D 대신 사용 하 여 scroller 또는 보드 게임과 같은 순수 2D 게임에 그래픽을 제공 하거나 Direct3D에서 사용 하 여 사용자 인터페이스 또는 헤드 표시와 같은 3D 게임에서 2D 그래픽 만들기를 간소화할 수 있습니다.</p>
    <p><strong>자세한 내용</strong></p>
    <p><a href="/windows/win32/Direct2D/direct2d-portal">Direct2D</a> 설명서를 참조 하세요.</p></td>
    </tr>
    <tr>
    <td align="left">DirectWrite</td>
    <td align="left"><p>DirectWrite은 텍스트 작업에 대 한 추가 기능을 제공 하며 Direct3D 또는 Direct2D와 함께 사용 하 여 사용자 인터페이스 또는 텍스트가 필요한 다른 영역에 대 한 텍스트 출력을 제공할 수 있습니다. DirectWrite는 다중 형식 텍스트에 대 한 측정, 그리기 및 적중 테스트를 지원 합니다. DirectWrite는 전역 및 지역화 된 응용 프로그램에 대해 지원 되는 모든 언어의 텍스트를 처리 합니다. 또한 DirectWrite는 자체 레이아웃 및 유니코드-문자 모양 처리를 수행 하려는 개발자를 위한 하위 수준의 문자 모양 렌더링 API를 제공 합니다.</p>
    <p><strong>사용 시기</strong></p>
    <p></p>
    <p><strong>자세한 내용</strong></p>
    <p><a href="/windows/win32/DirectWrite/direct-write-portal">DirectWrite</a> 설명서를 참조 하세요.</p></td>
    </tr>
    <tr>
    <td align="left">DirectComposition</td>
    <td align="left"><p>DirectComposition은 변환, 효과 및 애니메이션과 함께 고성능 비트맵 컴퍼지션을 가능 하 게 하는 Windows 구성 요소입니다. 응용 프로그램 개발자는 DirectComposition API를 사용 하 여 한 시각적 개체에서 다른 시각적 개체에 대 한 풍부 하 고 유체 애니메이션 전환을 기능 하는 사용자 인터페이스를 시각적으로 만들 수 있습니다.</p>
    <p><strong>사용 시기</strong></p>
    <p>DirectComposition은 시각적 개체를 작성 하 고 애니메이션 전환을 만드는 프로세스를 간소화 하도록 설계 되었습니다. 게임에 복잡 한 사용자 인터페이스가 필요한 경우 DirectComposition을 사용 하 여 UI의 생성 및 관리를 간소화할 수 있습니다.</p>
    <p><strong>자세한 내용</strong></p>
    <p><a href="/windows/win32/directcomp/directcomposition-portal">Directcomposition</a> 설명서를 참조 하세요.</p></td>
    </tr>
    </tbody>
    </table>

     

-   오디오-오디오를 재생 하 고 오디오 효과를 적용 하는 것과 관련 된 Api입니다. 게임에서 오디오 Api를 사용 하는 방법에 대 한 자세한 내용은 [게임 오디오](working-with-audio-in-your-directx-game.md)를 참조 하세요.

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
    <tr>
    <td align="left">XAudio2</td>
    <td align="left"><p>XAudio2는 신호 처리 및 혼합을 위한 기반을 제공 하는 하위 수준 오디오 API입니다. XAudio는 오디오 효과 및 필터의 사용자 지정 오디오 효과 및 복잡 한 체인을 만드는 기능을 유지 하면서 게임 오디오 엔진에 대해 매우 응답성을 유지 하도록 설계 되었습니다.</p>
    <p><strong>사용 시기</strong></p>
    <p>게임에서 최소한의 오버 헤드와 지연으로 소리를 재생 해야 하는 경우 XAudio2를 사용 합니다.</p>
    <p><strong>자세한 내용</strong></p>
    <p><a href="/windows/win32/xaudio2/xaudio2-apis-portal">XAudio2</a> 설명서를 참조 하세요.</p></td>
    </tr>
    <tr>
    <td align="left">오디오 그래프</td>
    <td align="left"><p>XAudio2를 사용 하 여 구현할 수 있는 기능의 경우 대신 Windows 런타임 audio graph Api를 사용 하는 대신 사용할 수 있습니다. 두 가지 방법 중 하나를 결정 하는 데 도움이 되도록 <a href="/windows/uwp/audio-video-camera/audio-graphs#choosing-windows-runtime-audiograph-or-xaudio2">Windows 런타임 오디오 그래프 또는 XAudio2 선택</a>을 참조 하세요.</p>
    <p><strong>사용 시기</strong></p>
    <p>게임에서 최소한의 오버 헤드와 지연으로 소리를 재생 해야 하지만 XAudio2 보다 훨씬 사용 하기 쉬운 API를 사용 하 고 c # 지원 옵션을 사용 하는 경우 오디오 그래프를 사용 합니다.</p>
    <p><strong>자세한 내용</strong></p>
    <p><a href="/windows/uwp/audio-video-camera/audio-graphs">오디오 그래프</a> 설명서를 참조 하세요.</p></td>
    </tr>
    <tr>
    <td align="left">미디어 파운데이션</td>
    <td align="left"><p>Microsoft 미디어 파운데이션는 오디오 및 비디오와 같은 미디어 파일 및 스트림을 재생 하기 위해 설계 되었지만 XAudio2 보다 높은 수준의 기능이 필요 하 고 일부 추가 오버 헤드가 허용 되는 경우 게임에서 사용할 수도 있습니다.</p>
    <p><strong>사용 시기</strong></p>
    <p>Media foundation은 게임의 시네마 또는 비 대화형 구성 요소에 특히 유용 합니다. Media foundation은 XAudio2를 사용 하 여 재생을 위해 오디오 파일을 디코딩하는 데도 유용 합니다.</p>
    <p><strong>자세한 내용</strong></p>
    <p><a href="/windows/win32/medfound/microsoft-media-foundation-sdk">Microsoft 미디어 파운데이션</a> 개요를 참조 하세요.</p></td>
    </tr>
    </tbody>
    </table>

     

-   입력-키보드, 마우스, 게임 패드 및 기타 사용자 입력 원본의 입력과 관련 된 Api입니다.

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
    <tr>
    <td align="left">XInput</td>
    <td align="left"><p>XInput Game Controller API를 사용 하면 응용 프로그램에서 게임 컨트롤러의 입력을 받을 수 있습니다.</p>
    <p><strong>사용 시기</strong></p>
    <p>게임에서 gampad 입력을 지원 해야 하 고 기존 XInput 코드를 사용 하는 경우 XInput을 계속 사용할 수 있습니다. XInput는 UWP에 대 한 XInput로 대체 되었으며, 새 입력 코드를 작성 하는 경우에는 대신 Windows를 사용 해야 합니다.</p>
    <p><strong>자세한 내용</strong></p>
    <p><a href="/windows/win32/xinput/xinput-game-controller-apis-portal">XInput</a> 설명서를 참조 하세요.</p></td>
    </tr>
    <tr>
    <td align="left">Windows. 게임 입력</td>
    <td align="left"><p>XInput API는 Xinput를 통해 다음과 같은 이점을 제공 하는 동일한 기능을 제공 합니다.</p>
    <ul>
    <li>리소스 사용량 감소</li>
    <li>입력을 검색 하기 위한 낮은 API 호출 대기 시간</li>
    <li>Gamepads를 한 번에 4 개 이상 사용할 수 있습니다.</li>
    <li>추가 Xbox One 게임 패드 기능에 액세스 하는 기능 (예: 트리거 진동 모터)</li>
    <li>컨트롤러가 폴링 대신 이벤트를 통해 연결/연결을 끊을 때 알림이 표시 되는 기능</li>
    <li>특정 사용자에 대 한 입력 특성 기능 (Windows. User)</li>
    </ul>
    <p><strong>사용 시기</strong></p>
    <p>게임에서 게임 패드 입력을 지원 해야 하 고 기존 XInput 코드를 사용 하 고 있지 않거나 위에 나열 된 이점 중 하나를 사용 해야 하는 경우에는를 사용 해야 합니다.</p>
    <p><strong>자세한 내용</strong></p>
    <p>이 문서 <a href="/uwp/api/Windows.Gaming.Input">를 참조 하세요.</a></p></td>
    </tr>
    <tr>
    <td align="left">CoreWindow.</td>
    <td align="left"><p>CoreWindow 클래스는 포인터 누름 및 이동, 키 다운 및 키 위로 이벤트 추적을 위한 이벤트를 제공 합니다.</p>
    <p><strong>사용 시기</strong></p>
    <p>게임에서 마우스 또는 키 누름을 추적 해야 하는 경우 CoreWindows 이벤트를 사용 합니다.</p>
    <p><strong>자세한 내용</strong></p>
    <p>게임에서 마우스나 키보드를 사용 하는 방법에 대 한 자세한 내용은 <a href="/windows/uwp/gaming/tutorial--adding-move-look-controls-to-your-directx-game">게임의 이동 모양 컨트롤</a> 을 참조 하세요.</p></td>
    </tr>
    </tbody>
    </table>

     

-   수학-일반적으로 사용 되는 수학적 작업 간소화와 관련 된 Api입니다.

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
    <tr>
    <td align="left">DirectXMath</td>
    <td align="left"><p>DirectXMath API는 일반적인 선형 대 수 및 게임에 공통적으로 제공 되는 그래픽 수학 연산에 대해 SIMD 친화적인 c + + 형식 및 함수를 제공 합니다.</p>
    <p><strong>사용 시기</strong></p>
    <p>DirectXMath를 사용 하는 것은 선택 사항이 며 일반적인 수학적 연산을 단순화 합니다.</p>
    <p><strong>자세한 내용</strong></p>
    <p><a href="/windows/win32/dxmath/directxmath-portal">Directxmath</a> 설명서를 참조 하세요.</p></td>
    </tr>
    </tbody>
    </table>

     

-   네트워킹-인터넷 또는 개인 네트워크를 통해 다른 컴퓨터 및 장치와 통신 하는 데 관련 된 Api입니다.

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
    <tr>
    <td align="left">Windows.Networking.Sockets</td>
    <td align="left"><p>Windows. p i t. p a t 네임 스페이스는 안정적 이거나 불안정 한 네트워크 통신을 허용 하는 TCP 및 UDP 소켓을 제공 합니다.</p>
    <p><strong>사용 시기</strong></p>
    <p>게임에서 네트워크를 통해 다른 컴퓨터 또는 장치와 통신 해야 하는 경우에는 Windows를 사용 합니다.</p>
    <p><strong>자세한 내용</strong></p>
    <p><a href="/windows/uwp/gaming/work-with-networking-in-your-directx-game">게임에서 네트워킹 사용을</a>참조 하세요.</p></td>
    </tr>
    <tr>
    <td align="left">Windows. 웹 HTTP</td>
    <td align="left"><p>Windows. HTTP 네임 스페이스는 웹 사이트에 액세스 하는 데 사용할 수 있는 HTTP 서버에 대 한 신뢰할 수 있는 연결을 제공 합니다.</p>
    <p><strong>사용 시기</strong></p>
    <p>게임에서 웹 사이트에 액세스 하 여 정보를 검색 하거나 저장 해야 하는 경우에는 Windows를 사용 합니다.</p>
    <p><strong>자세한 내용</strong></p>
    <p><a href="/windows/uwp/gaming/work-with-networking-in-your-directx-game">게임에서 네트워킹 사용을</a>참조 하세요.</p></td>
    </tr>
    </tbody>
    </table>

     

-   지원 유틸리티-Windows 10 Api를 기반으로 하는 라이브러리입니다.

    <table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th align="left">라이브러리</th>
    <th align="left">Description</th>
    </tr>
    </thead>
    <tbody>
    <tr>
    <td align="left">DirectX 도구 키트</td>
    <td align="left"><p>DirectX Tool Kit (DirectXTK)는 c + +에서 DirectX 11.x 코드를 작성 하기 위한 도우미 클래스의 컬렉션입니다.</p>
    <p><strong>사용 시기</strong></p>
    <p>레거시 D3DX 유틸리티 코드에 대 한 최신 대체를 찾고 있는 XNA Game Studio developer를 네이티브 c + +로 전환 하려는 경우 DirectX 도구 키트를 사용 합니다.</p>
    <p><strong>자세한 내용</strong></p>
    <p>DirectX Tool Kit 프로젝트 페이지를 참조 <a href="https://github.com/Microsoft/DirectXTK">https://github.com/Microsoft/DirectXTK</a> 하세요.</p></td>
    </tr>
    <tr>
    <td align="left">Win2D</td>
    <td align="left"><p>Win2D는 직접 실행 모드 2D 그래픽 렌더링을 위한 사용 하기 쉬운 Windows 런타임 API입니다.</p>
    <p><strong>사용 시기</strong></p>
    <p>Direct2D 및 DirectWrite용 WinRT 래퍼를 보다 쉽게 사용하기를 원하는 C++ 개발자이거나, Direct2D 및 DirectWrite를 사용하려는 C# 개발자인 경우 Win2D를 사용합니다.</p>
    <p><strong>자세한 내용</strong></p>
    <p>Win2D 프로젝트 페이지를 참조 <a href="https://github.com/Microsoft/Win2D">https://github.com/Microsoft/Win2D</a> 하세요.</p></td>
    </tr>
    </tbody>
    </table>

## <a name="xbox-live-services"></a>Xbox Live 서비스

[Xbox Live 크리에이터 프로그램](https://developer.microsoft.com/games/xbox/xboxlive/creator) 을 사용 하면 모든 개발자가 xbox LIVE를 UWP 게임에 통합 하 고 xbox One 및 Windows 10에 게시할 수 있습니다. 개발 시간을 최소화 하 여 로그인, 현재 상태, 순위표 등의 Xbox Live 소셜 환경을 타이틀에 통합 하세요. Xbox Live 소셜 기능은 유기적으를 증가 시키고 5500만 활성 게이머에 게 인식을 분산 하도록 설계 되었습니다.

더 많은 Xbox Live 기능에 액세스 하려는 경우 전용 마케팅 및 개발 지원 및 기본 Xbox One 스토어에서 추천 될 기회는 프로그램에 적용 됩니다 [ID@Xbox](https://www.xbox.com/developers/id) . Xbox Live 크리에이터 프로그램 및 프로그램에서 사용할 수 있는 기능을 확인 하려면 ID@Xbox [기능 표](/gaming/xbox-live/developer-program-overview.md#feature-table)를 참조 하세요.

자세한 정보는 [게임에 Xbox Live 추가](e2e.md#adding-xbox-live-to-your-game)로 이동 하세요.

##  <a name="alternatives-to-writing-games-with-directx-and-uwp"></a>DirectX 및 UWP를 사용 하 여 게임을 작성 하는 대안

### <a name="uwp-games-without-directx"></a>DirectX가 없는 UWP 게임

카드 게임 또는 보드 게임과 같은 최소 성능 요구 사항으로 간단한 게임은 DirectX 없이 작성할 수 있으며 c + +로 작성 하지 않아도 됩니다. 이러한 종류의 게임에서는 c #, Visual Basic, c + + 및 HTML/JavaScript와 같이 UWP에서 지 원하는 언어를 사용할 수 있습니다. 성능 및 집약적 그래픽이 게임의 요구 사항이 아닌 경우 [JavaScript 및 HTML5 touch game 샘플](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/master/Official%20Windows%20Platform%20Sample/JavaScript%20and%20HTML5%20touch%20game%20sample/%5BJavaScript%5D-JavaScript%20and%20HTML5%20touch%20game%20sample/JavaScript) 을 예제로 체크 아웃 합니다.

### <a name="game-engines"></a>게임 엔진

Windows 게임 개발 Api를 사용 하 여 게임 엔진을 직접 작성 하는 대신 windows 게임 개발 Api를 기반으로 하는 많은 고품질 게임 엔진을 Windows 플랫폼에서 게임을 개발 하는 데 사용할 수 있습니다. 게임 엔진 또는 라이브러리를 고려 하는 경우 다음과 같은 여러 가지 옵션을 사용할 수 있습니다.

-   전체 게임 엔진-게임 엔진을 처음부터 새로 작성할 때 사용 하는 대부분의 Windows 10 Api (예: 그래픽, 오디오, 입력 및 네트워킹)를 캡슐화 하는 전체 게임 엔진 전체 게임 엔진은 인공 지능 및 경로 찾기와 같은 게임 논리 기능을 제공할 수도 있습니다.
-   그래픽 엔진-그래픽 엔진은 Windows 10 그래픽 Api를 캡슐화 하 고, 그래픽 리소스를 관리 하 고, 다양 한 모델 및 세계 형식을 지원 합니다.
-   오디오 엔진-오디오 엔진은 Windows 10 오디오 Api를 캡슐화 하 고, 오디오 리소스를 관리 하 고, 고급 오디오 처리 및 효과를 제공 합니다.
-   네트워크 엔진-네트워크 엔진은 피어 투 피어 또는 서버 기반 멀티 플레이 지원을 게임에 추가 하기 위해 Windows 10 네트워킹 Api를 캡슐화 하며, 많은 플레이어를 지 원하는 고급 네트워킹 기능을 포함할 수 있습니다.
-   인공 지능 및 pathfinding 엔진-AI 및 pathfinding 엔진은 게임에서 에이전트 동작을 제어 하기 위한 프레임 워크를 제공 합니다.
-   특별 한 목적 엔진-인벤토리 시스템 및 대화 상자 트리 만들기와 같이 실행할 수 있는 거의 모든 게임 개발 관련 작업을 처리 하기 위한 다양 한 추가 엔진이 있습니다.

## <a name="submitting-a-game-to-the-microsoft-store"></a>Microsoft Store에 게임 제출

게임을 게시할 준비가 되 면 개발자 계정을 만들고 게임을 Microsoft Store에 제출 해야 합니다.

Microsoft Store에 게임을 제출 하는 방법에 대 한 자세한 내용은 [게임 제출 및 게시](e2e.md#submitting-and-publishing-your-game)를 참조 하세요.
