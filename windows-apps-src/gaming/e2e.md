---
title: Windows10 게임 개발 가이드
description: UWP(유니버설 Windows 플랫폼) 게임 개발용 리소스 및 정보에 대한 종합 가이드입니다.
ms.assetid: 6061F498-96A8-44EF-9711-68AE5A1218C9
ms.date: 04/16/2018
ms.topic: article
keywords: windows 10, uwp, 게임, 게임 개발
ms.localizationpriority: medium
ms.openlocfilehash: 58044fba24450c397ee58b1034429f2af8d23ed6
ms.sourcegitcommit: b5c9c18e70625ab770946b8243f3465ee1013184
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/28/2018
ms.locfileid: "7980562"
---
# <a name="windows-10-game-development-guide"></a>Windows10 게임 개발 가이드


Windows10 게임 개발 가이드입니다.

이 가이드에서는 UWP(유니버설 Windows 플랫폼) 게임을 개발하는 데 필요한 리소스 및 정보의 종단 간 컬렉션을 제공합니다. 이 가이드의 영어(미국) 버전은 [PDF](http://download.microsoft.com/download/9/C/9/9C9D344F-611F-412E-BB01-259E5C76B17F/Windev_Game_Dev_Guide_Oct_2017.pdf) 형식으로 제공됩니다.

## <a name="introduction-to-game-development-for-the-universal-windows-platform-uwp"></a>UWP(유니버설 Windows 플랫폼)용 게임 개발 기술 소개


Windows10 게임을 만드는 경우 휴대폰, PC 및 Xbox One을 통해 전 세계 수백만 명의 플레이어와 접촉할 수 있습니다. Windows의 Xbox, Xbox Live, 디바이스 간 멀티플레이어, 놀라운 게임 커뮤니티, UWP(유니버설 Windows 플랫폼) 및 DirectX 12와 같은 강력한 새 기능을 지원하는 Windows10 게임은 모든 연령 및 장르의 플레이어를 열광시킵니다. 새 UWP(유니버설 Windows 플랫폼)는 휴대폰, PC 및 Xbox One용 공통 API를 사용하는 Windows10 디바이스에서, 각 디바이스 환경에 맞게 게임을 조정하는 도구 및 옵션과 함께 게임에 대한 호환성을 제공합니다.

이 가이드에서는 게임을 개발할 때 도움이 되는 정보 및 리소스의 종단 간 컬렉션을 제공합니다. 게임 개발의 단계에 따라 섹션이 구성되어 있으므로 필요할 때 정보를 찾아 볼 위치를 알 수 있습니다.

Windows 또는 Xbox에서 게임을 처음 개발해보는 경우에는 [시작](getting-started.md) 가이드를 참조해서 시작할 수 있습니다. 또한 [게임 개발 리소스](#game-development-resources) 섹션에서는 설명서, 프로그램 및 게임을 만들 때 도움이 되는 기타 리소스에 대한 대략적인 설문 조사도 제공하고 있습니다. 대신에 몇몇 UWP 코드를 살펴보는 것부터 시작하고 싶다면 [게임 샘플](#game-samples)을 참조하세요.

이 가이드는 추가 Windows10 게임 개발 리소스 및 자료가 사용 가능해질 때 업데이트됩니다.

## <a name="game-development-resources"></a>게임 개발 리소스

설명서에서 개발자 프로그램, 포럼, 블로그 및 샘플에 이르기까지 게임 개발 과정에 도움을 줄 수 있는 많은 리소스가 있습니다. 다음은 Windows10 게임 개발을 시작할 때 알아야 할 리소스입니다.

> [!Note]
> 일부 기능은 다양한 프로그램을 통해 관리됩니다. 이 가이드에서는 광범위한 리소스를 설명하므로, 사용하는 프로그램 또는 특정 개발 역할에 따라 일부 리소스에는 액세스할 수 없을 수도 있습니다. 예제는 developer.xboxlive.com, forums.xboxlive.com, xdi.xboxlive.com 또는 GDN(게임 개발자 네트워크)으로 확인되는 링크입니다. Microsoft와의 파트너 제휴 방법에 대한 내용은 [개발자 프로그램](#developer-programs)을 참조하세요.


### <a name="game-development-documentation"></a>게임 개발 설명서

이 가이드 전체에서 작업, 기술 및 게임 개발 단계로 구성된 관련 설명서에 대한 딥 링크를 찾을 수 있습니다. 어떤 것들을 사용할 수 있는지 전체적으로 파악할 수 있도록 Windows10 게임 개발에 대한 주요 설명서 포털이 다음에 나와 있습니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Windows 개발자 센터 주 포털</td>
        <td><a href="https://dev.windows.com">Windows 개발자 센터</a></td>
    </tr>
    <tr>
        <td>Windows 앱 개발</td>
        <td><a href="https://dev.windows.com/develop">Windows 앱 개발</a></td>
    </tr>
    <tr>
        <td>유니버설 Windows 플랫폼 앱 개발</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt244352">Windows10 앱 사용 방법 가이드</a></td>
    </tr>
    <tr>
        <td>UWP 게임 사용 방법 가이드</td>
        <td><a href="index.md">게임 및 DirectX</a> </td>
    </tr>
    <tr>
        <td>DirectX 참조 및 개요</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/ee663274">DirectX 그래픽 및 게임</a></td>
    </tr>
    <tr>
        <td>게임용 Azure</td>
        <td><a href="https://azure.microsoft.com/solutions/gaming/">Azure를 사용하여 게임 빌드 및 확장</a></td>
    </tr>
    <tr>
        <td>PlayFab</td>
        <td><a href="https://api.playfab.com/">라이브 게임을 위한 완벽한 백 엔드 솔루션</a></td>
    </tr>
    <tr>
        <td>Xbox One의 UWP</td>
        <td><a href="https://msdn.microsoft.com/windows/uwp/xbox-apps/index">Xbox One의 UWP 앱 빌드</a></td>
    </tr>
    <tr>
        <td>HoloLens의 UWP</td>
        <td><a href="https://developer.microsoft.com/windows/mixed-reality/development_overview">HoloLens의 UWP 앱 빌드</a></td>
    </tr>
    <tr>
        <td>Xbox Live 설명서</td>
        <td><a href="../xbox-live/index.md">Xbox Live 개발자 가이드</a></td>
    </tr>
    <tr>
        <td>Xbox One 개발 설명서(XGD)</td>
        <td><a href="https://developer.microsoft.com/games/xbox/partner/development-home">Xbox One 개발</a></td>
    </tr>
    <tr>
        <td>Xbox One 개발 백서(XGD)</td>
        <td><a href="https://developer.microsoft.com/games/xbox/partner/development-education-whitepapers">백서</a></td>
    </tr>
    <tr>
        <td>Mixer 대화형 설명서</td>
        <td><a href="https://dev.mixer.com/reference/interactive/index.html">게임에 대화형 작업 추가</a></td>
    </tr>        
</table>

### <a name="partner-center"></a>파트너 센터

[파트너 센터에서 개발자 계정을 등록](https://developer.microsoft.com/store/register) 하는 것은 Windows 게임을 게시 하기 위한 첫 번째 단계입니다. 개발자 계정을 사용하여 게임의 이름을 예약하고, 무료 또는 유료 게임을 모든 Windows 장치용 Microsoft Store에 제출할 수 있습니다. 개발자 계정을 사용하여 게임 및 게임 내 제품을 관리하고, 자세하게 분석하며, 전 세계 플레이어를 위해 멋진 환경을 만드는 서비스를 사용하도록 설정할 수 있습니다. 

또한 Microsoft는 Windows 게임을 개발하고 게시하는 데 도움이 되도록 여러 개발자 프로그램을 제공합니다. 파트너 센터 계정에 등록 하기 전에 적합 한 프로그램이 있는지 보는 것이 좋습니다. 자세한 내용은 [개발자 프로그램](#developer-programs)을 참조하세요.


### <a name="developer-programs"></a>개발자 프로그램

Microsoft는 Windows 게임을 개발하고 게시하는 데 도움이 되도록 여러 개발자 프로그램을 제공합니다. Xbox One 게임을 개발하고 게임에 Xbox Live 기능을 통합하시려면 개발자 프로그램에 참여하는 방안을 고려해 보세요. Microsoft Store에 게임을 게시 하려면 [파트너 센터](https://partner.microsoft.com/dashboard) 에서 개발자 계정을 생성 해야 합니다.

#### <a name="xbox-live-creators-program"></a>Xbox Live 크리에이터스 프로그램

Xbox Live 크리에이터스 프로그램을 통해 누구든지 Xbox Live를 타이틀과 통합하고 Xbox One 및 Windows 10에 게시할 수 있습니다. 간소화된 인증 프로세스 덕분에 표준 [Microsoft Store 정책](https://msdn.microsoft.com/library/windows/apps/dn764944.aspx)을 벗어난 개념 승인이 필요하지 않습니다.

전용 개발 키트 없이도 오직 소매 하드웨어를 사용하여 크리에이터스 프로그램에 게임을 배포, 디자인 및 게시할 수 있습니다. 시작하려면 Xbox One에서 [개발자 모드 정품 인증 앱](https://docs.microsoft.com/windows/uwp/xbox-apps/devkit-activation)을 다운로드합니다.

훨씬 다양한 Xbox Live 기능을 사용하고 전용 마케팅 및 개발 지원을 이용하고 메인 Xbox One 스토어에 추천 앱으로 등록되고 싶다면 [ID@Xbox](http://www.xbox.com/Developers/id) 프로그램에 지원하세요.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Xbox Live 크리에이터스 프로그램</td>
        <td><a href="https://developer.microsoft.com/games/xbox/xboxlive/creator">Xbox Live 크리에이터스 프로그램에 대해 자세히 알아보기</a></td>
    </tr>
</table>

#### <a name="idxbox"></a>ID@Xbox

ID@Xbox 프로그램을 사용하면 정규 게임 개발자가 Windows 및 Xbox One에 게임을 직접 게시할 수 있습니다. Xbox One용 개발 또는 Windows10 게임에 게이머 점수, 도전 과제 및 순위표와 같은 Xbox Live 기능을 추가하려는 경우 ID@Xbox에 등록합니다. ID@Xbox 개발자로 등록하여 창의력을 발휘하고 성공 가능성을 극대화하는 데 필요한 도구와 지원을 받으시기 바랍니다. 에 적용 하는 것이 좋습니다 ID@Xbox 파트너 센터에서 개발자 계정에 등록 하기 전에 먼저 합니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>ID@Xbox 개발자 프로그램</td>
        <td><a href="http://go.microsoft.com/fwlink/p/?LinkID=526271">Xbox One용 단독 개발자 프로그램</a></td>
    </tr>
    <tr>
        <td>ID@Xbox 소비자 사이트</td>
        <td><a href="http://www.idatxbox.com/">ID@Xbox</a></td>
    </tr>
</table>

#### <a name="xbox-tools-and-middleware"></a>Xbox 도구 및 미들웨어

Xbox 도구와 미들웨어 프로그램에서는 게임 도구와 미들웨어의 전문 개발자에게 Xbox 개발자 키트 라이선스를 허여합니다. 이 프로그램에 참여한 개발자는 자신의 Xbox XDK 기술을 사용이 허가된 다른 Xbox 개발자와 공유하고 배포할 수 있습니다.

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

Windows10 게임 기능을 이해하고 게임 개발을 빠르게 시작하는 데 도움이 되는 많은 Windows10 게임 및 앱 샘플이 있습니다. 더 많은 샘플이 정기적으로 개발 및 게시되므로 가끔 샘플 포털을 다시 확인하여 새 소식을 확인하세요. 또한 GitHub 리포지토리가 변경 및 추가 사항에 대한 알림을 받는지 [확인](https://help.github.com/articles/watching-repositories/)할 수 있습니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>유니버설 Windows 플랫폼 앱 샘플</td>
        <td><a href="https://github.com/Microsoft/Windows-universal-samples">Windows-universal-samples</a></td>
    </tr>
    <tr>
        <td>Direct3D 12 그래픽 샘플</td>
        <td><a href="https://github.com/Microsoft/DirectX-Graphics-Samples">DirectX-Graphics-Samples</a></td>
    </tr>
    <tr>
        <td>Direct3D 11 그래픽 샘플</td>
        <td><a href="https://github.com/walbourn/directx-sdk-samples">directx-sdk-samples</a></td>
    </tr>
    <tr>
        <td>Direct3D 11 1인칭 게임 샘플</td>
        <td><a href="tutorial--create-your-first-uwp-directx-game.md">DirectX로 간단한 UWP 게임 만들기</a></td>
    </tr>
    <tr>
        <td>Direct2D 사용자 지정 이미지 효과 샘플</td>
        <td><a href="http://go.microsoft.com/fwlink/p/?LinkId=620531">D2DCustomEffects</a></td>
    </tr>
    <tr>
        <td>Direct2D 그라데이션 메시 샘플</td>
        <td><a href="http://go.microsoft.com/fwlink/p/?LinkId=620532">D2DGradientMesh</a></td>
    </tr>
    <tr>
        <td>Direct2D 사진 보정 샘플</td>
        <td><a href="http://go.microsoft.com/fwlink/p/?LinkId=620533">D2DPhotoAdjustment</a></td>
    </tr>
    <tr>
        <td>Xbox 고급 기술 그룹 공개 샘플</td>
        <td><a href="https://github.com/Microsoft/Xbox-ATG-Samples">Xbox-ATG-Samples</a></td>
    </tr>
    <tr>
        <td>Xbox Live 샘플</td>
        <td><a href="https://github.com/Microsoft/xbox-live-samples">xbox-live-samples</a></td>
    </tr>
    <tr>
        <td>Xbox One 게임 샘플(XGD)</td>
        <td><a href="https://developer.microsoft.com/games/xbox/partner/development-education-samples">샘플</a></td>
    </tr>
    <tr>
        <td>Windows 게임 샘플(MSDN 코드 갤러리)</td>
        <td><a href="https://code.msdn.microsoft.com/windowsapps/site/search?f%5B0%5D.Type=SearchText&f%5B0%5D.Value=game&f%5B1%5D.Type=Contributors&f%5B1%5D.Value=Microsoft&f%5B1%5D.Text=Microsoft">Microsoft Store 게임 샘플</a></td>
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
        <td>MonoGame 2D UWP 게임 샘플</td>
        <td><a href="../get-started/get-started-tutorial-game-mg2d.md">MonoGame 2D로 UWP 게임 만들기</a></td>
    </tr>      
</table>


### <a name="developer-forums"></a>개발자 포럼

개발자 포럼은 게임 개발 질문을 묻고 대답하며 게임 개발 커뮤니티와 연결하는 데 최적의 장입니다. 포럼은 또한 과거에 개발자가 직면해 해결했던 어려운 문제에 대한 기존 대답을 찾는 환상적인 리소스가 될 수 있습니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>게시 앱 및 게임 개발자 포럼</td>
        <td><a href="https://social.msdn.microsoft.com/Forums/en-us/home?category=windowsapps">게시 및 앱에서 광고</a></td>
    </tr>
    <tr>
        <td>UWP 앱 개발자 포럼</td>
        <td><a href="https://social.msdn.microsoft.com/Forums/en-us/home?forum=wpdevelop">유니버설 Windows 플랫폼 앱 개발</a></td>
    </tr>
    <tr>
        <td>데스크톱 응용 프로그램 개발자 포럼</td>
        <td><a href="https://social.msdn.microsoft.com/Forums/en-us/home?category=windowsdesktopdev">Windows 데스크톱 응용 프로그램 포럼</a></td>
    </tr>
    <tr>
        <td>DirectX Microsoft Store 게임(보관된 포럼 게시물)</td>
        <td><a href="https://social.msdn.microsoft.com/Forums/vstudio/home?forum=wingameswithdirectx">DirectX로 Microsoft Store 게임 빌드(보관)</a></td>
    </tr>
    <tr>
        <td>Windows10 관리 파트너 개발자 포럼</td>
        <td><a href="http://aka.ms/win10devforums">XBOX 개발자 포럼: Windows10</a></td>
    </tr>
    <tr>
        <td>DirectX 포럼</td>
        <td><a href="http://forums.directxtech.com/index.php">DirectX 12 포럼</a></td>
    </tr>
    <tr>
        <td>Azure 플랫폼 포럼</td>
        <td><a href="https://social.msdn.microsoft.com/Forums/en-us/home?category=windowsazureplatform">Azure 포럼</a></td>
    </tr>
    <tr>
        <td>Xbox Live 포럼</td>
        <td><a href="https://social.msdn.microsoft.com/Forums/en-US/home?forum=xboxlivedev">Xbox Live 개발 포럼</a></td>
    </tr>
    <tr>
        <td>PlayFab 포럼</td>
        <td><a href="https://community.playfab.com/index.html">PlayFab 포럼</a></td>
    </tr>
</table>


### <a name="developer-blogs"></a>개발자 블로그

개발자 블로그는 게임 개발에 대한 최신 정보의 또 다른 좋은 리소스입니다. 새로운 기능, 구현 세부 정보, 모범 사례, 아키텍처 배경 등에 대한 게시물을 찾을 수 있습니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Windows용 앱 빌드 블로그</td>
        <td><a href="http://blogs.windows.com/buildingapps/">Windows용 앱 빌드 블로그</a></td>
    </tr>
    <tr>
        <td>Windows10(블로그 게시물)</td>
        <td><a href="http://blogs.windows.com/blog/tag/windows-10/">Windows 10의 게시물</a></td>
    </tr>
    <tr>
        <td>Visual Studio 엔지니어링 팀 블로그</td>
        <td><a href="http://blogs.msdn.com/b/visualstudio/">Visual Studio 블로그</a></td>
    </tr>
    <tr>
        <td>Visual Studio 개발자 도구 블로그</td>
        <td><a href="http://blogs.msdn.com/b/developer-tools/">개발자 도구 블로그</a></td>
    </tr>
    <tr>
        <td>Somasegar의 개발자 도구 블로그</td>
        <td><a href="http://blogs.msdn.com/b/somasegar/">Somasegar의 블로그</a></td>
    </tr>
    <tr>
        <td>DirectX 개발자 블로그</td>
        <td><a href="http://blogs.msdn.com/b/directx">DirectX 개발자 블로그</a></td>
    </tr>
    <tr>
        <td>DirectX 12 소개(블로그 게시물)</td>
        <td><a href="http://blogs.msdn.com/b/directx/archive/2014/03/20/directx-12.aspx">DirectX 12입니다.</a></td>
    </tr>
    <tr>
        <td>Visual C++ 도구 팀 블로그</td>
        <td><a href="http://blogs.msdn.com/b/vcblog/">Visual C++ 팀 블로그</a></td>
    </tr>
    <tr>
        <td>PIX 팀 블로그</td>
        <td><a href="https://blogs.msdn.microsoft.com/pix/">Windows 및 Xbox에서 DirectX 12 게임을 위한 성능 조정 및 디버깅</a></td>
    </tr>
    <tr>
        <td>유니버설 Windows 앱 배포 팀 블로그</td>
        <td><a href="https://blogs.msdn.microsoft.com/appinstaller/">UWP 앱 빌드 및 배포 팀 블로그</a></td>
    </tr>
</table>
 

## <a name="concept-and-planning"></a>개념 및 계획


개념 및 계획 단계에서는 게임이 어떤 양상으로 나타나고 게임에 활력을 불어넣기 위해 어떤 기술과 도구를 사용할지 결정합니다.

### <a name="overview-of-game-development-technologies"></a>게임 개발 기술 개요

UWP 게임 개발을 시작할 때 그래픽, 입력, 오디오, 네트워킹, 유틸리티 및 라이브러리에 대한 사용 가능한 여러 옵션이 있습니다.

게임에 사용할 모든 기술을 이미 결정했다면 훌륭합니다. 그렇지 않다면 [UWP 앱용 게임 기술](game-development-platform-guide.md) 가이드는 사용 가능한 많은 기술에 대한 좋은 개요이며 옵션 및 옵션이 어떻게 함께 작동하는지를 이해하기 위해 읽는 것이 좋습니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>UWP 게임 기술의 설문 조사</td>
        <td><a href="game-development-platform-guide.md">UWP 앱용 게임 기술</a></td>
    </tr>
</table>
 

이러한 세 가지 GDC 2015 동영상에서는 Windows 10의 게임 개발 및 Windows10 게임 환경의 개요를 제공합니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Windows10 게임 개발 개요(동영상)</td>
        <td><a href="http://channel9.msdn.com/Events/GDC/GDC-2015/Developing-Games-for-Windows-10">Windows10 게임 개발</a></td>
    </tr>
    <tr>
        <td>Windows10 게임 환경(동영상)</td>
        <td><a href="http://channel9.msdn.com/Events/GDC/GDC-2015/Gaming-Consumer-Experience-on-Windows-10">Windows 10의 게임 소비자 환경</a></td>
    </tr>
    <tr>
        <td>Microsoft 에코시스템에서의 게임(동영상)</td>
        <td><a href="http://channel9.msdn.com/Events/GDC/GDC-2015/The-Future-of-Gaming-Across-the-Microsoft-Ecosystem">Microsoft 에코시스템에서의 게임의 미래</a></td>
    </tr>
</table>

### <a name="game-planning"></a>게임 계획

게임을 계획할 때 고려해야 할 높은 수준의 개념 및 계획 항목이 몇 가지 있습니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>게임에 접근성 구현</td>
        <td><a href="https://msdn.microsoft.com/windows/uwp/gaming/accessibility-for-games">게임의 접근성</a></td>
    </tr>
    <tr>
        <td>클라우드를 사용한 게임 빌드</td>
        <td><a href="https://msdn.microsoft.com/windows/uwp/gaming/cloud-for-games">게임의 클라우드</a></td>
    </tr>
    <tr>
        <td>게임에서 수익 창출</td>
        <td><a href="https://msdn.microsoft.com/windows/uwp/gaming/monetization-for-games">게임의 수익 창출</a></td>
    </tr>
</table>



### <a name="choosing-your-graphics-technology-and-programming-language"></a>그래픽 기술 및 프로그래밍 언어 선택

Windows10 게임에서 사용할 수 있는 프로그래밍 언어 및 그래픽 기술이 몇 가지 있습니다. 선택하는 경로와 방향은 개발하고 있는 게임 유형, 개발 회사의 경험 및 선호도, 게임의 고유한 기능 요구 사항에 따라 다릅니다. C#, C++ 또는 JavaScript를 사용하나요? DirectX, XAML 또는 HTML5를 사용하나요?

#### <a name="directx"></a>DirectX

Microsoft DirectX는 고성능 2D 및 3D 그래픽과 멀티미디어를 위한 최고의 선택입니다.

DirectX 12는 이전 버전보다 훨씬 빠르고 효율적입니다. Direct3D 12에서는 풍부한 장면, 더 많은 개체, 더 복잡한 효과를 활용할 수 있고 Windows 10 PC 및 Xbox One에서 최신 GPU 하드웨어를 완벽하게 활용할 수 있습니다.

Direct3D 11의 친숙한 그래픽 파이프라인을 사용하려는 경우에는 여전히 Direct3D 11.3에 추가된 새로운 렌더링 및 최적화 기능의 이점을 누릴 수 있습니다. Win32의 루트로 신뢰할 수 있는 데스크톱 Windows API 개발자인 경우 Windows 10에도 해당 옵션이 있습니다.

DirectX의 폭넓은 기능과 심층적인 플랫폼 통합은 가장 까다로운 게임에 필요한 기능과 성능을 제공합니다.

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
        <td>자습서: UWP DirectX 게임을 개발하는 방법</td>
        <td><a href="tutorial--create-your-first-uwp-directx-game.md">DirectX로 간단한 UWP 게임 만들기</a></td>
    </tr>
    <tr>
        <td>DirectX 개요 및 참조</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/ee663274">DirectX 그래픽 및 게임</a></td>
    </tr>
    <tr>
        <td>Direct3D 12 프로그래밍 가이드 및 참조</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/dn903821">Direct3D 12 그래픽</a></td>
    </tr>
    <tr>
        <td>그래픽 및 DirectX 12 개발 동영상(YouTube 채널)</td>
        <td><a href="https://www.youtube.com/channel/UCiaX2B8XiXR70jaN7NK-FpA">Microsoft DirectX 12 및 그래픽 교육</a></td>
    </tr>
</table>
 

#### <a name="xaml"></a>XAML

XAML은 애니메이션, 스토리보드, 데이터 바인딩, 확장 가능한 벡터 기반 그래픽, 동적 크기 재조정 및 장면 그래프와 같은 편리한 기능이 있는 사용하기 쉬운 선언적 UI 언어입니다. XAML은 게임 UI, 메뉴, 스프라이트 및 2D 그래픽에서 훌륭히 작동합니다. UI 레이아웃을 쉽게 조정할 수 있도록 XAML은 Expression Blend, Microsoft Visual Studio 등의 디자인 및 개발 도구와 호환됩니다. XAML은 주로 C#과 함께 사용되지만, C++를 더욱 선호하거나 게임이 고성능 CPU 사양을 요구하는 경우 C++와 함께 사용해도 좋습니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>XAML 플랫폼 개요</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt228259">XAML 플랫폼</a></td>
    </tr>
    <tr>
        <td>XAML UI 및 컨트롤</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt228348">컨트롤, 레이아웃 및 텍스트</a></td>
    </tr>
</table>
 

#### <a name="html-5"></a>HTML 5

HTML(HyperText Markup Language)은 웹 페이지, 앱 및 리치 클라이언트에서 사용하는 공용 UI 생성 언어입니다. Windows 게임은 HTML의 익숙한 기능, 유니버설 Windows 플랫폼에 대한 액세스, AppCache, 웹 작업자, 캔버스, 끌어서 놓기, 비동기 프로그래밍 및 SVG와 같은 최신 웹 기능에 대한 지원 등 모든 기능을 갖춘 표시 계층으로 HTML5를 사용할 수 있습니다. HTML 렌더링은 백그라운드에서 DirectX 하드웨어 가속 기능을 이용하므로 추가 코드를 작성하지 않고도 계속 DirectX의 성능 이점을 활용할 수 있습니다. HTML5는 웹 개발 및 웹 게임 포팅에 능숙하거나 다른 선택보다는 접근하기에 쉬울 수 있는 언어 및 그래픽 계층을 사용하려는 경우에 적합한 선택입니다. HTML5는 JavaScript와 함께 사용할 수 있지만, C# 또는 C++/CX로 만든 구성 요소로 호출할 수도 있습니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>HTML5 및 문서 개체 모델 정보</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/br212882.aspx">HTML 및 DOM 참조</a></td>
    </tr>
    <tr>
        <td>HTML5 W3C 권장 사항</td>
        <td><a href="http://go.microsoft.com/fwlink/p/?linkid=221374">HTML5</a></td>
    </tr>
</table>
 

#### <a name="combining-presentation-technologies"></a>프레젠테이션 기술 결합

Microsoft DXGI(DirectX Graphic Infrastructure)는 여러 그래픽 기술 간에 상호 운용성 및 호환성을 제공합니다. 고성능 그래픽을 위해 메뉴 및 기타 단순한 UI에는 XAML을 사용하고 복잡한 2D 및 3D 장면의 렌더링에는 DirectX를 사용하는 등 XAML과 DirectX를 조합할 수 있습니다. 또한 DXGI는 Direct2D, Direct3D, DirectWrite, DirectCompute, Microsoft 미디어 파운데이션 간에 호환성을 제공합니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>DirectX Graphic Infrastructure 프로그래밍 가이드 및 참조</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/hh404534">DXGI</a></td>
    </tr>
    <tr>
        <td>DirectX 및 XAML 결합</td>
        <td><a href="directx-and-xaml-interop.md">DirectX 및 XAML interop</a></td>
    </tr>
</table>
 

#### <a name="c"></a>C++

C++/CX는 속도, 호환성 및 플랫폼 액세스의 강력한 조합을 제공하는 오버헤드가 낮은 고성능 언어입니다. C++/CX를 사용하면 DirectX 및 Xbox Live를 비롯하여 Windows 10의 모든 멋진 게임 기능을 쉽게 활용할 수 있습니다. 또한 기존의 C++ 코드 및 라이브러리도 재사용할 수 있습니다. C++/CX는 가비지 수집의 오버헤드를 발생시키지 않는 빠른 네이티브 코드를 생성하므로 게임이 탁월한 성능을 발휘하고 전력 소모가 줄어들어 배터리 수명이 길어질 수 있습니다. DirectX 또는 XAML과 함께 C++/CX를 사용하거나 둘의 조합을 사용하는 게임을 만들어 보세요.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>C++/CX 참조 및 개요</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/hh699871.aspx">Visual C++ 언어 참조(C++/CX)</a></td>
    </tr>
    <tr>
        <td>Visual C++ 프로그래밍 가이드 및 참조</td>
        <td><a href="https://docs.microsoft.com/cpp/visual-cpp-in-visual-studio">Visual Studio 2017의 Visual C++</a></td>
    </tr>
</table>
 

#### <a name="c"></a>C#

C#("C 샤프"라고 발음)는 혁신적인 최신 언어로, 단순하고 강력하며 형식이 안전하고 개체 지향적입니다. C#를 사용하면 C 스타일 언어의 익숙함과 표현력은 유지하면서 신속하게 개발할 수 있습니다. C#는 사용하기 쉽지만, 다형성, 대리자, 람다, 종결, 반복기 메서드, 공변성(Covariance) 및 LINQ(Language-Integrated Query) 식과 같은 여러 고급 언어 기능을 제공합니다. XAML을 사용할 계획이거나 게임 개발을 서두르려는 경우 또는 이전에 C# 사용 경험이 있는 경우 C#를 선택하는 것이 좋습니다. C#은 대개 XAML과 함께 사용하므로 DirectX를 사용하려는 경우에는 C++를 선택하거나, DirectX를 조작하는 C++ 구성 요소로 게임의 일부를 작성하세요. 또는 C# 및 C++용 직접 실행 모드 Direct2D 그래픽 라이브러리인 [Win2D](https://github.com/Microsoft/Win2D) 사용을 고려합니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>C# 프로그래밍 가이드 및 참조</td>
        <td><a href="https://msdn.microsoft.com/library/kx37x362.aspx">C# 언어 참조</a></td>
    </tr>
</table>
 

#### <a name="javascript"></a>JavaScript

JavaScript는 최신 웹 및 리치 클라이언트 응용 프로그램에서 널리 사용되는 동적 스크립트 언어입니다.

Windows JavaScript 앱은 개체 지향 JavaScript 클래스의 메서드 및 속성으로 쉽고 직관적인 방식으로 유니버설 Windows 플랫폼의 강력한 기능에 액세스할 수 있습니다. 기존에 웹 개발 환경을 사용하고 있거나 JavaScript에 이미 익숙하거나 HTML5, CSS, WinJS 또는 JavaScript 라이브러리를 사용하려는 경우 JavaScript가 게임에 가장 적합한 선택입니다. DirectX 또는 XAML을 사용하려는 경우에는 JavaScript 대신에 C# 또는 C++/CX를 선택하세요.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>JavaScript 및 Windows 런타임 참조</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/jj613794">JavaScript 참조</a></td>
    </tr>
</table>


#### <a name="use-windows-runtime-components-to-combine-languages"></a>Windows 런타임 구성 요소를 사용하여 언어 조합

유니버설 Windows 플랫폼에서는 다른 언어로 작성된 구성 요소를 조합하기가 쉽습니다. C++, C# 또는 Visual Basic으로 Windows 런타임 구성 요소를 생성한 후 JavaScript, C#, C++ 또는 Visual Basic에서 해당 요소로 호출합니다. 이것은 선택한 언어로 게임 부분을 프로그래밍하는 훌륭한 방법입니다. 또한 구성 요소를 통해 특정 언어에서만 사용 가능한 외부 라이브러리는 물론 이미 작성해둔 레거시 코드를 사용할 수도 있습니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Windows 런타임 구성 요소를 만드는 방법</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/winrt-components/creating-windows-runtime-components-in-cpp">Windows 런타임 구성 요소 만들기</a></td>
    </tr>
</table>


### <a name="which-version-of-directx-should-your-game-use"></a>게임에 어떤 버전의 DirectX를 사용해야 하나요?

DirectX 게임을 선택 하는 경우 어떤 버전을 사용할지 결정 해야 합니다: Microsoft Direct3D12 또는 Microsoft Direct3D11 합니다.

DirectX 12는 이전 버전보다 훨씬 빠르고 효율적입니다. Direct3D 12에서는 풍부한 장면, 더 많은 개체, 더 복잡한 효과를 활용할 수 있고 Windows 10 PC 및 Xbox One에서 최신 GPU 하드웨어를 완벽하게 활용할 수 있습니다. Direct3D 12는 매우 낮은 수준에서 작동하므로 전문가 그래픽 개발 팀이나 숙련된 DirectX 11 개발 팀에게 그래픽 최적화를 최대화하는 데 필요한 모든 제어를 제공할 수 있습니다.

Direct3D 11.3은 익숙한 Direct3D 프로그래밍 모델을 사용하며 GPU 렌더링에 관련된 더 많은 복잡성을 자동으로 처리하는 낮은 수준의 그래픽 API입니다. Windows10 및 Xbox One에서도 지원됩니다. Direct3D 11에서 작성된 기존 엔진이 있고 Direct3D 12로 전환할 준비가 되지 않은 경우 12에서 Direct3D 11을 사용하여 일부 성능을 향상시킬 수 있습니다. 버전 11.3+에는 Direct3D 12에서도 사용되는 새로운 렌더링 및 최적화 기능이 포함되어 있습니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Direct3D12 또는 Direct3D11 선택</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/dn899228">Direct3D12 란 무엇 인가요?</a></td>
    </tr>
    <tr>
        <td>Direct3D11 개요</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/ff476080">Direct3D 11 그래픽</a></td>
    </tr>
    <tr>
        <td>12에서 Direct3D 11 개요</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/dn913195">12에서 Direct3D 11</a></td>
    </tr>
</table>


### <a name="bridges-game-engines-and-middleware"></a>브리지, 게임 엔진 및 미들웨어

게임의 요구 사항에 따라 브리지, 게임 엔진 또는 미들웨어를 사용하면 개발 및 테스트의 시간과 리소스를 절약할 수 있습니다. 다음은 브리지, 게임 엔진 및 미들웨어에 대한 몇 가지 개요 및 리소스입니다.

#### <a name="universal-windows-platform-bridges"></a>유니버설 Windows 플랫폼 브리지

유니버설 Windows 플랫폼 브리지는 기존 앱이나 게임을 UWP로 가져오는 기술입니다. 브리지는 UWP 게임 개발을 빠르게 시작할 수 있는 좋은 방법입니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>UWP 브리지</td>
        <td><a href="https://dev.windows.com/bridges/">Windows로 코드 가져오기</a></td>
    </tr>
    <tr>
        <td>iOS용 Windows 브리지</td>
        <td><a href="https://dev.windows.com/bridges/ios">Windows에 iOS 앱 가져오기</a></td>
    </tr>
    <tr>
        <td>데스크톱 응용 프로그램용 Windows 브리지(.NET 및 Win32)</td>
        <td><a href="https://developer.microsoft.com/windows/bridges/desktop">데스크톱 응용 프로그램을 UWP 앱으로 변환</a></td>
    </tr>
</table>

#### <a name="playfab"></a>PlayFab

이제 Microsoft 가족이 된 PlayFab는 라이브 게임을 위한 완벽한 백 엔드 플랫폼이며, 독립 스튜디오에서 강력하게 시작할 수 있는 방법입니다. 게임 서비스, 실시간 분석 및 LiveOps를 통해 비용은 줄이고 수익, 참여도, 고객 유지율은 높이세요.

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
        <td>시작하기</td>
        <td><a href="https://api.playfab.com/docs/general-getting-started">일반적인 시작 가이드</a></td>
    </tr>
    <tr>
        <td>동영상 자습서 시리즈</td>
        <td><a href="https://www.youtube.com/watch?v=fGNpiqVi5xU&list=PLHCfyL7JpoPbLpA_oh_T5PKrfzPgCpPT5">PlayFab의 핵심 시스템에 대한 데모 동영상 시리즈</a></td>
    </tr>
    <tr>
        <td>레시피</td>
        <td><a href="https://api.playfab.com/docs/tutorials/recipes-index">인기 게임 메커니즘 및 디자인 패턴 샘플</a></td>
    </tr>
    <tr>
        <td>플랫폼</td>
        <td><a href="https://api.playfab.com/platforms">다양한 플랫폼과 게임 엔진에 대한 구체적인 설명서</a></td>
    </tr>
    <tr>
        <td>GitHub 보고서</td>
        <td><a href="https://github.com/PlayFab">Android, iOS, Windows, Unity, Unreal 같은 다양한 플랫폼에 대한 스크립트와 SDK를 다운로드합니다.</a></td>
    </tr>
    <tr>
        <td>API 설명서</td>
        <td><a href="https://api.playfab.com/documentation/">REST 스타일의 웹 API를 통해 직접 PlayFab 서비스에 액세스</a></td>
    </tr>
    <tr>
        <td>포럼</td>
        <td><a href="https://community.playfab.com/index.html">PlayFab 포럼</a></td>
    </tr>
</table>
 

#### <a name="unity"></a>Unity

Unity는 아름답고 몰입도가 높은 2D, 3D, VR 및 AR 게임/앱을 개발할 수 있는 플랫폼을 제공합니다. 또한 신속하게 창의적인 비전을 실현할 수 있도록 지원하고 거의 모든 미디어나 디바이스에 개발한 콘텐츠를 제공합니다.

Unity는 Unity 5.4부터 Direct3D 12 개발을 지원합니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Unity 게임 엔진</td>
        <td><a href="http://unity3d.com/">Unity - 게임 엔진</a></td>
    </tr>
    <tr>
        <td>Unity 다운로드</td>
        <td><a href="http://unity3d.com/get-unity">Unity 다운로드</a></td>
    </tr>
    <tr>
        <td>Windows용 Unity 설명서</td>
        <td><a href="http://docs.unity3d.com/Manual/Windows.html">Unity 설명서/Windows</a></td>
    </tr>
    <tr>
        <td>PlayFab를 사용하여 LiveOps 추가</td>
        <td><a href="https://api.playfab.com/docs/getting-started/unity-getting-started">시작하기 - Unity 게임에서 첫 PlayFab API 호출</a></td>
    </tr>
    <tr>
        <td>Mixer Interactive를 사용하여 게임에 대화형 작업을 추가하는 방법</td>
        <td><a href="https://github.com/mixer/interactive-unity-plugin/wiki/Getting-started">시작 가이드</a></td>
    </tr>
    <tr>
        <td>Unity용 Mixer SDK</td>
        <td><a href="https://www.assetstore.unity3d.com/en/#!/content/88585">Mixer Unity 플러그 인</a></td>
    </tr>
    <tr>
        <td>Unity용 Mixer SDK 참조 설명서</td>
        <td><a href="https://dev.mixer.com/reference/interactive/csharp/index.html">Mixer Unity 플러그 인용 API 참조</a></td>
    </tr>
    <tr>
        <td>Unity 게임을 Microsoft Store에 게시</td>
        <td><a href="https://unity3d.com/partners/microsoft/porting-guides">포팅 가이드</a></td>
    </tr>
    <tr>
        <td>.NET API와 관련된 어셈블리 참조 누락 문제 해결</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/gaming/missing-dot-net-apis-in-unity-and-uwp">Unity 및 UWP에서 누락된 .NET API</a></td>
    </tr>
    <tr>
        <td>Unity 게임을 유니버설 Windows 플랫폼 앱으로 게시(동영상)</td>
        <td><a href="https://channel9.msdn.com/Blogs/One-Dev-Minute/How-to-publish-your-Unity-game-as-a-UWP-app">Unity 게임을 UWP 앱으로 게시하는 방법</a></td>
    </tr>
    <tr>
        <td>Unity로 Windows 게임 및 앱 만들기(동영상)</td>
        <td><a href="https://channel9.msdn.com/Blogs/One-Dev-Minute/Making-games-and-apps-with-Unity">Unity로 Windows 게임 및 앱 만들기</a></td>
    </tr>
    <tr>
        <td>Visual Studio를 사용한 Unity 게임 개발(동영상 시리즈)</td>
        <td><a href="http://go.microsoft.com/fwlink/?LinkId=722359">Visual Studio 2015에서 Unity 사용</a></td>
    </tr>
</table>
 

#### <a name="havok"></a>Havok

Havok의 모듈식 도구 및 기술 모음은 게임 작성자가 새로운 수준의 조작 및 몰입도에 도달하도록 지원합니다. Havok은 매우 현실적인 물리학, 조작 시뮬레이션 및 멋진 동영상을 사용할 수 있도록 해줍니다. 버전 2015.1 이상은 x86, 64비트, ARM에서 Visual Studio 2015에 UWP를 지원합니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Havok 웹 사이트</td>
        <td><a href="http://www.havok.com/">Havok</a></td>
    </tr>
    <tr>
        <td>Havok 도구 제품군</td>
        <td><a href="http://www.havok.com/products/">Havok 제품 개요</a></td>
    </tr>
    <tr>
        <td>Havok 지원 포럼</td>
        <td><a href="http://support.havok.com">Havok</a></td>
    </tr>
</table>
 

#### <a name="monogame"></a>MonoGame

MonoGame은 오픈 소스로 원래 Microsoft의 XNA Framework 4.0을 기반으로 하는 플랫폼 간 게임 개발 프레임워크입니다. Monogame는 현재 Windows, Windows Phone 및 Xbox뿐 아니라 Linux, macOS, iOS, Android 및 다른 여러 플랫폼을 지원합니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>MonoGame</td>
        <td><a href="http://www.monogame.net">MonoGame 웹 사이트</a></td>
    </tr>
    <tr>
        <td>MonoGame 설명서</td>
        <td><a href="http://www.monogame.net/documentation/">MonoGame 설명서(최신)</a></td>
    </tr>
    <tr>
        <td>Monogame 다운로드</td>
        <td>MonoGame 웹 사이트에서 <a href="http://www.monogame.net/downloads/">릴리스, 개발 빌드 및 소스 코드를 다운로드</a>하거나 <a href="https://www.nuget.org/profiles/MonoGame">NuGet을 통해 최신 릴리스를 다운로드</a>합니다.
    </tr>
    <tr>
        <td>MonoGame 2D UWP 게임 샘플</td>
        <td><a href="../get-started/get-started-tutorial-game-mg2d.md">MonoGame 2D로 UWP 게임 만들기</a></td>
    </tr>    
</table>


#### <a name="cocos2d"></a>Cocos2d

Cocos2d-x는 플랫폼 간 오픈 소스 게임 개발 엔진이며 UWP 게임 빌드를 지원하는 도구 제품군입니다. 버전 3부터는 3D 기능도 추가됩니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Cocos2d-x</td>
        <td><a href="http://www.cocos2d-x.org/">Cocos2d-x란 무엇입니까?</a></td>
    </tr>
    <tr>
        <td>Cocos2d-x 프로그래머 가이드</td>
        <td><a href="http://www.cocos2d-x.org/programmersguide/">Cocos2d-x 프로그래머 가이드</a></td>
    </tr>
    <tr>
        <td>Windows 10의 Cocos2d-x(블로그 게시물)</td>
        <td><a href="https://blogs.windows.com/buildingapps/2015/06/15/running-cocos2d-x-on-windows-10/">Windows 10에서 Cocos2d-x 실행</a></td>
    </tr>
    <tr>
        <td>PlayFab를 사용하여 LiveOps 추가</td>
        <td><a href="https://api.playfab.com/docs/getting-started/cocos2d-x-getting-started-guide">시작하기 - Cocos2d 게임에서 첫 PlayFab API 호출</a></td>
    </tr>
</table>


#### <a name="unreal-engine"></a>Unreal Engine

Unreal Engine 4는 모든 유형의 게임과 개발자를 위한 완벽한 게임 개발 도구 모음입니다. Unreal Engine은 가장 까다로운 콘솔 및 PC 게임에 이르기까지 전 세계의 다양한 게임 개발자가 사용합니다.

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
        <td>PlayFab - C++을 사용하여 LiveOps 추가</td>
        <td><a href="https://api.playfab.com/docs/getting-started/unreal-cpp-getting-started">시작하기 - Unreal 게임에서 첫 PlayFab API 호출</a></td>
    </tr>
    <tr>
        <td>Blueprints를 사용하여 LiveOps 추가</td>
        <td><a href="https://api.playfab.com/docs/getting-started/unreal-blueprints-getting-started">시작하기 - Unreal 게임에서 첫 PlayFab API 호출</a></td>
    </tr>
</table>

#### <a name="babylonjs"></a>BabylonJS

BabylonJS는 HTML5, WebGL, WebVR 및 웹 오디오를 사용하여 3D 게임을 빌드하기 위한 완벽한 JavaScript 프레임워크입니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>BabylonJS</td>
        <td><a href="http://www.babylonjs.com/">BabylonJS</a></td>
    </tr>
    <tr>
        <td>HTML5 및 BabylonJS를 사용한 WebGL 3D(비디오 시리즈)</td>
        <td><a href="https://channel9.msdn.com/Series/Introduction-to-WebGL-3D-with-HTML5-and-Babylonjs/01">WebGL 3D 및 BabylonJS 학습</a></td>
    </tr>
    <tr>
        <td>BabylonJS를 사용하여 플랫폼 간 WebGL 게임 빌드</td>
        <td><a href="https://www.smashingmagazine.com/2016/07/babylon-js-building-sponza-a-cross-platform-webgl-game/">BabylonJS를 사용하여 플랫폼 간 게임 개발</a></td>
    </tr>    
</table>

### <a name="porting-your-game"></a>게임 포팅

기존 게임이 있는 경우 게임을 UWP에 신속하게 가져오는 데 도움이 되는 많은 리소스 및 가이드가 있습니다. 포팅 작업을 신속하게 시작하려면 [유니버설 Windows 플랫폼 브리지](#universal-windows-platform-bridges) 사용 또한 고려할 수 있습니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Windows8 앱을 유니버설 Windows 플랫폼 앱으로 포팅</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt238322">Windows 런타임 8.x에서 UWP로 이동</a></td>
    </tr>
    <tr>
        <td>Windows8 앱을 유니버설 Windows 플랫폼 앱으로 포팅(동영상)</td>
        <td><a href="https://channel9.msdn.com/Series/A-Developers-Guide-to-Windows-10/21">8.1 앱을 Windows 10으로 포팅</a></td>
    </tr>
    <tr>
        <td>iOS 앱을 유니버설 Windows 플랫폼 앱으로 포팅</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt238320">iOS에서 UWP로 이동</a></td>
    </tr>
    <tr>
        <td>Silverlight 앱을 유니버설 Windows 플랫폼 앱으로 포팅</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt238323">Windows Phone Silverlight에서 UWP로 이동</a></td>
    </tr>
    <tr>
        <td>XAML 또는 Silverlight에서 유니버설 Windows 플랫폼 앱으로 포팅(동영상)</td>
        <td><a href="https://channel9.msdn.com/Events/Build/2015/3-741">앱을 XAML 또는 Silverlight에서 Windows 10으로 포팅</a></td>
    </tr>
    <tr>
        <td>Xbox 게임을 유니버설 Windows 플랫폼 앱으로 포팅</td>
        <td><a href="https://developer.xboxlive.com/en-us/platform/development/education/Documents/Porting%20from%20Xbox%20One%20to%20Windows%2010.aspx">Xbox One에서 Windows10 UWP로 포팅</a></td>
    </tr>
    <tr>
        <td>DirectX 9에서 DirectX 11로 포팅</td>
        <td><a href="porting-your-directx-9-game-to-windows-store.md">DirectX 9에서 UWP(유니버설 Windows 플랫폼)로 포팅</a></td>
    </tr>
    <tr>
        <td>Direct3D 11에서 Direct3D 12로 포팅</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/mt431709">Direct3D 11에서 Direct3D 12로 포팅</a></td>
    </tr>
    <tr>
        <td>OpenGL ES에서 Direct3D 11로 포팅</td>
        <td><a href="port-from-opengl-es-2-0-to-directx-11-1.md">OpenGL ES 2.0에서 Direct3D 11로 포팅</a></td>
    </tr>
    <tr>
        <td>ANGLE을 사용하여 OpenGL ES에서 Direct3D 11로</td>
        <td><a href="http://go.microsoft.com/fwlink/p/?linkid=618387">ANGLE</a></td>
    </tr>
    <tr>
        <td>UWP에도 제공되는 클래식 Windows API</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/hh464945">UWP(유니버설 Windows 플랫폼) 앱의 Windows API에 대한 대안</a></td>
    </tr>
</table>


## <a name="prototype-and-design"></a>프로토타입 및 디자인


이제 만들려는 게임 유형과 빌드하는 데 사용할 도구 및 그래픽 기술을 결정하였으므로 디자인 및 프로토타입을 시작할 준비가 되었습니다. 기본적으로 게임은 유니버설 Windows 플랫폼 앱이므로 이 사실에서부터 시작해야 합니다.

### <a name="introduction-to-the-universal-windows-platform-uwp"></a>UWP(유니버설 Windows 플랫폼) 소개

Windows 10에서는 Windows10 디바이스에서 공통 API 플랫폼을 제공하는 UWP(유니버설 Windows 플랫폼)를 도입합니다. UWP는 Windows 런타임 모델을 발전시키고 일관된 통합 코어로 향상합니다. UWP를 대상으로 하는 게임은 모든 디바이스에 공통인 WinRT API를 호출할 수 있습니다. UWP는 보장된 API 계층을 제공하기 때문에 선택에 따라 Windows10 디바이스에서 설치되는 단일 앱 패키지를 만들 수도 있습니다. 원하는 경우 게임에서 게임이 실행되는 디바이스와 관련된 API(Win32 및 .NET의 일부 클래식 Windows API 포함)를 계속 호출할 수 있습니다.

다음은 유니버설 Windows 플랫폼 앱에 대해 자세하게 설명한 가이드이므로 플랫폼 이해를 돕기 위해 읽는 것이 좋습니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>유니버설 Windows 플랫폼 소개</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/dn726767">유니버설 Windows 플랫폼 앱이란?</a></td>
    </tr>
    <tr>
        <td>UWP의 개요</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/dn894631">UWP 앱 가이드</a></td>
    </tr>
</table>
 

### <a name="getting-started-with-uwp-development"></a>UWP 개발 시작

유니버설 Windows 플랫폼 앱 개발을 설정하고 준비하는 작업은 빠르고 쉽습니다. 다음 가이드는 프로세스를 단계별로 안내합니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>UWP 개발 시작</td>
        <td><a href="https://dev.windows.com/getstarted">Windows 앱 시작</a></td>
    </tr>
    <tr>
        <td>UWP 개발 설정</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/dn726766">설정 방법</a></td>
    </tr>
</table>

UWP 프로그래밍에 "완전 초보자"이고 게임에서 XAML 사용을 고려하는 경우([그래픽 기술 및 프로그래밍 언어 선택](#choosing-your-graphics-technology-and-programming-language) 참조), [완전 초보자를 위한 Windows10 개발](https://channel9.msdn.com/Series/Windows-10-development-for-absolute-beginners) 동영상 시리즈부터 시작하는 것이 좋습니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>XAML로 Windows10 개발 초보자 가이드(동영상 시리즈)</td>
        <td><a href="https://channel9.msdn.com/Series/Windows-10-development-for-absolute-beginners">완전 초보자를 위한 Windows10 개발</a></td>
    </tr>
    <tr>
        <td>XAML을 사용하여 Windows10 완전 초보자 시리즈 발표(블로그 게시물)</td>
        <td><a href="http://blogs.windows.com/buildingapps/2015/09/30/windows-10-development-for-absolute-beginners/">완전 초보자를 위한 Windows10 개발</a></td>
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
        <td><a href="https://dev.windows.com/develop">Windows 앱 개발</a></td>
    </tr>
    <tr>
        <td>UWP의 네트워크 프로그래밍 개요</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt280378">네트워킹 및 웹 서비스</a></td>
    </tr>
    <tr>
        <td>게임에서 Windows.Web.HTTP 및 Windows.Networking.Sockets 사용</td>
        <td><a href="work-with-networking-in-your-directx-game.md">게임의 네트워킹</a></td>
    </tr>
    <tr>
        <td>UWP의 비동기 프로그래밍 개념</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt187335">비동기 프로그래밍</a></td>
    </tr>
</table>

### <a name="windows-desktop-apisto-uwp"></a>Windows 데스크톱 APIsto UWP

Windows 데스크톱 게임을 UWP로 이동하는 데 도움이 되는 몇 가지 링크입니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>UWP 게임 개발에 기존 C++ 코드 사용</td>
        <td><a href="https://docs.microsoft.com/cpp/porting/how-to-use-existing-cpp-code-in-a-universal-windows-platform-app">방법: UWP 앱에서 기존 C++ 코드 사용</a></td>
    </tr>
    <tr>
        <td>Win32 및 COM API용 UWP API</td>
        <td><a href="https://docs.microsoft.com/uwp/win32-and-com/win32-and-com-for-uwp-apps">UWP 앱용 Win32 및 COM API</a></td>
    </tr>
    <tr>
        <td>UWP에서 지원하지 않는 CRT 기능</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/jj606124.aspx">유니버설 Windows 플랫폼 앱에서 지원하지 않는 CRT 기능</a></td>
    </tr>
    <tr>
        <td>Windows API에 대한 대안</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt592894.aspx">UWP(유니버설 Windows 플랫폼) 앱의 Windows API에 대한 대안</a></td>
    </tr>
</table>
 

### <a name="process-lifetime-management"></a>프로세스 수명 관리

프로세스 수명 관리 또는 앱 수명 주기에서는 유니버설 Windows 플랫폼 앱이 전환할 수 있는 다양한 활성화 상태를 설명합니다. 게임이 활성화, 일시 중단, 다시 시작 또는 종료될 수 있고 다양한 방법으로 이러한 상태 간에 전환할 수 있습니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>앱 수명 주기 전환 처리</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt243287">앱 수명 주기</a></td>
    </tr>
    <tr>
        <td>Microsoft Visual Studio를 사용하여 앱 전환 트리거</td>
        <td><a href="https://msdn.microsoft.com/library/hh974425.aspx">Visual Studio에서 UWP 앱에 대한 일시 중단, 다시 시작 및 백그라운드 이벤트를 트리거하는 방법</a></td>
    </tr>
</table>
 

### <a name="designing-game-ux"></a>게임 UX 디자인

창조적인 영감이 담긴 디자인, 바로 여기서 멋진 게임이 탄생합니다.

게임은 앱과 몇 가지 일반 사용자 인터페이스 요소 및 디자인 원칙을 공유하지만, 게임에는 종종 사용자 환경에 대한 고유한 모습, 느낌 및 디자인 목표가 있습니다. 두 가지 측면, 즉 게임이 테스트된 UX를 사용해야 하는 시기와 게임이 확산 및 혁신해야 하는 시기 모두에 디자인을 신중하게 적용할 때 게임은 성공할 수 있습니다. 게임에 대해 선택한 프레젠테이션 기술, 즉 DirectX, XAML, HTML5 또는 이 세 기술의 일부 조합은 구현 세부 정보에 영향을 주지만, 적용하는 디자인 원칙은 해당 선택과는 크게 관계가 없습니다.

UX 디자인과는 별도로 게임 플레이 디자인(예: 레벨 디자인, 속도, 세계 디자인 및 다른 측면)은 개발자와 그 개발 팀에 달려 있는 고유한 예술 양식으로 이 개발 가이드에서 다루지 않습니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>UWP 디자인 기초 및 지침</td>
        <td><a href="https://dev.windows.com/design">UWP 앱 디자인</a></td>
    </tr>
    <tr>
        <td>앱 수명 주기 상태에 대한 디자인</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/dn611862">시작, 일시 중단 및 다시 시작에 대한 UX 지침</a></td>
    </tr>
    <tr>
        <td>Xbox One 및 텔레비젼 화면용 UWP 앱 디자인</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/design/devices/designing-for-tv">Xbox 및 TV용 디자인</a></td>
    </tr>
    <tr>
        <td>여러 장치 폼 팩터를 대상으로 지정(동영상)</td>
        <td><a href="http://channel9.msdn.com/Events/GDC/GDC-2015/Designing-Games-for-a-Windows-Core-World">Windows Core World용 게임 디자인</a></td>
    </tr>   
</table>
 

#### <a name="color-guideline-and-palette"></a>색 지침 및 색상표

게임에서 일관된 색 지침을 따르면 심미적인 측면을 개선하고 탐색을 지원하며 플레이어에게 메뉴 및 HUD 기능을 알려주는 강력한 도구로 작동할 수 있습니다. 경고, 손상, XP 및 도전 과제와 같은 게임 요소의 색을 일관되게 지정하면 UI가 더욱 깔끔해지고 명시적 레이블의 필요성이 줄어듭니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>색 가이드</td>
        <td><a href="https://assets.windowsphone.com/499cd2be-64ed-4b05-a4f5-cd0c9ad3f6a3/101_BestPractices_Color_InvariantCulture_Default.zip">모범 사례: 색</a></td>
    </tr>
</table>
 

#### <a name="typography"></a>입력 체계

입력 체계를 적절히 사용하면 UI 레이아웃, 탐색, 가독성, 분위기, 브랜드 및 플레이어의 몰입도를 비롯한 게임의 여러 측면을 강화할 수 있습니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>입력 체계 가이드</td>
        <td><a href="http://go.microsoft.com/fwlink/?LinkId=535007">모범 사례: 입력 체계</a></td>
    </tr>
</table>
 

#### <a name="ui-map"></a>UI 맵

UI 맵은 게임 탐색 및 순서도로 표현된 메뉴의 레이아웃입니다. UI 맵을 통해 참여하는 모든 관련자가 게임 인터페이스 및 탐색 경로를 쉽게 이해할 수 있으며 개발 주기의 초기에 잠재적인 장애물과 문제점을 알아낼 수 있습니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>UI 맵 가이드</td>
        <td><a href="http://go.microsoft.com/fwlink/?LinkId=535008">모범 사례: UI 맵</a></td>
    </tr>
</table>

### <a name="game-audio"></a>게임 오디오

XAudio2, XAPO 및 Windows Sonic을 사용하여 게임 내 오디오를 구현하기 위한 지침과 참고 자료를 제공합니다. XAudio2는 고성능 오디오 엔진을 개발하기 위한 신호 처리 및 믹싱 기반을 제공하는 하위 수준 오디오 API입니다. XAPO API를 사용하면 Windows와 Xbox 모두에서 XAudio2에 사용할 수 있는 XAPO(플랫폼 간 오디오 처리 개체)를 만들 수 있습니다. 개발자는 Windows Sonic 오디오 지원을 통해 게임에 Dolby Atmos for Home Theater, Dolby Atmos for Headphones 및 Windows HRTF 지원을 추가하거나 미디어 응용 프로그램을 스트리밍할 수 있습니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>XAudio2 API</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/hh405049.aspx">XAudio2에 대한 프로그래밍 가이드 및 API 참조</a></td>
    </tr>
    <tr>
        <td>플랫폼 간 오디오 처리 개체 만들기</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/ee415735.aspx">XAPO 개요</a></td>
    </tr>
    <tr>
        <td>오디오 개념 소개</td>
        <td><a href="working-with-audio-in-your-directx-game.md">게임용 오디오</a></td>
    </tr>
    <tr>
        <td>Windows Sonic 개요</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/mt807491.aspx">공간 음향</a></td>
    </tr>
    <tr>
        <td>Windows Sonic 공간 음향 샘플</td>
        <td><a href="https://github.com/Microsoft/Xbox-ATG-Samples/tree/master/UWPSamples/Audio">Xbox 고급 기술 그룹 오디오 샘플</a></td>
    </tr>
    <tr>
        <td>Windows Sonic을 게임에 통합하는 방법 알아보기(동영상)</td>
        <td><a href="https://channel9.msdn.com/Events/GDC/GDC-2017/GDC2017-002">Xbox andWindows 용 공간 오디오 기능 소개</a></td>
    </tr>
</table>

### <a name="directx-development"></a>DirectX 개발

DirectX 게임 개발에 대한 가이드 및 참조입니다.

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
        <td>자습서: UWP DirectX 게임을 개발하는 방법</td>
        <td><a href="tutorial--create-your-first-uwp-directx-game.md">DirectX로 간단한 UWP 게임 만들기</a></td>
    </tr>
    <tr>
        <td>DirectX의 UWP 앱 모델 조작</td>
        <td><a href="about-the-uwp-user-interface-and-directx.md">앱 개체 및 DirectX</a></td>
    </tr>
    <tr>
        <td>그래픽 및 DirectX 12 개발 동영상(YouTube 채널)</td>
        <td><a href="https://www.youtube.com/channel/UCiaX2B8XiXR70jaN7NK-FpA">Microsoft DirectX 12 및 그래픽 교육</a></td>
    </tr>
    <tr>
        <td>DirectX 개요 및 참조</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/ee663274">DirectX 그래픽 및 게임</a></td>
    </tr>
    <tr>
        <td>Direct3D 12 프로그래밍 가이드 및 참조</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/dn903821">Direct3D 12 그래픽</a></td>
    </tr>
    <tr>
        <td>DirectX 12 기본 사항(동영상)</td>
        <td><a href="http://channel9.msdn.com/Events/GDC/GDC-2015/Better-Power-Better-Performance-Your-Game-on-DirectX12">더 나은 파워, 더 나은 성능: DirectX 12의 게임</a></td>
    </tr>
</table>

#### <a name="learning-direct3d-12"></a>Direct3D 12 학습

Direct3D 12의 변경 사항 및 Direct3D 12를 사용하여 프로그래밍을 시작하는 방법을 알아봅니다. 

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>프로그래밍 환경 설정</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/dn899120.aspx">Direct3D 12 프로그래밍 환경 설정</a></td>
    </tr>
    <tr>
        <td>기본 구성 요소를 만드는 방법</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/dn859356.aspx">기본 Direct3D 12 구성 요소 만들기</a></td>
    </tr>
    <tr>
        <td>Direct3D 12의 변경 사항</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/dn899194.aspx">Direct3D 11에서 Direct3D 12로 마이그레이션 시의 중요 변경 사항</a></td>
    </tr>
    <tr>
        <td>Direct3D 11에서 Direct3D 12로 포팅하는 방법</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/mt431709.aspx">Direct3D 11에서 Direct3D 12로 포팅</a></td>
    </tr>
    <tr>
        <td>리소스 바인딩 개념(설명자, 설명자 테이블, 설명자 힙 및 루트 서명 설명) </td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/dn899206.aspx">Direct3D 12의 리소스 바인딩</a></td>
    </tr>
    <tr>
        <td>메모리 관리</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/dn899198.aspx">Direct3D 12의 메모리 관리</a></td>
    </tr>
</table>
 

#### <a name="directx-tool-kit-and-libraries"></a>DirectX 도구 키트 및 라이브러리

DirectX 도구 키트, DirectX 텍스처 처리 라이브러리, DirectXMesh 기하 도형 처리 라이브러리, UVAtlas 라이브러리 및 DirectXMath 라이브러리에서는 텍스처, 메시, 스프라이트 및 기타 유틸리티 기능과 DirectX 개발용 도우미 클래스를 제공합니다. 이러한 라이브러리를 사용하면 개발 시간과 노력을 절약할 수 있습니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>DirectX 11용 DirectX 도구 키트 가져오기</td>
        <td><a href="http://go.microsoft.com/fwlink/?LinkId=248929">DirectXTK</a></td>
    </tr>
    <tr>
        <td>DirectX 12용 DirectX 도구 키트 가져오기</td>
        <td><a href="http://go.microsoft.com/fwlink/?LinkID=615561">DirectXTK 12</a></td>
    </tr>
    <tr>
        <td>DirectX 텍스처 처리 라이브러리 가져오기</td>
        <td><a href="http://go.microsoft.com/fwlink/?LinkId=248926">DirectXTex</a></td>
    </tr>
    <tr>
        <td>DirectXMesh 기하 도형 처리 라이브러리 가져오기</td>
        <td><a href="http://go.microsoft.com/fwlink/?LinkID=324981">DirectXMesh</a></td>
    </tr>
    <tr>
        <td>isochart 텍스처 아틀라스의 생성 및 압축용 UVAtlas 가져오기</td>
        <td><a href="http://go.microsoft.com/fwlink/?LinkID=512686">UVAtlas</a></td>
    </tr>
    <tr>
        <td>DirectXMath 라이브러리 가져오기</td>
        <td><a href="http://go.microsoft.com/fwlink/?LinkID=615560">DirectXMath</a></td>
    </tr>
    <tr>
        <td>DirectXTK (블로그 게시물)에서 Direct3D12 지원</td>
        <td><a href="https://github.com/Microsoft/DirectXTK/issues/2">DirectX 12에 대한 지원</a></td>
    </tr>
</table>

#### <a name="directx-resources-from-partners"></a>파트너의 DirectX 리소스

외부 파트너가 만든 몇 가지 추가 DirectX 설명서입니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Nvidia: DX12 권장 사항 및 금지 사항(블로그 게시물) </td>
        <td><a href="https://developer.nvidia.com/dx12-dos-and-donts-updated">Nvidia GPU의 DirectX 12</a></td>
    </tr>
    <tr>
        <td>Intel: DirectX 12를 사용한 효율적인 렌더링</td>
        <td><a href="https://software.intel.com/sites/default/files/managed/4a/38/Efficient-Rendering-with-DirectX-12-on-Intel-Graphics.pdf">Intel Graphics의 DirectX 12 렌더링</a></td>
    </tr>
    <tr>
        <td>Intel: DirectX 12의 다중 어댑터 지원</td>
        <td><a href="https://software.intel.com/articles/multi-adapter-support-in-directx-12">DirectX 12를 사용하여 명시적 다중 어댑터 응용 프로그램을 구현하는 방법</a></td>
    </tr>
    <tr>
        <td>Intel: DirectX 12 자습서</td>
        <td><a href="https://software.intel.com/articles/tutorial-migrating-your-apps-to-directx-12-part-1">Intel, Suzhou Snail 및 Microsoft의 공동 백서</a></td>
    </tr>
</table>


## <a name="production"></a>프로덕션


이제 팀 전체에 작업이 배포되면서 스튜디오가 완전히 참여하고 프로덕션 주기로 이동합니다. 프로토타입을 다듬고 리팩터링하고 확장하면서 전체 게임으로 만들어갑니다.

### <a name="notifications-and-live-tiles"></a>알림 및 라이브 타일

타일은 시작 메뉴에서의 게임 표시를 말합니다. 타일 및 알림을 통해 현재 게임을 플레이하지 않는 경우에도 플레이어의 관심을 끌 수 있습니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>타일 및 배지 개발</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt185606">타일, 배지 및 알림</a></td>
    </tr>
    <tr>
        <td>라이브 타일 및 알림을 보여 주는 샘플</td>
        <td><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Notifications">알림 샘플</a></td>
    </tr>
    <tr>
        <td>적응형 타일 템플릿(블로그 게시물)</td>
        <td><a href="http://blogs.msdn.com/b/tiles_and_toasts/archive/2015/06/30/adaptive-tile-templates-schema-and-documentation.aspx">적응형 타일 템플릿 - 스키마 및 설명서</a></td>
    </tr>
    <tr>
        <td>타일 및 배지 디자인</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/hh465403">타일 및 배지에 대한 지침</a></td>
    </tr>
    <tr>
        <td>라이브 타일 템플릿을 대화형으로 개발하기 위한 Windows10 앱</td>
        <td><a href="https://www.microsoft.com/store/apps/9nblggh5xsl1">알림 시각화 도우미</a></td>
    </tr>
    <tr>
        <td>Visual Studio의 UWP 타일 생성기 확장</td>
        <td><a href="https://visualstudiogallery.msdn.microsoft.com/09611e90-f3e8-44b7-9c83-18dba8275bb2">단일 이미지를 사용하여 필요한 모든 타일을 만들기 위한 도구</a></td>
    </tr>
    <tr>
        <td>Visual Studio의 UWP 타일 생성기 확장(블로그 게시물)</td>
        <td><a href="https://blogs.windows.com/buildingapps/2016/02/15/uwp-tile-generator-extension-for-visual-studio/">UWP 타일 생성기 도구 사용에 대한 팁</a></td>
    </tr>
</table>
 

### <a name="enable-in-app-product-add-on-purchases"></a>(추가) 앱에서 바로 제품 구매 사용

추가 기능 (앱에서 바로 제품)는 플레이어가 게임에서 구입할 수 있는 보충 항목입니다. 추가 기능에는 게임 수준, 항목 또는 플레이어가 즐길 수 있는 다른 모든 사항이 될 수 있습니다. 추가 기능 적절 하 게 사용, 게임 환경을 개선 하는 동안 수익을 제공할 수 있습니다. 파트너 센터를 통해 게임의 추가 기능을 게시 및 게임의 코드에서 앱에서 바로 구매를 정의 합니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>지속형 추가 기능</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt219684">앱에서 바로 구매 제품 사용</a></td>
    </tr>
    <tr>
        <td>소모 성 추가 기능</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt219683">앱에서 바로 소모성 제품 구매 사용</a></td>
    </tr>
    <tr>
        <td>추가 기능 세부 정보 및 제출</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt148551">추가 기능 제출</a></td>
    </tr>
    <tr>
        <td>추가 기능 판매 및 게임에 대 한 인구 통계 모니터링</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt148538">추가 기능 구입 보고서</a></td>
    </tr>
</table>
 

### <a name="debugging-performance-optimization-and-monitoring"></a>디버깅, 성능 최적화 및 모니터링

성능을 최적화하려면 Windows 10의 게임 모드를 활용하여 게이머의 현재 하드웨어 성능을 최대로 이용하는 방식으로 가능한 최상의 게이밍 환경을 제공하세요.

Windows Performance Toolkit(WPT)은 Windows 운영 체제 및 응용 프로그램의 세부 성능 프로필을 생성하는 성능 모니터링 도구 집합으로 구성되어 있습니다. WPT는 메모리 사용량을 모니터링하고 게임 성능을 향상시키는 데 특히 유용합니다. Windows Performance Toolkit은 Windows10 SDK 및 Windows ADK에 포함되어 있습니다. 이 툴킷은 Windows Performance Recorder(WPR)와 Windows Performance Analyzer(WPA)의 두 가지 독립적인 도구로 구성됩니다. [Windows Sysinternals](https://technet.microsoft.com/sysinternals/default)에 포함된 ProcDump는 게임 충돌이 발생하는 동안 CPU 스파이크를 모니터링하고 덤프 파일을 생성하는 명령줄 유틸리티입니다. 

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>코드 성능 테스트</td>
        <td><a href="https://www.visualstudio.com/team-services/cloud-load-testing/">클라우드 기반 로드 테스트</a></td>
    </tr>
    <tr>
        <td>게임 디바이스 정보를 사용하여 Xbox 콘솔 형식 파악</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/mt825235">게임 장치 정보</a></td>
    </tr>
    <tr>
        <td>게임 모드 API를 통해 하드웨어 리소스에 독점적 또는 우선적으로 액세스하여 성능 개선</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/mt808808">게임 모드</a></td>
    </tr>
    <tr>
        <td>Windows10 SDK에서 Windows Performance Toolkit(WPT) 가져오기</td>
        <td><a href="https://developer.microsoft.com/windows/downloads/windows-10-sdk">Windows10 SDK</a></td>
    </tr>
    <tr>
        <td>Windows ADK에서 Windows Performance Toolkit(WPT) 가져오기</td>
        <td><a href="https://msdn.microsoft.com/windows/hardware/dn913721.aspx">Windows ADK</a></td>
    </tr>
    <tr>
        <td>Windows Performance Analyzer를 사용하여 반응하지 않는 UI 문제 해결(비디오)</td>
        <td><a href="https://channel9.msdn.com/Shows/Defrag-Tools/Defrag-Tools-156-Critical-Path-Analysis-with-Windows-Performance-Analyzer">WPA를 사용한 중요 경로 분석</a></td>
    </tr>
    <tr>
        <td>Windows Performance Recorder를 사용하여 메모리 사용량 및 누수 진단(비디오)</td>
        <td><a href="https://channel9.msdn.com/Shows/Defrag-Tools/Defrag-Tools-154-Memory-Footprint-and-Leaks">메모리 공간 및 누수</a></td>
    </tr>
    <tr>
        <td>ProcDump 가져오기</td>
        <td><a href="https://technet.microsoft.com/sysinternals/dd996900">ProcDump</a></td>
    </tr>
    <tr>
        <td>ProcDump 사용 방법 알아보기(비디오)</td>
        <td><a href="https://channel9.msdn.com/Shows/Defrag-Tools/Defrag-Tools-131-Windows-10-SDK">덤프 파일 생성을 위한 ProcDump 구성</a></td>
    </tr>
</table>

### <a name="advanced-directx-techniques-and-concepts"></a>고급 DirectX 기술 및 개념

DirectX 개발의 일부는 미묘하고 복잡할 수 있습니다. 프로덕션에서 DirectX 엔진의 세부 사항을 알아봐야 하는 지점에 이르거나 어려운 성능 문제를 디버그해야 하는 경우 이 섹션의 리소스 및 정보가 도움이 될 수 있습니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Windows의 PIX</td>
        <td><a href="https://blogs.msdn.microsoft.com/pix/2017/01/17/introducing-pix-on-windows-beta/">Windows 기반 DirectX 12용 성능 조정 및 디버깅 도구</a></td>
    </tr>
    <tr>
        <td>D3D12 개발을 위한 디버깅 및 유효성 검사 도구(동영상)</td>
        <td><a href="https://channel9.msdn.com/Events/GDC/GDC-2017/GDC2017-003">D3D12 성능 조정 및 PIX and GPUValidation로 디버깅</a></td>
    </tr>
    <tr>
        <td>그래픽 및 성능 최적화(동영상)</td>
        <td><a href="http://channel9.msdn.com/Events/GDC/GDC-2015/Advanced-DirectX12-Graphics-and-Performance">고급 DirectX 12 그래픽 및 성능</a></td>
    </tr>
    <tr>
        <td>DirectX 그래픽 디버깅(동영상)</td>
        <td><a href="http://channel9.msdn.com/Events/GDC/GDC-2015/Solve-the-Tough-Graphics-Problems-with-your-Game-Using-DirectX-Tools">DirectX 도구를 사용하여 게임의 그래픽 문제 해결</a></td>
    </tr>
    <tr>
        <td>DirectX 12 디버깅을 위한 Visual Studio 2015 도구(동영상)</td>
        <td><a href="https://channel9.msdn.com/Series/ConnectOn-Demand/212">Visual Studio 2015의 Windows 10용 DirectX 도구</a></td>
    </tr>
    <tr>
        <td>Direct3D 12 프로그래밍 가이드</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/dn903821">Direct3D 12 프로그래밍 가이드</a></td>
    </tr>
    <tr>
        <td>DirectX 및 XAML 결합</td>
        <td><a href="directx-and-xaml-interop.md">DirectX 및 XAML 상호 운용성</a></td>
    </tr>
</table>

### <a name="high-dynamic-range-hdr-content-development"></a>HDR(High Dynamic Range) 콘텐츠 개발

HDR의 전체 컬러 기능을 사용하는 게임 콘텐츠를 빌드합니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>HDR 및 색 개념(비디오) 소개</td>
        <td><a href="https://channel9.msdn.com/Events/Build/2017/P4061">DirectX의 HDR 및 고급 색 활성화</a></td>
    </tr>
    <tr>
        <td>HDR 콘텐츠를 렌더링하고 현재 디스플레이에서의 지원 여부를 감지하는 방법 확인</td>
        <td><a href="https://github.com/Microsoft/DirectX-Graphics-Samples/tree/master/Samples/UWP/D3D12HDR">HDR 샘플</a></td>
    </tr>
    <tr>
        <td>DirectX를 사용하여 고급 색 생성 및 구성</td>
        <td><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/D2DAdvancedColorImages">Direct2D 고급 색 이미지 렌더링 샘플</a></td>
    </tr>   
</table>


### <a name="globalization-and-localization"></a>세계화 및 지역화

전 세계에서 널리 사용되는 Windows 플랫폼용 게임을 개발하고 Microsoft의 인기 제품에 기본 제공되는 국가별 기능을 알아봅니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>글로벌 시장을 겨냥한 게임 준비</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt186453.aspx">글로벌 고객을 대상으로 개발 시 지침</a></td>
    </tr>
    <tr>
        <td>언어, 문화 및 기술 브리징</td>
        <td><a href="http://www.microsoft.com/Language/Default.aspx">언어 규칙 및 표준 Microsoft 용어에 대한 온라인 리소스</a></td>
    </tr>
</table>

### <a name="security"></a>보안

게이머가 공정하게 경쟁할 수 있는 게임 환경을 만듭니다. TruePlay에 등록된 게임은 보호된 프로세스에서 실행되기 때문에 일반적인 공격 수준을 완화할 수 있습니다. 또한 게임 모니터링 시스템은 일반적인 부정 행위 시나리오를 식별할 수 있도록 도와줍니다. 

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>PC 게임 내에서 부정 행위를 퇴치하기 위한 도구</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/mt808781">TruePlay</a></td>
    </tr>
</table>

## <a name="submitting-and-publishing-your-game"></a>게임 제출 및 게시

다음 가이드 및 정보는 게시 및 제출 프로세스를 가능한 한 매끄럽게 하는 데 도움이 됩니다.

### <a name="publishing"></a>게시

게시 하 고 게임 패키지를 관리 하는 [파트너 센터](https://partner.microsoft.com/dashboard) 를 사용 합니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>파트너 센터 앱 게시</td>
        <td><a href="https://dev.windows.com/publish">Windows 앱 게시</a></td>
    </tr>
    <tr>
        <td>파트너 센터 고급 게시 (GDN)</td>
        <td><a href="https://developer.xboxlive.com/en-us/windows/documentation/Pages/home.aspx">파트너 센터 고급 게시 가이드</a></td>
    </tr>
    <tr>
        <td>Azure Active Directory (AAD)를 사용 하 여 파트너 센터 계정에 사용자 추가</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/publish/manage-account-users">계정 사용자 관리</a></td>
    </tr>   
    <tr>
        <td>게임 평가(블로그 게시물)</td>
        <td><a href="https://blogs.windows.com/buildingapps/2016/01/06/now-available-single-age-rating-system-to-simplify-app-submissions/">IARC 시스템을 사용하여 연령별 등급을 할당하기 위한 단일 워크플로</a></td>
    </tr>
</table>

#### <a name="packaging-and-uploading"></a>패키징 및 업로드

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>스트리밍 설치 및 선택적 패키지 사용 방법 알아보기(동영상)</td>
        <td><a href="https://channel9.msdn.com/Events/Build/2017/B8093">차세대 UWP 앱 배포: 확장, 스트림 밀 componentizedapps 빌드</a></td>
    </tr>
    <tr>
        <td>스트리밍 설치가 가능하도록 콘텐츠 나누기 및 그룹화</td>
        <td><a href="../packaging/streaming-install.md">UWP 앱 스트리밍 설치</a></td>
    </tr>
    <tr>
        <td>DLC 게임 콘텐츠 같은 선택적 패키지 만들기</td>
        <td><a href="../packaging/optional-packages.md">선택형 패키지 및 관련 집합 제작</a></td>
    </tr>
    <tr>
        <td>UWP 게임 패키징</td>
        <td><a href="../packaging/index.md">앱 패키징</a></td>
    </tr>
    <tr>
        <td>UWP DirectX 게임 패키징</td>
        <td><a href="package-your-windows-store-directx-game.md">UWP DirectX 게임 패키징</a></td>
    </tr>
    <tr>
        <td>타사 개발자로서 게임 패키징(블로그 게시물)</td>
        <td><a href="https://blogs.windows.com/buildingapps/2015/12/15/building-an-app-for-a-3rd-party-how-to-package-their-store-app/">게시자의 스토어 계정 액세스 없이 업로드할 수 있는 패키지 만들기</a></td>
    </tr>
    <tr>
        <td>MakeAppx를 사용하여 앱 패키지 및 앱 패키지 번들 만들기</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/packaging/create-app-package-with-makeappx-tool">앱 패키지 작성 도구 MakeAppx.exe를 사용하여 패키지 만들기</a></td>
    </tr>
    <tr>
        <td>SignTool을 사용하여 파일에 디지털 서명</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/aa387764">SignTool을 사용하여 파일에 서명하고 파일의 서명 확인</a></td>
    </tr>    
    <tr>
        <td>게임 업로드 및 버전 관리</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt148542">앱 패키지 업로드</a></td>
    </tr>
</table>


### <a name="policies-and-certification"></a>정책 및 인증

인증 문제가 게임의 릴리스를 지연하지 않도록 합니다. 다음은 주의해야 할 정책 및 일반적인 인증 문제입니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Microsoft Store 앱 개발자 계약</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/hh694058">앱 개발자 계약</a></td>
    </tr>
    <tr>
        <td>Microsoft Store에 앱을 게시하기 위한 정책</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/dn764944">Microsoft Store 정책</a></td>
    </tr>
    <tr>
        <td>몇 가지 일반적인 앱 인증 문제를 방지하는 방법</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/jj657968">일반적인 인증 실패 방지</a></td>
    </tr>
</table>
 

### <a name="store-manifest-storemanifestxml"></a>스토어 매니페스트(StoreManifest.xml)

스토어 매니페스트(StoreManifest.xml)는 앱 패키지에 포함될 수 있는 선택적 구성 파일입니다. 스토어 매니페스트에서는 AppxManifest.xml 파일의 일부가 아닌 추가 기능을 제공합니다. 예를 들어 대상 디바이스에 지정된 최소 DirectX 기능 수준 또는 지정된 최소 시스템 메모리가 없는 경우 스토어 매니페스트를 사용하여 게임의 설치를 차단할 수 있습니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>스토어 매니페스트 스키마</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt617335">StoreManifest 스키마(Windows10)</a></td>
    </tr>
</table>
 

## <a name="game-lifecycle-management"></a>게임 수명 주기 관리


개발을 완료하고 게임을 제공한 후에도 "게임 오버"가 아닙니다. 한 버전의 개발은 완료된 것일 수 있지만 마켓플레이스에서 게임의 여정은 시작에 불과합니다. 사용 및 오류 보고를 모니터링하고, 사용자 피드백에 응답하며, 게임에 대한 업데이트를 게시하길 원할 것입니다.

### <a name="partner-center-analytics-and-promotion"></a>파트너 센터 분석 및 홍보

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>파트너 센터 분석</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt148522">앱 성능 분석</a></td>
    </tr>
    <tr>
        <td>고객들이 Xbox 기능으로 몰입감 높은 게임을 즐길 수 있는 방법 확인</td>
        <td><a href="../publish/xbox-analytics-report.md">Xbox 분석 보고서</a></td>
    </tr>
    <tr>
        <td>고객 리뷰에 응답</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt148546">고객 리뷰에 응답</a></td>
    </tr>
    <tr>
        <td>게임 홍보 방법</td>
        <td><a href="https://dev.windows.com/store-promotion">앱 홍보</a></td>
    </tr>
</table>
 

### <a name="visual-studio-application-insights"></a>Visual Studio Application Insights

Visual Studio Application Insights에서 게시된 게임에 대한 성능, 원격 분석 및 사용 현황 분석을 제공합니다. Application Insights는 게임이 릴리스된 후 문제를 검색 및 해결하고, 지속적으로 사용을 모니터링 및 개선하며, 플레이어가 어떻게 계속해서 게임과 상호 작용하는지를 이해하도록 도와줍니다. Application Insights는 [Azure Portal](http://portal.azure.com/)에 원격 분석을 전송하는 SDK를 앱에 추가하여 작동합니다.

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
        <td><a href="https://azure.microsoft.com/documentation/articles/app-insights-windows-get-started/">Windows Phone 및 스토어 앱용 Application Insights</a></td>
    </tr>
</table>


### <a name="third-party-solutions-for-analytics-and-promotion"></a>타사의 분석 및 홍보 솔루션

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>GameAnalytics를 사용하여 플레이어의 동작 이해</td>
        <td><a href="http://www.gameanalytics.com/">GameAnalytics</a></td>
    </tr>
    <tr>
        <td>UWP 앱을 Google Analytics에 연결</td>
        <td><a href="https://github.com/dotnet/windows-sdk-for-google-analytics">Google Analytics용 Windows SDK 다운로드</a></td>
    </tr>
    <tr>
        <td>Google Analytics용 Windows SDK 사용 방법 알아보기(동영상)</td>
        <td><a href="https://channel9.msdn.com/Events/Windows/Windows-Developer-Day-Creators-Update/Getting-started-with-the-Windows-SDK-for-Google-Analytics">Google Analytics용 Windows SDK 시작</a></td>
    </tr>    
    <tr>
        <td>Facebook 앱 설치 광고를 사용하여 Facebook 사용자에게 게임 홍보</td>
        <td><a href="https://github.com/Microsoft/winsdkfb">Facebook용 Windows SDK 다운로드</a></td>
    </tr>
    <tr>
        <td>Facebook 앱 설치 광고 사용 방법 알아보기(동영상)</td>
        <td><a href="https://channel9.msdn.com/Events/Windows/Windows-Developer-Day-Creators-Update/Getting-started-with-Facebook-App-Install-Ads">Facebook용 Windows SDK 시작</a></td>
    </tr>
    <tr>
        <td>Vungle을 사용하여 게임에 동영상 광고 추가</td>
        <td><a href="https://v.vungle.com/sdk">Vungle용 Windows SDK 다운로드</a></td>
    </tr>
</table>
 

### <a name="creating-and-managing-content-updates"></a>콘텐츠 업데이트 만들기 및 관리

게시된 게임을 업데이트하려면 더 높은 버전 번호를 사용하는 새 앱 패키지를 제출합니다. 패키지가 제출 및 인증을 거친 후 고객이 업데이트로 자동으로 사용할 수 있게 됩니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>게임 업데이트 및 버전 관리</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt188602">패키지 버전 번호</a></td>
    </tr>
    <tr>
        <td>게임 패키지 관리 지침</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt188602">앱 패키지 관리 지침</a></td>
    </tr>
</table>


## <a name="adding-xbox-live-to-your-game"></a>게임에 Xbox Live 추가

Xbox Live는 전 세계의 수 백만 게이머들을 연결하는 최상의 게임 네트워크입니다. 개발자들은 Xbox Live 프레전스, 리더보드, 클라우드 서비스, 게임 허브, 클럽, 파티 채팅, 게임 DVR을 포함하여 게임 청중들을 조직적으로 유치할 수 있는 Xbox Live 기능에 액세스할 수 있습니다.

> [!Note]
> Xbox Live가 지원되는 타이틀을 개발하고 싶다면 몇 가지 옵션을 사용할 수 있습니다. 다양한 프로그램에 대한 내용은 [개발자 프로그램 개요](../xbox-live/developer-program-overview.md)를 참조하세요.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Xbox Live 개요</td>
        <td><a href="../xbox-live/index.md">Xbox Live 개발자 가이드</a></td>
    </tr>
    <tr>
        <td>프로그램에 따라 사용할 수 있는 기능 알아보기</td>
        <td><a href="../xbox-live/developer-program-overview.md#feature-table">개발자 프로그램 개요: 기능표</a></td>
    </tr>
    <tr>
        <td>Xbox Live 게임을 개발하는데 유용한 리소스에 대한 링크</td>
        <td><a href="../xbox-live/xbox-live-resources.md">Xbox Live 리소스</a></td>
    </tr>
    <tr>
        <td>Xbox Live 서비스에서 정보를 얻는 방법 알아보기</td>
        <td><a href="../xbox-live/introduction-to-xbox-live-apis.md">Xbox Live API 소개</a></td>
    </tr>
</table>


### <a name="for-developers-in-the-xbox-live-creators-program"></a>Xbox Live 크리에이터스 프로그램의 개발자를 위한 자료

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>개요</td>
        <td><a href="../xbox-live/get-started-with-creators/get-started-with-xbox-live-creators.md">Xbox Live 크리에이터스 프로그램 시작</a></td>
    </tr>
    <tr>
        <td>게임에 Xbox Live 추가</td>
        <td><a href="../xbox-live/get-started-with-creators/creators-step-by-step-guide.md">Xbox Live 크리에이터스 프로그램을 통합하는 방법에 대한 단계별 가이드</a></td>
    </tr>
    <tr>
        <td>Unity를 사용하여 만든 UWP 게임에 Xbox Live 추가</td>
        <td><a href="../xbox-live/get-started-with-creators/develop-creators-title-with-unity.md">Unity 게임 엔진을 사용하여 Xbox Live 크리에이터스 프로그램 타이틀 개발 시작</a></td>
    </tr>
    <tr>
        <td>개발 샌드박스 설정</td>
        <td><a href="../xbox-live/get-started-with-creators/xbox-live-sandboxes-creators.md">Xbox Live 샌드박스 소개</a></td>
    </tr>
    <tr>
        <td>테스트를 위한 계정 설정</td>
        <td><a href="../xbox-live/get-started-with-creators/authorize-xbox-live-accounts.md">테스트 환경에서 Xbox Live 계정에 권한을 부여합니다</a></td>
    </tr>
    <tr>
        <td>Xbox Live 크리에이터스 프로그램 샘플</td>
        <td><a href="https://github.com/Microsoft/xbox-live-samples/tree/master/Samples/CreatorsSDK">크리에이터스 프로그램 개발자를 위한 코드 샘플</a></td>
    </tr>
    <tr>
        <td>플랫폼 간 Xbox Live 경험을 UWP 게임에 통합하는 방법 알아보기(동영상)</td>
        <td><a href="https://channel9.msdn.com/Events/GDC/GDC-2017/GDC2017-005">Xbox Live 크리에이터스 프로그램</a></td>
    </tr>  
</table>

### <a name="for-managed-partners-and-developers-in-the-idxbox-program"></a>ID@Xbox 프로그램의 관리 파트너 및 개발자를 위한 자료

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>개요</td>
        <td><a href="../xbox-live/get-started-with-partner/get-started-with-xbox-live-partner.md">관리 파트너 또는 ID 개발자로 Xbox Live 시작</a></td>
    </tr>
    <tr>
        <td>게임에 Xbox Live 추가</td>
        <td><a href="../xbox-live/get-started-with-partner/partners-step-by-step-guide.md">관리 파트너 및 ID 구성원에 대해 Xbox Live를 통합하는 방법에 대한 단계별 가이드</a></td>
    </tr>
    <tr>
        <td>Unity를 사용하여 만든 UWP 게임에 Xbox Live 추가</td>
        <td><a href="../xbox-live/get-started-with-partner/partner-unity-uwp-il2cpp.md">ID 파트너 및 관리 파트너를 위해 IL2CPP 스크립팅 백 엔드를 사용하여 UWP용 Unity에 Xbox Live 지원 추가</a></td>
    </tr>
    <tr>
        <td>개발 샌드박스 설정</td>
        <td><a href="../xbox-live/get-started-with-partner/advanced-xbox-live-sandboxes.md">고급 Xbox Live 샌드박스</a></td>
    </tr>
    <tr>
        <td>Xbox Live를 사용하는 게임에 대한 요구 사항(GDN)</td>
        <td><a href="http://go.microsoft.com/fwlink/?LinkId=533217">Windows 10의 Xbox Live에 대한 Xbox 요구 사항</a></td>
    </tr>
    <tr>
        <td>샘플</td>
        <td><a href="https://github.com/Microsoft/xbox-live-samples/tree/master/Samples/ID%40XboxSDK">ID@Xbox 개발자를 위한 코드 샘플</a></td>
    </tr>  
    <tr>
        <td>Xbox Live 게임 개발 개요(동영상)</td>
        <td><a href="http://channel9.msdn.com/Events/GDC/GDC-2015/Developing-with-Xbox-Live-for-Windows-10">Windows 10용 Xbox Live로 개발</a></td>
    </tr>
    <tr>
        <td>플랫폼 간 매치 메이킹(동영상)</td>
        <td><a href="http://channel9.msdn.com/Events/GDC/GDC-2015/Xbox-Live-Multiplayer-Introducing-services-for-cross-platform-matchmaking-and-gameplay">Xbox Live 멀티플레이어: 플랫폼 간 매치 메이킹 및 게임 플레이용 서비스 소개</a></td>
    </tr>
    <tr>
        <td>Fable Legends의 장치 간 게임 플레이(동영상)</td>
        <td><a href="http://channel9.msdn.com/Events/GDC/GDC-2015/Fable-Legends-Cross-device-Gameplay-with-Xbox-Live">Fable Legends: Xbox Live로 장치 간 게임 플레이</a></td>
    </tr>
    <tr>
        <td>Xbox Live 통계 및 도전 과제(동영상)</td>
        <td><a href="http://channel9.msdn.com/Events/GDC/GDC-2015/Best-Practices-for-Leveraging-Cloud-Based-User-Stats-and-Achievements-in-Xbox-Live">Xbox Live에서 클라우드 기반 사용자 통계 및 도전 과제 활용 모범 사례</a></td>
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
        <td><a href="https://docs.microsoft.com/windows/uwp/gaming/game-development-videos">GDC 및 //빌드 등의 주요 회의 동영상</a></td>
    </tr>
    <tr>
        <td>인디 게임 개발(동영상)</td>
        <td><a href="http://channel9.msdn.com/Events/GDC/GDC-2015/New-Opportunities-for-Independent-Developers">독립 개발자를 위한 새로운 기회</a></td>
    </tr>
    <tr>
        <td>멀티 코어 모바일 장치에 대한 고려 사항(동영상)</td>
        <td><a href="http://channel9.msdn.com/Events/GDC/GDC-2015/Sustained-gaming-performance-in-multi-core-mobile-devices">멀티 코어 모바일 장치에서 게임 성능 지속</a></td>
    </tr>
    <tr>
        <td>Windows10 데스크톱 게임 개발(동영상)</td>
        <td><a href="http://channel9.msdn.com/Events/GDC/GDC-2015/PC-Games-for-Windows-10">Windows 10용 PC 게임</a></td>
    </tr>
</table>



 

 

 
