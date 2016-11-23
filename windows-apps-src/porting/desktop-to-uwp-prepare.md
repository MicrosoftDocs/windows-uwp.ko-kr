---
author: awkoren
Description: "이 문서에는 데스크톱-UWP 브리지를 사용하여 앱을 변환하기 전에 알아야 할 사항이 나열되어 있습니다. 앱의 변환 프로세스를 준비하는 데 많은 작업을 수행하지 않아도 됩니다."
Search.Product: eADQiWindows 10XVcnh
title: "데스크톱-UWP 브리지용 앱 준비"
translationtype: Human Translation
ms.sourcegitcommit: 8429e6e21319a03fc2a0260c68223437b9aed02e
ms.openlocfilehash: 4cf9c509be52a8b2c03cdaa9ac68b98ba49b7094

---

# 데스크톱 브리지를 사용하여 앱 변환 준비

이 문서에는 데스크톱-UWP 브리지를 사용하여 앱을 변환하기 전에 알아야 할 사항이 나열되어 있습니다. 앱의 변환 프로세스를 준비하는 데 많은 작업을 수행하지 않아도 됩니다. 하지만 아래의 항목이 사용 중인 응용 프로그램에 적용되는 경우 변환 전에 문제를 해결해야 합니다. Windows 스토어는 라이선스 및 자동 업데이트를 처리하므로 코드베이스에서 해당 기능을 제거해도 됩니다.

+ __앱에서 4.6.1 이전 버전의 .NET을 사용합니다__. .NET 4.6.1만 지원됩니다. 변환하기 전에 앱의 대상을 .NET 4.6.1로 변경해야 합니다. 

+ __앱은 항상 관리자 보안 권한으로 실행됩니다__. 대화형 사용자로 실행하는 동안 앱은 계속 작동해야 합니다. Windows 스토어에서 앱을 설치하는 사용자가 시스템 관리자가 아닐 수 있으므로 앱을 관리자 권한으로 실행하도록 요구해도 표준 사용자의 경우는 제대로 진행되지 않습니다.

+ __앱에는 커널 모드 드라이버 또는 Windows 서비스가 필요합니다__. 브리지는 앱에 적합하지만 시스템 계정으로 실행해야 하는 커널 모드 드라이버 또는 Windows 서비스를 지원하지 않습니다. Windows 서비스 대신, [백그라운드 작업](https://msdn.microsoft.com/windows/uwp/launch-resume/create-and-register-a-background-task)을 사용합니다.

+ __앱 모듈은 AppX 패키지에 없는 프로세스에 in-process로 로드됩니다__. 이것은 허용되지 않습니다. 즉 [셸 확장](https://msdn.microsoft.com/library/windows/desktop/dd758089.aspx)과 같은 in-process 확장은 지원되지 않습니다. 그렇지만 두 앱이 같은 패키지에 있는 경우 두 앱의 프로세스 간 통신을 수행할 수 있습니다.

+ __앱에서 [SetDllDirectory](https://msdn.microsoft.com/library/windows/desktop/ms686203) 또는 [AddDllDirectory](https://msdn.microsoft.com/library/windows/desktop/hh310513)를 호출합니다__. 이러한 함수는 현재 변환된 앱에서 지원되지 않습니다. 이후 릴리스에서 지원을 추가하기 위해 노력 중입니다. 이 문제를 해결하기 위해 이러한 함수를 사용하여 찾은 모든 .dll을 패키지 루트에 복사할 수 있습니다. 

+ __앱이 사용자 지정 AUMID(응용 프로그램 사용자 모델 ID)를 사용합니다__. 프로세스가 [SetCurrentProcessExplicitAppUserModelID](https://msdn.microsoft.com/library/windows/desktop/dd378422.aspx)를 호출하여 자체 AUMID를 설정하는 경우 앱 모델 환경/AppX 패키지에 의해 생성된 AUMID만 사용할 수 있습니다. 사용자 지정 AUMID를 정의할 수 없습니다.

+ __앱은 HKEY_LOCAL_MACHINE (HKLM) 레지스트리 하이브를 수정합니다__. 앱에서 HKLM 키를 만들거나 수정을 위해 이 키를 열려고 하면 액세스 거부 오류가 발생합니다. 앱에는 레지스트리의 자체 가상화 보기가 있으므로 사용자 및 컴퓨터 전체 레지스트리 하이브(HKLM의 유형) 개념이 적용되지 않습니다. 대신 HKEY_CURRENT_USER(HKCU)에 쓰는 것과 같은 HKLM 사용 목적을 달성할 수 있는 다른 방법을 찾아야 합니다.

+ __앱은 다른 앱을 시작하는 수단으로 ddeexec 레지스트리 하위 키를 사용합니다__. 대신 [앱 패키지 매니페스트](https://msdn.microsoft.com/library/windows/apps/br211474.aspx)의 다양 한 Activatable* 확장에 의해 구성된 DelegateExecute 동사 처리기 중 하나를 사용합니다.

+ __앱은 다른 앱과 데이터를 공유하는 데 사용하려는 AppData 폴더에 씁니다__. 변환 후 AppData는 각 UWP 앱에 대한 개인 저장소인 로컬 앱 데이터 저장소로 리디렉션됩니다. 프로세스 간에 데이터를 공유하는 다른 방법을 사용합니다. 자세한 내용은 [설정 및 기타 앱 데이터 저장 및 검색](https://msdn.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data)을 참조하세요.

+ __앱에서 앱용 설치 디렉터리에 씁니다__. 예를 들어 앱은 exe와 동일한 디렉터리에 추가된 로그 파일에 씁니다. 이것이 지원되지 않으면 로컬 앱 데이터 저장소 등의 다른 위치를 찾아야 합니다.

+ __앱 설치에 사용자 조작이 필요합니다__. 앱 설치 관리자는 자동으로 실행될 수 있어야 하며 기본적으로 클린 OS 이미지에 없는 모든 필수 구성 요소를 설치해야 합니다.

+ __앱에서 현재 작업 디렉터리를 사용합니다__. 런타임에 변환된 앱은 이전에 데스크톱 .LNK 바로 가기에서 지정한 동일한 작업 디렉터리를 얻지 못합니다. 앱의 올바른 작동을 위해 올바른 디렉터리를 확보해야 하는 경우 런타임에 CWD를 변경해야 합니다.

+ __앱에는 UIAccess가 필요합니다__. 응용 프로그램에서 UAC 매니페스트의 `requestedExecutionLevel` 요소에 `UIAccess=true`를 지정하는 경우 현재는 UWP로 변환할 수 없습니다. 자세한 내용은 [UI 자동화 보안 개요](https://msdn.microsoft.com/library/ms742884.aspx)를 참조하세요.

+ __다른 프로세스에서 사용할 수 있도록 앱에서 COM 개체 또는 GAC 어셈블리를 노출합니다__. 현재 릴리스에서 앱은 AppX 패키지 외부의 실행 파일에서 발생한 프로세스에서 사용할 수 있도록 COM 개체 또는 GAC 어셈블리를 노출할 수 없습니다. 패키지 내의 프로세스는 정상적으로 COM 개체와 GAC 어셈블리를 등록하고 사용할 수 있지만 이들 항목이 외부에 표시되지는 않습니다. 즉, OLE와 같은 interop 시나리오는 외부 프로세스에서 호출되는 경우 작동하지 않습니다. 

+ __앱에서 지원되지 않는 방식으로 CRT(C 런타임 라이브러리)를 연결하고 있습니다__. Microsoft C/C++ 런타임 라이브러리는 Microsoft Windows 운영 체제용 프로그래밍 루틴을 제공합니다. 이러한 루틴은 C 및 C++ 언어에서 제공하지 않는 많은 일반적인 프로그래밍 작업을 자동화합니다. 앱에서 C/C++ 런타임 라이브러리를 사용하는 경우 해당 라이브러리가 지원되는 방식으로 연결되는지 확인해야 합니다. 
    
    Visual Studio 2015에서는 현재 버전의 CRT 코드에 대한 동적 연결(코드에서 공통 DLL 파일을 사용할 수 있도록 함)과 정적 연결(라이브러리를 코드에 직접 연결함)을 둘 다 지원합니다. 가능하면 앱에서 VS 2015와 함께 동적 연결을 사용하는 것이 좋습니다. 

    이전 버전의 Visual Studio에서의 지원은 각기 다릅니다. 자세한 내용은 다음 표를 참조하세요. 

    <table>
    <th>Visual Studio 버전</td><th>동적 연결</th><th>정적 연결</th></th>
    <tr><td>2005(VC 8)</td><td>지원되지 않음</td><td>지원함</td>
    <tr><td>2008(VC 9)</td><td>지원되지 않음</td><td>지원함</td>
    <tr><td>2010(VC 10)</td><td>지원</td><td>지원</td>
    <tr><td>2012(VC 11)</td><td>지원함</td><td>지원되지 않음</td>
    <tr><td>2013(VC 12)</td><td>지원함</td><td>지원되지 않음</td>
    <tr><td>2015(VC 14)</td><td>지원</td><td>지원</td>
    </table>
    
    참고: 모든 경우에 공개적으로 사용 가능한 최신 CRT에 연결해야 합니다.

+ __앱에서 Windows side-by-side 폴더의 어셈블리를 설치하고 로드합니다__. 예를 들어 앱에서 C 런타임 라이브러리 VC8 또는 VC9를 사용하고 Windows side-by-side 폴더에 있는 해당 라이브러리를 동적으로 연결하고 있는 경우 코드에서 공유 폴더의 공통 DLL 파일을 사용하고 있는 것입니다. 이는 지원되지 않습니다. 재배포 가능 라이브러리 파일을 코드에 직접 연결하여 정적으로 연결해야 합니다.


<!--HONumber=Nov16_HO1-->

