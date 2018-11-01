---
author: normesta
Description: This article lists things you need to know before packaging your desktop application. You may not need to do much to get your app ready for the packaging process.
Search.Product: eADQiWindows 10XVcnh
title: 데스크톱 응용 프로그램 (데스크톱 브리지) 패키징을 위한 준비
ms.author: normesta
ms.date: 05/18/20188
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 71a57ca2-ca00-471d-8ad9-52f285f3022e
ms.localizationpriority: medium
ms.openlocfilehash: 3a0b3a9f5ce7c03b8add9cc459bade684b9daf21
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/01/2018
ms.locfileid: "5886688"
---
# <a name="prepare-to-package-a-desktop-application"></a>데스크톱 응용 프로그램 패키징 준비

이 문서에는 데스크톱 앱을 패키지로 만들기 전에 알아야 할 사항이 나열되어 있습니다. 응용 프로그램 패키징 프로세스를 준비 하는 데 많은 작업을 수행 해야 할 수 있지만 응용 프로그램에 적용 된 아래 항목 경우 패키징 전에 문제를 해결 해야 합니다. Microsoft Store는 라이선스 및 자동 업데이트를 처리하므로 코드베이스에서 이러한 작업들과 관련된 기능을 자유롭게 제거할 수 있습니다.

>[!IMPORTANT]
>데스크톱 응용 프로그램용 Windows 앱 패키지를 만들 수 있습니다 (데스크톱 브리지도 알려진 Windows 10 버전 1607에에서 도입 그렇지 및 Windows 10 1 주년 업데이트 (10.0;를 대상으로 하는 프로젝트 에서만 사용할 수 있습니다 빌드 14393) 또는 Visual Studio의 최신 릴리스 합니다.

+ __응용 프로그램 버전의.NET 4.6.2 이전 필요__합니다. 응용 프로그램이.NET 4.6.2에서 실행 되는지 확인 해야 합니다. 4.6.2 이전 버전을 요구하거나 재배포할 수 없습니다. Windows 10 1주년 업데이트에서 제공되는 .NET 버전입니다. 이 버전에서 작동 하는 응용 프로그램을 확인 하면 응용 프로그램이 Windows 10의 향후 업데이트와 호환 되어야 계속 됩니다.  응용 프로그램이.NET Framework 4.0 이상을 대상으로 하는 경우.NET 4.6.2에서 실행 해야 하지만 여전히 테스트 해야 합니다.

+ __응용 프로그램을 항상 관리자 보안 권한으로 실행__됩니다. 응용 프로그램을 대화형 사용자로 실행 하는 동안 작동 해야 합니다. Microsoft Store에서 응용 프로그램을 설치 하는 사용자가 시스템 관리자가 표준 사용자가 올바르게 실행 되지 않습니다 관리자 권한 수단을 실행 하려면 응용 프로그램을 요구 하므로 수 없습니다. 기능 중 일부에 대해 권한 상승이 필요한 앱은 Microsoft Store에서 허용되지 않습니다.

+ __응용 프로그램에 커널 모드 드라이버 또는 Windows 서비스가 필요__합니다. 브리지는 앱에 적합하지만 시스템 계정으로 실행해야 하는 커널 모드 드라이버 또는 Windows 서비스를 지원하지 않습니다. Windows 서비스 대신, [백그라운드 작업](https://msdn.microsoft.com/windows/uwp/launch-resume/create-and-register-a-background-task)을 사용합니다.

+ __앱 모듈은 in-process 방식으로 Windows 앱 패키지에 없는 프로세스로 로드됩니다.__. 이것은 허용되지 않습니다. 즉 [셸 확장](https://msdn.microsoft.com/library/windows/desktop/dd758089.aspx)과 같은 in-process 확장은 지원되지 않습니다. 그렇지만 두 앱이 같은 패키지에 있는 경우 두 앱의 프로세스 간 통신을 수행할 수 있습니다.

+ __응용 프로그램을 사용자 지정 응용 프로그램 사용자 모델 ID (AUMID)를 사용__합니다. 프로세스 자체 AUMID를 설정 하려면 [SetCurrentProcessExplicitAppUserModelID](https://msdn.microsoft.com/library/windows/desktop/dd378422.aspx) 호출 하는 경우 응용 프로그램 모델 환경/Windows 앱 패키지에 의해 생성 된 AUMID만 사용할 수 있습니다 합니다. 사용자 지정 AUMID를 정의할 수 없습니다.

+ __응용 프로그램 (HKLM) HKEY_LOCAL_MACHINE 레지스트리 하이브 수정__합니다. 응용 프로그램에서 시도 HKLM 키를 만들거나 수정을 위해 열려고 하면 액세스 거부 오류가 발생 될 수 있습니다. 에 레지스트리의 응용 프로그램 레지스트리 개인 자체 가상화 보기가 있으므로 사용자 및 컴퓨터 전체 레지스트리 하이브 (hklm) 개념이 적용 되지 않습니다. 대신 HKEY_CURRENT_USER(HKCU)에 쓰는 것과 같은 HKLM 사용 목적을 달성할 수 있는 다른 방법을 찾아야 합니다.

+ __응용 프로그램이 다른 앱을 실행 하는 수단으로 ddeexec 레지스트리 하위 키를 사용__합니다. 대신 [앱 패키지 매니페스트](https://msdn.microsoft.com/library/windows/apps/br211474.aspx)의 다양 한 Activatable* 확장에 의해 구성된 DelegateExecute 동사 처리기 중 하나를 사용합니다.

+ __응용 프로그램 관련 된 다른 앱과 데이터를 공유 하기 위해 레지스트리 또는 AppData 폴더에 씁니다__. 변환 후 AppData는 각 UWP 앱에 대한 개인 저장소인 로컬 앱 데이터 저장소로 리디렉션됩니다.

  응용 프로그램에서 HKEY_LOCAL_MACHINE 레지스트리 하이브에 쓰는 모든 항목 격리 된 바이너리 파일로 리디렉션됩니다 및 응용 프로그램에서 HKEY_CURRENT_USER 레지스트리 하이브에 쓰는 모든 항목은 개인 사용자별, 앱 별 위치에 배치 됩니다. 파일 및 레지스트리 리디렉션에 대한 자세한 내용은 [데스크톱 브리지 이면](desktop-to-uwp-behind-the-scenes.md)을 참조합니다.  

  프로세스 간에 데이터를 공유하는 다른 방법을 사용합니다. 자세한 내용은 [설정 및 기타 앱 데이터 저장 및 검색](https://msdn.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data)을 참조하세요.

+ __응용 프로그램 앱의 설치 디렉터리에 씁니다__. 예를 들어, 응용 프로그램 exe와 동일한 디렉터리에 저장 된 로그 파일에 씁니다. 이것이 지원되지 않으면 로컬 앱 데이터 저장소 등의 다른 위치를 찾아야 합니다.

+ __응용 프로그램 설치에 사용자 조작이 필요__합니다. 응용 프로그램 설치 관리자는 자동으로 실행할 수 있어야 하 고 모든 기본적으로 클린 OS 이미지에 없는 필수 구성 요소를 설치 해야 합니다.

+ __응용 프로그램에서 현재 작업 디렉터리를 사용__합니다. 런타임 시 패키지로 만든된 데스크톱 응용 프로그램 데스크톱에서 이전에 지정 된 동일한 작업 디렉터리를 가져올 되지 않습니다. LNK 바로 가기입니다. 올바른 디렉터리는 제대로 작동 하려면 응용 프로그램에 대 한 중요 한 경우 런타임에 CWD를 변경 해야 합니다.

+ __UIAccess 응용 프로그램에 필요__합니다. 응용 프로그램에서 UAC 매니페스트의 `requestedExecutionLevel` 요소에 `UIAccess=true`를 지정하는 경우 현재는 UWP로 변환할 수 없습니다. 자세한 내용은 [UI 자동화 보안 개요](https://msdn.microsoft.com/library/ms742884.aspx)를 참조하세요.

+ __응용 프로그램은 COM 개체를 노출__합니다. 프로세스와 패키지 내에서의 확장은 in-process 및 OOP(Out-of-Process) 방식으로 COM 및 OLE 서버를 등록해 사용할 수 있습니다.  크리에이터 업데이트는 패키지 밖에서도 볼 수 있도록 OOP COM 및 OLE 서버를 등록할 수 있게 해주는 패키지형 COM 지원을 추가합니다.  [데스크톱 브리지에 대한 COM 서버 및 OLE 문서 지원](https://blogs.windows.com/buildingapps/2017/04/13/com-server-ole-document-support-desktop-bridge/#bjPyETFgtpZBGrS1.97)를 참조하세요.

   패키지형 COM 지원 기능은 기존 COM API에서는 작동하지만, 패키지형 COM에 대한 위치가 개인 위치에 있기 때문에 레지스트리를 직접 읽을 때 사용되는 응용 프로그램 확장에서는 작동하지 않습니다.

+ __응용 프로그램이 다른 프로세스에서 사용할 수 있도록 GAC 어셈블리를 노출__합니다. 현재 릴리스에서 응용 프로그램이 Windows 앱 패키지 외부의 실행 파일에서 발생 한 프로세스에서 사용할 수 있도록 GAC 어셈블리를 노출할 수 없습니다. 패키지 내의 프로세스는 정상적으로 GAC 어셈블리를 등록하고 사용할 수 있지만, 이들 항목이 외부에 표시되지는 않습니다. 즉, OLE와 같은 interop 시나리오는 외부 프로세스에서 호출되는 경우 작동하지 않습니다.

+ __응용 프로그램이 CRT (C 런타임 라이브러리)는 지원 되지 않는 방식으로 연결 됩니다__. Microsoft C/C++ 런타임 라이브러리는 Microsoft Windows 운영 체제용 프로그래밍 루틴을 제공합니다. 이러한 루틴은 C 및 C++ 언어에서 제공하지 않는 많은 일반적인 프로그래밍 작업을 자동화합니다. 응용 프로그램는 C/c + + 런타임 라이브러리를 사용 하는 경우 지원 되는 방식으로 연결 되는지 확인 해야 합니다.

    Visual Studio 2017에서는 현재 버전의 CRT 코드에 대한 정적 및 동적 연결(코드에서 공통 DLL 파일을 사용할 수 있도록 함) 또는 정적 연결(라이브러리를 코드에 직접 연결함)을 모두 지원합니다. 가능 하면 응용 프로그램 사용을 VS 2017와 함께 동적 연결을 사용 하는 것이 좋습니다.

    이전 버전의 Visual Studio에서의 지원은 각기 다릅니다. 자세한 내용은 다음 표를 참조하세요.

    <table>
    <th>Visual Studio 버전</td><th>동적 연결</th><th>정적 연결</th></th>
    <tr><td>2005(VC 8)</td><td>지원되지 않음</td><td>지원</td>
    <tr><td>2008(VC 9)</td><td>지원되지 않음</td><td>지원</td>
    <tr><td>2010(VC 10)</td><td>지원함</td><td>지원</td>
    <tr><td>2012(VC 11)</td><td>지원</td><td>지원되지 않음</td>
    <tr><td>2013(VC 12)</td><td>지원</td><td>지원되지 않음</td>
    <tr><td>2015 및 2017(VC 14)</td><td>지원</td><td>지원</td>
    </table>

    참고: 모든 경우에 공개적으로 사용 가능한 최신 CRT에 연결해야 합니다.

+ __응용 프로그램 설치 하 고 Windows-정렬 폴더에서 어셈블리를 로드__합니다. 예를 들어, 응용 프로그램이 VC8 또는 VC9 C 런타임 라이브러리를 사용 하 여 하며 코드는 공통 DLL 파일을 사용 하 여 공유 폴더에서 의미 Windows-정렬 폴더에서 압축이 연결 동적으로 됩니다. 이는 지원되지 않습니다. 재배포 가능 라이브러리 파일을 코드에 직접 연결하여 정적으로 연결해야 합니다.

+ __응용 프로그램에서 System32/SysWOW64 폴더의 종속성을 사용__합니다. 해당 DLL이 작동하려면 Windows 앱 패키지의 가상 파일 시스템 부분에 포함해야 합니다. 이렇게 하면 응용 프로그램이 **System32**Dll이 설치 된 경우에 따라 동작 하는/**SysWOW64** 폴더. 패키지의 루트에 **VFS**라는 폴더를 만듭니다. 해당 폴더 안에 **SystemX64** 및 **SystemX86** 폴더를 만듭니다. 그런 다음 32비트 버전의 DLL은 **SystemX86** 폴더에 넣고 64비트 버전은 **SystemX64** 폴더에 넣습니다.

+ __앱에서 VCLibs 프레임워크 패키지를 사용합니다__. Windows 앱 패키지에서 종속성으로 정의된 경우 Microsoft Store에서 직접 VCLibs 라이브러리를 설치할 수 있습니다. 예를 들어, 응용 프로그램 Dev11 VCLibs 패키지를 사용 하는 경우 다음과 같이 변경 응용 프로그램 패키지 매니페스트를: 아래는 `<Dependencies>` 노드를 추가 합니다.  
`<PackageDependency Name="Microsoft.VCLibs.110.00.UWPDesktop" MinVersion="11.0.24217.0" Publisher="CN=Microsoft Corporation, O=Microsoft Corporation, L=Redmond, S=Washington, C=US" />`  
Microsoft Store에서 설치하는 동안 앱이 설치되기 전에 적절한 버전(x86 또는 x64)의 VCLibs 프레임워크가 설치됩니다.  
응용 프로그램 테스트용으로 로드 하 여 설치 된 경우 종속성 설치 되지 않습니다. 수동으로 컴퓨터에 종속성을 설치하려면 데스크톱 브리지에 적합한 VCLibs 프레임워크 패키지를 다운로드하고 설치해야 합니다. 이러한 시나리오에 대한 자세한 내용은 [Centennial 프로젝트에서 Visual C++ 런타임 사용](https://blogs.msdn.microsoft.com/vcblog/2016/07/07/using-visual-c-runtime-in-centennial-project/)을 참조하세요.

  **프레임워크 패키지 **:

  * [데스크톱 브리지에 대한 VC 14.0 프레임워크 패키지](https://www.microsoft.com/download/details.aspx?id=53175)
  * [데스크톱 브리지에 대한 VC 12.0 프레임워크 패키지](https://www.microsoft.com/download/details.aspx?id=53176)
  * [데스크톱 브리지에 대한 VC 11.0 프레임워크 패키지](https://www.microsoft.com/download/details.aspx?id=53340)


+ __응용 프로그램에 사용자 지정 점프 목록이 포함 되어__있습니다. 점프 목록을 사용할 때 고려해야 할 몇 가지 문제와 주의 사항이 있습니다.

    - __앱의 아키텍처가 OS와 일치하지 않습니다.__  점프 목록이 현재 제대로 작동 하지 않습니다 응용 프로그램 및 운영 체제 아키텍처가 일치 하지 않는 경우 (예: x86 x64에서 실행 되는 응용 프로그램 Windows). 이때 해결 방법이 없습니다 이외의 아키텍처와 일치 하는 응용 프로그램을 다시 컴파일해야 합니다.

    - __응용 프로그램 점프 목록 항목을 만들고 [:: Setappid](https://msdn.microsoft.com/library/windows/desktop/dd378403(v=vs.85).aspx) 또는 [SetCurrentProcessExplicitAppUserModelID](https://msdn.microsoft.com/library/windows/desktop/dd378422(v=vs.85).aspx)를 호출__합니다. 코드에 프로그래밍 방식으로 AppID를 설정하지 마세요. 그러면 점프 목록 항목이 나타나지 않게 됩니다. 응용 프로그램에 사용자 지정 Id가 필요할 경우 매니페스트 파일을 사용 하 여 지정 합니다. 지침에 대 한 [데스크톱 응용 프로그램을](desktop-to-uwp-manual-conversion.md) 수동으로 패키징하 참조 하세요. 응용 프로그램의 AppID는 *YOUR_PRAID_HERE* 섹션에 지정됩니다.

    - __응용 프로그램 패키지의 실행 파일을 참조 하는 점프 목록 셸 링크를 추가__합니다. 점프 목록에서 바로 패키지의 실행 파일을 시작할 수는 없습니다(앱 자체의 .exe에 대한 절대 경로는 예외). 대신, (패키지로 만든된 데스크톱 응용 프로그램을 경로에 있는 것 처럼 키워드를 통해 시작할 수 있음)는 앱 실행 별칭을 등록 대신 별칭에 링크 대상 경로 설정 합니다. AppExecutionAlias 확장을 사용 하는 방법에 대 한 자세한 내용은 [Windows 10 데스크톱 응용 프로그램 통합](desktop-to-uwp-extensions.md)을 참조 하세요. 점프 목록에 원래 .exe와 일치하는 링크의 자산이 필요할 경우 [**SetIconLocation**](https://msdn.microsoft.com/library/windows/desktop/bb761047(v=vs.85).aspx)을 사용하여 아이콘과 같은 자산을 설정하고 다른 사용자 지정 항목에 사용하는 것 같은 PKEY_Title로 표시 이름을 설정해야 합니다.

    - __절대 경로 통해 앱의 패키지의 자산을 참조 하는 점프 목록 항목을 추가 하는 응용 프로그램__입니다. 패키지가 업데이트 되어 자산 (예: 아이콘, 문서, 실행 파일 등)의 위치를 변경 하는 경우 응용 프로그램의 설치 경로가 변경 될 수 있습니다. 점프 목록 항목이 절대 경로 통해 이러한 자산을 참조할 경우 응용 프로그램 (예: 응용 프로그램 시작) 점프 목록을 정기적으로 새로 고쳐야 경로 올바르게 확인할 수 있도록 합니다. 또는 package-relative ms-resource URI 스키마(언어, DPI 및 고대비 인식 가능)를 사용하여 문자열 및 이미지 자산을 참조할 수 있는 UWP [**Windows.UI.StartScreen.JumpList**](https://msdn.microsoft.com/library/windows/apps/windows.ui.startscreen.jumplist.aspx) API를 사용하세요.

+ __응용 프로그램이 작업을 수행 하는 유틸리티를 시작__합니다. PowerShell, Cmd.exe 같은 명령 유틸리티가 시작되지 않도록 방지합니다. 사실, 사용자가 Windows 10 S를 실행 하는 시스템에 응용 프로그램을 설치 하는 경우 다음 응용 프로그램 수 없습니다 전혀 시작 합니다. Microsoft Store에 제출 된 모든 앱은 Windows 10 S와 호환 되어야 하기 때문에 응용 프로그램의 Microsoft Store 제출 막을 수도 있습니다.

유틸리티를 시작하는 것이 운영 체제에서 정보를 얻거나 레지스트리를 액세스하거나 시스템 기능에 액세스할 수 있는 편리한 방법일 때가 많습니다. 한편, UWP API를 사용해 이러한 종류의 작업을 대신 수행할 수 있습니다. 이러한 Api는를 실행 하려면 별도 실행 파일이 필요 하지 않은 응용 프로그램 패키지 외부에 도달 하지 못하도록 뛰어나며 무엇 보다도 때문에 성능이 합니다. 앱의 디자인 일관성 격리, 신뢰 및 응용 프로그램을 패키징하고 및 응용 프로그램의 Windows 10 S를 실행 하는 시스템에서 예상 대로 동작을 함께 제공 되는 보안을 유지

+ __응용 프로그램 호스트 추가 기능, 플러그 인 또는 확장__합니다.   많은 경우에 COM 스타일의 확장은 확장이 패키지로 만들어지지 않고, 완전 신뢰 상태로 설치되기만 하면 계속해서 작동하게 됩니다. 설치 관리자 레지스트리를 수정 하 고 호스트 응용 프로그램 찾을 것으로 예상 하는 모든 확장 파일을 배치 하는 데 해당 완전 신뢰 접근 권한 값을 사용할 수 때문입니다.

   그러나 이러한 확장을 패키지와 다음 Windows 앱 패키지를 설치 하는 경우 각 패키지 (호스트 응용 프로그램 및 확장)은 서로 격리 되므로 작동 되지. 에 대 한 자세한 내용을 어떻게 응용 프로그램은 시스템에서 격리 된 [데스크톱 브리지의 백그라운드 작업](desktop-to-uwp-behind-the-scenes.md)을 참조 하십시오.

 사용자가 Windows 10 S를 실행하는 시스템에 설치하는 모든 응용 프로그램과 확장은 반드시 Windows 앱 패키지 형태로 설치해야 합니다. 따라서 확장을 패키징 하려는 참가자 패키징합니다 하는 것을 권장 하려는 경우, 호스트 응용 프로그램 패키지와 모든 확장 패키지 간의 통신을 쉽게 수 방법을 고려 합니다. 그 한 가지 방법은 [앱 서비스](../launch-resume/app-services.md)를 사용하는 것입니다.

+ __응용 프로그램 코드를 생성__합니다. 응용 프로그램 메모리에 사용 하는 코드를 생성할 수 있지만 Windows 앱 인증 프로세스는 앱 제출 하기 전에 해당 코드의 유효성을 검사할 수 없는 디스크에 쓰기 생성 된 코드를 방지 합니다. 또한 디스크에 코드를 작성 하는 앱 Windows 10 S를 실행 하는 시스템에서 제대로 실행 되지 않습니다. Microsoft Store에 제출 된 모든 앱은 Windows 10 S와 호환 되어야 하기 때문에 응용 프로그램의 Microsoft Store 제출 막을 수도 있습니다.

>[!IMPORTANT]
> Windows 앱 패키지를 만든 후 Windows 10 s가 실행 되는 시스템에서 올바르게 작동 하는지 확인 하는 응용 프로그램을 테스트 하세요 Microsoft Store에 제출 된 모든 앱와 호환 되어야 합니다. 호환 되지 않는 Windows 10 s가 앱을 스토어에서 허용 되지 않습니다. [Windows 10 S용 Windows 앱 테스트](desktop-to-uwp-test-windows-s.md)를 참조하세요.

## <a name="next-steps"></a>다음 단계

**질문에 대한 답변 찾기**

질문이 있으세요? Stack Overflow에서 질문해 주세요. 저희 팀은 이러한 [태그](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge)를 모니터링합니다. [여기](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D)에서 Microsoft에 문의할 수도 있습니다.

**피드백 제공 또는 기능 제안**

[UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial)를 참조하세요.

**데스크톱 앱용 Windows 앱 패키지 만들기**

[Windows 앱 패키지 만들기](desktop-to-uwp-root.md#convert) 참조
