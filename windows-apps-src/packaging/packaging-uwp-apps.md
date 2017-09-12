---
author: laurenhughes
ms.assetid: 96361CAF-C347-4671-9721-8208CE118CA4
title: "UWP 앱 패키징"
description: "UWP 앱을 배포 또는 판매하려면, 여기에 대한 앱 패키지를 만들어야 합니다."
ms.author: lahugh
ms.date: 08/09/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: c6aa08c8c2e62bfed388000944de5520cbe58501
ms.sourcegitcommit: 63c815f8c6665872987b5410cabf324f2b7e3c7c
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/10/2017
---
# <a name="package-a-uwp-app-with-visual-studio"></a>Visual Studio를 사용하여 UWP 앱 패키징

\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]

UWP(유니버설 Windows 플랫폼) 앱을 판매하거나 다른 사용자에게 배포하려면 UWP 앱 패키지를 만들어야 합니다. 스토어를 통해 앱을 배포하지 않는 경우, 앱 패키지를 장치에 직접 테스트 로드 할 수 있습니다. 이 문서에서는 Visual Studio를 사용하여, UWP 앱 패키지를 구성, 생성, 테스트 하는 프로세스에 대해 설명합니다. 기업 및 LOB 앱 테스트용 로드에 대한 자세한 내용은 [Windows 10 LOB 앱 테스트용 로드](https://docs.microsoft.com/windows/application-management/sideload-apps-in-windows-10)를 참조합니다.

Windows 10은 앱 패키지(.appx), 앱 번들(.appxbundle), 또는 전체 파일(.appxupload)을 업로드할 수 있습니다. .appxupload 파일은 앱과 패키지, 둘의 번들, 다른 중요 디버깅 정보가 모두 포함된 폴더입니다. .Appxupload 파일을 제출하는 것이 아주 좋습니다. 앱 패키지를 스토어에 제출하고 나면, Windows 10 장치에서 앱을 설치하거나 실행시킬 수 있습니다. 

다음은 앱 패키지를 준비해 만드는 단계에 대한 개요입니다.

1.  [앱 패키징을 하기 전에](#before-packaging-your-app), 앱을 스토어 제출용으로 패키징하도록 준비하려면 다음 단계를 따르세요.
2.  [앱 패키지를 구성](#configure-an-app-package)합니다. 매니페스트 디자이너를 사용하여 패키지를 구성합니다. 예를 들어 타일 이미지를 추가하고 앱에서 지원하는 방향을 선택합니다.
3.  [앱 패키지를 생성](#create-an-app-package)합니다. Visual Studio 앱 패키지 마법사를 사용해 앱 패키지를 생성합니다. 그런 다음 Windows 앱 인증 키트로 패키지를 인증합니다.
4.  [테스트용으로 앱 패키지 로드](#sideload-your-app-package). 테스트용으로 앱을 장치에 로드한 후에 앱이 올바르게 작동하는지 테스트할 수 있습니다.

위의 단계를 완료한 후에는 스토어에서 앱을 배포할 수 있습니다. 내부 사용자 전용이므로 판매하지 않을 LOB(기간 업무) 앱이 있는 경우 Windows 10 장치에 설치하기 위해 이 앱을 테스트용으로 로드할 수 있습니다.

## <a name="before-packaging-your-app"></a>앱을 패키징하기 전에

1.  **앱을 테스트합니다.** 스토어 제출용으로 앱을 패키지하기 전에 모든 장치 패밀리에서 앱이 지원하려는 계획대로 작동하는지 확인합니다. 이러한 장치 패밀리에는 데스크톱, 모바일, Surface Hub, Xbox, IoT 장치 등이 포함될 수 있습니다.
2.  **앱을 최적화합니다.** Visual Studio의 프로파일링 도구와 디버깅 도구를 사용하여 UWP 앱의 성능을 최적화할 수 있습니다. 예를 들어 UI 응답성을 위한 시간 표시 막대 도구, 메모리 사용량 도구, CPU 사용량 도구 등을 사용할 수 있습니다. 이러한 도구에 대한 자세한 내용은 [디버깅하지 않고 진단 도구 실행](https://msdn.microsoft.com/library/dn957936.aspx)을 참조하세요.
3.  **.NET 네이티브 호환성(VB 앱 및 C# 앱)을 확인합니다.** UWP에는 앱의 런타임 성능을 개선하는 기본 컴파일러가 있습니다. 이 변경으로 인해, 이 컴파일 환경에서 앱을 테스트하는 것이 좋습니다. 기본적으로 **Release** 빌드 구성에서는 .NET 네이티브 도구 체인을 사용하므로, 이 **Release** 구성을 사용하여 앱을 테스트하고 앱이 예상대로 작동하는지 확인해야 합니다. .NET 네이티브에서 흔히 발생할 수 있는 디버깅 문제에 대해서는 [여기 Debugging .NET Native Windows Universal Apps](http://blogs.msdn.com/b/visualstudioalm/archive/2015/07/29/debugging-net-native-windows-universal-apps.aspx)에서 자세히 설명합니다.

## <a name="configure-an-app-package"></a>앱 패키지 구성

앱 매니페스트 파일(Package.appxmanifest.xml)는 앱 패키지를 만드는 데 필요한 속성과 설정이 들어 있는 XML 파일입니다. 예를 들어 매니페스트 파일의 속성에서는 앱의 타일로 사용할 이미지 및 사용자가 장치를 회전할 때 앱에서 지원되는 방향을 설명합니다.

Visual Studio 매니페스트 디자이너를 이용하면 파일의 원시 XML을 편집하지 않고도 매니페스트 파일을 업데이트 할 수 있습니다.

**매니페스트 디자이너를 사용하여 패키지 구성**

1.  **Solution Explorer**에서 UWP 앱의 프로젝트 노드를 확장합니다.
2.  **Package.appxmanifest** 파일을 두 번 클릭합니다. 매니페스트 파일이 XML 코드 뷰에 이미 열린 경우 Visual Studio에서 파일을 닫으라는 메시지를 표시합니다.
3.  이제 앱을 구성하는 방법을 결정할 수 있습니다. 각 탭에는 앱에 대해 구성할 수 있는 정보 및 필요한 경우 자세한 정보의 링크가 포함되어 있습니다.<br/>
    ![Visual Studio의 매니페스트 디자이너](images/packaging-screen1.jpg)

    UWP 앱에 필요한 모든 이미지가 **시각적 자산** 탭에 있는지 확인합니다.

    **Packaging** 탭에서 게시 데이터를 입력할 수 있습니다. 이 위치에서 앱에 서명하는 데 사용할 인증서를 선택할 수 있습니다. 모든 UWP 앱은 인증서를 사용하여 서명해야 합니다. 앱 패키지를 테스트용으로 로드하려면 패키지를 신뢰해야 합니다. 패키지를 신뢰하려면 해당 디바이스에 인증서를 설치해야 합니다. 테스트용 로드에 대한 자세한 내용은 [개발을 위해 디바이스 사용](https://msdn.microsoft.com/library/windows/apps/Dn706236)을 참조하세요.

4.  앱에 필요한 편집을 수행한 후에 파일을 저장합니다.

Visual Studio는 패키지를 스토어와 연결할 수 있습니다. 이렇게 하면 매니페스트 디자이너의 패키징 탭에 있는 일부 필드가 자동으로 업데이트됩니다.

## <a name="create-an-app-package"></a>앱 패키지 만들기

스토어를 통해 앱을 배포하려면 앱 패키지(.appx), 앱 번들(.appxbundle), 또는 업로드 패키지(.appxupload)를 만들어야 합니다. **앱 패키지 생성** 마법사를 사용하여 만들 수 있습니다. Visual Studio를 사용하여 스토어 제출에 적합한 패키지를 만들려면 다음 단계를 따르세요.

**앱 패키지를 만들려면**

1.  **Solution Explorer**에서 UWP 앱 프로젝트용 솔루션을 엽니다.
2.  프로젝트를 마우스 오른쪽 단추로 클릭하고 **스토어**->**패키지 만들기**를 선택합니다. 이 옵션이 사용되지 않거나 전혀 표시되지 않는 경우에는 프로젝트가 UWP 프로젝트인지 확인합니다.<br/>
    ![앱 패키지 만들기로 이동할 수 있는 상황에 맞는 메뉴](images/packaging-screen2.jpg)

    **앱 패키지 만들기** 마법사가 나타납니다.

3.  첫 번째 대화 상자에서 Windows 스토어에 업로드할 패키지를 빌드할지 묻는 메시지가 나타나면 예를 선택하고 다음을 클릭합니다.<br/>
    ![표시되는 패키지 만들기 대화 창](images/packaging-screen3.jpg)

    여기서 아니요를 선택하면 Visual Studio에서 스토어 제출용으로 필요한 .appxupload 패키지가 생성되지 않습니다. 내부 장치에서만 앱을 테스트용으로 로드하여 실행하려는 경우에는 이 옵션을 선택하면 됩니다. 테스트용 로드에 대한 자세한 내용은 [개발을 위해 디바이스 사용](https://msdn.microsoft.com/library/windows/apps/Dn706236)을 참조하세요.

4.  Windows 개발자 센터에 개발자 계정으로 로그인합니다. (아직 개발자 계정이 없으면 마법사를 사용하여 만들 수 있습니다.)
5.  패키지의 앱 이름을 선택하거나 아직 Windows 개발자 센터 포털에서 예약하지 않은 경우 새 이름을 예약합니다.<br/>
    ![앱 이름 선택이 표시된 앱 패키지 만들기 창](images/packaging-screen4.jpg)
6.  **패키지 선택 및 구성** 대화 상자에서 세 개의 아키텍처 구성(x86, x64, ARM)을 모두 선택했는지 확인합니다. 이렇게 하면 가장 광범위한 디바이스에 앱을 배포할 수 있습니다. **앱 번들 생성** 목록 상자에서 **항상**을 선택합니다. 이렇게 하면 업로드할 파일(.appxupload)이 하나뿐이므로 스토어 제출 프로세스가 훨씬 더 간단합니다. 단일 번들에 각 프로세서 아키텍처에서 장치를 배포하는 데 필요한 모든 패키지와 디버깅 및 크래시 분석 정보가 포함되어 있습니다. 다양한 장치가 사용하는 패키지 아키텍처에 대한 자세한 내용은 [앱 패키지 아키텍처](https://docs.microsoft.com/windows/uwp/packaging/device-architecture)를 참조하세요.<br/>
    ![패키지 구성이 표시된 앱 패키지 만들기 창](images/packaging-screen5.jpg)
7.  Windows 개발자 센터에서 최상의 [크래시 분석](http://blogs.windows.com/buildingapps/2015/07/13/crash-analysis-in-the-unified-dev-center/) 환경을 얻으려면 전체 PDB 기호 파일을 포함하는 것이 아주 좋습니다. 기호를 사용한 디버깅에 대한 자세한 내용은 [기호를 사용한 디버깅](https://msdn.microsoft.com/library/windows/desktop/Ee416588)을 참조할 수 있습니다.
8.  이제 패키지를 만드는 세부 정보를 구성할 수 있습니다. 앱을 게시할 준비가 되면 출력 위치에서 패키지를 업로드합니다.
9.  **Create**를 클릭하여 appxupload 패키지를 생성합니다.
10. 이제 다음 대화 상자가 표시됩니다.<br/>
    ![유효성 검사 옵션이 표시된 패키지 만들기 완료 창](images/packaging-screen6.jpg)

    인증을 위해 스토어에 제출하기 전에 로컬 또는 원격 컴퓨터에서 앱의 유효성을 검사합니다. 앱 패키지의 릴리스 빌드만 유효성 검사할 수 있으며 디버그 빌드는 검사할 수 없습니다.

11. 로컬에서 유효성을 검사하려면 **로컬 컴퓨터** 옵션을 선택한 상태로 두고 **Launch Windows App Certification Kit**을 클릭합니다. Windows 앱 인증 키트로 앱을 테스트하는 방법에 대한 자세한 내용은 [Windows 앱 인증 키트](https://msdn.microsoft.com/library/windows/apps/Mt186449)를 참조하세요.

    Windows 앱 인증 키트는 다양한 테스트를 수행하고 결과를 보여 줍니다. 더 자세한 정보는 [Windows 앱 인증 키트](https://msdn.microsoft.com/library/windows/apps/mt186450)를 참조하세요.

    테스트에 사용할 원격 Windows 10 장치가 있는 경우 해당 장치에서 수동으로 Windows 앱 인증 키트를 설치해야 합니다. 다음 섹션에서 다음 단계를 안내합니다. 작업이 완료되면 **Remote machine**를 선택하고 **Launch Windows App Certification Kit**을 클릭하여 원격 디바이스에 연결하고 유효성 검사를 실행할 수 있습니다.

12. WACK가 완료된 후 앱이 테스트에 통과하면 스토어에 앱을 제출할 수 있습니다. 올바른 파일을 업로드하는지 확인합니다. 이 파일은 솔루션의 루트 폴더 \\\[AppName\]\\AppPackages에 있으며 이름이 .appxupload(또는 사용하고 있다면 .appx/.appxbundle 확장명) 파일 확장명으로 끝납니다. 이름의 형식은 \[AppName\]\_\[AppVersion\]\_x86\_x64\_arm\_bundle.appxupload입니다.

**원격 Windows 10 디바이스에서 앱 패키지 유효성 검사**

1.  [개발을 위해 디바이스 사용](https://msdn.microsoft.com/library/windows/apps/Dn706236) 지침에 따라 Windows 10 디바이스를 개발용으로 사용하도록 설정합니다.
    **중요**  Windows 10용 원격 ARM 장치에서는 앱 패키지의 유효성을 검사할 수 없습니다.
2.  Visual Studio용 원격 도구를 다운로드하여 설치합니다. 이러한 도구를 사용하여 Windows 앱 인증 키트를 원격으로 실행합니다. 이러한 도구를 다운로드할 위치를 포함하여 자세한 관련 내용은 [원격 컴퓨터에서 Windows 스토어 앱 실행](https://msdn.microsoft.com/library/hh441469.aspx#BKMK_Starting_the_Remote_Debugger_Monitor)을 참조하세요.
3.  필요한 [Windows 앱 인증 키트](http://go.microsoft.com/fwlink/p/?LinkID=309666)를 다운로드한 다음 원격 Windows 10 디바이스에 설치합니다.
4.  마법사 **Package Creation Completed** 페이지에서 **Remote Machine** 옵션 단추를 선택하고 **Test Connection** 단추 옆에 있는 줄임표 단추를 선택합니다.
    **참고**  **Remote Machine** 옵션 단추는 유효성 검사를 지원하는 솔루션 구성을 하나 이상 선택한 경우에만 사용할 수 있습니다. WACK로 앱을 테스트하는 방법에 대한 자세한 내용은 [Windows 앱 인증 키트](https://msdn.microsoft.com/library/windows/apps/Mt186449)를 참조하세요.
5.  서브넷 내부에서 디바이스 양식을 지정하거나 서브넷 외부 디바이스의 DNS(도메인 이름 서버) 이름 또는 IP 주소를 제공합니다.
6.  디바이스에서 Windows 자격 증명을 사용해 로그온하도록 요구하지 않는 경우 **Authentication Mode** 목록에서 **None**을 선택합니다.
7.  **Select** 단추를 선택한 다음 **Launch Windows App Certification Kit** 단추를 선택합니다. 해당 디바이스에서 원격 도구가 실행 중인 경우 Visual Studio는 디바이스에 연결한 다음 유효성 검사 테스트를 수행합니다. [Windows 앱 인증 키트 테스트](https://msdn.microsoft.com/library/windows/apps/mt186450)를 참조하세요.

## <a name="sideload-your-app-package"></a>테스트용으로 앱 패키지 로드

Windows 10 1주년 업데이트에 도입된 앱 패키지는 앱 패지 파일을 두 번 클릭하여 설치할 수 있습니다. 이 기능을 사용하려면, 앱 패키지(.appx)나 앱 번들(.appxbundle) 파일을 찾아 두 번 클릭합니다. 앱 설치 관리가 시작되고, 기본 앱 정보, 설치 단추, 설치 진행 막대, 기타 관련 오류 메시지를 제공합니다. 

![앱 설치 관리자에 샘플 앱인 Contoso 설치가 표시됩니다.](images/appinstaller-screen.png)

> [!NOTE]
> 앱 설치 관리자는 장치가 앱을 신뢰하는 것으로 간주합니다. 개발자나 기업용 앱을 테스트용으로 로드하는 경우, 장치의 신뢰할 수 있는 루트 인증 기관 스토어에 서명 인증서를 설치해야 합니다. 방법을 모를 경우 [테스트 인증서 설치](https://docs.microsoft.com/windows-hardware/drivers/install/installing-test-certificates)를 참조합니다.

### <a name="sideload-your-app-on-previous-versions-of-windows"></a>이전 버전 Windows에 앱 테스트용 로드
UWP 앱 패키지의 경우, 데스크톱 앱처럼 장치에 앱을 설치하지 않습니다. 일반적으로 스토어에서 UWP 앱을 다운로드하고, 장치에 앱을 설치합니다. 스토어에 제출하지 않고도 앱을 설치할 수 있습니다(테스트용 로드). 이렇게 하면 만든 앱 패키지(.appx)를 사용하여 앱을 설치하고 테스트할 수 있습니다. 스토어에서 판매하지 않을 앱(예: LOB(기간 업무) 앱)이 있는 경우에는 회사 내의 다른 사용자가 사용할 수 있도록 이 앱을 테스트용으로 로드할 수 있습니다.

다음 목록은 테스트용으로 앱을 로드하는 데 요구되는 사항입니다.

-   [개발을 위해 장치를 사용](https://msdn.microsoft.com/library/windows/apps/Dn706236)하도록 설정해야 합니다.
-   Windows 10 Mobile 장치에 테스트용으로 앱을 로드하려면 [WinAppDeployCmd.exe](install-universal-windows-apps-with-the-winappdeploycmd-tool.md) 도구를 사용해야 합니다.

**데스크톱, 노트북 또는 태블릿에 테스트용으로 앱 로드**

1.  대상 장치에 설치하려는 버전의 폴더를 복사합니다.

    앱 번들을 만든 경우에는 버전 번호와`*\_Test` 폴더를 기반으로 한 폴더가 생깁니다. 다음 두 폴더를 예로 들 수 있습니다(설치할 버전은 1.0.2.0).

    -   `C:\\Projects\\MyApp\\MyApp\\AppPackages\\MyApp\_1.0.2.0`
    -   `C:\\Projects\\MyApp\\MyApp\\AppPackages\\MyApp\_1.0.2.0\_Test`

    앱 번들이 없는 경우에는 올바른 아키텍처의 폴더 및 해당 `*\_Test` 폴더를 복사할 수 있습니다. 이 두 폴더는 x64 아키텍처와 `*\_Test` 폴더 앱 패키지의 예입니다.

    -   `C:\\Projects\\MyApp\\MyApp\\AppPackages\\MyApp\_1.0.2.0\_x64`
    -   `C:\\Projects\\MyApp\\MyApp\\AppPackages\\MyApp\_1.0.2.0\_x64\_Test`

2.  대상 장치에서 `*\_Test` 폴더를 엽니다.
3.  마우스 오른쪽 단추로 **AppDevPackage.ps1** 파일을 클릭합니다. **Run with PowerShell**을 선택한 다음 프롬프트를 따릅니다.<br/>
    ![PowerShell 스크립트로 이동된 파일 탐색기](images/packaging-screen7.jpg)

    앱 패키지가 설치되면 PowerShell 창에 **앱이 설치되었습니다**라는 메시지가 나타납니다.

    **참고**: 태블릿에서 바로 가기 메뉴를 열려면 마우스 오른쪽 단추로 클릭할 위치에서 화면을 터치하고 완전한 원이 나타날 때까지 계속 누르고 있다가 손가락을 뗍니다. 손가락을 뗀 후에 바로 가기 메뉴가 나타납니다.
4.  시작 버튼을 클릭한 다음 앱 이름을 입력해 검색한 후 시작합니다.
