---
author: normesta
Description: "이 문서에는 데스크톱-UWP 브리지를 사용하여 앱을 패키지로 만들기 전에 알아야 할 사항이 나열되어 있습니다. 앱의 패키징 프로세스를 준비하는 데 많은 작업을 수행하지 않아도 됩니다."
Search.Product: eADQiWindows 10XVcnh
title: "앱 패키징을 위한 준비(데스크톱 브리지)"
ms.author: normesta
ms.date: 05/25/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: 71a57ca2-ca00-471d-8ad9-52f285f3022e
ms.openlocfilehash: f7337ee7bf78730e2300a11d7d606f328c7c418b
ms.sourcegitcommit: 77bbd060f9253f2b03f0b9d74954c187bceb4a30
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/11/2017
---
# <a name="prepare-to-package-an-app-desktop-bridge"></a>앱 패키징을 위한 준비(데스크톱 브리지)

이 문서에는 데스크톱 앱을 패키지로 만들기 전에 알아야 할 사항이 나열되어 있습니다. 앱의 패키징 프로세스를 준비하는 데 많은 작업을 수행하지 않아도 됩니다. 하지만 아래의 항목이 사용 중인 응용 프로그램에 적용되는 경우에는 패키징 전에 문제를 해결해야 합니다. Windows 스토어는 라이선스 및 자동 업데이트를 처리하므로 코드베이스에서 이러한 작업들과 관련된 기능을 자유롭게 제거할 수 있습니다.

+ __앱에서 4.6.1 이전 버전의 .NET을 사용합니다__. .NET 4.6.1만 지원됩니다. 패키지로 만들기 전에 앱의 대상을 .NET 4.6.1로 변경해야 합니다.

+ __앱은 항상 관리자 보안 권한으로 실행됩니다__. 대화형 사용자로 실행하는 동안 앱은 계속 작동해야 합니다. Windows 스토어에서 앱을 설치하는 사용자가 시스템 관리자가 아닐 수 있으므로 앱을 관리자 권한으로 실행하도록 요구해도 표준 사용자의 경우는 제대로 진행되지 않습니다.

+ __앱에는 커널 모드 드라이버 또는 Windows 서비스가 필요합니다__. 브리지는 앱에 적합하지만 시스템 계정으로 실행해야 하는 커널 모드 드라이버 또는 Windows 서비스를 지원하지 않습니다. Windows 서비스 대신, [백그라운드 작업](https://msdn.microsoft.com/windows/uwp/launch-resume/create-and-register-a-background-task)을 사용합니다.

+ __앱 모듈은 in-process 방식으로 Windows 앱 패키지에 없는 프로세스로 로드됩니다.__. 이것은 허용되지 않습니다. 즉 [셸 확장](https://msdn.microsoft.com/library/windows/desktop/dd758089.aspx)과 같은 in-process 확장은 지원되지 않습니다. 그렇지만 두 앱이 같은 패키지에 있는 경우 두 앱의 프로세스 간 통신을 수행할 수 있습니다.

+ __앱이 사용자 지정 AUMID(응용 프로그램 사용자 모델 ID)를 사용합니다__. 프로세스가 [SetCurrentProcessExplicitAppUserModelID](https://msdn.microsoft.com/library/windows/desktop/dd378422.aspx)를 호출하여 자체 AUMID를 설정하는 경우 앱 모델 환경/Windows 앱 패키지에 의해 생성된 AUMID만 사용할 수 있습니다. 사용자 지정 AUMID를 정의할 수 없습니다.

+ __앱은 HKEY_LOCAL_MACHINE (HKLM) 레지스트리 하이브를 수정합니다__. 앱에서 HKLM 키를 만들거나 수정을 위해 이 키를 열려고 하면 액세스 거부 오류가 발생합니다. 앱에는 레지스트리의 자체 가상화 보기가 있으므로 사용자 및 컴퓨터 전체 레지스트리 하이브(HKLM의 유형) 개념이 적용되지 않습니다. 대신 HKEY_CURRENT_USER(HKCU)에 쓰는 것과 같은 HKLM 사용 목적을 달성할 수 있는 다른 방법을 찾아야 합니다.

+ __앱은 다른 앱을 시작하는 수단으로 ddeexec 레지스트리 하위 키를 사용합니다__. 대신 [앱 패키지 매니페스트](https://msdn.microsoft.com/library/windows/apps/br211474.aspx)의 다양 한 Activatable* 확장에 의해 구성된 DelegateExecute 동사 처리기 중 하나를 사용합니다.

+ __앱은 다른 앱과 데이터를 공유하는 데 사용하려는 AppData 폴더나 레지스트리에 씁니다__. 변환 후 AppData는 각 UWP 앱에 대한 개인 저장소인 로컬 앱 데이터 저장소로 리디렉션됩니다.

  앱이 HKEY_LOCAL_MACHINE 레지스트리 하이브에 쓰는 모든 항목은 격리된 바이너리 파일로 리디렉션됩니다. 또 앱이 HKEY_CURRENT_USER 레지스트리 하이브에 쓰는 모든 항목은 개인 사용자 별, 앱 별 위치에 배치됩니다. 파일 및 레지스트리 리디렉션에 대한 자세한 내용은 [데스크톱 브리지 이면](desktop-to-uwp-behind-the-scenes.md)을 참조합니다.  

  프로세스 간에 데이터를 공유하는 다른 방법을 사용합니다. 자세한 내용은 [설정 및 기타 앱 데이터 저장 및 검색](https://msdn.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data)을 참조하세요.

+ __앱에서 앱용 설치 디렉터리에 씁니다__. 예를 들어 앱은 exe와 동일한 디렉터리에 추가된 로그 파일에 씁니다. 이것이 지원되지 않으면 로컬 앱 데이터 저장소 등의 다른 위치를 찾아야 합니다.

+ __앱 설치에 사용자 조작이 필요합니다__. 앱 설치 관리자는 자동으로 실행될 수 있어야 하며 기본적으로 클린 OS 이미지에 없는 모든 필수 구성 요소를 설치해야 합니다.

+ __앱에서 현재 작업 디렉터리를 사용합니다__. 런타임에 패키지 데스크톱 앱은 이전에 데스크톱 .LNK 바로 가기에서 지정한 동일한 작업 디렉터리를 얻지 못합니다. 앱의 올바른 작동을 위해 올바른 디렉터리를 확보해야 하는 경우 런타임에 CWD를 변경해야 합니다.

+ __앱에는 UIAccess가 필요합니다__. 응용 프로그램에서 UAC 매니페스트의 `requestedExecutionLevel` 요소에 `UIAccess=true`를 지정하는 경우 현재는 UWP로 변환할 수 없습니다. 자세한 내용은 [UI 자동화 보안 개요](https://msdn.microsoft.com/library/ms742884.aspx)를 참조하세요.

+ __앱은 COM 개체를 노출합니다__. 프로세스와 패키지 내에서의 확장은 in-process 및 OOP(Out-of-Process) 방식으로 COM 및 OLE 서버를 등록해 사용할 수 있습니다.  크리에이터 업데이트는 패키지 밖에서도 볼 수 있도록 OOP COM 및 OLE 서버를 등록할 수 있게 해주는 패키지형 COM 지원을 추가합니다.  [데스크톱 브리지에 대한 COM 서버 및 OLE 문서 지원](https://blogs.windows.com/buildingapps/2017/04/13/com-server-ole-document-support-desktop-bridge/#bjPyETFgtpZBGrS1.97)를 참조하세요.

   패키지형 COM 지원 기능은 기존 COM API에서는 작동하지만, 패키지형 COM에 대한 위치가 개인 위치에 있기 때문에 레지스트리를 직접 읽을 때 사용되는 응용 프로그램 확장에서는 작동하지 않습니다.

+ __앱은 다른 프로세스에서 사용할 수 있도록 GAC 어셈블리를 노출합니다__. 현재 릴리스에서는 앱이 Windows 앱 패키지 외부의 실행 파일에서 발생한 프로세스에서 사용할 수 있도록 GAC 어셈블리를 노출할 수 없습니다. 패키지 내의 프로세스는 정상적으로 GAC 어셈블리를 등록하고 사용할 수 있지만, 이들 항목이 외부에 표시되지는 않습니다. 즉, OLE와 같은 interop 시나리오는 외부 프로세스에서 호출되는 경우 작동하지 않습니다.

+ __앱에서 지원되지 않는 방식으로 CRT(C 런타임 라이브러리)를 연결하고 있습니다__. Microsoft C/C++ 런타임 라이브러리는 Microsoft Windows 운영 체제용 프로그래밍 루틴을 제공합니다. 이러한 루틴은 C 및 C++ 언어에서 제공하지 않는 많은 일반적인 프로그래밍 작업을 자동화합니다. 앱에서 C/C++ 런타임 라이브러리를 사용하는 경우 해당 라이브러리가 지원되는 방식으로 연결되는지 확인해야 합니다.

    Visual Studio 2015에서는 현재 버전의 CRT 코드에 대한 동적 연결(코드에서 공통 DLL 파일을 사용할 수 있도록 함)과 정적 연결(라이브러리를 코드에 직접 연결함)을 둘 다 지원합니다. 가능하면 앱에서 VS 2015와 함께 동적 연결을 사용하는 것이 좋습니다.

    이전 버전의 Visual Studio에서의 지원은 각기 다릅니다. 자세한 내용은 다음 표를 참조하세요.

    <table>
    <th>Visual Studio 버전</td><th>동적 연결</th><th>정적 연결</th></th>
    <tr><td>2005(VC 8)</td><td>지원되지 않음</td><td>지원함</td>
    <tr><td>2008(VC 9)</td><td>지원되지 않음</td><td>지원</td>
    <tr><td>2010(VC 10)</td><td>지원함</td><td>지원</td>
    <tr><td>2012(VC 11)</td><td>지원함</td><td>지원되지 않음</td>
    <tr><td>2013(VC 12)</td><td>지원</td><td>지원되지 않음</td>
    <tr><td>2015(VC 14)</td><td>지원</td><td>지원</td>
    </table>

    참고: 모든 경우에 공개적으로 사용 가능한 최신 CRT에 연결해야 합니다.

+ __앱에서 Windows side-by-side 폴더의 어셈블리를 설치하고 로드합니다__. 예를 들어 앱에서 C 런타임 라이브러리 VC8 또는 VC9를 사용하고 Windows side-by-side 폴더에 있는 해당 라이브러리를 동적으로 연결하고 있는 경우 코드에서 공유 폴더의 공통 DLL 파일을 사용하고 있는 것입니다. 이는 지원되지 않습니다. 재배포 가능 라이브러리 파일을 코드에 직접 연결하여 정적으로 연결해야 합니다.

+ __앱에서 System32/SysWOW64 폴더의 종속성을 사용합니다__. 해당 DLL이 작동하려면 Windows 앱 패키지의 가상 파일 시스템 부분에 포함해야 합니다. 이렇게 하면 **System32**/**SysWOW64** 폴더에 DLL이 설치된 것처럼 앱이 동작합니다. 패키지의 루트에 **VFS**라는 폴더를 만듭니다. 해당 폴더 안에 **SystemX64** 및 **SystemX86** 폴더를 만듭니다. 그런 다음 32비트 버전의 DLL은 **SystemX86** 폴더에 넣고 64비트 버전은 **SystemX64** 폴더에 넣습니다.

+ __앱에서 Dev11 VCLibs 프레임워크 패키지를 사용합니다__. Windows 앱 패키지에서 종속성으로 정의된 경우 Windows 스토어에서 직접 VCLibs 11 라이브러리를 설치할 수 있습니다. 이렇게 하려면 앱 패키지 매니페스트를 다음과 같이 변경합니다. `<Dependencies>` 노드 아래에 다음을 추가합니다.  
`<PackageDependency Name="Microsoft.VCLibs.110.00.UWPDesktop" MinVersion="11.0.24217.0" Publisher="CN=Microsoft Corporation, O=Microsoft Corporation, L=Redmond, S=Washington, C=US" />`  
Windows 스토어에서 설치하는 동안 앱이 설치되기 전에 적절한 버전(x86 또는 x64)의 VCLibs 11 프레임워크가 설치됩니다.  
테스트용으로 로드하여 앱을 설치하는 경우에는 종속성이 설치되지 않습니다. 수동으로 컴퓨터에 종속성을 설치하려면 [데스크톱 브리지용 VC 11.0 프레임워크 패키지](https://www.microsoft.com/download/details.aspx?id=53340&WT.mc_id=DX_MVP4025064)를 다운로드하고 설치해야 합니다. 이러한 시나리오에 대한 자세한 내용은 [Centennial 프로젝트에서 Visual C++ 런타임 사용](https://blogs.msdn.microsoft.com/vcblog/2016/07/07/using-visual-c-runtime-in-centennial-project/)을 참조하세요.

+ __앱에 사용자 지정 점프 목록이 포함됩니다__. 점프 목록을 사용할 때 고려해야 할 몇 가지 문제와 주의 사항이 있습니다.

    - __앱의 아키텍처가 OS와 일치하지 않습니다.__  점프 목록이 현재 제대로 작동 하지 않습니다. 앱과 운영 체제 아키텍처가 일치 하지 않는 경우(예: x64 Windows에서 x86앱을 실행)일 수 있습니다. 현재로서는 앱을 아키텍처와 일치하도록 다시 컴파일하는 해결 방법 외에는 없습니다.

    - __앱이 점프 목록 항목을 만들고 [ICustomDestinationList::SetAppID](https://msdn.microsoft.com/library/windows/desktop/dd378403(v=vs.85).aspx) 또는 [SetCurrentProcessExplicitAppUserModelID](https://msdn.microsoft.com/library/windows/desktop/dd378422(v=vs.85).aspx)__를 호출합니다. 코드에 프로그래밍 방식으로 AppID를 설정하지 마세요. 그러면 점프 목록 항목이 나타나지 않게 됩니다. 앱에 사용자 지정 ID가 필요할 경우 매니페스트 파일을 사용하여 지정하세요. 지침은 [수동으로 앱 패키지(데스크톱 브리지)](desktop-to-uwp-manual-conversion.md)를 참조하세요. 응용 프로그램의 AppID는 *YOUR_PRAID_HERE* 섹션에 지정됩니다.

    - __앱은 패키지의 실행 파일을 참조하는 점프 목록 셸 링크를 추가합니다__. 점프 목록에서 바로 패키지의 실행 파일을 시작할 수는 없습니다(앱 자체의 .exe에 대한 절대 경로는 예외). 대신, 앱 실행 별칭을 등록하고 별칭에 링크 대상 경로를 설정하면 패키지 데스크톱 앱이 해당 경로에 있는 것처럼 키워드를 통해 시작할 수 있습니다. AppExecutionAlias 확장을 사용하는 방법에 대한 자세한 내용은 [Windows 10(데스크톱 브리지)에 앱 통합](desktop-to-uwp-extensions.md)을 참조하세요. 점프 목록에 원래 .exe와 일치하는 링크의 자산이 필요할 경우 [**SetIconLocation**](https://msdn.microsoft.com/library/windows/desktop/bb761047(v=vs.85).aspx)을 사용하여 아이콘과 같은 자산을 설정하고 다른 사용자 지정 항목에 사용하는 것 같은 PKEY_Title로 표시 이름을 설정해야 합니다.

    - __앱은 절대 경로를 사용하여 앱 패키지의 자산을 참조하는 점프 목록 항목을 추가합니다__. 패키지가 업데이트되어 아이콘, 문서, 실행 파일 등과 같은 자산의 위치가 변경될 경우 앱의 설치 경로가 변경될 수 있습니다. 점프 목록 항목이 절대 경로를 사용하여 이러한 자산을 참조할 경우 앱은 경로를 정확히 확인하기 위해 점프 목록을 정기적으로(예: 앱 시작 시) 새로 고쳐야 합니다. 또는 package-relative ms-resource URI 스키마(언어, DPI 및 고대비 인식 가능)를 사용하여 문자열 및 이미지 자산을 참조할 수 있는 UWP [**Windows.UI.StartScreen.JumpList**](https://msdn.microsoft.com/library/windows/apps/windows.ui.startscreen.jumplist.aspx) API를 사용하세요.

+ __앱이 작업 수행을 위한 유틸리티를 시작합니다__. PowerShell, Cmd.exe 같은 명령 유틸리티가 시작되지 않도록 방지합니다. 사실, 사용자가 Windows 10 S를 실행하는 시스템에 앱을 설치하는 경우에는 앱이 이러한 유틸리티를 절대로 시작할 수 없습니다. 이것이 앱의 Windows 스토어 제출을 막을 수도 있습니다. Windows 스토어에 제출되는 앱은 Windows 10 S와 호환되어야 하기 때문입니다.

유틸리티를 시작하는 것이 운영 체제에서 정보를 얻거나 레지스트리를 액세스하거나 시스템 기능에 액세스할 수 있는 편리한 방법일 때가 많습니다. 한편, UWP API를 사용해 이러한 종류의 작업을 대신 수행할 수 있습니다. 이러한 API는 별도의 실행 파일이 필요하지 않기 때문에 성능이 뛰어나며, 무엇보다도 앱이 패키지 외부에 도달하지 않도록 막아줍니다. 앱의 디자인은 데스크톱 브리지 앱에서 제공되는 격리, 신뢰 및 보안을 그대로 따르지만, 앱은 Windows 10 S를 실행하는 시스템에서 예상대로 작동하게 됩니다.

+ __앱은 추가 기능, 플러그 인 또는 확장을 호스트합니다__.   많은 경우에 COM 스타일의 확장은 확장이 패키지로 만들어지지 않고, 완전 신뢰 상태로 설치되기만 하면 계속해서 작동하게 됩니다. 왜냐하면 이러한 설치 관리자가 호스트 앱이 완전 신뢰 접근 권한 값을 찾을 수 있는 곳이라면 어디서든 이를 사용해 레지스트리 수정하고 확장 파일을 배치할 수 있기 때문입니다.

   그러나 이러한 확장이 패키지로 만들어지고, Windows 앱 패키지로서 설치된 경우에는 각 패키지(호스트 앱 및 확장)가 다른 패키지와 격리가 되기 때문에 확장이 작동하지 않습니다. 데스크톱 브리지가 시스템에서 응용 프로그램을 격리하는 방법은 [데스크톱 브리지의 백그라운드](desktop-to-uwp-behind-the-scenes.md)를 참조하세요.

 사용자가 Windows 10 S를 실행하는 시스템에 설치하는 모든 응용 프로그램과 확장은 반드시 Windows 앱 패키지 형태로 설치해야 합니다. 따라서 확장을 패키지로 만들거나 기여자에게 패키지로 만드는 것을 권장하려는 경우에 호스트 앱 패키지와 확장 패키지 간의 통신을 원활하게 할 수 있는 방법을 고려해 보는 것이 좋습니다. 그 한 가지 방법은 [앱 서비스](../launch-resume/app-services.md)를 사용하는 것입니다.

+ __앱이 코드를 생성합니다__. 앱은 메모리에서 사용하는 코드를 생성하되, 생성된 코드가 디스크에 기록되는 것을 방지할 수 있습니다. Windows 앱 인증 프로세스가 앱 제출에 앞서 해당 코드를 확인할 수 없기 때문입니다. 또한 디스크에 쓰기를 한 앱은 Windows 10 S가 실행되는 시스템에서 실행되지 않습니다. 이것이 앱의 Windows 스토어 제출을 막을 수도 있습니다. Windows 스토어에 제출되는 앱은 Windows 10 S와 호환되어야 하기 때문입니다.

+ __앱은 MAPI API를 사용합니다__. [Outlook MAPI API](https://msdn.microsoft.com/library/office/cc765775.aspx(d=robot))는 데스크톱 브리지 앱에서 현재 지원되지 않습니다.

## <a name="next-steps"></a>다음 단계

**데스크톱 앱용 Windows 앱 패키지 생성**

[Windows 앱 패키지 생성](desktop-to-uwp-root.md#convert) 참조

**특정 질문에 대한 답변 찾기**

저희 팀은 이러한 [StackOverflow 태그](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge)를 모니터링합니다.

**이 문서에 대한 피드백 제공**

아래의 설명 섹션을 사용합니다.
