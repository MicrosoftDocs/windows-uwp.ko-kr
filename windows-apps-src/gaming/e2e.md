---
title: Windows 10 게임 개발 가이드
description: UWP(유니버설 Windows 플랫폼) 게임 개발용 리소스 및 정보에 대한 종단 간 가이드입니다.
ms.assetid: 6061F498-96A8-44EF-9711-68AE5A1218C9
---

# Windows 10 게임 개발 가이드


\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]

Windows 10 게임 개발 가이드입니다.

이 가이드에서는 UWP(유니버설 Windows 플랫폼) 게임을 개발하는 데 필요한 리소스 및 정보의 종단 간 컬렉션을 제공합니다.

## UWP(유니버설 Windows 플랫폼)용 게임 개발 기술 소개


Windows 10 게임을 만드는 경우 휴대폰, PC 및 Xbox One을 통해 전 세계 수백만 명의 플레이어와 접촉할 수 있습니다. Windows의 Xbox, Xbox Live, 디바이스 간 멀티플레이어, 놀라운 게임 커뮤니티, UWP(유니버설 Windows 플랫폼) 및 DirectX 12와 같은 강력한 새 기능을 지원하는 Windows 10 게임은 모든 연령 및 장르의 플레이어를 열광시킵니다. 새 UWP(유니버설 Windows 플랫폼)는 휴대폰, PC 및 Xbox One용 공통 API를 사용하는 Windows 10 디바이스에서, 각 디바이스 환경에 맞게 게임을 조정하는 도구 및 옵션과 함께 게임에 대한 호환성을 제공합니다.

이 가이드에서는 게임을 개발할 때 도움이 되는 정보 및 리소스의 종단 간 컬렉션을 제공합니다. 게임 개발의 단계에 따라 섹션이 구성되어 있으므로 필요할 때 정보를 찾아 볼 위치를 알 수 있습니다.

시작하기 위해 [게임 개발 리소스](#resources) 섹션에서 설명서, 프로그램 및 게임을 만들 때 도움이 되는 기타 리소스의 고급 설문 조사를 제공합니다.

이 가이드는 추가 Windows 10 게임 개발 리소스 및 자료가 사용 가능해질 때 업데이트됩니다.

## 게임 개발 리소스


설명서에서 개발자 프로그램, 포럼, 블로그 및 샘플에 이르기까지 게임 개발 과정에 도움을 줄 수 있는 많은 리소스가 있습니다. 다음은 Windows 10 게임 개발을 시작할 때 알아야 할 리소스입니다.

> **참고** Xbox One 개발 및 선택 Windows 10 게임 기능(예: Xbox Live 서비스)은 ID@Xbox 및 Microsoft Studios와 같은 프로그램을 통해 관리됩니다. 이 가이드에서는 광범위한 리소스를 설명하므로, 사용하는 프로그램 또는 특정 개발 역할에 따라 일부 리소스에는 액세스할 수 없을 수도 있습니다. 예제는 developer.xboxlive.com, forums.xboxlive.com, xdi.xboxlive.com 또는 GDN(게임 개발자 네트워크)으로 확인되는 링크입니다. Microsoft와의 파트너 제휴 방법에 대한 내용은 [개발자 프로그램](#programs)을 참조하세요.

 

### 게임 개발 설명서

이 가이드 전체에서 작업, 기술 및 게임 개발 단계로 구성된 관련 설명서에 대한 딥 링크를 찾을 수 있습니다. 어떤 것들을 사용할 수 있는지 전체적으로 파악할 수 있도록 Windows 10 게임 개발에 대한 주요 설명서 포털이 다음에 나와 있습니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Windows 개발자 센터 주 포털</td>
        <td>[Windows Dev Center](https://dev.windows.com)</td>
    </tr>
    <tr>
        <td>Windows 앱 개발</td>
        <td>[Develop Windows apps](https://dev.windows.com/develop)</td>
    </tr>
    <tr>
        <td>유니버설 Windows 플랫폼 앱 개발</td>
        <td>[How-to guides for Windows 10 apps](https://msdn.microsoft.com/library/windows/apps/mt244352)</td>
    </tr>
    <tr>
        <td>UWP 게임 사용 방법 가이드</td>
        <td>[Games and DirectX](index.md) </td>
    </tr>
    <tr>
        <td>DirectX 참조 및 개요</td>
        <td>[DirectX Graphics and Gaming](https://msdn.microsoft.com/library/windows/desktop/ee663274)</td>
    </tr>
    <tr>
        <td>Xbox Live 설명서</td>
        <td>[Xbox Live SDK](http://aka.ms/xsapi2)</td>
    </tr>
    <tr>
        <td>Xbox One 개발자 설명서(GDN)</td>
        <td>[Xbox One XDK documentation](https://developer.xboxlive.com/platform/development/documentation/Pages/home.aspx)</td>
    </tr>
    <tr>
        <td>Xbox One 개발자 백서(GDN)</td>
        <td>[White Papers](https://developer.xboxlive.com/platform/development/education/Pages/WhitePapers.aspx)</td>
    </tr>     
</table>


### 개발자 프로그램

Microsoft는 Windows 게임을 개발하고 게시하는 데 도움이 되도록 여러 개발자 프로그램을 제공합니다. Windows 스토어에 게임을 게시하려면 Windows 개발자 센터에서 개발자 계정을 만들어야 합니다. 게임과 스튜디오 요구 사항에 따라 다른 프로그램이 유용할 수 있으며 Xbox One 개발 및 Xbox Live 통합과 같은 기회를 만들 수 있습니다.

### Windows 개발자 센터

Windows 개발자 센터에서 개발자 계정을 등록하는 작업은 Windows 게임을 게시하기 위한 첫 번째 단계입니다. 개발자 계정을 사용하여 게임의 이름을 예약하고, 무료 또는 유료 게임을 모든 Windows 디바이스용 Windows 스토어에 제출할 수 있습니다. 개발자 계정을 사용하여 게임 및 게임 내 제품을 관리하고, 자세하게 분석하며, 전 세계 플레이어를 위해 멋진 환경을 만드는 서비스를 사용하도록 설정할 수 있습니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>개발자 계정 등록</td>
        <td>[Ready to sign up?](https://msdn.microsoft.com/library/windows/apps/bg124287)</td>
    </tr>
</table>  


### ID@Xbox

ID@Xbox 프로그램을 사용하면 정규 게임 개발자가 Windows 및 Xbox One에 게임을 직접 게시할 수 있습니다. Xbox One용 개발 또는 Windows 10 게임에 게이머 점수, 도전 과제 및 순위표와 같은 Xbox Live 기능을 추가하려는 경우 ID@Xbox에 등록합니다. ID@Xbox 개발자로 등록하여 창의력을 발휘하고 성공 가능성을 극대화하는 데 필요한 도구와 지원을 받으시기 바랍니다. ID@Xbox에 적용하기 전에 Windows 개발자 센터에서 개발자 계정을 등록하세요.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>ID@Xbox 개발자 프로그램</td>
        <td>[Independent Developer Program for Xbox One](http://go.microsoft.com/fwlink/p/?LinkID=526271)</td>
    </tr>
    <tr>
        <td>ID@Xbox 소비자 사이트</td>
        <td>[ID@Xbox](http://www.idatxbox.com/)</td>
    </tr>
</table>


### DirectX 미리 경험하기 프로그램

Direct3D 12 API 변경 사항을 미리 경험하고 포럼에 대한 피드백을 제공하려는 전문 게임 개발자는 DirectX 미리 경험하기 프로그램에 참가할 수 있습니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>DirectX 12 미리 경험하기 프로그램 등록</td>
        <td>[DirectX Early Access Program](http://1drv.ms/1dgelm6)</td>
    </tr>
</table>


### Xbox 도구 및 미들웨어

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


### 게임 샘플

Windows 10 게임 기능을 이해하고 게임 개발을 빠르게 시작하는 데 도움이 되는 많은 Windows 10 게임 및 앱 샘플이 있습니다. 더 많은 샘플이 정기적으로 개발 및 게시되므로 가끔 샘플 포털을 다시 확인하여 새 소식을 확인하세요. 또한 GitHub 리포지토리가 변경 및 추가 사항에 대한 알림을 받는지 [확인](https://help.github.com/articles/watching-repositories/)할 수 있습니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>유니버설 Windows 플랫폼 앱 샘플</td>
        <td>[Windows-universal-samples](https://github.com/Microsoft/Windows-universal-samples)</td>
    </tr>
    <tr>
        <td>Direct3D 12 그래픽 샘플</td>
        <td>[DirectX-Graphics-Samples](https://github.com/Microsoft/DirectX-Graphics-Samples)</td>
    </tr>
    <tr>
        <td>Direct3D 11 1인칭 게임 샘플</td>
        <td>[Create a simple UWP game with DirectX](tutorial--create-your-first-metro-style-directx-game.md)</td>
    </tr>
    <tr>
        <td>Direct2D 사용자 지정 이미지 효과 샘플</td>
        <td>[D2DCustomEffects](http://go.microsoft.com/fwlink/p/?LinkId=620531)</td>
    </tr>
    <tr>
        <td>Direct2D 그라데이션 메시 샘플</td>
        <td>[D2DGradientMesh](http://go.microsoft.com/fwlink/p/?LinkId=620532)</td>
    </tr>
    <tr>
        <td>Direct2D 사진 보정 샘플</td>
        <td>[D2DPhotoAdjustment](http://go.microsoft.com/fwlink/p/?LinkId=620533)</td>
    </tr>
    <tr>
        <td>Xbox One 게임 샘플(GDN)</td>
        <td>[Samples](https://developer.xboxlive.com/platform/development/education/Pages/Samples.aspx)</td>
    </tr>
    <tr>
        <td>Windows 8 게임 샘플(MSDN 코드 갤러리)</td>
        <td>[Windows Store game samples](https://code.msdn.microsoft.com/windowsapps/site/search?f%5B0%5D.Type=SearchText&f%5B0%5D.Value=game&f%5B1%5D.Type=Contributors&f%5B1%5D.Value=Microsoft&f%5B1%5D.Text=Microsoft)</td>
    </tr>
    <tr>
        <td>JavaScript 및 HTML5 게임 샘플</td>
        <td>[JavaScript and HTML5 touch game sample](https://code.msdn.microsoft.com/windowsapps/JavaScript-and-HTML5-touch-d96f6031)</td>
    </tr>      
</table>


### 개발자 포럼

개발자 포럼은 게임 개발 질문을 묻고 대답하며 게임 개발 커뮤니티와 연결하는 데 최적의 장입니다. 포럼은 또한 과거에 개발자가 직면해 해결했던 어려운 문제에 대한 기존 대답을 찾는 환상적인 리소스가 될 수 있습니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Windows 개발자 센터 앱 개발자 포럼</td>
        <td>[Windows app forums](https://social.msdn.microsoft.com/Forums/windowsapps/home)</td>
    </tr>
    <tr>
        <td>Windows 개발자 센터 데스크톱 응용 프로그램 개발자 포럼</td>
        <td>[Windows desktop application forums](https://social.msdn.microsoft.com/Forums/windowsdesktop/home)</td>
    </tr>
    <tr>
        <td>DirectX Windows 스토어 게임(보관된 포럼 게시물)</td>
        <td>[Building Windows Store games with DirectX (archived)](https://social.msdn.microsoft.com/Forums/vstudio/en-US/home?forum=wingameswithdirectx)</td>
    </tr>
    <tr>
        <td>Windows 10 관리 파트너 개발자 포럼</td>
        <td>[XBOX Developer Forums: Windows 10](http://aka.ms/win10devforums)</td>
    </tr>
    <tr>
        <td>DirectX 미리 경험하기 프로그램 포럼</td>
        <td>[DirectX 12 forum](http://directx12forum.azurewebsites.net/index.php)</td>
    </tr>
</table>


### 개발자 블로그

개발자 블로그는 게임 개발에 대한 최신 정보의 또 다른 좋은 리소스입니다. 새로운 기능, 구현 세부 정보, 모범 사례, 아키텍처 배경 등에 대한 게시물을 찾을 수 있습니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Windows용 앱 빌드 블로그</td>
        <td>[Building Apps for Windows](http://blogs.windows.com/buildingapps/)</td>
    </tr>
    <tr>
        <td>Windows 10(블로그 게시물)</td>
        <td>[Posts in Windows 10](http://blogs.windows.com/blog/tag/windows-10/)</td>
    </tr>
    <tr>
        <td>Visual Studio 엔지니어링 팀 블로그</td>
        <td>[The Visual Studio Blog](http://blogs.msdn.com/b/visualstudio/)</td>
    </tr>
    <tr>
        <td>Visual Studio 개발자 도구 블로그</td>
        <td>[Developer Tools Blogs](http://blogs.msdn.com/b/developer-tools/)</td>
    </tr>
    <tr>
        <td>Somasegar의 개발자 도구 블로그</td>
        <td>[Somasegar’s blog](http://blogs.msdn.com/b/somasegar/)</td>
    </tr>
    <tr>
        <td>DirectX 개발자 블로그</td>
        <td>[DirectX Developer blog](http://blogs.msdn.com/b/directx)</td>
    </tr>
    <tr>
        <td>DirectX 12 소개(블로그 게시물)</td>
        <td>[DirectX 12](http://blogs.msdn.com/b/directx/archive/2014/03/20/directx-12.aspx)</td>
    </tr>
    <tr>
        <td>Visual C++ 도구 팀 블로그</td>
        <td>[Visual C++ team blog](http://blogs.msdn.com/b/vcblog/)</td>
    </tr>
    <tr>
        <td>ID@Xbox 개발자 블로그</td>
        <td>[ID@XBOX Developer Blog](http://www.idatxbox.com/category/developer-blog/)</td>
    </tr>
</table>
 

## 개념 및 계획


개념 및 계획 단계에서는 게임이 어떤 양상으로 나타나고 게임에 활력을 불어넣기 위해 어떤 기술과 도구를 사용할지 결정합니다.

### 게임 개발 기술 개요

UWP 게임 개발을 시작할 때 그래픽, 입력, 오디오, 네트워킹, 유틸리티 및 라이브러리에 대한 사용 가능한 여러 옵션이 있습니다.

게임에 사용할 모든 기술을 이미 결정했다면 훌륭합니다. 그렇지 않다면 [UWP 앱용 게임 기술](game-development-platform-guide.md) 가이드는 사용 가능한 많은 기술에 대한 좋은 개요이며 옵션 및 옵션이 어떻게 함께 작동하는지를 이해하기 위해 읽는 것이 좋습니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>UWP 게임 기술의 설문 조사</td>
        <td>[Game technologies for UWP apps](game-development-platform-guide.md)</td>
    </tr>
</table>
 

이러한 세 가지 GDC 2015 동영상에서는 Windows 10의 게임 개발 및 Windows 10 게임 환경의 개요를 제공합니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Windows 10 게임 개발 개요(동영상)</td>
        <td>[Developing Games for Windows 10](http://channel9.msdn.com/Events/GDC/GDC-2015/Developing-Games-for-Windows-10)</td>
    </tr>
    <tr>
        <td>Windows 10 게임 환경(동영상)</td>
        <td>[Gaming Consumer Experience on Windows 10](http://channel9.msdn.com/Events/GDC/GDC-2015/Gaming-Consumer-Experience-on-Windows-10)</td>
    </tr>
    <tr>
        <td>Microsoft 에코시스템에서의 게임(동영상)</td>
        <td>[The Future of Gaming Across the Microsoft Ecosystem](http://channel9.msdn.com/Events/GDC/GDC-2015/The-Future-of-Gaming-Across-the-Microsoft-Ecosystem)</td>
    </tr>
</table>
 

### 그래픽 기술 및 프로그래밍 언어 선택

Windows 10 게임에서 사용할 수 있는 프로그래밍 언어 및 그래픽 기술이 몇 가지 있습니다. 선택하는 경로와 방향은 개발하고 있는 게임 유형, 개발 회사의 경험 및 선호도, 게임의 고유한 기능 요구 사항에 따라 다릅니다. C#, C++ 또는 JavaScript를 사용하나요? DirectX, XAML 또는 HTML5를 사용하나요?

### DirectX

Microsoft DirectX는 고성능 2D 및 3D 그래픽과 멀티미디어를 위한 최고의 선택입니다. Windows 10의 새 기능인 Direct3D 12는 콘솔과 유사한 수준의 API 성능을 제공하며 이전보다 더욱 빠르고 효율적입니다. 게임은 최신 그래픽 하드웨어를 완벽히 활용하고 더 많은 개체, 더욱 풍부한 장면 및 향상된 효과를 구현할 수 있습니다. Direct3D 12는 Windows 10 PC 및 Xbox One에 최적화된 그래픽을 제공합니다. Direct3D 11의 친숙한 그래픽 파이프라인을 사용하려는 경우에는 여전히 Direct3D 11.3에 추가된 새로운 렌더링 및 최적화 기능의 이점을 누릴 수 있습니다. Win32의 루트로 신뢰할 수 있는 데스크톱 Windows API 개발자인 경우 Windows 10에도 해당 옵션이 있습니다.

DirectX의 폭넓은 기능과 심층적인 플랫폼 통합은 가장 까다로운 게임에 필요한 기능과 성능을 제공합니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>DirectX 게임 사용 방법 가이드</td>
        <td>[Games and DirectX](index.md)</td>
    </tr>
    <tr>
        <td>DirectX 개요 및 참조</td>
        <td>[DirectX Graphics and Gaming](https://msdn.microsoft.com/library/windows/desktop/ee663274)</td>
    </tr>
    <tr>
        <td>Direct3D 12 프로그래밍 가이드 및 참조</td>
        <td>[Direct3D 12 Graphics](https://msdn.microsoft.com/library/windows/desktop/dn903821)</td>
    </tr>
</table>
 

### XAML

XAML은 애니메이션, 스토리보드, 데이터 바인딩, 확장 가능한 벡터 기반 그래픽, 동적 크기 재조정 및 장면 그래프와 같은 편리한 기능이 있는 사용하기 쉬운 선언적 UI 언어입니다. XAML은 게임 UI, 메뉴, 스프라이트 및 2D 그래픽에서 훌륭히 작동합니다. UI 레이아웃을 쉽게 조정할 수 있도록 XAML은 Expression Blend, Microsoft Visual Studio 등의 디자인 및 개발 도구와 호환됩니다. XAML은 주로 C#과 함께 사용되지만, C++를 더욱 선호하거나 게임이 고성능 CPU 사양을 요구하는 경우 C++와 함께 사용해도 좋습니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>XAML 플랫폼 개요</td>
        <td>[XAML platform](https://msdn.microsoft.com/library/windows/apps/mt228259)</td>
    </tr>
    <tr>
        <td>XAML UI 및 컨트롤</td>
        <td>[Controls, layouts, and text](https://msdn.microsoft.com/library/windows/apps/mt228348)</td>
    </tr>
</table>
 

### HTML 5

HTML(HyperText Markup Language)은 웹 페이지, 앱 및 리치 클라이언트에서 사용하는 공용 UI 생성 언어입니다. Windows 게임은 HTML의 익숙한 기능, 유니버설 Windows 플랫폼에 대한 액세스, AppCache, 웹 작업자, 캔버스, 끌어서 놓기, 비동기 프로그래밍 및 SVG와 같은 최신 웹 기능에 대한 지원 등 모든 기능을 갖춘 표시 계층으로 HTML5를 사용할 수 있습니다. HTML 렌더링은 백그라운드에서 DirectX 하드웨어 가속 기능을 이용하므로 추가 코드를 작성하지 않고도 계속 DirectX의 성능 이점을 활용할 수 있습니다. HTML5는 웹 개발 및 웹 게임 포팅에 능숙하거나 다른 선택보다는 접근하기에 쉬울 수 있는 언어 및 그래픽 계층을 사용하려는 경우에 적합한 선택입니다. HTML5는 JavaScript와 함께 사용할 수 있지만, C# 또는 C++/CX로 만든 구성 요소로 호출할 수도 있습니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>HTML5 및 문서 개체 모델 정보</td>
        <td>[HTML and DOM reference](https://msdn.microsoft.com/library/windows/apps/br212882.aspx)</td>
    </tr>
    <tr>
        <td>HTML5 W3C 권장 사항</td>
        <td>[HTML5](http://go.microsoft.com/fwlink/p/?linkid=221374)</td>
    </tr>
</table>
 

### 프레젠테이션 기술 결합

Microsoft DXGI(DirectX Graphic Infrastructure)는 여러 그래픽 기술 간에 상호 운용성 및 호환성을 제공합니다. 고성능 그래픽을 위해 메뉴 및 기타 단순한 UI에는 XAML을 사용하고 복잡한 2D 및 3D 장면의 렌더링에는 DirectX를 사용하는 등 XAML과 DirectX를 조합할 수 있습니다. 또한 DXGI는 Direct2D, Direct3D, DirectWrite, DirectCompute, Microsoft 미디어 파운데이션 간에 호환성을 제공합니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>DirectX Graphic Infrastructure 프로그래밍 가이드 및 참조</td>
        <td>[DXGI](https://msdn.microsoft.com/library/windows/desktop/hh404534)</td>
    </tr>
    <tr>
        <td>DirectX 및 XAML 결합</td>
        <td>[DirectX and XAML interop](directx-and-xaml-interop.md)</td>
    </tr>
</table>
 

### C++

C++/CX는 속도, 호환성 및 플랫폼 액세스의 강력한 조합을 제공하는 오버헤드가 낮은 고성능 언어입니다. C++/CX를 사용하면 DirectX 및 Xbox Live를 비롯하여 Windows 10의 모든 멋진 게임 기능을 쉽게 활용할 수 있습니다. 또한 기존의 C++ 코드 및 라이브러리도 재사용할 수 있습니다. C++/CX는 가비지 수집의 오버헤드를 발생시키지 않는 빠른 네이티브 코드를 생성하므로 게임이 탁월한 성능을 발휘하고 전력 소모가 줄어들어 배터리 사용 시간이 늘어날 수 있습니다. DirectX 또는 XAML과 함께 C++/CX를 사용하거나 둘의 조합을 사용하는 게임을 만들어 보세요.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>C++/CX 참조 및 개요</td>
        <td>[Visual C++ Language Reference (C++/CX)](https://msdn.microsoft.com/library/windows/apps/hh699871.aspx)</td>
    </tr>
    <tr>
        <td>Visual C++ 프로그래밍 가이드 및 참조</td>
        <td>[Visual C++ in Visual Studio 2015](https://msdn.microsoft.com/library/60k1461a.aspx)</td>
    </tr>
</table>
 

### C#

C#("C 샤프"라고 발음)는 혁신적인 최신 언어로, 단순하고 강력하며 형식이 안전하고 개체 지향적입니다. C#를 사용하면 C 스타일 언어의 익숙함과 표현력은 유지하면서 신속하게 개발할 수 있습니다. C#는 사용하기 쉽지만, 다형성, 대리자, 람다, 종결, 반복기 메서드, 공변성(Covariance) 및 LINQ(Language-Integrated Query) 식과 같은 여러 고급 언어 기능을 제공합니다. XAML을 사용할 계획이거나 게임 개발을 서두르려는 경우 또는 이전에 C# 사용 경험이 있는 경우 C#를 선택하는 것이 좋습니다. C#은 대개 XAML과 함께 사용하므로 DirectX를 사용하려는 경우에는 C++를 선택하거나, DirectX를 조작하는 C++ 구성 요소로 게임의 일부를 작성하세요. 또는 C# 및 C++용 직접 실행 모드 Direct2D 그래픽 라이브러리인 [Win2D](https://github.com/Microsoft/Win2D) 사용을 고려합니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>C# 프로그래밍 가이드 및 참조</td>
        <td>[C# language reference](https://msdn.microsoft.com/library/kx37x362.aspx)</td>
    </tr>
</table>
 

### JavaScript

JavaScript는 최신 웹 및 리치 클라이언트 응용 프로그램에서 널리 사용되는 동적 스크립트 언어입니다.

Windows JavaScript 앱은 개체 지향 JavaScript 클래스의 메서드 및 속성으로 쉽고 직관적인 방식으로 유니버설 Windows 플랫폼의 강력한 기능에 액세스할 수 있습니다. 기존에 웹 개발 환경을 사용하고 있거나 JavaScript에 이미 익숙하거나 HTML5, CSS, WinJS 또는 JavaScript 라이브러리를 사용하려는 경우 JavaScript가 게임에 가장 적합한 선택입니다. DirectX 또는 XAML을 사용하려는 경우에는 JavaScript 대신에 C# 또는 C++/CX를 선택하세요.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>JavaScript 및 Windows 런타임 참조</td>
        <td>[JavaScript reference](https://msdn.microsoft.com/library/windows/apps/jj613794)</td>
    </tr>
</table>


### Windows 런타임 구성 요소를 사용하여 언어 조합

유니버설 Windows 플랫폼에서는 다른 언어로 작성된 구성 요소를 조합하기가 쉽습니다. C++, C# 또는 Visual Basic으로 Windows 런타임 구성 요소를 생성한 후 JavaScript, C#, C++ 또는 Visual Basic에서 해당 요소로 호출합니다. 이것은 선택한 언어로 게임 부분을 프로그래밍하는 훌륭한 방법입니다. 또한 구성 요소를 통해 특정 언어에서만 사용 가능한 외부 라이브러리는 물론 이미 작성해둔 레거시 코드를 사용할 수도 있습니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Windows 런타임 구성 요소를 만드는 방법</td>
        <td>[Creating Windows Runtime Components](https://msdn.microsoft.com/library/windows/apps/hh441572.aspx)</td>
    </tr>
</table>


### 게임에 어떤 버전의 DirectX를 사용해야 하나요?

게임을 위해 DirectX를 선택하는 경우 Microsoft Direct3D 12 또는 Microsoft Direct3D 11 중 어떤 버전을 사용할지 결정해야 합니다.

Direct3D 11.3은 익숙한 Direct3D 프로그래밍 모델을 사용하는 낮은 수준의 그래픽 API입니다. Direct3D 11은 여전히 유니버설 Windows 앱에 대한 옵션이며, Direct3D 11.3에 추가된 새로운 렌더링 및 최적화 기능에 액세스할 수 있습니다.

Direct3D 12는 Windows 10의 새로운 기능이며 새로운 파이프라인 프로그래밍 모델을 제공합니다. Direct3D 12는 하드웨어에 더 가깝고, 추상화를 덜 사용하며 게임에 리소스 사용에 대한 강화된 제어를 제공합니다. Direct3D 12에는 더 나은 CPU, GPU 및 전원 성능이 있습니다.

Direct3D 11에서 작성된 기존 엔진이 있고 Direct3D 12로 전환할 준비가 되지 않은 경우 12에서 Direct3D 11을 사용하여 일부 성능을 향상시키고 Direct3D 12로 전환을 시작할 수 있습니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Direct3D 12 또는 Direct3D 11 선택</td>
        <td>[What is Direct3D 12?](https://msdn.microsoft.com/library/windows/desktop/dn899228)</td>
    </tr>
    <tr>
        <td>Direct3D 11 개요</td>
        <td>[Direct3D 11 Graphics](https://msdn.microsoft.com/library/windows/desktop/ff476080)</td>
    </tr>
    <tr>
        <td>12에서 Direct3D 11 개요</td>
        <td>[Direct3D 11 on 12](https://msdn.microsoft.com/library/windows/desktop/dn913195)</td>
    </tr>
</table>


### 브리지, 게임 엔진 및 미들웨어

게임의 요구 사항에 따라 브리지, 게임 엔진 또는 미들웨어를 사용하면 개발 및 테스트의 시간과 리소스를 절약할 수 있습니다. 다음은 적합한 사항을 결정하는 데 도움이 되는 브리지, 게임 엔진 및 미들웨어에 대한 몇 가지 개요 및 리소스입니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Windows 10용 브리지 및 게임 엔진(블로그 게시물)</td>
        <td>[More ways to bring your code to fast-growing Windows 10 Store](http://blogs.windows.com/buildingapps/2015/09/17/more-ways-to-bring-your-code-to-fast-growing-windows-10-store/)</td>
    </tr>
    <tr>
        <td>미들웨어로 게임 개발(동영상)</td>
        <td>[Accelerating Windows Store Game Development with Middleware](https://channel9.msdn.com/Events/Build/2013/3-187)</td>
    </tr>
    <tr>
        <td>Visual Studio 및 Unity, Unreal 및 Cocos2d(블로그 게시물)</td>
        <td>[Visual Studio for Game Development: New Partnerships with Unity, Unreal Engine and Cocos2d](http://blogs.msdn.com/b/somasegar/archive/2015/04/17/visual-studio-for-game-development-new-partnerships-with-unity-unreal-engine-and-cocos2d.aspx)</td>
    </tr>
    <tr>
        <td>게임 미들웨어 소개(블로그 게시물)</td>
        <td>[Game Development Middleware - What is it? Do I need it?](http://blogs.msdn.com/b/wsdevsol/archive/2014/05/02/game-development-middleware-what-is-it-do-i-need-it.aspx)</td>
    </tr>
</table>
 

### 유니버설 Windows 플랫폼 브리지

유니버설 Windows 플랫폼 브리지는 기존 앱이나 게임을 UWP로 가져오는 기술입니다. 브리지는 UWP 게임 개발을 빠르게 시작할 수 있는 좋은 방법입니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>UWP 브리지</td>
        <td>[Bring your code to Windows](https://dev.windows.com/bridges/)</td>
    </tr>
    <tr>
        <td>iOS용 Windows 브리지</td>
        <td>[Bring your iOS apps to Windows](https://dev.windows.com/bridges/ios)</td>
    </tr>
    <tr>
        <td>.NET 및 Win32용 Windows 브리지("Project Centennial") 미리 보기</td>
        <td>[Windows Developer Preview Programs](http://go.microsoft.com/fwlink/p/?LinkID=624543)</td>
    </tr>
</table>
 

### Unity

Unity 5는 수상 경력이 있는 차세대 개발 플랫폼으로, 2D 및 3D 게임과 대화형 환경을 만드는 데 적합합니다. Unity 5는 완전히 새로운 예술적 성능, 향상된 그래픽 기능, 개선된 효율성을 보장합니다.

[Unity 로드맵](https://unity3d.com/unity/roadmap)에서 DirectX 12는 Unity의 이후 버전에서 지원됩니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Unity 게임 엔진</td>
        <td>[Unity - Game Engine](http://unity3d.com/)</td>
    </tr>
    <tr>
        <td>Unity 5 다운로드</td>
        <td>[Get Unity](http://unity3d.com/get-unity)</td>
    </tr>
    <tr>
        <td>Unity 5.2의 유니버설 Windows 플랫폼 앱 지원(블로그 게시물)</td>
        <td>[Windows 10 Universal Platform apps in Unity 5.2](http://blogs.unity3d.com/2015/09/09/windows-10-universal-apps-in-unity-5-2/)</td>
    </tr>
    <tr>
        <td>Windows용 Unity 설명서</td>
        <td>[Unity Manual / Windows](http://docs.unity3d.com/Manual/Windows.mdl)</td>
    </tr>
    <tr>
        <td>Unity 게임을 유니버설 Windows 플랫폼 앱으로 게시(동영상)</td>
        <td>[How to publish your Unity game as a UWP app](https://channel9.msdn.com/Blogs/One-Dev-Minute/How-to-publish-your-Unity-game-as-a-UWP-app)</td>
    </tr>
    <tr>
        <td>Unity로 Windows 게임 및 앱 만들기(동영상)</td>
        <td>[Making Windows games and apps with Unity](https://channel9.msdn.com/Blogs/One-Dev-Minute/Making-games-and-apps-with-Unity)</td>
    </tr>
    <tr>
        <td>Visual Studio를 사용한 Unity 게임 개발(동영상 시리즈)</td>
        <td>[Using Unity with Visual Studio 2015](http://go.microsoft.com/fwlink/?LinkId=722359)</td>
    </tr>
</table>
 

### Havok

Havok의 모듈식 도구 및 기술 모음은 게임 작성자가 새로운 수준의 조작 및 몰입도에 도달하도록 지원합니다. Havok은 매우 현실적인 물리학, 조작 시뮬레이션 및 멋진 동영상을 사용할 수 있도록 해줍니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Havok 웹 사이트</td>
        <td>[Havok](http://www.havok.com/)</td>
    </tr>
    <tr>
        <td>Havok 도구 제품군</td>
        <td>[Havok Product Overview](http://www.havok.com/products/)</td>
    </tr>
    <tr>
        <td>Havok 지원 포럼</td>
        <td>[Havok](https://software.intel.com/forums/havok/)</td>
    </tr>
</table>
 

### Cocos2d

Cocos2d-X는 플랫폼 간 오픈 소스 게임 개발 엔진이며 UWP 게임 빌드를 지원하는 도구 제품군입니다. 버전 3부터는 3D 기능도 추가됩니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Cocos2d-x</td>
        <td>[What is Cocos2d-X?](http://www.cocos2d-x.org/)</td>
    </tr>
    <tr>
        <td>Cocos2d-x 프로그래머 가이드</td>
        <td>[Cocos2d-x Programmers Guide v3.8](http://www.cocos2d-x.org/programmersguide/)</td>
    </tr>
    <tr>
        <td>Windows 10의 Cocos2d-x(블로그 게시물)</td>
        <td>[Running Cocos2d-x on Windows 10](https://blogs.windows.com/buildingapps/2015/06/15/running-cocos2d-x-on-windows-10/)</td>
    </tr>
    <tr>
        <td>Cocos2d-x Windows 스토어 게임(동영상)</td>
        <td>[Build a Game with Cocos2d-x for Windows Devices](http://www.microsoftvirtualacademy.com/training-courses/build-a-game-with-cocos2d-x-for-windows-devices)</td>
    </tr>
</table>


### Unreal Engine

Unreal Engine 4는 모든 유형의 게임과 개발자를 위한 완벽한 게임 개발 도구 모음입니다. Unreal Engine은 가장 까다로운 콘솔 및 PC 게임에 이르기까지 전 세계의 다양한 게임 개발자가 사용합니다. Unreal Engine 4에 가입한 [DirectX 12 미리 경험하기 프로그램](#dxeap) 회원은 DirectX 12를 지원하는 Unreal Engine 4.4 개발 프로젝트에 액세스할 수 있습니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Unreal Engine 개요</td>
        <td>[What is Unreal Engine 4](https://www.unrealengine.com/what-is-unreal-engine-4)</td>
    </tr>
</table>
 

### 미들웨어 및 파트너

게임 개발 요구 사항에 따라 솔루션을 제공할 수 있는 다른 미들웨어 및 엔진 파트너가 많이 있습니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Windows 개발자 센터 게임 파트너</td>
        <td>[Dev Center Partners (Gaming)](https://devcenterpartners.windows.com/directory#filter=gaming)</td>
    </tr>
    <tr>
        <td>Windows 개발자 센터 파트너</td>
        <td>[Dev Center Partners](https://devcenterpartners.windows.com/directory)</td>
    </tr>
</table>
 

### 게임 포팅

기존 게임이 있는 경우 게임을 UWP에 신속하게 가져오는 데 도움이 되는 많은 리소스 및 가이드가 있습니다. 포팅 작업을 신속하게 시작하려면 [유니버설 Windows 플랫폼 브리지](#uwp_bridges) 사용 또한 고려할 수 있습니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Windows 8 앱을 유니버설 Windows 플랫폼 앱으로 포팅</td>
        <td>[Move from Windows Runtime 8.x to UWP](https://msdn.microsoft.com/library/windows/apps/mt238322)</td>
    </tr>
    <tr>
        <td>Windows 8 앱을 유니버설 Windows 플랫폼 앱으로 포팅(동영상)</td>
        <td>[Porting 8.1 Apps to Windows 10](https://channel9.msdn.com/Series/A-Developers-Guide-to-Windows-10/21)</td>
    </tr>
    <tr>
        <td>iOS 앱을 유니버설 Windows 플랫폼 앱으로 포팅</td>
        <td>[Move from iOS to UWP](https://msdn.microsoft.com/library/windows/apps/mt238320)</td>
    </tr>
    <tr>
        <td>Silverlight 앱을 유니버설 Windows 플랫폼 앱으로 포팅</td>
        <td>[Move from Windows Phone Silverlight to UWP](https://msdn.microsoft.com/library/windows/apps/mt238323)</td>
    </tr>
    <tr>
        <td>XAML 또는 Silverlight에서 유니버설 Windows 플랫폼 앱으로 포팅(동영상)</td>
        <td>[Porting an App from XAML or Silverlight to Windows 10](https://channel9.msdn.com/Events/Build/2015/3-741)</td>
    </tr>
    <tr>
        <td>Xbox 게임을 유니버설 Windows 플랫폼 앱으로 포팅</td>
        <td>[Porting from Xbox One to Windows 10 UWP](https://developer.xboxlive.com/platform/development/education/Documents/Porting%20from%20Xbox%20One%20to%20Windows%2010.aspx)</td>
    </tr>
    <tr>
        <td>DirectX 9에서 DirectX 11로 포팅</td>
        <td>[Port from DirectX 9 to Universal Windows Platform (UWP)](porting-your-directx-9-game-to-windows-store.md)</td>
    </tr>
    <tr>
        <td>Direct3D 11에서 Direct3D 12로 포팅</td>
        <td>[Porting from Direct3D 11 to Direct3D 12](https://msdn.microsoft.com/library/windows/desktop/mt431709)</td>
    </tr>
    <tr>
        <td>OpenGL ES에서 Direct3D 11로 포팅</td>
        <td>[Port from OpenGL ES 2.0 to Direct3D 11](port-from-opengl-es-2-0-to-directx-11-1.md)</td>
    </tr>
    <tr>
        <td>ANGLE을 사용하여 OpenGL ES에서 Direct3D 11로</td>
        <td>[ANGLE](http://go.microsoft.com/fwlink/p/?linkid=618387)</td>
    </tr>
    <tr>
        <td>UWP에도 제공되는 클래식 Windows API</td>
        <td>[Alternatives to Windows APIs in Universal Windows Platform (UWP) apps](https://msdn.microsoft.com/library/windows/apps/hh464945)</td>
    </tr>
</table>


## 프로토타입 및 디자인


이제 만들려는 게임 유형과 빌드하는 데 사용할 도구 및 그래픽 기술을 결정하였으므로 디자인 및 프로토타입을 시작할 준비가 되었습니다. 기본적으로 게임은 유니버설 Windows 플랫폼 앱이므로 이 사실에서부터 시작해야 합니다.

### UWP(유니버설 Windows 플랫폼) 소개

Windows 10에서는 Windows 10 디바이스에서 공통 API 플랫폼을 제공하는 UWP(유니버설 Windows 플랫폼)를 도입합니다. UWP는 Windows 런타임 모델을 발전시키고 일관된 통합 코어로 향상합니다. UWP를 대상으로 하는 게임은 모든 디바이스에 공통인 WinRT API를 호출할 수 있습니다. UWP에서 보장된 핵심 API 계층을 제공하므로, Windows 10 디바이스에서 설치하는 단일 앱 패키지를 만들 수도 있습니다. 원하는 경우 게임에서 게임이 실행되는 디바이스와 관련된 API(Win32 및 .NET의 일부 클래식 Windows API 포함)를 계속 호출할 수 있습니다.

UWP의 목표는 다음을 보유하는 것입니다.

-   하나의 핵심 운영 체제
-   하나의 응용 프로그램 플랫폼
-   하나의 게임 소셜 네트워크
-   하나의 스토어
-   하나의 수집 경로

다음은 유니버설 Windows 플랫폼 앱에 대해 자세하게 설명한 가이드이므로 플랫폼 이해를 돕기 위해 읽는 것이 좋습니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>유니버설 Windows 플랫폼 소개</td>
        <td>[What's a Universal Windows Platform app?](https://msdn.microsoft.com/library/windows/apps/dn726767)</td>
    </tr>
    <tr>
        <td>UWP의 개요</td>
        <td>[Guide to UWP apps](https://msdn.microsoft.com/library/windows/apps/dn894631)</td>
    </tr>
</table>
 

### UWP 개발 시작

유니버설 Windows 플랫폼 앱 개발을 설정하고 준비하는 작업은 빠르고 쉽습니다. 다음 가이드는 프로세스를 단계별로 안내합니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>UWP 개발 시작</td>
        <td>[Get started with Windows apps](https://dev.windows.com/getstarted)</td>
    </tr>
    <tr>
        <td>UWP 개발 설정</td>
        <td>[Get set up](https://msdn.microsoft.com/library/windows/apps/dn726766)</td>
    </tr>
</table>

UWP 프로그래밍에 "완전 초보자"이고 게임에서 XAML 사용을 고려하는 경우([그래픽 기술 및 프로그래밍 언어 선택](#choosing_technology) 참조), [완전 초보자를 위한 Windows 10 개발](https://channel9.msdn.com/Series/Windows-10-development-for-absolute-beginners) 동영상 시리즈부터 시작하는 것이 좋습니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>XAML로 Windows 10 개발 초보자 가이드(동영상 시리즈)</td>
        <td>[Windows 10 development for absolute beginners](https://channel9.msdn.com/Series/Windows-10-development-for-absolute-beginners)</td>
    </tr>
    <tr>
        <td>XAML을 사용하여 Windows 10 완전 초보자 시리즈 발표(블로그 게시물)</td>
        <td>[Windows 10 development for absolute beginners](http://blogs.windows.com/buildingapps/2015/09/30/windows-10-development-for-absolute-beginners/)</td>
    </tr>
</table>

### UWP 개발 개념

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>유니버설 Windows 플랫폼 앱 개발 개요</td>
        <td>[Develop Windows apps](https://dev.windows.com/develop)</td>
    </tr>
    <tr>
        <td>UWP의 네트워크 프로그래밍 개요</td>
        <td>[Networking and web services](https://msdn.microsoft.com/library/windows/apps/mt280378)</td>
    </tr>
    <tr>
        <td>게임에서 Windows.Web.HTTP 및 Windows.Networking.Sockets 사용</td>
        <td>[Networking for games](work-with-networking-in-your-directx-game.md)</td>
    </tr>
    <tr>
        <td>UWP의 비동기 프로그래밍 개념</td>
        <td>[Asynchronous programming](https://msdn.microsoft.com/library/windows/apps/mt187335)</td>
    </tr>
</table>
 

### 프로세스 수명 관리

프로세스 수명 관리 또는 앱 수명 주기에서는 유니버설 Windows 플랫폼 앱이 전환할 수 있는 다양한 활성화 상태를 설명합니다. 게임이 활성화, 일시 중단, 다시 시작 또는 종료될 수 있고 다양한 방법으로 이러한 상태 간에 전환할 수 있습니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>앱 수명 주기 전환 처리</td>
        <td>[App lifecycle](https://msdn.microsoft.com/library/windows/apps/mt243287)</td>
    </tr>
    <tr>
        <td>Microsoft Visual Studio를 사용하여 앱 전환 트리거</td>
        <td>[How to trigger suspend, resume, and background events for Windows Store apps in Visual Studio](https://msdn.microsoft.com/library/hh974425.aspx)</td>
    </tr>
</table>
 

### 게임 UX 디자인

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
        <td>[Designing UWP apps](https://dev.windows.com/design)</td>
    </tr>
    <tr>
        <td>앱 수명 주기 상태에 대한 디자인</td>
        <td>[UX guidelines for launch, suspend, and resume](https://msdn.microsoft.com/library/windows/apps/dn611862)</td>
    </tr>
    <tr>
        <td>여러 디바이스 폼 팩터를 대상으로 지정(동영상)</td>
        <td>[Designing Games for a Windows Core World](http://channel9.msdn.com/Events/GDC/GDC-2015/Designing-Games-for-a-Windows-Core-World)</td>
    </tr>
</table>
 

### 색 지침 및 색상표

게임에서 일관된 색 지침을 따르면 심미적인 측면을 개선하고 탐색을 지원하며 플레이어에게 메뉴 및 HUD 기능을 알려주는 강력한 도구로 작동할 수 있습니다. 경고, 손상, XP 및 도전 과제와 같은 게임 요소의 색을 일관되게 지정하면 UI가 더욱 깔끔해지고 명시적 레이블의 필요성이 줄어듭니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>색 가이드</td>
        <td>[Best Practices: Color](https://assets.windowsphone.com/499cd2be-64ed-4b05-a4f5-cd0c9ad3f6a3/101_BestPractices_Color_InvariantCulture_Default.zip)</td>
    </tr>
</table>
 

### 입력 체계

입력 체계를 적절히 사용하면 UI 레이아웃, 탐색, 가독성, 분위기, 브랜드 및 플레이어의 몰입도를 비롯한 게임의 여러 측면을 강화할 수 있습니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>입력 체계 가이드</td>
        <td>[Best Practices: Typography](http://go.microsoft.com/fwlink/?LinkId=535007)</td>
    </tr>
</table>
 

### UI 맵

UI 맵은 게임 탐색 및 순서도로 표현된 메뉴의 레이아웃입니다. UI 맵을 통해 참여하는 모든 관련자가 게임 인터페이스 및 탐색 경로를 쉽게 이해할 수 있으며 개발 주기의 초기에 잠재적인 장애물과 문제점을 알아낼 수 있습니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>UI 맵 가이드</td>
        <td>[Best Practices: UI Map](http://go.microsoft.com/fwlink/?LinkId=535008)</td>
    </tr>
</table>
 

### DirectX 개발

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>UWP의 DirectX 게임 개발</td>
        <td>[Games and DirectX](index.md)</td>
    </tr>
    <tr>
        <td>DirectX의 UWP 앱 모델 조작</td>
        <td>[The app object and DirectX](about-the-metro-style-user-interface-and-directx.md)</td>
    </tr>
    <tr>
        <td>DirectX 개요 및 참조</td>
        <td>[DirectX Graphics and Gaming](https://msdn.microsoft.com/library/windows/desktop/ee663274)</td>
    </tr>
    <tr>
        <td>Direct3D 12 프로그래밍 가이드 및 참조</td>
        <td>[Direct3D 12 Graphics](https://msdn.microsoft.com/library/windows/desktop/dn903821)</td>
    </tr>
    <tr>
        <td>DirectX 12 기본 사항(동영상)</td>
        <td>[Better Power, Better Performance: Your Game on DirectX 12](http://channel9.msdn.com/Events/GDC/GDC-2015/Better-Power-Better-Performance-Your-Game-on-DirectX12)</td>
    </tr>
</table>
 

DirectX 도구 키트, DirectX 텍스처 처리 라이브러리 및 DirectXMesh 기하 도형 처리 라이브러리에서는 텍스처, 메시, 스프라이트 및 기타 유틸리티 기능과 DirectX 개발용 도우미 클래스를 제공합니다. 이러한 라이브러리를 사용하면 이러한 기능을 직접 구현하는 데 비해 많은 시간과 노력을 절약할 수 있습니다. 주로 Direct3D 11용으로 구현되었지만 이러한 라이브러리의 일부는 Direct3D 12에서도 작동합니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>DirectX 도구 키트 가져오기(DirectX 11)</td>
        <td>[DirectXTK](http://go.microsoft.com/fwlink/?LinkId=248929)</td>
    </tr>
    <tr>
        <td>DirectX 텍스처 처리 라이브러리 가져오기(DirectX 11)</td>
        <td>[DirectXTex](http://go.microsoft.com/fwlink/?LinkId=248926)</td>
    </tr>
    <tr>
        <td>DirectXMesh 기하 도형 처리 라이브러리 가져오기</td>
        <td>[DirectXMesh](http://go.microsoft.com/fwlink/?LinkID=324981)</td>
    </tr>
    <tr>
        <td>DirectXTK에서 Direct3D 12 지원(블로그 게시물)</td>
        <td>[Support for DirectX 12](https://github.com/Microsoft/DirectXTK/issues/2)</td>
    </tr>
</table>


## 프로덕션


이제 팀 전체에 작업이 배포되면서 스튜디오가 완전히 참여하고 프로덕션 주기로 이동합니다. 프로토타입을 다듬고 리팩터링하고 확장하면서 전체 게임으로 만들어갑니다.

### 알림 및 라이브 타일

타일은 시작 메뉴에서의 게임 표시를 말합니다. 타일 및 알림을 통해 현재 게임을 플레이하지 않는 경우에도 플레이어의 관심을 끌 수 있습니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>타일 및 배지 개발</td>
        <td>[Tiles, badges, and notifications](https://msdn.microsoft.com/library/windows/apps/mt185606)</td>
    </tr>
    <tr>
        <td>라이브 타일 및 알림을 보여 주는 샘플</td>
        <td>[Notifications sample](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Notifications)</td>
    </tr>
    <tr>
        <td>적응형 타일 템플릿(블로그 게시물)</td>
        <td>[Adaptive Tile Templates - Schema and Documentation](http://blogs.msdn.com/b/tiles_and_toasts/archive/2015/06/30/adaptive-tile-templates-schema-and-documentation.aspx)</td>
    </tr>
    <tr>
        <td>타일 및 배지 디자인</td>
        <td>[Guidelines for tiles and badges](https://msdn.microsoft.com/library/windows/apps/hh465403)</td>
    </tr>
    <tr>
        <td>라이브 타일 템플릿을 대화형으로 개발하기 위한 Windows 10 앱</td>
        <td>[Notifications Visualizer](https://www.microsoft.com/store/apps/9nblggh5xsl1)</td>
    </tr>
</table>
 

### IAP(앱에서 바로 구매 제품) 구매 사용

IAP(앱에서 바로 구매 제품)는 플레이어가 게임 내에서 구입할 수 있는 보충 항목입니다. IAP는 새 추가 기능, 게임 수준, 항목 또는 플레이어가 즐길 수 있는 다른 모든 사항이 될 수 있습니다. IAP를 적절하게 사용하면 게임 환경을 개선하는 한편 수익을 얻을 수 있습니다. Windows 개발자 센터 대시보드를 통해 게임의 IAP를 정의 및 게시하고 게임 코드에서 앱에서 바로 구매를 사용하도록 합니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>앱에서 바로 구매 지속형 제품</td>
        <td>[Enable in-app product purchases](https://msdn.microsoft.com/library/windows/apps/mt219684)</td>
    </tr>
    <tr>
        <td>앱에서 바로 구매 소모성 제품</td>
        <td>[Enable consumable in-app product purchases](https://msdn.microsoft.com/library/windows/apps/mt219683)</td>
    </tr>
    <tr>
        <td>앱에서 바로 구매 제품 세부 정보 및 제출</td>
        <td>[IAP submissions](https://msdn.microsoft.com/library/windows/apps/mt148551)</td>
    </tr>
    <tr>
        <td>게임에 대한 IAP 판매 및 인구 통계 모니터링</td>
        <td>[IAP acquisitions report](https://msdn.microsoft.com/library/windows/apps/mt148538)</td>
    </tr>
</table>
 

### 고급 DirectX 기술 및 개념

DirectX 개발의 일부는 미묘하고 복잡할 수 있습니다. 프로덕션에서 DirectX 엔진의 세부 사항을 알아봐야 하는 지점에 이르거나 어려운 성능 문제를 디버그해야 하는 경우 이 섹션의 리소스 및 정보가 도움이 될 수 있습니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>그래픽 및 성능 최적화(동영상)</td>
        <td>[Advanced DirectX 12 Graphics and Performance](http://channel9.msdn.com/Events/GDC/GDC-2015/Advanced-DirectX12-Graphics-and-Performance)</td>
    </tr>
    <tr>
        <td>DirectX 그래픽 디버깅(동영상)</td>
        <td>[Solve the Tough Graphics Problems with Your Game Using DirectX Tools](http://channel9.msdn.com/Events/GDC/GDC-2015/Solve-the-Tough-Graphics-Problems-with-your-Game-Using-DirectX-Tools)</td>
    </tr>
    <tr>
        <td>Direct3D 12 프로그래밍 가이드</td>
        <td>[Direct3D 12 Programming Guide](https://msdn.microsoft.com/library/windows/desktop/dn903821)</td>
    </tr>
    <tr>
        <td>DirectX 및 XAML 결합</td>
        <td>[DirectX and XAML interop](directx-and-xaml-interop.md)</td>
    </tr>
</table>
 

Microsoft DirectX 제품 팀에서 DirectX 12 개발에 대한 자세한 동영상 시리즈를 만들었습니다. 이러한 동영상에서는 리소스 바인딩, 프레젠테이션 모드, 디버깅, 리소스 장벽 및 기타 많은 DirectX 12 개념의 세부 사항을 설명합니다. 이 시리즈도 가끔 게스트 프리젠터를 특징으로 합니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>그래픽 및 DirectX 12 개발 동영상(YouTube 채널)</td>
        <td>[Microsoft DirectX 12 and Graphics Education](https://www.youtube.com/channel/UCiaX2B8XiXR70jaN7NK-FpA)</td>
    </tr>
    <tr>
        <td>Direct3D 12 리소스 및 힙 관리(동영상)</td>
        <td>[Heaps and Resources in DirectX 12](https://www.youtube.com/channel/UCiaX2B8XiXR70jaN7NK-FpA)</td>
    </tr>
</table>
 

### 세계화 및 지역화

전 세계에서 널리 사용되는 Windows 플랫폼용 게임을 개발하고 Microsoft의 인기 제품에 기본 제공되는 국가별 기능을 알아봅니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>글로벌 시장을 겨냥한 게임 준비</td>
        <td>[Guidelines when developing for a global audience](https://msdn.microsoft.com/library/windows/apps/xaml/mt186453.aspx)</td>
    </tr>
    <tr>
        <td>언어, 문화 및 기술 브리징</td>
        <td>[Online resource for language conventions and standard Microsoft terminology](http://www.microsoft.com/Language/Default.aspx)</td>
    </tr>
</table>


## 게임 제출 및 게시


다음 가이드 및 정보는 게시 및 제출 프로세스를 가능한 한 매끄럽게 하는 데 도움이 됩니다.

### 패키징 및 업로드

게임 패키지를 게시하고 관리하는 데 새 통합 Windows 개발자 센터 대시보드를 사용하게 됩니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Windows 개발자 센터 앱 게시</td>
        <td>[Publish Windows apps](https://dev.windows.com/publish)</td>
    </tr>
    <tr>
        <td>게임 평가(블로그 게시물)</td>
        <td>[Single workflow to assign age ratings using IARC system](https://blogs.windows.com/buildingapps/2016/01/06/now-available-single-age-rating-system-to-simplify-app-submissions/)</td>
    </tr>
    <tr>
        <td>게임 패키지</td>
        <td>[Package your UWPDirectX game](package-your-windows-store-directx-game.md)</td>
    </tr>
    <tr>
        <td>타사 개발자로서 게임 패키징(블로그 게시물)</td>
        <td>[Create uploadable packages without publisher's store account access](https://blogs.windows.com/buildingapps/2015/12/15/building-an-app-for-a-3rd-party-how-to-package-their-store-app/)</td>
    </tr>
    <tr>
        <td>게임 업로드 및 버전 관리</td>
        <td>[Upload app packages](https://msdn.microsoft.com/library/windows/apps/mt148542)</td>
    </tr>
</table>
 

### 정책 및 인증

인증 문제가 게임의 릴리스를 지연하지 않도록 합니다. 다음은 주의해야 할 정책 및 일반적인 인증 문제입니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Windows 스토어 앱 개발자 계약</td>
        <td>[App Developer Agreement](https://msdn.microsoft.com/library/windows/apps/hh694058)</td>
    </tr>
    <tr>
        <td>Windows 스토어에 앱을 게시하기 위한 정책</td>
        <td>[Windows Store Policies](https://msdn.microsoft.com/library/windows/apps/dn764944)</td>
    </tr>
    <tr>
        <td>몇 가지 일반적인 앱 인증 문제를 방지하는 방법</td>
        <td>[Avoid common certification failures](https://msdn.microsoft.com/library/windows/apps/jj657968)</td>
    </tr>
</table>
 

### 스토어 매니페스트(StoreManifest.xml)

스토어 매니페스트(StoreManifest.xml)는 앱 패키지에 포함될 수 있는 선택적 구성 파일입니다. 스토어 매니페스트에서는 AppxManifest.xml 파일의 일부가 아닌 추가 기능을 제공합니다. 예를 들어 대상 디바이스에 지정된 최소 DirectX 기능 수준 또는 지정된 최소 시스템 메모리가 없는 경우 스토어 매니페스트를 사용하여 게임의 설치를 차단할 수 있습니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>스토어 매니페스트 스키마</td>
        <td>[StoreManifest schema (Windows 10)](https://msdn.microsoft.com/library/windows/apps/mt617335)</td>
    </tr>
</table>
 

## 게임 수명 주기 관리


개발을 완료하고 게임을 제공한 후에도 "게임 오버"가 아닙니다. 한 버전의 개발은 완료된 것일 수 있지만 마켓플레이스에서 게임의 여정은 시작에 불과합니다. 사용 및 오류 보고를 모니터링하고, 사용자 피드백에 응답하며, 게임에 대한 업데이트를 게시하길 원할 것입니다.

### Windows 개발자 센터 분석 및 홍보

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Windows 개발자 센터 분석</td>
        <td>[Analytics](https://msdn.microsoft.com/library/windows/apps/mt148522)</td>
    </tr>
    <tr>
        <td>고객 리뷰에 응답</td>
        <td>[Respond to customer reviews](https://msdn.microsoft.com/library/windows/apps/mt148546)</td>
    </tr>
    <tr>
        <td>게임 홍보 방법</td>
        <td>[Promote your apps](https://dev.windows.com/store-promotion)</td>
    </tr>
</table>
 

### Visual Studio Application Insights

Visual Studio Application Insights에서 게시된 게임에 대한 성능, 원격 분석 및 사용 현황 분석을 제공합니다. Application Insights는 게임이 릴리스된 후 문제를 검색 및 해결하고, 지속적으로 사용을 모니터링 및 개선하며, 플레이어가 어떻게 계속해서 게임과 상호 작용하는지를 이해하도록 도와줍니다. Application Insights는 [Azure 포털](http://portal.azure.com/)에 원격 분석을 전송하는 SDK를 앱에 추가하여 작동합니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>응용 프로그램 성능 및 사용 현황 분석</td>
        <td>[Visual Studio Application Insights](https://azure.microsoft.com/documentation/articles/app-insights-get-started/)</td>
    </tr>
    <tr>
        <td>Windows 앱에서 Application Insights 사용</td>
        <td>[Application Insights for Windows Phone and Store apps](https://azure.microsoft.com/documentation/articles/app-insights-windows-get-started/)</td>
    </tr>
</table>
 

### 콘텐츠 업데이트 만들기 및 관리

게시된 게임을 업데이트하려면 더 높은 버전 번호를 사용하는 새 앱 패키지를 제출합니다. 패키지가 제출 및 인증을 거친 후 고객이 업데이트로 자동으로 사용할 수 있게 됩니다.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>게임 업데이트 및 버전 관리</td>
        <td>[Package version numbering](https://msdn.microsoft.com/library/windows/apps/mt188602)</td>
    </tr>
    <tr>
        <td>게임 패키지 관리 참고 자료</td>
        <td>[Guidance for app package management](https://msdn.microsoft.com/library/windows/apps/mt188602)</td>
    </tr>
</table>


## 게임에 Xbox Live 추가


> **참고** Xbox Live 개발은 ID@Xbox 및 Microsoft Studios와 같은 프로그램을 통해 관리됩니다. 이 가이드에서는 광범위한 리소스를 다루므로 일부 리소스는 프로그램 참여 또는 특정 개발 역할에 따라 액세스할 수 없을 수 있습니다. 예제는 developer.xboxlive.com, forums.xboxlive.com, xdi.xboxlive.com 또는 GDN(게임 개발자 네트워크)으로 확인되는 링크입니다. Microsoft와의 파트너 제휴 방법에 대한 내용은 [개발자 프로그램](#programs)을 참조하세요.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>최신 Xbox Live SDK 다운로드</td>
        <td>[Xbox Live SDK](http://aka.ms/xsapi2)</td>
    </tr>
    <tr>
        <td>유니버설 Windows 플랫폼 앱에 Xbox Live 추가</td>
        <td>[How to - Add Xbox Live SDK to Universal Windows Platform (UWP) Apps](http://aka.ms/xsapi2uwp)</td>
    </tr>
    <tr>
        <td>Xbox Live를 사용하는 게임에 대한 요구 사항</td>
        <td>[Xbox Requirements for Xbox Live on Windows 10](http://go.microsoft.com/fwlink/?LinkId=533217)</td>
    </tr>
    <tr>
        <td>Xbox Live 게임 개발 개요(동영상)</td>
        <td>[Developing with Xbox Live for Windows 10](http://channel9.msdn.com/Events/GDC/GDC-2015/Developing-with-Xbox-Live-for-Windows-10)</td>
    </tr>
    <tr>
        <td>플랫폼 간 매치 메이킹(동영상)</td>
        <td>[Xbox Live Multiplayer: Introducing services for cross-platform matchmaking and gameplay](http://channel9.msdn.com/Events/GDC/GDC-2015/Xbox-Live-Multiplayer-Introducing-services-for-cross-platform-matchmaking-and-gameplay)</td>
    </tr>
    <tr>
        <td>Fable Legends의 디바이스 간 게임 플레이(동영상)</td>
        <td>[Fable Legends: Cross-device Gameplay with Xbox Live](http://channel9.msdn.com/Events/GDC/GDC-2015/Fable-Legends-Cross-device-Gameplay-with-Xbox-Live)</td>
    </tr>
    <tr>
        <td>Xbox Live 통계 및 도전 과제(동영상)</td>
        <td>[Best Practices for Leveraging Cloud-Based User Stats and Achievements in Xbox Live](http://channel9.msdn.com/Events/GDC/GDC-2015/Best-Practices-for-Leveraging-Cloud-Based-User-Stats-and-Achievements-in-Xbox-Live)</td>
    </tr>
</table>
 

## 추가 리소스

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>인디 게임 개발(동영상)</td>
        <td>[New Opportunities for Independent Developers](http://channel9.msdn.com/Events/GDC/GDC-2015/New-Opportunities-for-Independent-Developers)</td>
    </tr>
    <tr>
        <td>멀티 코어 모바일 디바이스에 대한 고려 사항(동영상)</td>
        <td>[Sustained Gaming Performance in multi-core mobile devices](http://channel9.msdn.com/Events/GDC/GDC-2015/Sustained-gaming-performance-in-multi-core-mobile-devices)</td>
    </tr>
    <tr>
        <td>Windows 10 데스크톱 게임 개발(동영상)</td>
        <td>[PC Games for Windows 10](http://channel9.msdn.com/Events/GDC/GDC-2015/PC-Games-for-Windows-10)</td>
    </tr>
</table>



 

 

 


<!--HONumber=Mar16_HO4-->


