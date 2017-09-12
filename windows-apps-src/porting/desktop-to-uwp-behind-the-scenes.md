---
author: normesta
Description: "이 문서에서는 데스크톱 브리지가 이면에서 작동하는 방식에 대해 깊이 있게 살펴봅니다."
title: "데스크톱 브리지의 백그라운드 작업"
ms.author: normesta
ms.date: 05/25/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: a399fae9-122c-46c4-a1dc-a1a241e5547a
ms.openlocfilehash: 050499baaf383fc135d833ae1e4733c95f2b5fa1
ms.sourcegitcommit: 7540962003b38811e6336451bb03d46538b35671
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/26/2017
---
# <a name="behind-the-scenes-of-the-desktop-bridge"></a>데스크톱 브리지의 백그라운드 작업

이 문서에서는 데스크톱 브리지가 이면에서 작동하는 방식에 대해 깊이 있게 살펴봅니다.

데스크톱 브리지의 주요 목표는 다른 앱과의 호환성을 최대한 유지하면서 응용 프로그램 상태를 시스템 상태에서 분리하는 것입니다. UWP(유니버설 Windows 플랫폼) 내부에 응용 프로그램을 배치한 다음 런타임에 파일 시스템과 레지스트리에 일부 변경한 내용을 검색하고 리디렉션하면 이 목표가 달성됩니다.

데스크톱을 위해 만든 패키지는 데스크톱 전용으로 완전 신뢰 응용 프로그램으로 가상화되거나 샌드박스가 적용되지 않습니다. 이렇게 하면 기존의 데스크톱 응용 프로그램이 수행한 방식과 동일하게 다른 앱을 조작할 수 있습니다.

## <a name="installation"></a>설치

앱 패키지는 *C:\Program Files\WindowsApps\package_name* 아래 *app_name.exe* 실행 파일을 사용하여 설치됩니다. 각 패키지 폴더에는 패키지 앱에 대한 특수 XML 네임스페이스를 포함하는 매니페스트(AppxManifest.xml)가 포함됩니다. 매니페스트 파일 내에는 완전 신뢰 앱을 참조하는 ```<EntryPoint>``` 요소가 있습니다. 앱이 시작되면 앱 컨테이너 내에서 실행되지 않고 대신 평소와 같이 해당 사용자 권한으로 실행됩니다.

배포 후 패키지 파일은 읽기 전용으로 표시되며 운영 체제에 의해 엄격하게 잠깁니다. Windows에서는 이러한 파일이 손상되는 경우 앱이 실행되지 않게 합니다.

## <a name="file-system"></a>파일 시스템

브리지는 앱 상태를 포함하기 위해 앱에서 수행한 변경 내용을 AppData로 캡처하려고 합니다. 만들기, 삭제 및 업데이트를 포함하여 AppData 폴더(예: *C:\Users\user_name\AppData*)에 대한 사용자의 모든 쓰기가 사용자별/앱별 개인 위치에 기록 중 복사됩니다. 이 경우 패키지로 만든 앱에서 실제로 개인 복사본을 수정하는 경우 실제 AppData를 편집하고 있다고 착각할 수 있습니다. 이 방식으로 쓰기를 리디렉션하면 시스템에서는 앱에서 수정한 모든 파일 내용을 추적할 수 있습니다. 이렇게 하면 앱을 제거하는 경우 시스템에서 해당 파일을 제거하여 시스템 문제가 줄어들고 사용자를 위해 더 나은 앱 제거 환경이 제공됩니다.

AppData 리디렉션 외에도 브리지는 Windows의 잘 알려진 폴더(System32, Program Files (x86) 등)를 앱 패키지의 해당 디렉터리와 동적으로 병합합니다. 각 패키지에는 루트에 "VFS"라는 이름의 폴더가 포함되어 있습니다. VFS 디렉터리에 있는 파일이나 디렉터리의 읽기 권한은 런타임 시 각각 해당하는 네이티브 권한으로 병합됩니다. 예를 들어 앱에는 *C:\Program Files\WindowsApps\package_name\VFS\SystemX86\vc10.dll*이 앱 패키지의 일부로 포함될 수 있지만 파일은 *C:\Windows\System32\vc10.dll*에 설치되는 것으로 나타납니다.  이렇게 하면 패키지가 없는 위치에서도 파일이 있을 수 있다고 예상하는 데스크톱 응용 프로그램과 호환성을 유지합니다.

앱 패키지에서 파일/폴더에 대한 쓰기는 허용되지 않습니다. 패키지에 포함되지 않은 파일과 폴더에 대한 쓰기는 브리지에서 무시되며 사용자에게 권한이 있어야만 허용됩니다.

### <a name="common-operations"></a>일반 작업

다음 간단한 참조 테이블에서는 일반적인 파일 시스템 작업과, 브리지에서 처리하는 방법을 보여 줍니다.

작업 | 결과 | 예제
:--- | :--- | :---
잘 알려진 Windows 파일이나 폴더를 읽거나 열거 | 해당하는 로컬 시스템 항목과 *C:\Program Files\package_name\VFS\well_known_folder*의 동적 병합입니다. | *C:\Windows\System32*를 읽으면 *C:\Windows\System32* 콘텐츠와 *C:\Program Files\WindowsApps\package_name\VFS\SystemX86* 콘텐츠가 반환됩니다.
AppData에서 쓰기 | 사용자별/앱별 위치에 기록 중 복사됩니다. | 일반적으로 AppData는 *C:\Users\user_name\AppData*입니다.  
패키지 내 기록 | 허용되지 않음 패키지는 읽기 전용입니다. | *C:\Program Files\WindowsApps\package_name* 내 기록은 허용되지 않습니다.
패키지 외부에 기록 | 브리지에서 무시됩니다. 사용자에게 권한이 있으면 허용됩니다. | *C:\Windows\System32\foo.dll*에 대한 기록은 패키지에 *C:\Program Files\WindowsApps\package_name\VFS\SystemX86\foo.dll*이 없고 사용자에게 권한이 있으면 허용됩니다.

### <a name="packaged-vfs-locations"></a>패키지에 포함된 VFS 위치

다음 표에서 패키지의 일부로 제공된 파일이 앱에 대한 시스템에 오버레이된 위치를 보여 줍니다. 앱은 이러한 파일이 실제로 *C:\Program Files\WindowsApps\package_name\VFS* 내 리디렉션된 위치에 있을 경우 나열된 시스템 위치에 있다고 인식합니다. FOLDERID 위치는 [**KNOWNFOLDERID**](https://msdn.microsoft.com/library/windows/desktop/dd378457.aspx) 상수입니다.

시스템 위치 | 리디렉션된 위치([PackageRoot]\VFS\ 아래) | 아키텍처에서 유효
 :--- | :--- | :---
FOLDERID_SystemX86 | SystemX86 | x86, amd64
FOLDERID_System | SystemX64 | amd64
FOLDERID_ProgramFilesX86 | ProgramFilesX86 | x86, amd6
FOLDERID_ProgramFilesX64 | ProgramFilesX64 | amd64
FOLDERID_ProgramFilesCommonX86 | ProgramFilesCommonX86 | x86, amd64
FOLDERID_ProgramFilesCommonX64 | ProgramFilesCommonX64 | amd64
FOLDERID_Windows | Windows | x86, amd64
FOLDERID_ProgramData | Common AppData | x86, amd64
FOLDERID_System\catroot | AppVSystem32Catroot | x86, amd64
FOLDERID_System\catroot2 | AppVSystem32Catroot2 | x86, amd64
FOLDERID_System\drivers\etc | AppVSystem32DriversEtc | x86, amd64
FOLDERID_System\driverstore | AppVSystem32Driverstore | x86, amd64
FOLDERID_System\logfiles | AppVSystem32Logfiles | x86, amd64
FOLDERID_System\spool | AppVSystem32Spool | x86, amd64

## <a name="registry"></a>레지스트리

브리지는 파일 시스템과 유사하게 레지스트리를 처리합니다. 앱 패키지에는 registry.dat 파일이 포함되어 있어 실제 레지스트리의 *HKLM\Software*와 논리적으로 같은 역할을 합니다. 런타임에 이 가상 레지스트리는 이 하이브의 콘텐츠를 네이티브 시스템 하이브로 병합하여 두 하이브에 대해 단일 보기를 제공합니다. 예를 들어, registry.dat에 단일 키 "Foo"가 포함되어 있으면 런타임에 *HKLM\Software*를 읽으면 모든 네이티브 시스템 키뿐 아니라 “Foo”도 포함되어 표시됩니다.

*HKLM\Software*의 키만 패키지에 포함됩니다. *HKCU* 또는 레지스트리의 다른 부분에 있는 키는 포함되지 않습니다. 패키지에서 키 또는 값에 대한 기록은 허용되지 않습니다. 패키지에 포함되지 않은 키 또는 값에 대한 쓰기는 브리지에서 무시되며 사용자에게 권한이 있어야만 허용됩니다.

HKCU에서 모든 쓰기는 사용자별/앱별 위치에 기록 중 복사됩니다. 이렇게 하면 제거 정리와 관련하여 브리지에서 파일 시스템을 처리할 때와 동일한 이점이 제공됩니다. 일반적으로 설치 제거 프로그램에서는 *HKEY_CURRENT_USER*를 정리할 수 없습니다. 로그아웃된 사용자의 레지스트리 데이터는 마운트되지 않고 사용할 수 없습니다.

모든 쓰기는 패키지 업그레이드 중에는 유지 되며 앱을 완전히 제거해야 만 삭제됩니다.

### <a name="common-operations"></a>일반 작업

다음 간단한 참조 테이블에서는 일반적인 레지스트리 작업과, 브리지에서 처리하는 방법을 보여 줍니다.

작업 | 결과 | 예제
:--- | :--- | :---
*HKLM\Software*를 읽거나 열거 | 로컬 시스템에 해당하는 하이브와 패키지 하이브를 동적으로 병합합니다. | registry.dat에 단일 키 "Foo"가 포함되어 있으면 런타임에 *HKLM\Software*에서는 *HKLM\Software*와 *HKLM\Software\Foo*의 두 콘텐츠가 모두 표시됩니다.
HKCU에서 쓰기 | 사용자별/앱별 개인 위치에 기록 중 복사됩니다. | 파일용 AppData와 동일합니다.
패키지 내에 씁니다. | 허용되지 않음 패키지는 읽기 전용입니다. | *HKLM\Software* 내 쓰기의 경우 패키지 하이브에 해당 키/값이 있는 경우 허용되지 않습니다.
패키지 외부에 기록 | 브리지에서 무시됩니다. 사용자에게 권한이 있으면 허용됩니다. | *HKLM\Software* 내 쓰기의 경우 패키지 하이브에 해당 키/값이 없고 사용자에게 정확한 액세스 권한이 있는 한 허용됩니다.

## <a name="uninstallation"></a>제거

사용자가 패키지를 제거하면 *C:\Program Files\WindowsApps\package_name*에 있는 모든 파일 및 폴더가 제거되고, 브리지에서 캡처한 레지스트리 또는 AppData로 리디렉션된 기록도 제거됩니다.

## <a name="next-steps"></a>다음 단계

**특정 질문에 대한 답변 찾기**

저희 팀은 이러한 [StackOverflow 태그](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge)를 모니터링합니다.

**이 문서에 대한 피드백 제공**

아래의 설명 섹션을 사용합니다.
