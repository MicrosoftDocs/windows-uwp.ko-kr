---
title: Windows 10 게임 개발 가이드
description: UWP (유니버설 Windows 플랫폼) 게임을 개발 하기 위한 리소스 및 정보에 대 한 종단 간 가이드입니다.
ms.assetid: 6061F498-96A8-44EF-9711-68AE5A1218C9
ms.date: 04/16/2018
ms.topic: article
keywords: windows 10, uwp, 게임, 게임 개발
ms.localizationpriority: medium
ms.openlocfilehash: 24414ba36e2ee1af8f391eec38b04d9e17bb7237
ms.sourcegitcommit: 2e597438dafedde3bde24424ef005bb4c24ba3bf
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/16/2020
ms.locfileid: "84800329"
---
# <a name="windows-10-game-development-guide"></a>Windows 10 게임 개발 가이드

Windows 10 게임 개발 가이드를 시작 합니다.

이 가이드에서는 유니버설 Windows 플랫폼 (UWP) 게임을 개발 하는 데 필요한 리소스 및 정보의 종단 간 컬렉션을 제공 합니다. 이 가이드의 영어 (미국) 버전은 [PDF](https://download.microsoft.com/download/9/C/9/9C9D344F-611F-412E-BB01-259E5C76B17F/Windev_Game_Dev_Guide_Oct_2017.pdf) 형식으로 제공 됩니다.

## <a name="introduction-to-game-development-for-the-universal-windows-platform-uwp"></a>유니버설 Windows 플랫폼에 대 한 게임 개발 소개 (UWP)

Windows 10 게임을 만들면 휴대폰, PC 및 Xbox One을 통해 전 세계 수백만 명의 플레이어에게 도달할 기회를 가질 수 있습니다. Xbox on Windows, Xbox Live, 디바이스 간 멀티플레이, 멋진 게임 커뮤니티, UWP(유니버설 Windows 플랫폼) 및 DirectX 12와 같은 강력한 새 기능을 갖춘 Windows 10 게임은 모든 연령과 장르의 플레이어를 설레게 합니다. 새 UWP(유니버설 Windows 플랫폼)는 휴대폰, PC 및 Xbox One용 공용 API와 각 디바이스 환경에 맞춰 게임을 조정할 수 있는 도구 및 옵션과 함께 Windows 10 디바이스 간 게임에 대한 호환성을 제공합니다.

이 가이드에서는 게임을 개발 하는 데 도움이 되는 정보 및 리소스의 종단 간 수집을 제공 합니다. 섹션은 게임 개발 단계에 따라 구성 되므로 필요할 때 정보를 찾을 수 있는 위치를 알 수 있습니다.

Windows 또는 Xbox에서 게임을 개발 하는 데 익숙하지 않은 경우 [시작 가이드를](getting-started.md) 시작 하려는 위치를 사용할 수 있습니다. [게임 개발 리소스](#game-development-resources) 섹션에서는 게임을 만들 때 유용한 설명서, 프로그램 및 기타 리소스에 대 한 개략적인 조사도 제공 합니다. 대신 일부 UWP 코드를 살펴보면 시작 하려면 [Game samples](#game-samples)를 참조 하십시오.

이 가이드는 추가 Windows 10 게임 개발 리소스 및 자료를 사용할 수 있게 되 면 업데이트 됩니다.

## <a name="game-development-resources"></a>게임 개발 리소스

설명서에서 개발자 프로그램, 포럼, 블로그 및 샘플에 이르기까지 게임 개발 과정에서 도움이 되는 다양 한 리소스를 사용할 수 있습니다. 다음은 Windows 10 게임 개발을 시작할 때 알아야 할 리소스에 대 한 roundup입니다.

> [!Note]
> 일부 기능은 다양 한 프로그램을 통해 관리 됩니다. 이 가이드에서는 광범위 한 리소스를 다루며,이를 통해 사용자의 프로그램 또는 특정 개발 역할에 따라 일부 리소스에 액세스할 수 없다는 것을 알 수 있습니다. 예는 developer.xboxlive.com, forums.xboxlive.com, xdi.xboxlive.com 또는 GDN (Game Developer Network)으로 확인 되는 링크입니다. Microsoft와의 파트너 관계에 대 한 자세한 내용은 [개발자 프로그램](#developer-programs)을 참조 하십시오.

### <a name="game-development-documentation"></a>게임 개발 설명서

이 가이드 전체에서 작업, 기술 및 게임 개발 단계 별로 구성 된 관련 설명서에 대 한 딥 링크를 찾을 수 있습니다. 사용 가능한 기능에 대 한 광범위 한 보기를 제공 하기 위해 Windows 10 게임 개발에 대 한 주요 설명서 포털은 다음과 같습니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Windows 개발자 센터 기본 포털</td>
        <td><a href="https://developer.microsoft.com/windows">Windows 개발자 센터</a></td>
    </tr>
    <tr>
        <td>Windows 앱 개발</td>
        <td><a href="https://developer.microsoft.com/windows/apps/develop">Windows 앱 개발</a></td>
    </tr>
    <tr>
        <td>유니버설 Windows 플랫폼 앱 개발</td>
        <td><a href="https://developer.microsoft.com/windows/apps">Windows 10 앱에 대 한 방법 가이드</a></td>
    </tr>
    <tr>
        <td>UWP 게임에 대 한 방법 가이드</td>
        <td><a href="index.md">게임 및 DirectX</a> </td>
    </tr>
    <tr>
        <td>DirectX 참조 및 개요</td>
        <td><a href="https://docs.microsoft.com/windows/desktop/directx">DirectX 그래픽 및 게임</a></td>
    </tr>
    <tr>
        <td>게임용 Azure</td>
        <td><a href="https://azure.microsoft.com/solutions/gaming/">Azure를 사용 하 여 게임 빌드 및 크기 조정</a></td>
    </tr>
    <tr>
        <td>PlayFab</td>
        <td><a href="https://api.playfab.com/">라이브 게임을 위한 전체 백 엔드 솔루션</a></td>
    </tr>
    <tr>
        <td>Xbox One의 UWP</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/xbox-apps/index">Xbox One에서 UWP 앱 빌드</a></td>
    </tr>
    <tr>
        <td>HoloLens의 UWP</td>
        <td><a href="https://developer.microsoft.com/windows/mixed-reality/development_overview">HoloLens에서 UWP 앱 빌드</a></td>
    </tr>
    <tr>
        <td>Xbox Live 설명서</td>
        <td><a href="https://docs.microsoft.com/gaming/xbox-live/">Xbox Live 개발자 가이드</a></td>
    </tr>
    <tr>
        <td>Xbox One development 설명서 (XGD)</td>
        <td><a href="https://developer.microsoft.com/games/xbox/partner/development-home">Xbox One 개발</a></td>
    </tr>
    <tr>
        <td>Xbox One 개발 백서 (XGD)</td>
        <td><a href="https://developer.microsoft.com/games/xbox/partner/development-education-whitepapers">백서</a></td>
    </tr>
    <tr>
        <td>믹서 대화형 설명서</td>
        <td><a href="https://dev.mixer.com/reference/interactive/index.html">게임에 대화형 작업 추가</a></td>
    </tr>        
</table>

### <a name="partner-center"></a>파트너 센터

[파트너 센터에서 개발자 계정을 등록](https://developer.microsoft.com/store/register) 하는 것은 Windows 게임을 게시 하기 위한 첫 번째 단계입니다. 개발자 계정을 사용 하 여 게임 이름을 예약 하 고 모든 Windows 장치에 대해 무료 또는 유료 게임을 Microsoft Store에 제출할 수 있습니다. 개발자 계정을 사용 하 여 게임 및 게임 중인 제품을 관리 하 고, 자세한 분석을 얻고, 전 세계의 플레이어에 게 뛰어난 환경을 만드는 서비스를 사용할 수 있습니다. 

Microsoft는 Windows 게임을 개발 하 고 게시 하는 데 도움이 되는 몇 가지 개발자 프로그램도 제공 합니다. 파트너 센터 계정에 등록 하기 전에 권한이 있는지 확인 하는 것이 좋습니다. 자세한 내용은 [개발자 프로그램](#developer-programs) (영문)을 참조 하세요.

### <a name="developer-programs"></a>개발자 프로그램

Microsoft는 Windows 게임을 개발 하 고 게시 하는 데 도움이 되는 몇 가지 개발자 프로그램을 제공 합니다. Xbox One 용 게임을 개발 하 고 게임에서 Xbox Live 기능을 통합 하려는 경우 개발자 프로그램에 참여 하는 것이 좋습니다. Microsoft Store에서 게임을 게시 하려면 [파트너 센터](https://partner.microsoft.com/dashboard) 에서도 개발자 계정을 만들어야 합니다.

#### <a name="xbox-live-creators-program"></a>Xbox Live 크리에이터 프로그램

Xbox Live 크리에이터 프로그램을 사용 하면 누구나 Xbox Live를 타이틀에 통합 하 고 Xbox One 및 Windows 10에 게시할 수 있습니다. 간단한 인증 프로세스가 있으며 표준 [Microsoft Store 정책](https://docs.microsoft.com/legal/windows/agreements/store-policies)외에 개념 승인이 필요 하지 않습니다.

소매점 하드웨어만 사용 하 여 전용 개발자 키트 없이 작성자 프로그램에서 게임을 배포 하 고 디자인 하 고 게시할 수 있습니다. 시작 하려면 Xbox One에서 [개발 모드 정품 인증 앱](https://docs.microsoft.com/windows/uwp/xbox-apps/devkit-activation) 을 다운로드 하세요.

더 많은 Xbox Live 기능에 액세스 하려는 경우 전용 마케팅 및 개발 지원 및 기본 Xbox One 스토어에서 추천 될 기회는 프로그램에 적용 됩니다 [ID@Xbox](https://www.xbox.com/Developers/id) .

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Xbox Live 크리에이터스 프로그램</td>
        <td><a href="https://developer.microsoft.com/games/xbox/xboxlive/creator">Xbox Live 크리에이터 프로그램에 대 한 자세한 정보</a></td>
    </tr>
</table>

#### <a name="idxbox"></a>ID@Xbox

이 ID@Xbox 프로그램을 사용 하면 Windows 및 Xbox one에서 자격을 갖춘 게임 개발자에 게 자동으로 게시할 수 있습니다. Xbox One 용으로 개발 하거나 Gamerscore, 성과, 순위표 등의 Xbox Live 기능을 Windows 10 게임에 추가 하려면로 등록 하세요 ID@Xbox . 개발자가 되어 성과를 ID@Xbox 발휘할 하 고 성공을 최대화 하는 데 필요한 도구와 지원을 받으세요. ID@Xbox파트너 센터에서 개발자 계정을 등록 하기 전에 먼저를 적용 하는 것이 좋습니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>ID@Xbox개발자 프로그램</td>
        <td><a href="https://www.xbox.com/Developers/id">Xbox One 용 독립 개발자 프로그램</a></td>
    </tr>
    <tr>
        <td>ID@Xbox소비자 사이트</td>
        <td><a href="https://www.idatxbox.com/">ID@Xbox</a></td>
    </tr>
</table>

#### <a name="xbox-tools-and-middleware"></a>Xbox 도구 및 미들웨어

Xbox Tools 및 미들웨어 프로그램은 게임 도구 및 미들웨어 전문 개발자에 게 Xbox 개발 키트를 라이선스 합니다. 프로그램에 동의 하는 개발자는 Xbox XDK 기술을 공유 하 고 사용이 허가 된 다른 Xbox 개발자에 게 배포할 수 있습니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>도구 및 미들웨어 프로그램에 문의</td>
        <td><xboxtlsm@microsoft.com></td>
    </tr>
</table>

### <a name="game-samples"></a>게임 샘플

Windows 10 게임 기능을 이해 하 고 게임 개발을 신속 하 게 시작 하는 데 사용할 수 있는 많은 Windows 10 게임 및 앱 샘플이 있습니다. 더 많은 샘플이 정기적으로 개발 및 게시 되므로, 언제 든 지 샘플 포털을 확인 하 여 새로운 기능을 확인 하는 것을 잊지 마세요. GitHub 리포지토리을 [시청](https://help.github.com/en/articles/watching-and-unwatching-repositories) 하 여 변경 내용과 추가 사항을 알릴 수도 있습니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>유니버설 Windows 플랫폼 앱 샘플</td>
        <td><a href="https://github.com/Microsoft/Windows-universal-samples">Windows-유니버설-샘플</a></td>
    </tr>
    <tr>
        <td>Direct3D 12 그래픽 샘플</td>
        <td><a href="https://github.com/Microsoft/DirectX-Graphics-Samples">DirectX-그래픽-샘플</a></td>
    </tr>
    <tr>
        <td>Direct3D 11 그래픽 샘플</td>
        <td><a href="https://github.com/walbourn/directx-sdk-samples">directx-sdk-샘플</a></td>
    </tr>
    <tr>
        <td>Direct3D 11 first person game 샘플</td>
        <td><a href="tutorial--create-your-first-uwp-directx-game.md">DirectX를 사용 하 여 간단한 UWP 게임 만들기</a></td>
    </tr>
    <tr>
        <td>Direct2D 사용자 지정 이미지 효과 샘플</td>
        <td><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/D2DCustomEffects">D2DCustomEffects</a></td>
    </tr>
    <tr>
        <td>Direct2D 그라데이션 망 샘플</td>
        <td><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/D2DGradientMesh">D2DGradientMesh</a></td>
    </tr>
    <tr>
        <td>Direct2D 사진 조정 샘플</td>
        <td><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/D2DPhotoAdjustment">D2DPhotoAdjustment</a></td>
    </tr>
    <tr>
        <td>Xbox Advanced 기술 그룹 공용 샘플</td>
        <td><a href="https://github.com/Microsoft/Xbox-ATG-Samples">Xbox-ATG-샘플</a></td>
    </tr>
    <tr>
        <td>Xbox Live 샘플</td>
        <td><a href="https://github.com/Microsoft/xbox-live-samples">xbox 라이브 샘플</a></td>
    </tr>
    <tr>
        <td>Xbox One game 샘플 (XGD)</td>
        <td><a href="https://developer.microsoft.com/games/xbox/partner/development-education-samples">샘플</a></td>
    </tr>
    <tr>
        <td>Windows 게임 샘플 (MSDN 코드 갤러리)</td>
        <td><a href="https://docs.microsoft.com/samples/browse/?term=games">Microsoft Store 게임 샘플</a></td>
    </tr>
    <tr>
        <td>JavaScript 2D 게임 샘플</td>
        <td><a href="../get-started/get-started-tutorial-game-js2d.md">JavaScript로 UWP 게임 만들기</a></td>
    </tr>
    <tr>
        <td>JavaScript 3D 게임 샘플</td>
        <td><a href="../get-started/get-started-tutorial-game-js3d.md">three.js를 사용하여 3D JavaScript 게임 만들기</a></td>
    </tr>
    <tr>
        <td>MonoGame 2D UWP game 샘플</td>
        <td><a href="../get-started/get-started-tutorial-game-mg2d.md">MonoGame 2D로 UWP 게임 만들기</a></td>
    </tr>      
</table>

### <a name="developer-forums"></a>개발자 포럼

개발자 포럼은 게임 개발에 대 한 질문을 하 고 답변 하 고 게임 개발 커뮤니티와 연결 하는 데 매우 좋은 장소입니다. 또한 포럼은 개발자가 이전에 직면 하 고 해결 한 어려운 문제에 대 한 기존 답변을 찾기 위한 환상적인 리소스 일 수 있습니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>앱 및 게임 개발자 포럼 게시</td>
        <td><a href="https://social.msdn.microsoft.com/Forums/en/home?forum=windowsstore%2Cwpsubmit%2Caiaads%2Caiasdk%2Caiamgr">게시 및 광고 앱</a></td>
    </tr>
    <tr>
        <td>UWP 앱 개발자 포럼</td>
        <td><a href="https://docs.microsoft.com/answers/topics/uwp.html">유니버설 Windows 플랫폼 앱 개발</a></td>
    </tr>
    <tr>
        <td>데스크톱 응용 프로그램 개발자 포럼</td>
        <td><a href="https://social.msdn.microsoft.com/Forums/en/home?forum=windowsgeneraldevelopmentissues">Windows 데스크톱 응용 프로그램 포럼</a></td>
    </tr>
    <tr>
        <td>DirectX Microsoft Store 게임 (보관 된 포럼 게시물)</td>
        <td><a href="https://social.msdn.microsoft.com/Forums/en/home?forum=wingameswithdirectx">DirectX를 사용 하 여 Microsoft Store 게임 빌드 (보관)</a></td>
    </tr>
    <tr>
        <td>Windows 10 관리 파트너 개발자 포럼</td>
        <td><a href="https://forums.xboxlive.com/users/login.html">XBOX 개발자 포럼: Windows 10</a></td>
    </tr>
    <tr>
        <td>Xbox Live 포럼</td>
        <td><a href="https://social.msdn.microsoft.com/Forums/en/home?forum=xboxlivedev">Xbox Live 개발 포럼</a></td>
    </tr>
    <tr>
        <td>PlayFab 포럼</td>
        <td><a href="https://community.playfab.com/index.html">PlayFab 포럼</a></td>
    </tr>
</table>

### <a name="developer-blogs"></a>개발자 블로그

개발자 블로그는 게임 개발에 대 한 최신 정보를 설명 하는 또 다른 유용한 리소스입니다. 새 기능, 구현 세부 정보, 모범 사례, 아키텍처 배경 등에 대 한 게시물을 찾을 수 있습니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Windows 블로그 용 앱 빌드</td>
        <td><a href="https://blogs.windows.com/buildingapps/">Windows 용 앱 빌드</a></td>
    </tr>
    <tr>
        <td>Windows 10 (블로그 게시물)</td>
        <td><a href="https://blogs.windows.com/blog/tag/windows-10/">Windows 10의 게시물</a></td>
    </tr>
    <tr>
        <td>Visual Studio 엔지니어링 팀 블로그</td>
        <td><a href="https://devblogs.microsoft.com/visualstudio/">Visual Studio 블로그</a></td>
    </tr>
    <tr>
        <td>Visual Studio 개발자 도구 블로그</td>
        <td><a href="https://devblogs.microsoft.com/visualstudio/">개발자 도구 블로그</a></td>
    </tr>
    <tr>
        <td>Somasegar 's 개발자 도구 블로그</td>
        <td><a href="https://devblogs.microsoft.com/somasegar/">Somasegar의 블로그</a></td>
    </tr>
    <tr>
        <td>DirectX 개발자 블로그</td>
        <td><a href="https://devblogs.microsoft.com/directx/">DirectX 개발자 블로그</a></td>
    </tr>
    <tr>
        <td>DirectX 12 소개 (블로그 게시물)</td>
        <td><a href="https://devblogs.microsoft.com/directx/directx-12/">DirectX 12</a></td>
    </tr>
    <tr>
        <td>Visual C++ 도구 팀 블로그</td>
        <td><a href="https://devblogs.microsoft.com/cppblog/">Visual C++ 팀 블로그</a></td>
    </tr>
    <tr>
        <td>PIX 팀 블로그</td>
        <td><a href="https://devblogs.microsoft.com/pix/">Windows 및 Xbox에서 DirectX 12 게임의 성능 조정 및 디버깅</a></td>
    </tr>
    <tr>
        <td>유니버설 Windows 앱 배포 팀 블로그</td>
        <td><a href="https://blogs.msdn.microsoft.com/appinstaller/">UWP 앱 팀 블로그 빌드 및 배포</a></td>
    </tr>
</table>

## <a name="concept-and-planning"></a>개념 및 계획

개념 및 계획 단계에서는 게임의 대상 및 개발에 사용 하는 기술 및 도구를 결정 하 게 됩니다.

### <a name="overview-of-game-development-technologies"></a>게임 개발 기술 개요

UWP 용 게임 개발을 시작 하면 그래픽, 입력, 오디오, 네트워킹, 유틸리티 및 라이브러리에 대 한 여러 가지 옵션을 사용할 수 있습니다.

게임에서 사용 하는 모든 기술을 이미 결정 한 경우 좋은 방법입니다! 그렇지 않은 경우 [UWP 앱에 대 한 게임 기술](game-development-platform-guide.md) 가이드는 사용할 수 있는 많은 기술에 대 한 뛰어난 개요 이며, 옵션 및 해당 옵션을 함께 사용 하는 방법을 이해 하는 데 도움이 되는 읽기를 권장 합니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>UWP 게임 기술에 대 한 설문 조사</td>
        <td><a href="game-development-platform-guide.md">UWP 앱에 대 한 게임 기술</a></td>
    </tr>
</table>

이러한 3 개의 GDC 2015 비디오는 Windows 10 게임 개발 및 Windows 10 게임 환경에 대 한 유용한 개요를 제공 합니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Windows 10 게임 개발 개요 (비디오)</td>
        <td><a href="https://channel9.msdn.com/Events/GDC/GDC-2015/Developing-Games-for-Windows-10">Windows 10 용 게임 개발</a></td>
    </tr>
    <tr>
        <td>Windows 10 게임 환경 (비디오)</td>
        <td><a href="https://channel9.msdn.com/Events/GDC/GDC-2015/Gaming-Consumer-Experience-on-Windows-10">Windows 10의 게임 소비자 환경</a></td>
    </tr>
    <tr>
        <td>Microsoft 에코 시스템에서 게임 (비디오)</td>
        <td><a href="https://channel9.msdn.com/Events/GDC/GDC-2015/The-Future-of-Gaming-Across-the-Microsoft-Ecosystem">Microsoft 에코 시스템 전반의 게임의 미래</a></td>
    </tr>
</table>

### <a name="game-planning"></a>게임 계획

게임을 계획할 때 고려해 야 할 몇 가지 개략적인 개념 및 계획 항목이 여기에 나와 있습니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>게임에 액세스할 수 있도록 설정</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/gaming/accessibility-for-games">게임의 내게 필요한 옵션</a></td>
    </tr>
    <tr>
        <td>클라우드를 사용 하 여 게임 빌드</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/gaming/cloud-for-games">게임을 위한 클라우드</a></td>
    </tr>
    <tr>
        <td>게임 수익 창출</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/gaming/monetization-for-games">게임 수익 화</a></td>
    </tr>
</table>

### <a name="choosing-your-graphics-technology-and-programming-language"></a>그래픽 기술 및 프로그래밍 언어 선택

Windows 10 게임에서 사용할 수 있는 몇 가지 프로그래밍 언어 및 그래픽 기술이 있습니다. 수행 하는 경로는 개발 중인 게임의 유형, 개발 스튜디오의 환경 및 기본 사항, 게임의 특정 기능 요구 사항에 따라 달라 집니다. C #, c + + 또는 JavaScript를 사용 하 시겠습니까? DirectX, XAML 또는 HTML5?

#### <a name="directx"></a>DirectX

Microsoft DirectX는 최고 성능의 2D 및 3D 그래픽 및 멀티미디어를 위해 선택 하는 것입니다.

DirectX 12는 이전 버전 보다 더 빠르고 효율적입니다. Direct3D 12를 사용 하면 Windows 10 Pc 및 Xbox One에서 최신 GPU 하드웨어를 더 다양 하 게 사용 하 고 더 복잡 한 효과를 활용할 수 있습니다.

Direct3D 11의 친숙 한 그래픽 파이프라인을 사용 하려는 경우에도 Direct3D 11.3에 추가 된 새로운 렌더링 및 최적화 기능을 활용 하는 것이 좋습니다. 또한 Win32의 루트를 사용 하 여 Windows API developer를 사용 하는 경우에도 Windows 10에서이 옵션을 사용할 수 있습니다.

DirectX의 광범위 한 기능 및 심층적인 플랫폼 통합은 가장 까다로운 게임에 필요한 기능과 성능을 제공 합니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>UWP 개발용 DirectX</td>
        <td><a href="directx-programming.md">DirectX 프로그래밍</a></td>
    </tr>
    <tr>
        <td>자습서: UWP DirectX 게임을 만드는 방법</td>
        <td><a href="tutorial--create-your-first-uwp-directx-game.md">DirectX를 사용 하 여 간단한 UWP 게임 만들기</a></td>
    </tr>
    <tr>
        <td>DirectX 개요 및 참조</td>
        <td><a href="https://docs.microsoft.com/windows/desktop/directx">DirectX 그래픽 및 게임</a></td>
    </tr>
    <tr>
        <td>Direct3D 12 프로그래밍 가이드 및 참조</td>
        <td><a href="https://docs.microsoft.com/windows/desktop/direct3d12/direct3d-12-graphics">Direct3D 12 그래픽</a></td>
    </tr>
    <tr>
        <td>그래픽 및 DirectX 12 개발 비디오 (YouTube 채널)</td>
        <td><a href="https://www.youtube.com/channel/UCiaX2B8XiXR70jaN7NK-FpA">Microsoft DirectX 12 및 그래픽 교육</a></td>
    </tr>
</table>

#### <a name="xaml"></a>XAML

XAML은 애니메이션, storyboard, 데이터 바인딩, 확장 가능한 벡터 기반 그래픽, 동적 크기 조정, 장면 그래프 등의 편리한 기능을 제공 하는 사용 하기 쉬운 선언적 UI 언어입니다. XAML은 게임 UI, 메뉴, 스프라이트 및 2D 그래픽에 적합 합니다. UI 레이아웃을 쉽게 만들기 위해 XAML은 Expression Blend 및 Microsoft Visual Studio 같은 디자인 및 개발 도구와 호환 됩니다. XAML은 일반적으로 c #과 함께 사용 되지만 c + +는 선호 하는 언어 또는 게임의 CPU 수요가 높은 경우에도 적합 합니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>XAML 플랫폼 개요</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/xaml-platform/index">XAML 플랫폼</a></td>
    </tr>
    <tr>
        <td>XAML UI 및 컨트롤</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/design/basics/">컨트롤, 레이아웃 및 텍스트</a></td>
    </tr>
</table>

#### <a name="html-5"></a>HTML 5

HTML (하이퍼텍스트 태그 언어)은 웹 페이지, 앱 및 리치 클라이언트에 사용 되는 일반적인 UI 태그 언어입니다. Windows 게임은 HTML의 친숙 한 기능을 갖춘 모든 기능을 갖춘 프레젠테이션 계층으로 HTML5를 사용 하 고, 유니버설 Windows 플랫폼에 액세스 하 고, AppCache, 웹 작업자, 캔버스, 끌어서 놓기, 비동기 프로그래밍 및 SVG와 같은 최신 웹 기능을 지원할 수 있습니다. 백그라운드에서 HTML 렌더링은 DirectX 하드웨어 가속 기능을 활용 하므로 추가 코드를 작성 하지 않고도 DirectX의 성능 이점을 얻을 수 있습니다. HTML5는 웹 개발에 능숙 하거나 웹 게임을 이식 하거나 다른 선택 사항 보다 더 쉽게 사용할 수 있는 언어 및 그래픽 계층을 사용 하려는 경우에 적합 합니다. HTML5는 JavaScript와 함께 사용 되지만 c # 또는 c + +를 사용 하 여 만든 구성 요소를 호출할 수도 있습니다/CX

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>HTML5 및 문서 개체 모델 정보</td>
        <td><a href="https://developer.mozilla.org/docs/Web">HTML 및 DOM 참조</a></td>
    </tr>
    <tr>
        <td>HTML5 W3C 권장 사항</td>
        <td><a href="https://www.w3.org/TR/html5/">HTML5</a></td>
    </tr>
</table>
 
####프레젠테이션 기술 결합

Microsoft의 DXGI (DirectX Graphics Infrastructure)는 여러 그래픽 기술 간의 상호 운용성과 호환성을 제공 합니다. 고성능 그래픽의 경우 XAML과 DirectX를 결합 하 고, 메뉴 및 기타 간단한 UI에 대해 XAML을 사용 하 고, 복잡 한 2D 및 3D 장면을 렌더링 하기 위한 DirectX를 사용할 수 있습니다. 또한 DXGI는 Direct2D, Direct3D, DirectWrite, DirectCompute 및 Microsoft 미디어 파운데이션 간 호환성을 제공 합니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>DirectX Graphic Infrastructure 프로그래밍 가이드 및 참조</td>
        <td><a href="https://docs.microsoft.com/windows/desktop/direct3ddxgi/dx-graphics-dxgi">DXGI</a></td>
    </tr>
    <tr>
        <td>DirectX 및 XAML 결합</td>
        <td><a href="directx-and-xaml-interop.md">DirectX 및 XAML interop</a></td>
    </tr>
</table>
 
#### C++

C + +/CX는 속도, 호환성 및 플랫폼 액세스를 강력 하 게 조합 하 여 제공 하는 고성능의 낮은 오버 헤드 언어입니다. C + +/CX를 사용 하면 DirectX 및 Xbox Live를 비롯 하 여 Windows 10의 우수한 게임 기능을 쉽게 사용할 수 있습니다. 기존 c + + 코드 및 라이브러리를 다시 사용할 수도 있습니다. C + +/CX는 가비지 수집의 오버 헤드를 초래 하지 않는 빠른 네이티브 코드를 만들기 때문에 게임을 통해 성능이 향상 되 고 전원 소비가 감소 하 여 배터리 수명이 길어질 수 있습니다. DirectX 또는 XAML을 사용 하는 c + +/CX를 사용 하거나 둘의 조합을 사용 하는 게임을 만듭니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>C + +/CX 참조 및 개요</td>
        <td><a href="https://docs.microsoft.com/cpp/cppcx/visual-c-language-reference-c-cx">Visual C++ 언어 참조 (c + +/CX)</a></td>
    </tr>
    <tr>
        <td>Visual C++ 프로그래밍 가이드 및 참조</td>
        <td><a href="https://docs.microsoft.com/cpp/visual-cpp-in-visual-studio">Visual Studio 2019의 Visual C++</a></td>
    </tr>
</table>
 
#### C#

C # ("C 샵"으로 발음)은 간단 하 고, 강력 하며, 형식이 안전 하 고, 개체 지향적인 현대적인 혁신적인 언어입니다. C #을 사용 하면 C 스타일 언어의 친숙 한 표현을 유지 하면서 신속 하 게 개발할 수 있습니다. 쉽게 사용할 수 있지만 c #에는 다형성, 대리자, 람다, 클로저, 반복기 메서드, 공 분산 및 LINQ (통합 언어 쿼리) 식과 같은 다양 한 고급 언어 기능이 있습니다. C #은 XAML을 대상으로 하는 경우 빠른 시작을 시작 하거나 이전 c # 환경을 제공 하려는 경우에 매우 적합 합니다. C #은 주로 XAML과 함께 사용 되므로 DirectX를 사용 하려면 대신 c + +를 선택 하거나 DirectX와 상호 작용 하는 c + + 구성 요소로 게임의 일부를 작성 합니다. 또는 c # 및 c + +에 대 한 직접 모드 Direct2D 그래픽 라이브러리인 [Win2D](https://github.com/Microsoft/Win2D)을 고려 합니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>C # 프로그래밍 가이드 및 참조</td>
        <td><a href="https://docs.microsoft.com/dotnet/articles/csharp/csharp">C # 언어 참조</a></td>
    </tr>
</table>
 
####JavaScript

JavaScript는 최신 웹 및 리치 클라이언트 응용 프로그램에 널리 사용 되는 동적 스크립팅 언어입니다.

Windows JavaScript 앱은 개체 지향 JavaScript 클래스의 메서드 및 속성을 사용 하 여 쉽고 직관적인 방법으로 유니버설 Windows 플랫폼의 강력한 기능에 액세스할 수 있습니다. JavaScript는 웹 개발 환경에서 발생 하거나, JavaScript에 이미 친숙 하거나, HTML5, CSS, WinJS 또는 JavaScript 라이브러리를 사용 하려는 경우에 적합 합니다. DirectX 또는 XAML을 대상으로 하는 경우 대신 c # 또는 c + +/CX를 선택 합니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>JavaScript 및 Windows 런타임 참조</td>
        <td><a href="https://docs.microsoft.com/scripting/javascript/javascript-language-reference">JavaScript 참조</a></td>
    </tr>
</table>

#### <a name="use-windows-runtime-components-to-combine-languages"></a>Windows 런타임 구성 요소를 사용 하 여 언어 결합

유니버설 Windows 플랫폼를 사용 하면 다양 한 언어로 작성 된 구성 요소를 쉽게 결합할 수 있습니다. C + +, c # 또는 Visual Basic에서 Windows 런타임 구성 요소를 만든 다음 JavaScript, c #, c + + 또는 Visual Basic에서이를 호출 합니다. 이 방법은 선택한 언어로 게임의 일부를 프로그래밍 하는 좋은 방법입니다. 또한 구성 요소를 사용 하면 특정 언어로만 제공 되는 외부 라이브러리를 사용 하 고 이미 작성 한 레거시 코드를 사용할 수 있습니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Windows 런타임 구성 요소를 만드는 방법</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/winrt-components/creating-windows-runtime-components-in-cpp">C++/CX가 포함된 Windows 런타임 구성 요소</a></td>
    </tr>
</table>

### <a name="which-version-of-directx-should-your-game-use"></a>게임에서 사용할 DirectX 버전은 무엇 인가요?

게임에 DirectX를 선택 하는 경우 사용할 버전 (Microsoft Direct3D 12 또는 Microsoft Direct3D 11)을 결정 해야 합니다.

DirectX 12는 이전 버전 보다 더 빠르고 효율적입니다. Direct3D 12를 사용 하면 Windows 10 Pc 및 Xbox One에서 최신 GPU 하드웨어를 더 다양 하 게 사용 하 고 더 복잡 한 효과를 활용할 수 있습니다. Direct3D 12는 매우 낮은 수준에서 작동 하므로 전문 그래픽 개발 팀 또는 숙련 된 DirectX 11 개발 팀에 게 그래픽 최적화를 최대화 하는 데 필요한 모든 제어를 제공할 수 있습니다.

Direct3D 11.3은 친숙 한 Direct3D 프로그래밍 모델 및 핸들을 사용 하 여 GPU 렌더링과 관련 된 복잡성을 더 주는 낮은 수준의 그래픽 API입니다. Windows 10 및 Xbox One 에서도 지원 됩니다. Direct3D 11에서 기존 엔진을 작성 하 고 Direct3D 12로 점프할 준비가 되지 않은 경우 12에서 Direct3D 11을 사용 하 여 몇 가지 성능 개선을 수행할 수 있습니다. 버전 11.3 이상에는 Direct3D 12 에서도 사용 되는 새로운 렌더링 및 최적화 기능이 포함 되어 있습니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Direct3D 12 또는 Direct3D 11 선택</td>
        <td><a href="https://docs.microsoft.com/windows/desktop/direct3d12/what-is-directx-12-">Direct3D 12란?</a></td>
    </tr>
    <tr>
        <td>Direct3D 11 개요</td>
        <td><a href="https://docs.microsoft.com/windows/desktop/direct3d11/atoc-dx-graphics-direct3d-11">Direct3D 11 그래픽</a></td>
    </tr>
    <tr>
        <td>12의 Direct3D 11 개요</td>
        <td><a href="https://docs.microsoft.com/windows/desktop/direct3d12/direct3d-11-on-12">12에서 Direct3D 11</a></td>
    </tr>
</table>

### <a name="bridges-game-engines-and-middleware"></a>브리지, 게임 엔진 및 미들웨어

게임의 요구에 따라 브리지, 게임 엔진 또는 미들웨어를 사용 하면 개발 및 테스트 시간 및 리소스를 절약할 수 있습니다. 다음은 브리지, 게임 엔진 및 미들웨어에 대 한 몇 가지 개요와 리소스입니다.

#### <a name="universal-windows-platform-bridges"></a>유니버설 Windows 플랫폼 브리지

유니버설 Windows 플랫폼 브리지는 기존 앱 또는 게임을 UWP로 가져오는 기술입니다. 브리지는 UWP 게임 개발을 신속 하 게 시작할 수 있는 좋은 방법입니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>UWP 브리지</td>
        <td><a href="https://developer.microsoft.com/windows/bridges">코드를 Windows로 가져오기</a></td>
    </tr>
    <tr>
        <td>IOS 용 Windows 브리지</td>
        <td><a href="https://developer.microsoft.com/windows/bridges/ios">IOS 앱을 Windows로 가져오기</a></td>
    </tr>
    <tr>
        <td>데스크톱 응용 프로그램용 Windows 브리지 (.NET 및 Win32)</td>
        <td><a href="https://developer.microsoft.com/windows/bridges/desktop">데스크톱 응용 프로그램을 UWP 앱으로 변환</a></td>
    </tr>
</table>

#### <a name="playfab"></a>PlayFab

이제 Microsoft 제품군에 포함 된 PlayFab은 라이브 게임을 위한 완벽 한 백 엔드 플랫폼 이며 독립적인 스튜디오 시작 하는 강력한 방법입니다. 게임 서비스, 실시간 분석 및 LiveOps를 사용 하 여 비용을 절감 하면서 수익, 참여 및 재방문 주기를 보강 하세요.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>PlayFab</td>
        <td><a href="https://playfab.com/">도구 및 서비스 개요</a></td>
    </tr>
    <tr>
        <td>시작</td>
        <td><a href="https://api.playfab.com/docs/general-getting-started">일반 시작 가이드</a></td>
    </tr>
    <tr>
        <td>비디오 자습서 시리즈</td>
        <td><a href="https://www.youtube.com/watch?v=fGNpiqVi5xU&list=PLHCfyL7JpoPbLpA_oh_T5PKrfzPgCpPT5">PlayFab의 핵심 시스템에 대 한 시리즈 시리즈 비디오</a></td>
    </tr>
    <tr>
        <td>레시피</td>
        <td><a href="https://api.playfab.com/docs/tutorials/recipes-index">인기 게임 메커니즘 및 디자인 패턴 샘플</a></td>
    </tr>
    <tr>
        <td>플랫폼</td>
        <td><a href="https://api.playfab.com/platforms">다양 한 플랫폼 및 게임 엔진에 대 한 특정 설명서</a></td>
    </tr>
    <tr>
        <td>GitHub 리포지토리</td>
        <td><a href="https://github.com/PlayFab">Android, iOS, Windows, Unity 및 Unreal을 비롯 한 다양 한 플랫폼에 대 한 스크립트 및 Sdk를 다운로드 하세요.</a></td>
    </tr>
    <tr>
        <td>API 설명서</td>
        <td><a href="https://api.playfab.com/documentation/">REST와 유사한 웹 Api를 통해 PlayFab 서비스에 직접 액세스</a></td>
    </tr>
    <tr>
        <td>포럼</td>
        <td><a href="https://community.playfab.com/index.html">PlayFab 포럼</a></td>
    </tr>
</table>
 
#### Unity

Unity는 멋진 및 매력적인 2D, 3D, VR 및 AR 게임과 앱을 만들기 위한 플랫폼을 제공 합니다. 이를 통해 독창적인 비전을 신속 하 게 실현 하 고 콘텐츠를 거의 모든 미디어 나 장치에 제공할 수 있습니다.

Unity 5.4부터 Unity는 Direct3D 12 개발을 지원 합니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Unity 게임 엔진</td>
        <td><a href="https://unity.com/">Unity-게임 엔진</a></td>
    </tr>
    <tr>
        <td>Unity 가져오기</td>
        <td><a href="https://store.unity.com/">Unity 가져오기</a></td>
    </tr>
    <tr>
        <td>Windows 용 Unity 설명서</td>
        <td><a href="https://docs.unity3d.com/Manual/Windows.html">Unity 수동/Windows</a></td>
    </tr>
    <tr>
        <td>PlayFab를 사용 하 여 LiveOps 추가</td>
        <td><a href="https://api.playfab.com/docs/getting-started/unity-getting-started">시작 하기-Unity 게임에서 첫 번째 PlayFab API 호출을 수행 합니다.</a></td>
    </tr>
    <tr>
        <td>믹서 Interactive를 사용 하 여 게임에 대화형 작업을 추가 하는 방법</td>
        <td><a href="https://github.com/mixer/interactive-unity-plugin/wiki/Getting-started">시작 가이드</a></td>
    </tr>
    <tr>
        <td>Unity 용 믹서 SDK</td>
        <td><a href="https://www.assetstore.unity3d.com/en/#!/content/88585">믹서 Unity 플러그 인</a></td>
    </tr>
    <tr>
        <td>Unity 용 믹서 SDK 참조 문서</td>
        <td><a href="https://dev.mixer.com/reference/interactive/csharp/index.html">믹서 Unity 플러그 인에 대 한 API 참조</a></td>
    </tr>
    <tr>
        <td>Microsoft Store에 Unity 게임 게시</td>
        <td><a href="https://unity3d.com/partners/microsoft/porting-guides">포팅 가이드</a></td>
    </tr>
    <tr>
        <td>.NET Api와 관련 된 누락 된 어셈블리 참조 문제 해결</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/gaming/missing-dot-net-apis-in-unity-and-uwp">Unity 및 UWP에서 누락된 .NET API</a></td>
    </tr>
    <tr>
        <td>Unity 게임을 유니버설 Windows 플랫폼 앱으로 게시 (비디오)</td>
        <td><a href="https://channel9.msdn.com/Blogs/One-Dev-Minute/How-to-publish-your-Unity-game-as-a-UWP-app">Unity 게임을 UWP 앱으로 게시 하는 방법</a></td>
    </tr>
    <tr>
        <td>Unity를 사용 하 여 Windows 게임 및 앱 만들기 (비디오)</td>
        <td><a href="https://channel9.msdn.com/Blogs/One-Dev-Minute/Making-games-and-apps-with-Unity">Unity를 사용 하 여 Windows 게임 및 앱 만들기</a></td>
    </tr>
    <tr>
        <td>Visual Studio를 사용 하 여 Unity 게임 개발 (비디오 시리즈)</td>
        <td><a href="https://www.youtube.com/playlist?list=PLReL099Y5nRfseAg0k1SJOlpqdcsDs8Em">Visual Studio 2015에서 Unity 사용</a></td>
    </tr>
</table>
 
####Havok

Havok의 모듈식 도구 및 기술 제품군은 게임 작성자가 새로운 수준의 대화형 작업 및 집중 교육에 도달 하는 데 도움이 됩니다. Havok를 통해 매우 현실적인 물리학, 대화형 시뮬레이션 및 뛰어난 cinematics을 사용할 수 있습니다. 2015.1 이상 버전은 x86, 64 비트 및 ARM의 Visual Studio 2015에서 UWP를 공식적으로 지원 합니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Havok 웹 사이트</td>
        <td><a href="https://www.havok.com/">Havok</a></td>
    </tr>
    <tr>
        <td>Havok 도구 제품군</td>
        <td><a href="https://www.havok.com/products/">Havok 제품 개요</a></td>
    </tr>
    <tr>
        <td>Havok 지원 포럼</td>
        <td><a href="https://www.havok.com/">Havok</a></td>
    </tr>
</table>
 
####MonoGame

MonoGame는 원래 Microsoft의 XNA Framework 4.0을 기반으로 하는 오픈 소스 플랫폼 간 게임 개발 프레임 워크입니다. Monogame는 현재 Windows, Windows Phone 및 Xbox 뿐만 아니라 Linux, macOS, iOS, Android 및 몇 가지 다른 플랫폼을 지원 합니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>MonoGame</td>
        <td><a href="https://www.monogame.net">MonoGame 웹 사이트</a></td>
    </tr>
    <tr>
        <td>MonoGame 설명서</td>
        <td><a href="https://www.monogame.net/documentation/">MonoGame 설명서 (최신 버전)</a></td>
    </tr>
    <tr>
        <td>Monogame 다운로드</td>
        <td>MonoGame 웹 사이트에서 <a href="https://www.monogame.net/downloads/">릴리스, 개발 빌드 및 소스 코드를 다운로드</a> 하거나 <a href="https://www.nuget.org/profiles/MonoGame">NuGet을 통해 최신 릴리스를 가져옵니다</a>.
    </tr>
    <tr>
        <td>MonoGame 2D UWP game 샘플</td>
        <td><a href="../get-started/get-started-tutorial-game-mg2d.md">MonoGame 2D로 UWP 게임 만들기</a></td>
    </tr>    
</table>

#### <a name="cocos2d"></a>Cocos2d

Cocos2d-x는 UWP 게임 빌드를 지 원하는 플랫폼 간 오픈 소스 게임 개발 엔진 및 도구 모음입니다. 버전 3부터 3D 기능도 추가 됩니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Cocos2d-x</td>
        <td><a href="https://www.cocos2d-x.org/">Cocos2d 란?</a></td>
    </tr>
    <tr>
        <td>Cocos2d 프로그래머 가이드</td>
        <td><a href="https://www.cocos2d-x.org/programmersguide/">Cocos2d 프로그래머 가이드</a></td>
    </tr>
    <tr>
        <td>Windows 10의 Cocos2d-x (블로그 게시물)</td>
        <td><a href="https://blogs.windows.com/buildingapps/2015/06/15/running-cocos2d-x-on-windows-10/">Windows 10에서 Cocos2d-x 실행</a></td>
    </tr>
    <tr>
        <td>PlayFab를 사용 하 여 LiveOps 추가</td>
        <td><a href="https://api.playfab.com/docs/getting-started/cocos2d-x-getting-started-guide">시작 하기-Cocos2d 게임에서 첫 번째 PlayFab API 호출을 수행 합니다.</a></td>
    </tr>
</table>

#### <a name="unreal-engine"></a>언리얼 엔진

Unreal Engine 4는 모든 종류의 게임과 개발자를 위한 완전 한 게임 개발 도구 제품군입니다. 가장 까다로운 콘솔 및 PC 게임의 경우 전 세계의 게임 개발자가 Unreal Engine을 사용 합니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Unreal Engine 개요</td>
        <td><a href="https://www.unrealengine.com/what-is-unreal-engine-4">Unreal Engine 4</a></td>
    </tr>
    <tr>
        <td>PlayFab를 사용 하 여 LiveOps 추가-c + +</td>
        <td><a href="https://api.playfab.com/docs/getting-started/unreal-cpp-getting-started">시작 하기-실제 게임에서 첫 번째 PlayFab API 호출을 수행 합니다.</a></td>
    </tr>
    <tr>
        <td>PlayFab를 사용 하 여 LiveOps 추가-청사진</td>
        <td><a href="https://api.playfab.com/docs/getting-started/unreal-blueprints-getting-started">시작 하기-실제 게임에서 첫 번째 PlayFab API 호출을 수행 합니다.</a></td>
    </tr>
</table>

#### <a name="babylonjs"></a>BabylonJS

BabylonJS는 HTML5, WebGL, WebVR 및 웹 오디오를 사용 하 여 3D 게임을 빌드하기 위한 전체 JavaScript 프레임 워크입니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>BabylonJS</td>
        <td><a href="https://www.babylonjs.com/">BabylonJS</a></td>
    </tr>
    <tr>
        <td>HTML5 및 BabylonJS를 사용 하는 WebGL 3D (비디오 시리즈)</td>
        <td><a href="https://channel9.msdn.com/Series/Introduction-to-WebGL-3D-with-HTML5-and-Babylonjs/01">Learning WebGL 3D 및 BabylonJS</a></td>
    </tr>
    <tr>
        <td>BabylonJS를 사용 하 여 플랫폼 간 WebGL 게임 빌드</td>
        <td><a href="https://www.smashingmagazine.com/2016/07/babylon-js-building-sponza-a-cross-platform-webgl-game/">BabylonJS를 사용 하 여 플랫폼 간 게임 개발</a></td>
    </tr>    
</table>

### <a name="porting-your-game"></a>게임 포팅

기존 게임을 사용 하는 경우 많은 리소스 및 가이드를 사용 하 여 UWP로 신속 하 게 게임을 제공할 수 있습니다. 포팅 작업을 신속 하 게 시작 하기 위해 [유니버설 Windows 플랫폼 브리지](#universal-windows-platform-bridges)를 사용 하는 것을 고려할 수도 있습니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>유니버설 Windows 플랫폼 앱에 Windows 8 앱 포팅</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/porting/w8x-to-uwp-root">Windows 런타임 8.x에서 UWP로 이동</a></td>
    </tr>
    <tr>
        <td>유니버설 Windows 플랫폼 앱에 Windows 8 앱 포팅 (비디오)</td>
        <td><a href="https://channel9.msdn.com/Series/A-Developers-Guide-to-Windows-10/21">8.1 앱을 Windows 10으로 포팅</a></td>
    </tr>
    <tr>
        <td>유니버설 Windows 플랫폼 앱에 iOS 앱 포팅</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/porting/ios-to-uwp-root">iOS에서 UWP로 이동</a></td>
    </tr>
    <tr>
        <td>유니버설 Windows 플랫폼 앱에 Silverlight 앱 포팅</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/porting/wpsl-to-uwp-root">Windows Phone Silverlight에서 UWP로 이동</a></td>
    </tr>
    <tr>
        <td>XAML 또는 Silverlight에서 유니버설 Windows 플랫폼 앱으로 포팅 (비디오)</td>
        <td><a href="https://channel9.msdn.com/Events/Build/2015/3-741">XAML 또는 Silverlight에서 Windows 10으로 앱 포팅</a></td>
    </tr>
    <tr>
        <td>유니버설 Windows 플랫폼 앱으로 Xbox 게임 포팅</td>
        <td><a href="https://developer.xboxlive.com/platform/development/education/Documents/Porting%20from%20Xbox%20One%20to%20Windows%2010.aspx">Xbox One에서 Windows 10 UWP로 포팅</a></td>
    </tr>
    <tr>
        <td>DirectX 9에서 DirectX 11로 포팅</td>
        <td><a href="porting-your-directx-9-game-to-windows-store.md">DirectX 9에서 유니버설 Windows 플랫폼 (UWP) 까지의 포트</a></td>
    </tr>
    <tr>
        <td>Direct3D 11에서 Direct3D 12로 포팅</td>
        <td><a href="https://docs.microsoft.com/windows/desktop/direct3d12/porting-from-direct3d-11-to-direct3d-12">Direct3D 11에서 Direct3D 12로 포팅</a></td>
    </tr>
    <tr>
        <td>OpenGL ES에서 Direct3D 11로 포팅</td>
        <td><a href="port-from-opengl-es-2-0-to-directx-11-1.md">OpenGL ES 2.0에서 Direct3D 11로의 포트</a></td>
    </tr>
    <tr>
        <td>각도를 사용 하 여 Direct3D 11로의 OpenGL</td>
        <td><a href="https://github.com/microsoft/angle/wiki">각도</a></td>
    </tr>
    <tr>
        <td>UWP에 해당 하는 클래식 Windows API</td>
        <td><a href="https://docs.microsoft.com/uwp/win32-and-com/win32-and-com-for-uwp-apps">UWP (유니버설 Windows 플랫폼) 앱의 Windows Api에 대 한 대안</a></td>
    </tr>
</table>

## <a name="prototype-and-design"></a>프로토타입 및 디자인

만들려는 게임의 유형 및이를 구축 하는 데 사용할 도구 및 그래픽 기술을 결정 했으므로 디자인 및 프로토타입을 시작할 준비가 되었습니다. 코어의 경우 게임은 유니버설 Windows 플랫폼 앱 이므로 시작 하 게 됩니다.

### <a name="introduction-to-the-universal-windows-platform-uwp"></a>유니버설 Windows 플랫폼 소개 (UWP)

Windows 10에는 Windows 10 장치에서 공통 API 플랫폼을 제공 하는 UWP (유니버설 Windows 플랫폼)가 도입 되었습니다. UWP는 Windows 런타임 모델을 진화 하 고 확장 하 여 통합 된 통합 코어로 확장 합니다. UWP를 대상으로 하는 게임은 모든 장치에 공통적인 WinRT Api를 호출할 수 있습니다. UWP는 보장 된 API 계층을 제공 하므로 Windows 10 장치에 설치 되는 단일 앱 패키지를 만들도록 선택할 수 있습니다. 원하는 경우 게임이 실행 되는 장치에만 적용 되는 Api (Win32 및 .NET의 일부 클래식 Windows Api 포함)를 계속 해 서 호출할 수 있습니다.

다음은 유니버설 Windows 플랫폼 앱에 대해 자세히 설명 하는 뛰어난 가이드 이며 플랫폼을 이해 하는 데 도움이 되는 권장 사항입니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>유니버설 Windows 플랫폼 apps 소개</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/get-started/whats-a-uwp">유니버설 Windows 플랫폼 앱 이란?</a></td>
    </tr>
    <tr>
        <td>UWP 개요</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/get-started/universal-application-platform-guide">UWP 앱 가이드</a></td>
    </tr>
</table>
 
###UWP 개발 시작 하기

유니버설 Windows 플랫폼 앱을 설정 하 고 개발 하도록 준비 하는 작업은 빠르고 간단 합니다. 다음 가이드에서는이 과정을 단계별로 안내 합니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>UWP 개발 시작 하기</td>
        <td><a href="https://developer.microsoft.com/windows/apps/getstarted">Windows 앱 시작</a></td>
    </tr>
    <tr>
        <td>UWP 개발을 위한 설정</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/get-started/get-set-up">설정</a></td>
    </tr>
</table>

UWP 프로그래밍에 대 한 "절대적인 초보자"이 고 게임에서 XAML을 사용 하는 것을 고려 하는 경우 ( [그래픽 기술 및 프로그래밍 언어 선택](#choosing-your-graphics-technology-and-programming-language)참조), [절대 초보자를 위한 Windows 10 개발](https://channel9.msdn.com/Series/Windows-10-development-for-absolute-beginners) 비디오 시리즈를 시작 하는 것이 좋습니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>XAML을 사용 하 여 Windows 10 개발에 대 한 초보자 가이드 (비디오 시리즈)</td>
        <td><a href="https://channel9.msdn.com/Series/Windows-10-development-for-absolute-beginners">절대적인 초보자를 위한 Windows 10 개발</a></td>
    </tr>
    <tr>
        <td>XAML을 사용 하 여 Windows 10 절대 초보자를 위한 시리즈 발표 (블로그 게시물)</td>
        <td><a href="https://blogs.windows.com/buildingapps/2015/09/30/windows-10-development-for-absolute-beginners/">절대적인 초보자를 위한 Windows 10 개발</a></td>
    </tr>
</table>

### <a name="uwp-development-concepts"></a>UWP 개발 개념

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>유니버설 Windows 플랫폼 앱 개발 개요</td>
        <td><a href="https://developer.microsoft.com/windows/apps/develop">Windows 앱 개발</a></td>
    </tr>
    <tr>
        <td>UWP의 네트워크 프로그래밍 개요</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/networking/index">네트워킹 및 웹 서비스</a></td>
    </tr>
    <tr>
        <td>게임에서 사용 하 여 Windows. HTTP 및 Windows.</td>
        <td><a href="work-with-networking-in-your-directx-game.md">게임의 네트워킹</a></td>
    </tr>
    <tr>
        <td>UWP의 비동기 프로그래밍 개념</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/threading-async/asynchronous-programming-universal-windows-platform-apps">비동기 프로그래밍</a></td>
    </tr>
</table>

### <a name="windows-desktop-apisto-uwp"></a>Windows 데스크톱 Api-UWP

Windows 데스크톱 게임을 UWP로 이동 하는 데 도움이 되는 몇 가지 링크를 제공 합니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>UWP 게임 개발에 기존 c + + 코드 사용</td>
        <td><a href="https://docs.microsoft.com/cpp/porting/how-to-use-existing-cpp-code-in-a-universal-windows-platform-app">방법: UWP 앱에서 기존 c + + 코드 사용</a></td>
    </tr>
    <tr>
        <td>Win32 및 COM Api 용 Windows 런타임 Api</td>
        <td><a href="https://docs.microsoft.com/uwp/win32-and-com/win32-and-com-for-uwp-apps">UWP 앱용 Win32 및 COM API</a></td>
    </tr>
    <tr>
        <td>UWP에서 지원 되지 않는 CRT 함수</td>
        <td><a href="https://docs.microsoft.com/cpp/cppcx/crt-functions-not-supported-in-universal-windows-platform-apps">유니버설 Windows 플랫폼 앱에서 지원되지 않는 CRT 함수</a></td>
    </tr>
    <tr>
        <td>Windows Api의 대안</td>
        <td><a href="https://docs.microsoft.com/uwp/win32-and-com/alternatives-to-windows-apis-uwp">UWP (유니버설 Windows 플랫폼) 앱의 Windows Api에 대 한 대안</a></td>
    </tr>
</table>
 
###프로세스 수명 관리

프로세스 수명 관리 또는 앱 수명 주기는 유니버설 Windows 플랫폼 앱이 전환할 수 있는 다양 한 정품 인증 상태를 설명 합니다. 게임을 활성화, 일시 중단, 다시 시작 또는 종료할 수 있으며, 다양 한 방법으로 이러한 상태를 전환할 수 있습니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>앱 수명 주기 전환 처리</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/launch-resume/app-lifecycle">앱 수명 주기</a></td>
    </tr>
    <tr>
        <td>Microsoft Visual Studio를 사용 하 여 앱 전환 트리거</td>
        <td><a href="https://docs.microsoft.com/visualstudio/debugger/how-to-trigger-suspend-resume-and-background-events-for-windows-store-apps-in-visual-studio?view=vs-2015">Visual Studio에서 UWP 앱에 대 한 일시 중단, 다시 시작 및 백그라운드 이벤트를 트리거하는 방법</a></td>
    </tr>
</table>
 
###게임 UX 디자인

훌륭한 게임의 genesis 디자인에 대 한 것입니다.

게임은 몇 가지 일반적인 사용자 인터페이스 요소와 디자인 원칙을 앱과 공유 하지만 게임의 사용자 환경에는 독특한 모양, 느낌 및 디자인 목표가 있습니다. 게임에서 테스트 된 UX를 사용 하 고 언제 분기 및 혁신을 해야 하나요? 게임에 대해 선택 하는 프레젠테이션 기술 (예를 들어, DirectX, XAML, HTML5 또는 세 가지 조합)은 구현 세부 사항에 영향을 주지만 적용 되는 디자인 원칙은 대부분의 선택과 독립적입니다.

UX 디자인과 별도로 수준 디자인, 속도, 전 세계 디자인 및 기타 측면과 같은 게임 플레이 디자인은 사용자와 사용자의 팀이 개발 가이드에서 다루지 않은 아트의 아트 형태입니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>UWP 디자인 기본 사항 및 지침</td>
        <td><a href="https://developer.microsoft.com/windows/apps/design">UWP 앱 디자인</a></td>
    </tr>
    <tr>
        <td>앱 수명 주기 상태에 대 한 디자인</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/launch-resume/index">시작, 일시 중단 및 다시 시작에 대 한 UX 지침</a></td>
    </tr>
    <tr>
        <td>Xbox One 및 텔레비전 화면에 대 한 UWP 앱 디자인</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/design/devices/designing-for-tv">Xbox 및 TV용 디자인</a></td>
    </tr>
    <tr>
        <td>여러 장치 폼 팩터 대상 지정 (비디오)</td>
        <td><a href="https://channel9.msdn.com/Events/GDC/GDC-2015/Designing-Games-for-a-Windows-Core-World">Windows Core 환경의 게임 디자인</a></td>
    </tr>   
</table>
 
####색 안내선 및 색상표

게임에서 일관 된 색 지침에 따라 미관를 개선 하 고, 탐색 기능을 지원 하며, 플레이어에 게 메뉴 및 HUD 기능을 알리는 강력한 도구입니다. 경고, 손상, XP, 성과 등의 게임 요소에 대 한 일관 된 색 지정은 클리너 UI를 발생 시킬 수 있으며 명시적 레이블의 필요성을 줄입니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>색 안내선</td>
        <td><a href="https://assets.windowsphone.com/499cd2be-64ed-4b05-a4f5-cd0c9ad3f6a3/101_BestPractices_Color_InvariantCulture_Default.zip">모범 사례: 색</a></td>
    </tr>
</table>

#### <a name="typography"></a>입력 체계

입력 체계를 적절 하 게 사용 하면 UI 레이아웃, 탐색, 가독성, 분위기, 브랜드 및 플레이어 집중 교육을 포함 하 여 게임의 많은 측면이 향상 됩니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>입력 체계 가이드</td>
        <td><a href="https://cmsresources.windowsphone.com/devcenter/common/resources/content/101_BestPractices_Typography.pdf">모범 사례: 입력 체계</a></td>
    </tr>
</table>

#### <a name="ui-map"></a>UI 맵

UI 맵은 게임 탐색 및 순서도로 표현 된 메뉴의 레이아웃입니다. UI 맵은 관련 된 모든 이해 관계자가 게임의 인터페이스 및 탐색 경로를 이해 하 고 개발 주기 초기에 잠재적 장애물 및 데드 종료를 노출할 수 있도록 합니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>UI 맵 가이드</td>
        <td><a href="https://cmsresources.windowsphone.com/devcenter/common/resources/content/101_BestPractices_UI_Map.pdf">모범 사례: UI 맵</a></td>
    </tr>
</table>

### <a name="game-audio"></a>게임 오디오

XAudio2, XAPO 및 Windows Sonic을 사용 하 여 게임에서 오디오를 구현 하기 위한 가이드 및 참조입니다. XAudio2은 고성능 오디오 엔진 개발을 위한 신호 처리 및 혼합 토대를 제공 하는 하위 수준 오디오 API입니다. XAPO API를 사용 하면 Windows 및 Xbox에서 XAudio2에 사용 하기 위한 XAPO (플랫폼 간 오디오 처리 개체)를 만들 수 있습니다. Windows Sonic 오디오 지원을 통해 Home 극장 용 돌비 Atmos, 헤드폰에 대 한 돌비 Atmos, 게임 또는 스트리밍 미디어 응용 프로그램에 Windows HRTF 지원을 추가할 수 있습니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>XAudio2 Api</td>
        <td><a href="https://docs.microsoft.com/windows/desktop/xaudio2/xaudio2-apis-portal">XAudio2에 대 한 프로그래밍 가이드 및 API 참조</a></td>
    </tr>
    <tr>
        <td>플랫폼 간 오디오 처리 개체 만들기</td>
        <td><a href="https://docs.microsoft.com/windows/desktop/xaudio2/xapo-overview">XAPO 개요</a></td>
    </tr>
    <tr>
        <td>오디오 개념 소개</td>
        <td><a href="working-with-audio-in-your-directx-game.md">게임 오디오</a></td>
    </tr>
    <tr>
        <td>Windows Sonic 개요</td>
        <td><a href="https://docs.microsoft.com/windows/desktop/CoreAudio/spatial-sound">공간 음향</a></td>
    </tr>
    <tr>
        <td>Windows Sonic 공간 사운드 샘플</td>
        <td><a href="https://github.com/Microsoft/Xbox-ATG-Samples/tree/master/UWPSamples/Audio">Xbox 고급 기술 그룹 오디오 샘플</a></td>
    </tr>
    <tr>
        <td>게임에 Windows Sonic을 통합 하는 방법 알아보기 (비디오)</td>
        <td><a href="https://channel9.msdn.com/Events/GDC/GDC-2017/GDC2017-002">Xbox 및 Windows에 대 한 공간 오디오 기능 소개</a></td>
    </tr>
</table>

### <a name="directx-development"></a>DirectX 개발

DirectX 게임 개발에 대 한 가이드 및 참조입니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>UWP 개발용 DirectX</td>
        <td><a href="directx-programming.md">DirectX 프로그래밍</a></td>
    </tr>
    <tr>
        <td>자습서: UWP DirectX 게임을 만드는 방법</td>
        <td><a href="tutorial--create-your-first-uwp-directx-game.md">DirectX를 사용 하 여 간단한 UWP 게임 만들기</a></td>
    </tr>
    <tr>
        <td>UWP 앱 모델과의 DirectX 상호 작용</td>
        <td><a href="about-the-uwp-user-interface-and-directx.md">앱 개체 및 DirectX</a></td>
    </tr>
    <tr>
        <td>그래픽 및 DirectX 12 개발 비디오 (YouTube 채널)</td>
        <td><a href="https://www.youtube.com/channel/UCiaX2B8XiXR70jaN7NK-FpA">Microsoft DirectX 12 및 그래픽 교육</a></td>
    </tr>
    <tr>
        <td>DirectX 개요 및 참조</td>
        <td><a href="https://docs.microsoft.com/windows/desktop/directx">DirectX 그래픽 및 게임</a></td>
    </tr>
    <tr>
        <td>Direct3D 12 프로그래밍 가이드 및 참조</td>
        <td><a href="https://docs.microsoft.com/windows/desktop/direct3d12/direct3d-12-graphics">Direct3D 12 그래픽</a></td>
    </tr>
    <tr>
        <td>DirectX 12 기본 사항 (비디오)</td>
        <td><a href="https://channel9.msdn.com/Events/GDC/GDC-2015/Better-Power-Better-Performance-Your-Game-on-DirectX12">더 나은 성능, 더 나은 성능: DirectX 12의 게임</a></td>
    </tr>
</table>

#### <a name="learning-direct3d-12"></a>Direct3D 12 학습

Direct3d 12에서 변경 된 내용과 Direct3D 12를 사용 하 여 프로그래밍을 시작 하는 방법을 알아봅니다. 

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>프로그래밍 환경 설정</td>
        <td><a href="https://docs.microsoft.com/windows/desktop/direct3d12/directx-12-programming-environment-set-up">Direct3D 12 프로그래밍 환경 설정</a></td>
    </tr>
    <tr>
        <td>기본 구성 요소를 만드는 방법</td>
        <td><a href="https://docs.microsoft.com/windows/desktop/direct3d12/creating-a-basic-direct3d-12-component">기본 Direct3D 12 구성 요소 만들기</a></td>
    </tr>
    <tr>
        <td>Direct3D 12의 변경 내용</td>
        <td><a href="https://docs.microsoft.com/windows/desktop/direct3d12/important-changes-from-directx-11-to-directx-12">Direct3D 11에서 Direct3D 12로 마이그레이션하는 중요 한 변경 내용</a></td>
    </tr>
    <tr>
        <td>Direct3D 11에서 Direct3D 12로 이식 하는 방법</td>
        <td><a href="https://docs.microsoft.com/windows/desktop/direct3d12/porting-from-direct3d-11-to-direct3d-12">Direct3D 11에서 Direct3D 12로 포팅</a></td>
    </tr>
    <tr>
        <td>리소스 바인딩 개념 (설명자, 설명자 테이블, 설명자 힙 및 루트 서명 포함) </td>
        <td><a href="https://docs.microsoft.com/windows/desktop/direct3d12/resource-binding">Direct3D 12의 리소스 바인딩</a></td>
    </tr>
    <tr>
        <td>메모리 관리</td>
        <td><a href="https://docs.microsoft.com/windows/desktop/direct3d12/memory-management">Direct3D 12의 메모리 관리</a></td>
    </tr>
</table>
 
####DirectX 도구 키트 및 라이브러리

Directx 도구 키트, DirectX 텍스처 처리 라이브러리, DirectXMesh geometry 처리 라이브러리, UVAtlas library 및 Directxmesh library는 DirectX 개발용 텍스처, 메시, 스프라이트 및 기타 유틸리티 기능 및 도우미 클래스를 제공 합니다. 이러한 라이브러리는 개발 시간과 노력을 절감 하는 데 도움이 됩니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Directx 11 용 DirectX 도구 키트 받기</td>
        <td><a href="https://github.com/Microsoft/DirectXTK">DirectXTK</a></td>
    </tr>
    <tr>
        <td>DirectX 12 용 DirectX 도구 키트 받기</td>
        <td><a href="https://github.com/Microsoft/DirectXTK12">DirectXTK 12</a></td>
    </tr>
    <tr>
        <td>DirectX 텍스처 처리 라이브러리 가져오기</td>
        <td><a href="https://github.com/Microsoft/DirectXTex">DirectXTex</a></td>
    </tr>
    <tr>
        <td>DirectXMesh geometry 처리 라이브러리 가져오기</td>
        <td><a href="https://github.com/Microsoft/DirectXMesh">DirectXMesh</a></td>
    </tr>
    <tr>
        <td>Isochart 텍스처 아틀라스 만들기 및 압축을 위한 UVAtlas 가져오기</td>
        <td><a href="https://github.com/Microsoft/UVAtlas">UVAtlas</a></td>
    </tr>
    <tr>
        <td>DirectXMath 라이브러리 가져오기</td>
        <td><a href="https://github.com/Microsoft/DirectXMath">DirectXMath</a></td>
    </tr>
    <tr>
        <td>DirectXTK의 Direct3D 12 지원 (블로그 게시물)</td>
        <td><a href="https://github.com/Microsoft/DirectXTK/issues/2">DirectX 12에 대 한 지원</a></td>
    </tr>
</table>

#### <a name="directx-resources-from-partners"></a>파트너의 DirectX 리소스

외부 파트너가 만든 몇 가지 추가 DirectX 설명서가 있습니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Nvidia: DX12 Do 및 일과 (블로그 게시물) </td>
        <td><a href="https://developer.nvidia.com/dx12-dos-and-donts-updated">Nvidia Gpu의 DirectX 12</a></td>
    </tr>
    <tr>
        <td>Intel: DirectX 12를 사용 하 여 효율적인 렌더링</td>
        <td><a href="https://software.intel.com/sites/default/files/managed/4a/38/Efficient-Rendering-with-DirectX-12-on-Intel-Graphics.pdf">Intel 그래픽의 DirectX 12 렌더링</a></td>
    </tr>
    <tr>
        <td>Intel: DirectX 12에서 다중 어댑터 지원</td>
        <td><a href="https://software.intel.com/articles/multi-adapter-support-in-directx-12">DirectX 12를 사용 하 여 명시적 다중 어댑터 응용 프로그램을 구현 하는 방법</a></td>
    </tr>
    <tr>
        <td>Intel: DirectX 12 자습서</td>
        <td><a href="https://software.intel.com/articles/tutorial-migrating-your-apps-to-directx-12-part-1">Intel, Suzhou 달팽이 및 Microsoft의 공동 작업 백서</a></td>
    </tr>
</table>

## <a name="production"></a>프로덕션

이제 사용자의 스튜디오는 팀 전체에 걸쳐 분산 된 작업을 통해 완벽 하 게 참여 하 고 프로덕션 주기로 전환 됩니다. 개선 하 고,이를 활용 하 여 전체 게임으로 만들 수 있습니다.

### <a name="notifications-and-live-tiles"></a>알림 및 라이브 타일

타일은 시작 메뉴의 게임 표현입니다. 타일 및 알림은 현재 게임을 플레이 하 고 있지 않은 경우에도 플레이어 관심을 받을 수 있습니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>타일 및 배지 개발</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/controls-and-patterns/tiles-badges-notifications">타일, 배지 및 알림</a></td>
    </tr>
    <tr>
        <td>라이브 타일 및 알림을 보여 주는 샘플</td>
        <td><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Notifications">알림 샘플</a></td>
    </tr>
    <tr>
        <td>적응 타일 템플릿 (블로그 게시물)</td>
        <td><a href="https://blogs.msdn.microsoft.com/tiles_and_toasts/2015/06/30/adaptive-tile-templates-schema-and-documentation/">적응 타일 템플릿-스키마 및 설명서</a></td>
    </tr>
    <tr>
        <td>타일 및 배지 디자인</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/controls-and-patterns/tiles-and-notifications-creating-tiles">타일 및 배지에 대한 지침</a></td>
    </tr>
    <tr>
        <td>라이브 타일 템플릿을 대화형으로 개발 하기 위한 Windows 10 앱</td>
        <td><a href="https://www.microsoft.com/store/apps/9nblggh5xsl1">알림 시각화 도우미</a></td>
    </tr>
    <tr>
        <td>Visual Studio 용 UWP 타일 생성기 확장</td>
        <td><a href="https://marketplace.visualstudio.com/items?itemName=shenchauhan.UWPTileGenerator">단일 이미지를 사용 하 여 필요한 모든 타일을 만들기 위한 도구</a></td>
    </tr>
    <tr>
        <td>Visual Studio 용 UWP 타일 생성기 확장 (블로그 게시물)</td>
        <td><a href="https://blogs.windows.com/buildingapps/2016/02/15/uwp-tile-generator-extension-for-visual-studio/">UWP 타일 생성기 도구 사용에 대 한 팁</a></td>
    </tr>
</table>

### <a name="enable-in-app-product-add-on-purchases"></a>앱 내 제품 (추가 기능) 구매 사용

추가 기능 (앱 내 제품)은 플레이어에서 게임을 통해 구매할 수 있는 보조 항목입니다. 추가 기능은 게임 수준, 항목 또는 플레이어가 누릴 수 있는 다른 모든 항목 일 수 있습니다. 적절 한 추가 기능을 사용 하면 게임 환경을 개선 하는 동시에 수익을 제공할 수 있습니다. 파트너 센터를 통해 게임의 추가 기능을 정의 및 게시 하 고 게임 코드에서 앱 내 구매를 사용 하도록 설정 합니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>내구성이 있는 추가 기능</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/monetize/enable-in-app-product-purchases">앱에서 바로 구매 제품 사용</a></td>
    </tr>
    <tr>
        <td>사용할 때 추가 기능</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/monetize/enable-consumable-in-app-product-purchases">앱에서 바로 소모성 제품 구매 사용</a></td>
    </tr>
    <tr>
        <td>추가 정보 및 제출</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/publish/iap-submissions">추가 기능 제출</a></td>
    </tr>
    <tr>
        <td>게임의 추가 판매 및 인구 통계 모니터링</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/publish/iap-acquisitions-report">추가 기능 인수 보고서</a></td>
    </tr>
</table>
 
###디버깅, 성능 최적화 및 모니터링

성능을 최적화 하려면 Windows 10의 게임 모드를 활용 하 여 현재 하드웨어의 용량을 완전히 활용 하 여 최적의 게임 환경을 게이머에 게 제공할 수 있습니다.

WPT (Windows 성능 도구 키트)는 Windows 운영 체제 및 응용 프로그램의 심층 성능 프로필을 생성 하는 성능 모니터링 도구로 구성 됩니다. 특히 메모리 사용을 모니터링 하 고 게임 성능을 향상 시키는 데 유용 합니다. Windows 성능 도구 키트는 Windows 10 SDK 및 Windows ADK에 포함 되어 있습니다. 이 도구 키트는 Windows 성능 레코더 (WPR)와 Windows 성능 분석기 (WPA)의 두 가지 독립 도구로 구성 됩니다. [Windows Sysinternals](https://technet.microsoft.com/sysinternals/default)의 일부인 PROCDUMP는 CPU 급증을 모니터링 하 고 게임 충돌 시 덤프 파일을 생성 하는 명령줄 유틸리티입니다. 

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>코드 성능 테스트</td>
        <td><a href="https://azure.microsoft.com/services/devops/test-plans/">클라우드 기반 부하 테스트</a></td>
    </tr>
    <tr>
        <td>게임 장치 정보를 사용 하 여 Xbox 콘솔 유형 가져오기</td>
        <td><a href="https://docs.microsoft.com/previous-versions/windows/desktop/gamingdvcinfo/gaming-device-information-portal">게임 디바이스 정보</a></td>
    </tr>
    <tr>
        <td>게임 모드 Api를 사용 하 여 하드웨어 리소스에 대 한 단독 또는 우선 순위 액세스를 통해 성능을 향상 시킵니다.</td>
        <td><a href="https://docs.microsoft.com/previous-versions/windows/desktop/gamemode/game-mode-portal">게임 모드</a></td>
    </tr>
    <tr>
        <td>Windows 10 SDK에서 WPT (Windows 성능 도구 키트) 받기</td>
        <td><a href="https://developer.microsoft.com/windows/downloads/windows-10-sdk">Windows 10 SDK</a></td>
    </tr>
    <tr>
        <td>Windows ADK에서 WPT (Windows 성능 도구 키트) 받기</td>
        <td><a href="https://msdn.microsoft.com/windows/hardware/dn913721.aspx">Windows ADK</a></td>
    </tr>
    <tr>
        <td>Windows 성능 분석기를 사용 하 여 책임 없는 UI 문제 해결 (비디오)</td>
        <td><a href="https://channel9.msdn.com/Shows/Defrag-Tools/Defrag-Tools-156-Critical-Path-Analysis-with-Windows-Performance-Analyzer">WPA를 사용 하 여 중요 한 경로 분석</a></td>
    </tr>
    <tr>
        <td>Windows 성능 레코더를 사용 하 여 메모리 사용 및 누수 진단 (비디오)</td>
        <td><a href="https://channel9.msdn.com/Shows/Defrag-Tools/Defrag-Tools-154-Memory-Footprint-and-Leaks">메모리 사용 공간 및 누수</a></td>
    </tr>
    <tr>
        <td>ProcDump 가져오기</td>
        <td><a href="https://technet.microsoft.com/sysinternals/dd996900">ProcDump</a></td>
    </tr>
    <tr>
        <td>ProcDump 사용 방법 알아보기 (비디오)</td>
        <td><a href="https://channel9.msdn.com/Shows/Defrag-Tools/Defrag-Tools-131-Windows-10-SDK">ProcDump를 구성 하 여 덤프 파일 만들기</a></td>
    </tr>
</table>

### <a name="advanced-directx-techniques-and-concepts"></a>고급 DirectX 기술 및 개념

DirectX 개발의 일부 부분은 미묘한 및 복잡할 수 있습니다. DirectX 엔진의 세부 정보를 자세히 확인 하거나 어려운 성능 문제를 디버그 해야 하는 프로덕션 환경에 있는 경우이 섹션의 리소스와 정보가 도움이 될 수 있습니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Windows의 PIX</td>
        <td><a href="https://devblogs.microsoft.com/pix/introducing-pix-on-windows-beta/">Windows의 DirectX 12에 대 한 성능 조정 및 디버깅 도구</a></td>
    </tr>
    <tr>
        <td>D3D12 개발용 디버깅 및 유효성 검사 도구 (비디오)</td>
        <td><a href="https://channel9.msdn.com/Events/GDC/GDC-2017/GDC2017-003">PIX 및 GPU 유효성 검사를 사용 하 여 성능 튜닝 및 디버깅 D3D12</a></td>
    </tr>
    <tr>
        <td>그래픽 및 성능 최적화 (비디오)</td>
        <td><a href="https://channel9.msdn.com/Events/GDC/GDC-2015/Advanced-DirectX12-Graphics-and-Performance">고급 DirectX 12 그래픽 및 성능</a></td>
    </tr>
    <tr>
        <td>DirectX 그래픽 디버깅 (비디오)</td>
        <td><a href="https://channel9.msdn.com/Events/GDC/GDC-2015/Solve-the-Tough-Graphics-Problems-with-your-Game-Using-DirectX-Tools">DirectX Tools를 사용 하 여 게임의 견고한 그래픽 문제 해결</a></td>
    </tr>
    <tr>
        <td>DirectX 12를 디버깅 하기 위한 Visual Studio 2015 도구 (비디오)</td>
        <td><a href="https://channel9.msdn.com/Series/ConnectOn-Demand/212">Visual Studio 2015의 Windows 10 용 DirectX 도구</a></td>
    </tr>
    <tr>
        <td>Direct3D 12 프로그래밍 가이드</td>
        <td><a href="https://docs.microsoft.com/windows/desktop/direct3d12/direct3d-12-graphics">Direct3D 12 프로그래밍 가이드</a></td>
    </tr>
    <tr>
        <td>DirectX 및 XAML 결합</td>
        <td><a href="directx-and-xaml-interop.md">DirectX 및 XAML interop</a></td>
    </tr>
</table>

### <a name="high-dynamic-range-hdr-content-development"></a>HDR (High dynamic range) 콘텐츠 개발

HDR의 전체 색 기능을 사용 하는 게임 콘텐츠를 빌드합니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>HDR 및 색 개념 소개 (비디오)</td>
        <td><a href="https://channel9.msdn.com/Events/Build/2017/P4061">DirectX에서 HDR 및 고급 색의 조명</a></td>
    </tr>
    <tr>
        <td>HDR 콘텐츠를 렌더링 하 고 현재 표시에서 지원 하는지 여부를 검색 하는 방법을 알아봅니다.</td>
        <td><a href="https://github.com/Microsoft/DirectX-Graphics-Samples/tree/master/Samples/UWP/D3D12HDR">HDR 샘플</a></td>
    </tr>
    <tr>
        <td>DirectX를 사용 하 여 고급 색 만들기 및 구성</td>
        <td><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/D2DAdvancedColorImages">Direct2D 고급 색 이미지 렌더링 샘플</a></td>
    </tr>   
</table>

### <a name="globalization-and-localization"></a>전역화 및 지역화

Windows 플랫폼용 세계 시장 대응성 게임을 개발 하 고 Microsoft의 인기 제품에 내장 된 국가별 기능에 대해 알아보세요.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>글로벌 시장을 위한 게임 준비</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/globalizing/globalizing-portal">전 세계 사용자를 위한 개발 지침</a></td>
    </tr>
    <tr>
        <td>언어, 문화 및 기술 브리징</td>
        <td><a href="https://www.microsoft.com/Language/Default.aspx">언어 규칙 및 표준 Microsoft 용어에 대 한 온라인 리소스</a></td>
    </tr>
</table>

## <a name="submitting-and-publishing-your-game"></a>게임 제출 및 게시

다음 가이드와 정보는 최대한 원활 하 게 게시 및 제출 프로세스를 수행 하는 데 도움이 됩니다.

### <a name="publishing"></a>게시

[파트너 센터](https://partner.microsoft.com/dashboard) 를 사용 하 여 게임 패키지를 게시 하 고 관리 합니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>파트너 센터 앱 게시</td>
        <td><a href="https://developer.microsoft.com/store/publish-apps">Windows 앱 게시</a></td>
    </tr>
    <tr>
        <td>GDN (파트너 센터 고급 게시)</td>
        <td><a href="https://developer.xboxlive.com/windows/documentation/Pages/home.aspx">파트너 센터 고급 게시 가이드</a></td>
    </tr>
    <tr>
        <td>AAD (Azure Active Directory)를 사용 하 여 파트너 센터 계정에 사용자 추가</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/publish/manage-account-users">계정 사용자 관리</a></td>
    </tr>   
    <tr>
        <td>게임 등급 (블로그 게시물)</td>
        <td><a href="https://blogs.windows.com/buildingapps/2016/01/06/now-available-single-age-rating-system-to-simplify-app-submissions/">IARC 시스템을 사용 하 여 연령 등급을 할당 하는 단일 워크플로</a></td>
    </tr>
</table>

#### <a name="packaging-and-uploading"></a>패키징 및 업로드

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>스트리밍 설치 및 선택적 패키지 사용 방법 알아보기 (비디오)</td>
        <td><a href="https://channel9.msdn.com/Events/Build/2017/B8093">Nextgen UWP 앱 배포: 확장 가능 하 고, 스트림 가능 하며, 구성 가능한 앱 빌드</a></td>
    </tr>
    <tr>
        <td>콘텐츠를 나누고 그룹화 하 여 스트리밍 설치 사용</td>
        <td><a href="https://docs.microsoft.com/windows/msix/package/streaming-install">UWP 앱 스트리밍 설치</a></td>
    </tr>
    <tr>
        <td>DLC 게임 콘텐츠와 같은 선택적 패키지 만들기</td>
        <td><a href="https://docs.microsoft.com/windows/msix/package/optional-packages">선택형 패키지 및 관련 세트 제작</a></td>
    </tr>
    <tr>
        <td>UWP 게임 패키지</td>
        <td><a href="../packaging/index.md">앱 패키징</a></td>
    </tr>
    <tr>
        <td>UWP DirectX 게임 패키지</td>
        <td><a href="package-your-windows-store-directx-game.md">UWP DirectX 게임 패키지</a></td>
    </tr>
    <tr>
        <td>타사 개발자로 게임 패키징 (블로그 게시물)</td>
        <td><a href="https://blogs.windows.com/buildingapps/2015/12/15/building-an-app-for-a-3rd-party-how-to-package-their-store-app/">게시자 저장소 계정 액세스 권한이 없는 uploadable 패키지 만들기</a></td>
    </tr>
    <tr>
        <td>MakeAppx를 사용 하 여 앱 패키지 및 앱 패키지 번들 만들기</td>
        <td><a href="https://docs.microsoft.com/windows/msix/package/create-app-package-with-makeappx-tool">앱 패키지 작성 도구를 사용 하 여 패키지 만들기 MakeAppx.exe</a></td>
    </tr>
    <tr>
        <td>SignTool를 사용 하 여 파일에 디지털 서명</td>
        <td><a href="https://docs.microsoft.com/windows/desktop/SecCrypto/signtool">SignTool를 사용 하 여 파일에 서명 및 파일의 서명 확인</a></td>
    </tr>    
    <tr>
        <td>게임 업로드 및 버전 관리</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/publish/upload-app-packages">앱 패키지 업로드</a></td>
    </tr>
</table>

### <a name="policies-and-certification"></a>정책 및 인증

인증 문제가 게임의 릴리스를 지연 하지 않도록 합니다. 유의 해야 할 정책 및 일반적인 인증 문제는 다음과 같습니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Microsoft Store 앱 개발자 계약</td>
        <td><a href="https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement">앱 개발자 계약</a></td>
    </tr>
    <tr>
        <td>Microsoft Store에 앱을 게시 하기 위한 정책</td>
        <td><a href="https://docs.microsoft.com/legal/windows/agreements/store-policies">Microsoft Store 정책</a></td>
    </tr>
    <tr>
        <td>몇 가지 일반적인 앱 인증 문제를 방지 하는 방법</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/publish/avoid-common-certification-failures">일반적인 인증 실패 방지</a></td>
    </tr>
</table>

### <a name="store-manifest-storemanifestxml"></a>저장소 매니페스트 (StoreManifest.xml)

저장소 매니페스트 (StoreManifest.xml)는 앱 패키지에 포함 될 수 있는 선택적 구성 파일입니다. 저장소 매니페스트는 AppxManifest.xml 파일에 포함 되지 않은 추가 기능을 제공 합니다. 예를 들어 대상 장치에 지정 된 최소 DirectX 기능 수준 또는 지정 된 최소 시스템 메모리가 없는 경우 스토어 매니페스트를 사용 하 여 게임의 설치를 차단할 수 있습니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>스토어 매니페스트 스키마</td>
        <td><a href="https://docs.microsoft.com/uwp/schemas/storemanifest/storemanifestschema2015/schema-root">StoreManifest 스키마(Windows 10)</a></td>
    </tr>
</table>

## <a name="game-lifecycle-management"></a>게임 수명 주기 관리

개발을 완료 하 고 게임을 배송 한 후에는 "게임을 진행" 하지 않습니다. 버전 1에 대 한 개발을 완료 했을 수 있지만 marketplace의 게임 경험이 막 시작 되었습니다. 사용 및 오류 보고를 모니터링 하 고, 사용자 피드백에 응답 하 고, 게임에 업데이트를 게시 하는 것이 좋습니다.

### <a name="partner-center-analytics-and-promotion"></a>파트너 센터 분석 및 수준 올리기

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>파트너 센터 분석</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/publish/analytics">앱 성능 분석</a></td>
    </tr>
    <tr>
        <td>고객이 게임에서 Xbox 기능을 사용 하는 방법을 알아봅니다.</td>
        <td><a href="../publish/xbox-analytics-report.md">Xbox 분석 보고서</a></td>
    </tr>
    <tr>
        <td>고객 리뷰에 응답</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/publish/respond-to-customer-reviews">고객 리뷰에 응답</a></td>
    </tr>
    <tr>
        <td>게임을 홍보 하는 방법</td>
        <td><a href="https://developer.microsoft.com/store/promote-your-apps">앱 홍보</a></td>
    </tr>
</table>
 
###Visual Studio Application Insights

Visual Studio Application Insights는 게시 된 게임에 대 한 성능, 원격 분석 및 사용량 분석을 제공 합니다. Application Insights는 게임 출시 후의 문제를 감지 및 해결 하 고, 사용 현황을 지속적으로 모니터링 및 개선 하 고, 플레이어가 게임과 상호 작용 하는 방식을 이해 하는 데 도움이 됩니다. Application Insights는 앱에 SDK를 추가하여 작동하며, [Azure 포털](https://portal.azure.com/)에 원격 분석을 보냅니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>응용 프로그램 성능 및 사용 현황 분석</td>
        <td><a href="https://azure.microsoft.com/documentation/articles/app-insights-get-started/">Visual Studio Application Insights</a></td>
    </tr>
    <tr>
        <td>Windows 앱에서 Application Insights 사용</td>
        <td><a href="https://azure.microsoft.com/documentation/articles/app-insights-windows-get-started/">Windows Phone 및 스토어 앱에 대 한 Application Insights</a></td>
    </tr>
</table>

### <a name="third-party-solutions-for-analytics-and-promotion"></a>분석 및 홍보를 위한 타사 솔루션

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>GameAnalytics를 사용 하 여 플레이어 동작 이해</td>
        <td><a href="https://gameanalytics.com/">GameAnalytics</a></td>
    </tr>
    <tr>
        <td>UWP 게임을 Google Analytics에 연결</td>
        <td><a href="https://github.com/dotnet/windows-sdk-for-google-analytics">Google 분석을 위한 Windows SDK 가져오기</a></td>
    </tr>
    <tr>
        <td>Google 분석을 위해 Windows SDK를 사용 하는 방법 알아보기 (비디오)</td>
        <td><a href="https://channel9.msdn.com/Events/Windows/Windows-Developer-Day-Creators-Update/Getting-started-with-the-Windows-SDK-for-Google-Analytics">Google Analytics에 대 한 Windows SDK 시작</a></td>
    </tr>    
    <tr>
        <td>Facebook 앱을 사용 하 여 게임을 Facebook 사용자에 게 홍보</td>
        <td><a href="https://github.com/Microsoft/winsdkfb">Facebook에 대 한 Windows SDK 가져오기</a></td>
    </tr>
    <tr>
        <td>Facebook 앱 설치 광고를 사용 하는 방법 알아보기 (비디오)</td>
        <td><a href="https://channel9.msdn.com/Events/Windows/Windows-Developer-Day-Creators-Update/Getting-started-with-Facebook-App-Install-Ads">Facebook에 대 한 Windows SDK 시작</a></td>
    </tr>
    <tr>
        <td>Vungle를 사용 하 여 게임에 비디오 광고 추가</td>
        <td><a href="https://publisher.vungle.com/sdk/">Vungle에 대 한 Windows SDK 가져오기</a></td>
    </tr>
</table>

### <a name="creating-and-managing-content-updates"></a>콘텐츠 업데이트 만들기 및 관리

게시 된 게임을 업데이트 하려면 더 높은 버전 번호를 사용 하 여 새 앱 패키지를 제출 합니다. 패키지의 전송 및 인증을 통해 패키지를 만든 후에는 고객이 업데이트로 자동으로 사용할 수 있습니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>게임 업데이트 및 버전 관리</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/publish/package-version-numbering">패키지 버전 번호 매기기</a></td>
    </tr>
    <tr>
        <td>게임 패키지 관리 지침</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/publish/package-version-numbering">앱 패키지 관리에 대 한 지침</a></td>
    </tr>
</table>

## <a name="adding-xbox-live-to-your-game"></a>게임에 Xbox Live 추가

Xbox Live는 전 세계 수 백만 명의 게이머를 연결하는 최고의 게임 네트워크입니다. 개발자는 Xbox live 현재 상태, 순위표, 클라우드 저장, 게임 허브, 클럽, 파티 채팅, 게임 DVR 등을 포함 하 여 게임의 사용자를 유기적으 수 있는 Xbox Live 기능에 액세스할 수 있습니다.

> [!Note]
> Xbox Live 사용 제목을 개발 하려는 경우 몇 가지 옵션을 사용할 수 있습니다. 다양 한 프로그램에 대 한 정보는 [개발자 프로그램 개요](https://docs.microsoft.com/gaming/xbox-live/developer-program-overview)를 참조 하세요.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Xbox Live 개요</td>
        <td><a href="https://docs.microsoft.com/gaming/xbox-live/index.md">Xbox Live 개발자 가이드</a></td>
    </tr>
    <tr>
        <td>프로그램에 따라 사용할 수 있는 기능 이해</td>
        <td><a href="https://docs.microsoft.com/gaming/xbox-live/developer-program-overview.md#feature-table">개발자 프로그램 개요: 기능 테이블</a></td>
    </tr>
    <tr>
        <td>Xbox Live 게임 개발을 위한 유용한 리소스에 대 한 링크</td>
        <td><a href="https://docs.microsoft.com/gaming/xbox-live/xbox-live-resources.md">Xbox Live 리소스</a></td>
    </tr>
    <tr>
        <td>Xbox Live 서비스에서 정보를 가져오는 방법 알아보기</td>
        <td><a href="https://docs.microsoft.com/gaming/xbox-live/introduction-to-xbox-live-apis.md">Xbox Live Api 소개</a></td>
    </tr>
</table>

### <a name="for-developers-in-the-xbox-live-creators-program"></a>Xbox Live 크리에이터 프로그램의 개발자

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>개요</td>
        <td><a href="https://docs.microsoft.com/gaming/xbox-live/get-started-with-creators/get-started-with-xbox-live-creators.md">Xbox Live 크리에이터 프로그램 시작</a></td>
    </tr>
    <tr>
        <td>게임에 Xbox Live 추가</td>
        <td><a href="https://docs.microsoft.com/gaming/xbox-live/get-started-with-creators/creators-step-by-step-guide.md">Xbox Live 크리에이터 프로그램 통합에 대 한 단계별 가이드</a></td>
    </tr>
    <tr>
        <td>Unity를 사용 하 여 만든 UWP 게임에 Xbox Live 추가</td>
        <td><a href="https://docs.microsoft.com/gaming/xbox-live/get-started-with-creators/develop-creators-title-with-unity.md">Unity 게임 엔진을 사용 하 여 Xbox Live 크리에이터 프로그램 타이틀 개발 시작</a></td>
    </tr>
    <tr>
        <td>개발 샌드박스 설정</td>
        <td><a href="https://docs.microsoft.com/gaming/xbox-live/get-started-with-creators/xbox-live-sandboxes-creators.md">Xbox Live 샌드박스 소개</a></td>
    </tr>
    <tr>
        <td>테스트를 위한 계정 설정</td>
        <td><a href="https://docs.microsoft.com/gaming/xbox-live/get-started-with-creators/authorize-xbox-live-accounts.md">테스트 환경에서 Xbox Live 계정 권한 부여</a></td>
    </tr>
    <tr>
        <td>Xbox Live 크리에이터 프로그램 샘플</td>
        <td><a href="https://github.com/Microsoft/xbox-live-samples/tree/master/Samples/CreatorsSDK">작성자 프로그램 개발자를 위한 코드 샘플</a></td>
    </tr>
    <tr>
        <td>UWP 게임에서 플랫폼 간 Xbox Live 환경을 통합 하는 방법 알아보기 (비디오)</td>
        <td><a href="https://channel9.msdn.com/Events/GDC/GDC-2017/GDC2017-005">Xbox Live 크리에이터스 프로그램</a></td>
    </tr>  
</table>

### <a name="for-managed-partners-and-developers-in-the-idxbox-program"></a>프로그램의 관리 되는 파트너 및 개발자 용 ID@Xbox

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>개요</td>
        <td><a href="https://docs.microsoft.com/gaming/xbox-live/get-started-with-partner/get-started-with-xbox-live-partner.md">Xbox Live를 관리 되는 파트너 또는 ID 개발자로 시작 하기</a></td>
    </tr>
    <tr>
        <td>게임에 Xbox Live 추가</td>
        <td><a href="https://docs.microsoft.com/gaming/xbox-live/get-started-with-partner/partners-step-by-step-guide.md">관리 되는 파트너 및 ID 구성원에 대해 Xbox Live를 통합 하는 단계별 가이드</a></td>
    </tr>
    <tr>
        <td>Unity를 사용 하 여 만든 UWP 게임에 Xbox Live 추가</td>
        <td><a href="https://docs.microsoft.com/gaming/xbox-live/get-started-with-partner/partner-unity-uwp-il2cpp.md">ID 및 관리 되는 파트너에 대 한 IL2CPP scripting 백 엔드가 포함 된 UWP 용 Unity에 Xbox Live 지원 추가</a></td>
    </tr>
    <tr>
        <td>개발 샌드박스 설정</td>
        <td><a href="https://docs.microsoft.com/gaming/xbox-live/get-started-with-partner/advanced-xbox-live-sandboxes.md">고급 Xbox Live 샌드박스</a></td>
    </tr>
    <tr>
        <td>Xbox Live (GDN)를 사용 하는 게임에 대 한 요구 사항</td>
        <td><a href="https://edadfs.partners.extranet.microsoft.com/adfs/ls/?wa=wsignin1.0&wtrealm=https%3a%2f%2fdeveloper.xboxlive.com&wctx=rm%3d0%26id%3dpassive%26ru%3d%252fen-us%252flive%252fcertification%252frequirements%252fPages%252fTCR.aspx&wct=2019-11-20T19%3a55%3a26Z">Windows 10에서 Xbox Live를 위한 xbox 요구 사항</a></td>
    </tr>
    <tr>
        <td>샘플</td>
        <td><a href="https://github.com/Microsoft/xbox-live-samples/tree/master/Samples/ID%40XboxSDK">개발자를 위한 코드 샘플 ID@Xbox</a></td>
    </tr>  
    <tr>
        <td>Xbox Live 게임 개발 개요 (비디오)</td>
        <td><a href="https://channel9.msdn.com/Events/GDC/GDC-2015/Developing-with-Xbox-Live-for-Windows-10">Windows 10 용 Xbox Live를 사용 하 여 개발</a></td>
    </tr>
    <tr>
        <td>플랫폼 간 matchmaking (비디오)</td>
        <td><a href="https://channel9.msdn.com/Events/GDC/GDC-2015/Xbox-Live-Multiplayer-Introducing-services-for-cross-platform-matchmaking-and-gameplay">Xbox Live 여럿이: 플랫폼 간 matchmaking 및 게임을 위한 서비스 소개</a></td>
    </tr>
    <tr>
        <td>Fable 범례에서 장치 교차 게임 (비디오)</td>
        <td><a href="https://channel9.msdn.com/Events/GDC/GDC-2015/Fable-Legends-Cross-device-Gameplay-with-Xbox-Live">Fable 범례: Xbox Live를 사용 하 여 장치 간 게임</a></td>
    </tr>
    <tr>
        <td>Xbox Live 통계 및 성과 (비디오)</td>
        <td><a href="https://channel9.msdn.com/Events/GDC/GDC-2015/Best-Practices-for-Leveraging-Cloud-Based-User-Stats-and-Achievements-in-Xbox-Live">Xbox Live에서 클라우드 기반 사용자 통계 및 성과를 활용 하기 위한 모범 사례</a></td>
    </tr>
</table>

## <a name="additional-resources"></a>추가 리소스

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>게임 개발 동영상</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/gaming/game-development-videos">GDC 및 \build와 같은 주요 회의 비디오</a></td>
    </tr>
    <tr>
        <td>Indie 게임 개발 (비디오)</td>
        <td><a href="https://channel9.msdn.com/Events/GDC/GDC-2015/New-Opportunities-for-Independent-Developers">독립적인 개발자를 위한 새로운 기회</a></td>
    </tr>
    <tr>
        <td>다중 코어 모바일 장치에 대 한 고려 사항 (비디오)</td>
        <td><a href="https://channel9.msdn.com/Events/GDC/GDC-2015/Sustained-gaming-performance-in-multi-core-mobile-devices">다중 코어 모바일 장치의 지속적인 게임 성능</a></td>
    </tr>
    <tr>
        <td>Windows 10 desktop 게임 개발 (비디오)</td>
        <td><a href="https://channel9.msdn.com/Events/GDC/GDC-2015/PC-Games-for-Windows-10">Windows 10 용 PC 게임</a></td>
    </tr>
</table>
