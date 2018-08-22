---
author: PatrickFarley
ms.assetid: 1526FF4B-9E68-458A-B002-0A5F3A9A81FD
title: Windows 앱 인증 키트 테스트
description: Windows 앱 인증 키트 앱 Microsoft 저장소에 게시를 수행할 준비가 되었는지 확인 하는 데 도움이 되는 테스트의 숫자를 포함 합니다.
ms.author: pafarley
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 앱 인증 (영문)
ms.localizationpriority: medium
ms.openlocfilehash: 49ecc472c8c1d4adebd8376fce9d2d5e6e2a955e
ms.sourcegitcommit: f2f4820dd2026f1b47a2b1bf2bc89d7220a79c1a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/22/2018
ms.locfileid: "2802044"
---
# <a name="windows-app-certification-kit-tests"></a>Windows 앱 인증 키트 테스트


[Windows 앱 인증 키트](windows-app-certification-kit.md) 앱은 Microsoft 저장소에 게시할 수를 확인 하는 데 도움이 되는 테스트의 숫자를 포함 합니다. 테스트 세부 정보를 자신의 조건으로 아래 나열 된 및 실패의 경우 작업을 제시 합니다.

## <a name="deployment-and-launch-tests"></a>배포 및 시작 테스트

인증 테스트 동안 앱을 모니터하여 크래시가 발생하거나 작동이 중단되는 경우를 기록합니다.

### <a name="background"></a>배경

앱이 응답하지 않거나 크래시가 발생하면 사용자 데이터가 손실되고 성능이 저하될 수 있습니다.

Windows 호환성 모드, AppHelp 메시지 또는 호환성 수정을 사용하지 않고 앱이 제대로 작동해야 합니다.

앱이 로드할 DLL을 HKEY\-LOCAL\-MACHINE\\Software\\Microsoft\\Windows NT\\CurrentVersion\\Windows\\AppInit\-DLLs 레지스트리 키에 나열하지 않아야 합니다.

### <a name="test-details"></a>테스트 정보

인증 테스트 전체에서 앱 복원력 및 안정성을 테스트합니다.

Windows 앱 인증 키트에서 [**IApplicationActivationManager::ActivateApplication**](https://msdn.microsoft.com/library/windows/desktop/Hh706903)를 호출하여 앱을 시작합니다. **ActivateApplication**에서 앱을 시작하려면 UAC(사용자 계정 컨트롤)을 사용할 수 있어야 하며 화면 해상도가 1024 x 768 또는 768 x 1024 이상이어야 합니다. 두 조건 중 하나가 충족되지 않으면 앱이 이 테스트에 실패합니다.

### <a name="corrective-actions"></a>수정 작업

테스트 컴퓨터에서 UAC를 사용할 수 있는지 확인합니다.

화면이 충분히 큰 컴퓨터에서 테스트를 실행하고 있는지 확인합니다.

앱이 시작되지 않는 경우 테스트 플랫폼이 [**ActivateApplication**](https://msdn.microsoft.com/library/windows/desktop/Hh706903)의 필수 조건을 충족하면 활성화 이벤트 로그를 검토하여 문제를 해결할 수 있습니다. 이벤트 로그에서 이러한 항목을 찾으려면

1.  eventvwr.exe를 열고 Application and Services Log\\Microsoft\\Windows\\Immersive-Shell 폴더로 이동합니다.
2.  보기를 필터링하여 이벤트 ID 5900-6000을 표시합니다.
3.  로그 항목에서 앱이 시작되지 않은 이유를 설명하는 정보를 검토합니다.

문제가 있는 파일을 식별하고 문제를 해결합니다. 앱을 다시 빌드하고 다시 테스트하세요. 앱을 디버그하는 데 사용할 수 있는 덤프 파일이 Windows 앱 인증 키트 로그 폴더에 생성되었는지 확인할 수도 있습니다.

## <a name="platform-version-launch-test"></a>플랫폼 버전 시작 테스트

Windows 앱이 OS의 이후 버전에서 실행할 수 있는지 확인합니다. 이 테스트는 기존에 데스크톱 앱 워크플로에만 적용되었지만 이제는 스토어 및 유니버설 Windows 플랫폼(UWP) 워크플로에 사용할 수 있습니다.

### <a name="background"></a>배경

운영 체제 버전 정보는 Microsoft 저장소에 대 한 사용 현황을 제한 합니다. 이 정보는 종종 앱에서 앱 OS 버전과 관련된 기능을 사용자에게 제공할 수 있도록 OS 버전을 확인하는 데 잘못 사용되었습니다.

### <a name="test-details"></a>테스트 정보

Windows 앱 인증 키트는 HighVersionLie를 사용하여 OS 버전 확인 방법을 검색합니다. 앱이 충돌 하는 경우 앱은 이 테스트에 실패합니다.

### <a name="corrective-action"></a>수정 작업

앱은 버전 API 도우미 함수를 사용하여 이를 확인해야 합니다. 자세한 내용은 [운영 체제 버전](https://msdn.microsoft.com/library/windows/desktop/ms724832)을 참조하세요.

## <a name="background-tasks-cancellation-handler-validation"></a>백그라운드 작업 취소 처리기 유효성 검사

선언된 백그라운드 작업에 대한 취소 처리기가 앱에 있는지 확인합니다. 작업이 취소될 때 호출되는 전용 함수가 있어야 합니다. 이 테스트는 배포된 앱에만 적용됩니다.

### <a name="background"></a>배경

스토어 앱은 백그라운드에서 실행하는 프로세스를 등록할 수 있습니다. 예를 들어 메일 앱에서 서버로 때때로 ping할 수 있습니다. 그러나 이러한 리소스가 OS에 필요한 경우 백그라운드 작업을 취소하고 앱에서 이 취소를 적절하게 처리해야 합니다. 취소 처리기가 없는 앱의 경우 크래시가 발생하거나 사용자가 앱을 닫으려고 할 때 닫히지 않을 수 있습니다.

### <a name="test-details"></a>테스트 정보

앱이 시작되고, 일시 중단되며 앱의 백그라운드가 아닌 부분이 종료됩니다. 그런 다음 이 앱과 연결된 백그라운드 작업이 취소됩니다. 앱의 상태가 확인되며, 앱이 계속 실행 중인 경우 이 테스트에 실패 합니다.

### <a name="corrective-action"></a>수정 작업

앱에 취소 처리기를 추가합니다. 자세한 내용은 [백그라운드 작업을 사용하여 앱 지원](https://msdn.microsoft.com/library/windows/apps/Mt299103)을 참조하세요.

## <a name="app-count"></a>앱 개수

이 앱 패키지(APPX, 앱 번들)에 응용 프로그램이 하나 포함되어 있는지 확인합니다. 키트에서 독립 실행형 테스트가 되도록 이 검사가 변경되었습니다.

### <a name="background"></a>배경

이 테스트는 스토어 정책에 따라 구현되었습니다.

### <a name="test-details"></a>테스트 정보

Windows Phone 8.1 앱의 경우 테스트는 번들에 포함된 총 appx 패키지 수가 512개 미만(&lt; 512)이고, 번들에 기본 패키지가 하나만 있으며, 번들에 포함된 기본 패키지의 아키텍처가 ARM 또는 중립으로 표시되는지 확인합니다.

Windows 10 앱의 경우 테스트는 번들 버전의 수정 번호가 0으로 설정되었는지 확인합니다.

### <a name="corrective-action"></a>수정 작업

테스트 정보에서 앱 패키지 및 번들이 위의 요구 사항을 충족하는지 확인합니다.

## <a name="app-manifest-compliance-test"></a>앱 매니페스트 준수 테스트

앱 매니페스트의 내용을 테스트하여 내용이 올바른지 확인합니다.

### <a name="background"></a>배경

앱의 매니페스트가 형식이 올발라야 합니다.

### <a name="test-details"></a>테스트 정보

앱 매니페스트를 검사하여 [앱 패키지 요구 사항](https://msdn.microsoft.com/library/windows/apps/Mt148525)에 설명된 대로 내용이 올바른지 확인합니다.

-   **파일 확장명 및 프로토콜**

    앱이 연결되어야 하는 파일 확장명을 선언할 수 있습니다. 잘못 사용될 경우 앱은 많은 파일 확장명을 선언할 수 있으며, 대부분의 확장명은 사용할 수도 없어 좋지 않은 사용자 환경을 제공할 수 있습니다. 이 테스트에서는 검사를 추가하여 앱이 연결할 수 있는 파일 확장명 수를 제한합니다.

-   **프레임워크 종속성 규칙**

    이 테스트에서는 앱이 UWP에 적절하게 종속되는 요구 사항을 적용합니다. 부적절한 종속성이 있으면 이 테스트가 실패합니다.

    앱이 적용되는 OS 버전과 프레임워크 종속성 간에 불일치가 있으면 테스트가 실패합니다. 또한 앱이 프레임워크 dll의 미리 보기 버전을 참조하는 경우에도 테스트가 실패합니다.

-   **IPC(프로세스 간 통신) 검증**

    이 테스트에서는 UWP 앱 데스크톱 구성 요소를 app 컨테이너의 외부 통신 하지 않는 요구 사항을 적용 합니다. 프로세스 간 통신은 병렬 로드된 앱만을 대상으로 합니다. "DesktopApplicationPath"와 동일한 이름으로 [**ActivatableClassAttribute**](https://msdn.microsoft.com/library/windows/apps/BR211414)를 지정하는 앱은 이 테스트에 실패합니다.

### <a name="corrective-action"></a>수정 작업

앱의 매니페스트가 [앱 패키지 요구 사항](https://msdn.microsoft.com/library/windows/apps/Mt148525)에 설명된 요구 사항에 맞는지 검토합니다.

## <a name="windows-security-features-test"></a>Windows 보안 기능 테스트

### <a name="background"></a>배경

기본 Windows 보안 보호 기능을 변경하면 고객이 더 큰 위험에 노출될 수 있습니다.

### <a name="test-details"></a>테스트 정보

[BinScope 이진 분석기](#binscope-binary-analyzer-tests)를 실행하여 앱 보안을 테스트합니다.

Binscope 이진 분석기 테스트는 앱의 이진 파일을 검사하여 앱을 공격이나 공격 벡터로 사용되는 것에 덜 취약하게 만드는 코딩 및 빌드 규칙을 확인합니다.

BinScope 이진 분석기 테스트는 다음과 같은 보안 관련 기능이 올바로 사용되는지 확인합니다.

-   BinScope 이진 분석기 테스트
-   전용 코드 서명

### <a name="binscope-binary-analyzer-tests"></a>BinScope 이진 분석기 테스트

[Binscope 이진 분석기](https://www.microsoft.com/en-us/download/details.aspx?id=44995) 테스트는 앱의 이진 파일을 검사하여 앱을 공격이나 공격 벡터로 사용되는 것에 덜 취약하게 만드는 코딩 및 빌드 규칙을 확인합니다.

BinScope 이진 분석기 테스트는 다음과 같은 보안 관련 기능이 올바로 사용되는지 확인합니다.

-   [AllowPartiallyTrustedCallersAttribute](#binscope-1)
-   [/SafeSEH 예외 처리 보호](#binscope-2)
-   [데이터 실행 방지](#binscope-3)
-   [ASLR(Address Space Layout Randomization)](#binscope-4)
-   [읽기/쓰기 공유 PE 섹션](#binscope-5)
-   [AppContainerCheck](#appcontainercheck)
-   [ExecutableImportsCheck](#binscope-7)
-   [WXCheck](#binscope-8)

### <a name="span-idbinscope-1spanallowpartiallytrustedcallersattribute"></a><span id="binscope-1"></span>AllowPartiallyTrustedCallersAttribute

**Windows 앱 인증 키트 오류 메시지:** APTCACheck 테스트 실패

APTCA(AllowPartiallyTrustedCallersAttribute) 특성을 사용하면 서명된 어셈블리의 부분적으로 신뢰할 수 있는 코드에서 완전히 신뢰할 수 있는 코드에 액세스할 수 있습니다. APTCA 특성을 어셈블리에 적용하면 부분적으로 신뢰할 수 있는 호출자가 어셈블리 수명 동안 해당 어셈블리에 액세스할 수 있으므로 보안이 손상될 수 있습니다.

**앱이 이 테스트에 실패할 경우 수행할 작업**

프로젝트에 필요하고, 그 위험성을 잘 이해하고 있는 경우가 아니면 강력한 이름이 지정된 어셈블리에 APTCA 특성을 사용하지 마세요. 필요한 경우 모든 API를 적절한 코드 액세스 보안 요구로 보호하세요. 어셈블리가 UWP(유니버설 Windows 플랫폼) 앱의 일부인 경우 APTCA는 적용되지 않습니다.

**설명**

이 테스트는 관리 코드(C#, .NET 등)에서만 수행됩니다.

### <a name="span-idbinscope-2spansafeseh-exception-handling-protection"></a><span id="binscope-2"></span>/SafeSEH 예외 처리 보호

**Windows 앱 인증 키트 오류 메시지:** SafeSEHCheck 테스트 실패

앱에서 0으로 나누기 오류 등의 예외 상태가 발생하면 예외 처리기가 실행됩니다. 예외 처리기의 주소는 함수 호출 시 스택에 저장되기 때문에 일부 악성 소프트웨어가 스택을 덮어쓰려고 할 경우 버퍼 오버플로 공격자에게 취약할 수 있습니다.

**앱이 이 테스트에 실패할 경우 수행할 작업**

앱을 빌드할 때 링커 명령에서 /SAFESEH 옵션을 사용하도록 설정합니다. 이 옵션은 Visual Studio의 릴리스 구성에서 기본적으로 설정되어 있습니다. 빌드 지침에서 앱의 모든 실행 모듈에 대해 이 옵션을 사용하도록 설정했는지 확인하세요.

**설명**

이 테스트는 예외 처리기 주소를 스택에 저장하지 않는 64비트 이진이나 ARM 칩세트 이진에서 수행되지 않습니다.

### <a name="span-idbinscope-3spandata-execution-prevention"></a><span id="binscope-3"></span>데이터 실행 방지

**Windows 앱 인증 키트 오류 메시지:** NXCheck 테스트 실패

이 테스트는 앱이 데이터 세그먼트에 저장된 코드를 실행하지 않는지 확인합니다.

**앱이 이 테스트에 실패할 경우 수행할 작업**

앱을 빌드할 때 링커 명령에서 /NXCOMPAT 옵션을 사용하도록 설정하세요. 이 옵션은 DEP(데이터 실행 방지)를 지원하는 링커 버전에서 기본적으로 설정되어 있습니다.

**설명**

DEP 가능 CPU에서 앱을 테스트하고 DEP에서 발생하는 모든 오류를 수정하는 것이 좋습니다.

### <a name="span-idbinscope-4spanaddress-space-layout-randomization"></a><span id="binscope-4"></span>ASLR(Address Space Layout Randomization)

**Windows 앱 인증 키트 오류 메시지:** DBCheck 테스트 실패

ASLR(Address Space Layout Randomization)은 실행 가능 이미지를 예측할 수 없는 메모리 위치에 로드하므로 프로그램이 특정 가상 주소에 로드될 것을 예상하는 악성 소프트웨어가 예측대로 작동하기 어렵습니다. 앱과 앱이 사용하는 모든 구성 요소가 ASLR을 지원해야 합니다.

**앱이 이 테스트에 실패할 경우 수행할 작업**

앱을 빌드할 때 링커 명령에서 /DYNAMICBASE 옵션을 사용하도록 설정하세요. 앱이 사용하는 모든 모듈에서 이 링커 옵션도 사용하는지 확인하세요.

**설명**

일반적으로 ASLR은 성능에 영향을 주지 않습니다. 그러나 32비트 시스템에서 성능이 약간 향상되는 경우도 있습니다. 많은 이미지가 각기 다른 메모리 위치에 로드되어 있는 혼잡한 시스템에서는 성능이 저하될 수 있습니다.

이 테스트는 C 또는 C++ 등의 관리되지 않는 언어로 작성된 앱에서만 수행합니다.

### <a name="span-idbinscope-5spanreadwrite-shared-pe-section"></a><span id="binscope-5"></span>읽기/쓰기 공유 PE 섹션

**Windows 앱 인증 키트 오류 메시지:** SharedSectionsCheck 테스트 실패.

공유로 표시된 쓰기 가능한 섹션이 있는 이진 파일은 보안 위협이 됩니다. 필요한 경우가 아니면 쓰기 가능한 공유 섹션을 사용하여 앱을 빌드하지 마세요. [**CreateFileMapping**](https://msdn.microsoft.com/library/windows/desktop/Aa366537) 또는 [**MapViewOfFile**](https://msdn.microsoft.com/library/windows/desktop/Aa366761)을 사용하여 보안이 제대로 설정된 공유 메모리 개체를 만드세요.

**앱이 이 테스트에 실패할 경우 수행할 작업**

앱에서 공유 섹션을 제거하고 적절한 보안 특성으로 [**CreateFileMapping**](https://msdn.microsoft.com/library/windows/desktop/Aa366537) 또는 [**MapViewOfFile**](https://msdn.microsoft.com/library/windows/desktop/Aa366761)을 호출하여 공유 메모리 개체를 만든 다음 앱을 다시 빌드하세요.

**설명**

이 테스트는 C 또는 C++ 등의 관리되지 않는 언어로 작성된 앱에서만 수행합니다.

### <a name="appcontainercheck"></a>AppContainerCheck

**Windows 앱 인증 키트 오류 메시지:** AppContainerCheck 테스트 실패.

AppContainerCheck는 실행 가능 이진 파일의 PE(이식 가능 파일) 헤더에 **appcontainer** 비트가 설정되어 있는지 확인합니다. 앱이 올바로 실행되려면 모든 .exe 파일과 모든 관리되지 않는 DLL에 **appcontainer** 비트가 올바로 설정되어 있어야 합니다.

**앱이 이 테스트에 실패할 경우 수행할 작업**

기본 실행 파일이 테스트에 실패할 경우 최신 컴파일러 및 링커를 사용하여 파일을 빌드했고 링커에서 */appcontainer* 플래그를 사용하는지 확인하세요.

관리 되는 실행 파일에는 테스트 실패 하면 UWP 응용 프로그램을 구축 하는 최신 컴파일러 및 링커 예: Microsoft Visual Studio를 사용 해야 합니다.

**설명**

이 테스트는 모든 .exe 파일 및 모든 관리되지 않는 DLL에서 수행됩니다.

### <a name="span-idbinscope-7spanexecutableimportscheck"></a><span id="binscope-7"></span>ExecutableImportsCheck

**Windows 앱 인증 키트 오류 메시지:** ExecutableImportsCheck 테스트 실패.

PE(이식 가능 파일) 이미지의 가져오기 테이블이 실행 코드 섹션에 배치된 경우 이 테스트에 실패합니다. 이 오류는 Visual C++ 링커의 */merge* 플래그를 */merge:.rdata=.text*로 설정하여 PE 이미지에 대해 .rdata 병합을 사용하도록 설정한 경우에 발생할 수 있습니다.

**앱이 이 테스트에 실패할 경우 수행할 작업**

가져오기 테이블을 실행 코드 섹션에 병합하지 마세요. Visual C++ 링커의 */merge* 플래그를 설정하여 ".rdata" 섹션을 코드 섹션에 병합하지 않아야 합니다.

**설명**

이 테스트는 관리되는 어셈블리를 제외한 모든 이진 코드에서 수행합니다.

### <a name="span-idbinscope-8spanwxcheck"></a><span id="binscope-8"></span>WXCheck

**Windows 앱 인증 키트 오류 메시지:** WXCheck 테스트 실패.

이러한 검사를 통해 쓰기 가능 및 실행 가능으로 매핑된 페이지가 이진에 없는지 확인할 수 있습니다. 쓰기 가능 및 실행 가능 섹션이 이진에 있거나 이진 *SectionAlignment*가 *PAGE\-SIZE*보다 작은 경우 이러한 오류가 발생할 수 있습니다.

**앱이 이 테스트에 실패할 경우 수행할 작업**

쓰기 가능 및 실행 가능 섹션이 이진에 없고 이진 *SectionAlignment* 값이 해당 *PAGE\-SIZE* 이상인지 확인합니다.

**설명**

이 테스트는 모든 .exe 파일 및 관리되지 않는 네이티브 DLL에서 수행됩니다.

편집 및 계속이 사용 설정된 상태로 실행 파일이 작성된 경우 쓰기 가능 및 실행 가능 섹션이 실행 파일에 있을 수 있습니다(/ZI). 편집 및 계속을 사용하지 않도록 설정하면 잘못된 섹션이 표시되지 않습니다.

*PAGE\-SIZE*는 실행 파일의 기본 *SectionAlignment*입니다.

### <a name="private-code-signing"></a>전용 코드 서명

앱 패키지 내에 전용 코드 서명 이진이 있는지 테스트합니다.

### <a name="background"></a>배경

전용 코드 서명 파일은 손상될 경우 악의적인 용도로 사용될 수 있으므로 비공개로 보관해야 합니다.

### <a name="test-details"></a>테스트 정보

앱 패키지 내에서 .pfx 또는 .snk 확장명을 사용하며 개인 서명 키가 포함되었음을 나타내는 파일을 테스트합니다.

### <a name="corrective-actions"></a>수정 작업

패키지에서 전용 코드 서명 키(예: .pfx 및 .snk 파일)를 제거하세요.

## <a name="supported-api-test"></a>지원되는 API 테스트

앱에서 비규격 API를 사용하는지 테스트합니다.

### <a name="background"></a>배경

앱은 UWP 앱 (Windows 런타임 또는 지원 되는 Win32 Api) Microsoft 저장소에 대 한 인증 되어야 하는 Api를 사용 해야 합니다. 이 테스트는 관리되는 이진 파일이 승인된 프로필 외부의 기능에 종속하는 경우도 식별합니다.

### <a name="test-details"></a>테스트 정보

-   응용 프로그램 패키지 내의 각 이진 이진 파일의 가져오기 주소 테이블을 확인 하 여 UWP 응용 프로그램 개발을 위한 지원 하지 않는 Win32 API에 의존 관계를 없으면 있는지 확인 합니다.
-   앱 패키지 내의 각 관리되는 이진 파일이 승인된 프로필 외부의 기능에 종속하지 않는지 확인합니다.

### <a name="corrective-actions"></a>수정 작업

앱이 디버그 빌드가 아니라 릴리스 빌드로 컴파일되었는지 확인하세요.

> **참고 사항**  응용 프로그램의 디버그 빌드 앱 [UWP 앱에 대 한 api (영문)를](https://msdn.microsoft.com/library/windows/apps/xaml/bg124285.aspx)사용 하는 경우에이 테스트를 실패 합니다.

[UWP 앱에 대 한 API](https://msdn.microsoft.com/library/windows/apps/xaml/bg124285.aspx)하지 않은 응용 프로그램에서 사용 하는 API를 식별 하는 오류 메시지를 검토 합니다.

> **참고 사항**  디버그 구성에는 기본 제공 된 c + + 응용 프로그램 구성만 UWP 앱에 대 한 Windows SDK에서 api (영문)를 사용 하는 경우에이 테스트에 실패 합니다. 추가 정보에 대 한 [UWP 응용 프로그램에서 Windows Api에 대 한 대안](http://go.microsoft.com/fwlink/p/?LinkID=244022) 참조 하십시오.

## <a name="performance-tests"></a>성능 테스트

앱이 빠르고 유연한 사용자 환경을 제공하려면 사용자 조작 및 시스템 명령에 빠르게 응답해야 합니다.

테스트를 수행하는 컴퓨터의 특성이 테스트 결과에 영향을 줄 수 있습니다. 앱 인증을 위한 성능 테스트 임계값은 절전형 컴퓨터가 빠르고 유연한 환경에 대한 고객의 기대를 충족하도록 설정됩니다. 앱 성능을 확인하려면 화면 해상도가 1366x768 이상인 Intel Atom 프로세서 기반 컴퓨터, 회전형 하드 드라이브(반도체 하드 드라이브 반대) 등의 저성능 컴퓨터에서 앱을 테스트하는 것이 좋습니다.

### <a name="bytecode-generation"></a>바이트코드 생성

JavaScript 실행 시간을 가속화하기 위한 성능 최적화로서, .js 확장명으로 끝나는 JavaScript 파일은 앱이 배포될 때 바이트코드를 생성합니다. 이렇게 하면 JavaScript 작업의 시작 및 진행 중인 실행 시간이 훨씬 향상됩니다.

### <a name="test-details"></a>테스트 정보

앱 배포를 검사하여 모든 .js 파일이 바이트코드로 변환되었는지 확인합니다.

### <a name="corrective-action"></a>수정 작업

이 테스트가 실패할 경우 문제를 해결할 때 다음을 고려하세요.

-   이벤트 로깅이 사용되는지 확인합니다.
-   모든 JavaScript 파일의 구문이 올바른지 확인합니다.
-   앱의 이전 버전이 모두 제거되었는지 확인합니다.
-   식별된 파일을 앱 패키지에서 제외합니다.

### <a name="optimized-binding-references"></a>최적화된 바인딩 참조

바인딩을 사용하는 경우 메모리 사용을 최적화하려면 WinJS.Binding.optimizeBindingReferences를 true로 설정해야 합니다.

### <a name="test-details"></a>테스트 정보

WinJS.Binding.optimizeBindingReferences의 값을 확인합니다.

### <a name="corrective-action"></a>수정 작업

앱 JavaScript에서 WinJS.Binding.optimizeBindingReferences를 **true**로 설정합니다.

## <a name="app-manifest-resources-test"></a>앱 매니페스트 리소스 테스트

### <a name="app-resources-validation"></a>앱 리소스 유효성 검사

앱의 매니페스트에 선언된 문자열 또는 이미지가 잘못된 경우 앱이 설치되지 않을 수 있습니다. 앱을 설치할 때 이러한 오류가 발생하면 앱의 로고나 앱이 사용하는 다른 이미지가 올바로 표시되지 않을 수 있습니다.

### <a name="test-details"></a>테스트 정보

앱 매니페스트에 정의된 리소스를 검사하여 리소스가 있고 유효한지 확인합니다.

### <a name="corrective-action"></a>수정 작업

다음 표의 지침을 따르세요.

<table>
<tr><th>오류 메시지</th><th>설명</th></tr>
<tr><td>
<p>{image name} 이미지에서 Scale 및 TargetSize 한정자를 정의합니다. 한정자는 한 번에 하나만 정의할 수 있습니다.</p>
</td><td>
<p>서로 다른 해상도로 이미지를 사용자 지정할 수 있습니다.</p>
<p>실제 메시지에서는 {image name}에 오류가 있는 이미지 이름이 포함됩니다.</p>
<p> 각 이미지에서 Scale 또는 TargetSize를 한정자로 정의하는지 확인하세요.</p>
</td></tr>
<tr><td>
<p>{image name} 이미지에서 크기 제한에 실패했습니다.</p>
</td><td>
<p>모든 앱 이미지가 적절한 크기 제한을 준수하는지 확인하세요.</p>
<p>실제 메시지에서는 {image name}에 오류가 있는 이미지 이름이 포함됩니다.</p>
</td></tr>
<tr><td>
<p>{image name} 이미지가 패키지에서 누락되었습니다.</p>
</td><td>
<p>필요한 이미지가 없습니다.</p>
<p>실제 메시지에서는 {image name}에 누락된 이미지 이름이 포함됩니다.</p>
</td></tr>
<tr><td>
<p>{image name} 이미지가 유효한 이미지 파일이 아닙니다.</p>
</td><td>
<p>모든 앱 이미지가 적절한 파일 형식 유형 제한을 준수하는지 확인하세요.</p>
<p>실제 메시지에서는 {image name}에 유효하지 않은 이미지 이름이 포함됩니다.</p>
</td></tr>
<tr><td>
<p>"BadgeLogo" 이미지의 (x, y) 위치에 유효하지 않은 ABGR 값 {value}이(가) 있습니다. 픽셀은 흰색(##FFFFFF) 또는 투명(00######)이어야 합니다.</p>
</td><td>
<p>배지 로고는 잠금 화면에서 앱을 식별하기 위해 배지 알림 옆에 표시되는 이미지입니다. 이 이미지는 단색이어야 합니다. 즉, 흰색 및 투명 픽셀만 포함할 수 있습니다.</p>
<p>실제 메시지에서는 {value}에 유효하지 않은 이미지 색 값이 포함됩니다.</p>
</td></tr>
<tr><td>
<p>"BadgeLogo" 이미지의 (x, y) 위치에 고대비 흰색 이미지에 유효하지 않은 ABGR 값 {value}이(가) 있습니다. 픽셀은 (##2A2A2A)보다 어둡거나 투명(00######)이어야 합니다.</p>
</td><td>
<p>배지 로고는 잠금 화면에서 앱을 식별하기 위해 배지 알림 옆에 표시되는 이미지입니다.   배지 로고는 흰색 배경에 표시되므로 고대비 흰색일 경우 일반적인 배지 로고의 어두운 버전이어야 합니다. 고대비 흰색에서 배지 로고에는 (##2A2A2A)보다 어둡거나 투명인 픽셀만 포함될 수 있습니다.</p>
<p>실제 메시지에서는 {value}에 유효하지 않은 이미지 색 값이 포함됩니다.</p>
</td></tr>
<tr><td>
<p>이미지에서 TargetSize 한정자를 사용하지 않고 둘 이상의 변형을 정의해야 합니다. 이미지에서 Scale 한정자를 정의하거나 Scale 및 TargetSize를 지정하지 않은 상태로 유지하여 기본값 Scale-100으로 설정해야 합니다.</p>
</td><td>
<p>자세한 내용은 <a href="https://msdn.microsoft.com/library/windows/apps/xaml/dn958435.aspx">UWP 앱에 대한 반응형 디자인 101</a> 및 <a href="https://msdn.microsoft.com/library/windows/apps/xaml/hh465241.aspx">앱 리소스에 대한 지침</a>을 참조하세요.</p>
</td></tr>
<tr><td>
<p>패키지에 "resources.pri" 파일이 없습니다.</p>
</td><td>
<p>앱 매니페스트에 지역화 가능한 콘텐츠가 있는 경우 앱 패키지에 유효한 resources.pri 파일이 있는지 확인하세요.</p>
</td></tr>
<tr><td>
<p>"resources.pri" 파일에는 패키지 이름 {package full name}과(와) 일치하는 이름을 사용하는 리소스 맵이 있어야 합니다.</p>
</td><td>
<p>매니페스트가 변경되어 resources.pri의 리소스 맵 이름이 매니페스트의 패키지 이름과 더 이상 일치하지 않는 경우 이 오류가 발생할 수 있습니다.</p>
<p>실제 메시지에서는 resources.pri에 포함되어야 하는 패키지 이름이 {package full name}에 포함됩니다.</p>
<p>이 오류를 해결하려면 resources.pri를 다시 빌드해야 하며 앱 패키지를 다시 빌드하면 이 작업을 가장 간단하게 수행할 수 있습니다.</p>
</td></tr>
<tr><td>
<p>"resources.pri" 파일에서 자동 병합을 사용할 수 있도록 설정해서는 안 됩니다.</p>
</td><td>
<p>MakePRI.exe는 <strong>AutoMerge</strong>라는 옵션을 지원합니다. <strong>AutoMerge</strong>의 기본값은 <strong>off</strong>입니다. 이 옵션을 사용하면 <strong>AutoMerge</strong>에서 런타임에 앱의 언어 팩 리소스를 단일 resources.pri에 병합합니다. 이 Microsoft 저장소를 통해 배포 하려는 앱에 대 한 권장 하지 합니다. Microsoft 저장소를 통해 배포 되는 응용 프로그램의 resources.pri 응용 프로그램의 패키지의 루트에 하 고 앱이 지 원하는 모든 언어 참조를 포함 해야 합니다.</p>
</td></tr>
<tr><td>
<p>{string} 문자열에서 {number}자의 최대 길이 제한을 준수하지 못했습니다.</p>
</td><td>
<p><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt148525.aspx">앱 패키지 요구 사항</a>을 참조하세요.</p>
<p>실제 메시지에서는 {string}이(가) 오류가 있는 문자열로 대체되고 {number}에는 최대 길이가 포함됩니다.</p>
</td></tr>
<tr><td>
<p>{string} 문자열에는 선행/후행 공백이 없어야 합니다.</p>
</td><td>
<p>앱 매니페스트에 있는 요소에 대한 스키마에서는 선행 또는 후행 공백 문자를 허용하지 않습니다.</p>
<p>실제 메시지에서는 {string}이(가) 오류가 있는 문자열로 대체됩니다.</p>
<p>resources.pri에 있는 매니페스트 필드의 지역화된 값에 선행 또는 후행 공백 문자가 없는지 확인하세요.</p>
</td></tr>
<tr><td>
<p>문자열은 비어 있으면 안 되며 길이가 0보다 커야 합니다.</p>
</td><td>
<p>자세한 내용은 <a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt148525.aspx">앱 패키지 요구 사항</a>을 참조하세요.</p>
</td></tr>
<tr><td>
<p>"resources.pri" 파일에 지정된 기본 리소스가 없습니다.</p>
</td><td>
<p>자세한 내용은 <a href="https://msdn.microsoft.com/library/windows/apps/xaml/hh465241.aspx">앱 리소스에 대한 지침</a>을 참조하세요.</p>
<p>기본 빌드 구성에서 Visual Studio는 번들을 생성할 때 앱 패키지에 scale-200 이미지 리소스만 포함하고 다른 리소스는 리소스 패키지에 배치합니다. scale-200 이미지 리소스를 포함하거나 갖고 있는 리소스를 포함하도록 프로젝트를 구성해야 합니다.</p>
</td></tr>
<tr><td>
<p>"resources.pri" 파일에 지정된 리소스 값이 없습니다.</p>
</td><td>
<p>앱 매니페스트의 resources.pri에 유효한 리소스가 정의되어 있는지 확인하세요.</p>
</td></tr>
<tr><td>
<p>{filename} 이미지 파일은 204800바이트보다 작아야 합니다.\*\*</p>
</td><td>
<p>표시된 이미지의 크기를 줄이세요.</p>
</td></tr>
<tr><td>
<p>{filename} 파일에는 리버스 맵 섹션이 없어야 합니다.\*\*</p>
</td><td>
<p>Visual Studio 'F5 디버깅' 중 makepri.exe를 호출할 때 리버스 맵이 생성된 경우에는 pri 파일을 생성할 때 /m 매개 변수 없이 makepri.exe를 실행하여 제거할 수 있습니다.</p>
</td></tr>
<tr><td colspan="2">
<p>\*\* 테스트가 Windows 8.1용 Windows 앱 인증 키트 3.3에서 추가되었음을 나타내며 해당 버전 이상의 키트를 사용할 때만 적용됩니다.</p>
</td></tr>
</table>



 

### <a name="branding-validation"></a>브랜딩 유효성 검사

UWP 앱이 완전 하 고 모든 기능을 갖춘 것으로 예상 됩니다. 템플릿 또는 SDK 샘플의 기본 이미지를 사용하는 앱은 부적절한 사용자 환경을 제공하며 스토어 카탈로그에서 쉽게 식별할 수 없습니다.

### <a name="test-details"></a>테스트 정보

이 테스트는 앱이 사용하는 이미지가 SDK 샘플 또는 Visual Studio의 기본 이미지가 아닌지 검증합니다.

### <a name="corrective-actions"></a>수정 작업

기본 이미지를 보다 개성적이고 앱을 잘 나타내는 이미지로 바꿉니다.

## <a name="debug-configuration-test"></a>디버그 구성 테스트

앱을 테스트하여 디버그 빌드가 아닌지 확인합니다.

### <a name="background"></a>배경

Microsoft 저장소에 대 한 인증을 앱 해야 컴파일되지 디버그에 대 한 하 고 실행 파일의 디버그 버전을 참조 하지 않아야 합니다. 또한 앱에서 이 테스트를 통과하려면 최적화된 상태로 코드를 빌드해야 합니다.

### <a name="test-details"></a>테스트 정보

앱을 테스트하여 디버그 빌드가 아니고 디버그 프레임워크에 링크되지 않았는지 확인합니다.

### <a name="corrective-actions"></a>수정 작업

-   Microsoft 저장소에 전송 하기 전에 릴리스 빌드도 응용 프로그램을 작성 합니다.
-   올바른 버전의 .NET Framework를 설치했는지 확인합니다.
-   앱이 디버그 버전의 프레임워크에 연결되어 있지 않은지, 릴리스 버전으로 빌드되고 있는지 확인합니다. 이 앱에 .NET 구성 요소가 포함되어 있을 경우 올바른 버전의 .NET Framework를 설치했는지 확인합니다.

## <a name="file-encoding-test"></a>파일 인코딩 테스트

### <a name="utf-8-file-encoding"></a>UTF-8 파일 인코딩

### <a name="background"></a>배경

바이트코드 캐싱의 장점을 이용하고 다른 런타임 오류 조건을 방지하려면 HTML, CSS 및 JavaScript 파일은 해당 BOM(바이트 순서 표시)을 사용하여 UTF-8 형식으로 인코딩해야 합니다.

### <a name="test-details"></a>테스트 정보

앱 패키지의 내용을 테스트하여 올바른 파일 인코딩을 사용하는지 확인합니다.

### <a name="corrective-action"></a>수정 작업

영향을 받은 파일을 열고 Visual Studio의 **파일** 메뉴에서 **다른 이름으로 저장**을 선택합니다. **저장** 단추 옆에 있는 드롭다운 컨트롤을 선택하고 **Save with Encoding**을 선택합니다. **고급** 저장 옵션 대화 상자에서 유니코드(서명 있는 UTF-8) 옵션을 선택하고 **확인**을 클릭합니다.

## <a name="direct3d-feature-level-test"></a>Direct3D 기능 수준 테스트

### <a name="direct3d-feature-level-support"></a>Direct3D 기능 수준 지원

Microsoft Direct3D 앱을 테스트하여 이전 그래픽 하드웨어가 있는 디바이스에서 작동이 중단되지 않는지 확인합니다.

### <a name="background"></a>배경

Microsoft 저장소 Direct3D를 사용 하 여 제대로 렌더링 하거나 기능 수준 9\ 1 그래픽 카드에 적절 하 게 실패 하는 모든 응용 프로그램이 필요 합니다.

사용자는 앱 설치 후 디바이스의 그래픽 하드웨어를 변경할 수 있으므로 9\-1 이상의 최소 기능 수준을 선택하는 경우 실행 시 현재 하드웨어가 최소 요구 사항을 충족하는지 여부를 앱에서 확인해야 합니다. 최소 요구 사항을 충족하지 않는 경우 앱은 Direct3D 요구 사항이 자세히 설명된 메시지를 사용자에게 표시해야 합니다. 또한 호환되지 않는 디바이스에서 앱을 다운로드하는 경우 앱은 시작 시 이 사항을 감지하고 요구 사항을 자세히 설명하는 메시지를 고객에게 표시해야 합니다.

### <a name="test-details"></a>테스트 정보

이 테스트는 앱이 기능 수준 9\-1에서 정확하게 렌더링되는지 검증합니다.

### <a name="corrective-action"></a>수정 작업

높은 기능 수준에서 앱을 실행하더라도 앱이 Direct3D 기능 수준 9\-1에서 올바르게 렌더링되는지 확인합니다. 자세한 내용은 [각 Direct3D 기능 수준에 대한 개발](http://go.microsoft.com/fwlink/p/?LinkID=253575)을 참조하세요.

### <a name="direct3d-trim-after-suspend"></a>일시 중단 후 Direct3D 자르기

> **참고 사항**  이 테스트는 Windows 8.1 하 고 나중에 개발 된 UWP 앱에만 적용 됩니다.

### <a name="background"></a>배경

앱이 Direct3D 디바이스에서 [**Trim**](https://msdn.microsoft.com/library/windows/desktop/Dn280346)을 호출하지 않는 경우 앱은 이전 3D 작업에 할당된 메모리를 해제하지 않습니다. 이 경우 시스템 메모리 부족으로 인해 앱이 종료될 가능성이 커집니다.

### <a name="test-details"></a>테스트 정보

앱이 d3d 요구 사항을 준수하는지 검사하고 앱이 일시 중단 콜백에서 새 [**Trim**](https://msdn.microsoft.com/library/windows/desktop/Dn280346) API를 호출하는지 확인합니다.

### <a name="corrective-action"></a>수정 작업

앱이 일시 중단될 때마다 해당 [**IDXGIDevice3**](https://msdn.microsoft.com/library/windows/desktop/Dn280345) 인터페이스에서 [**Trim**](https://msdn.microsoft.com/library/windows/desktop/Dn280346) API를 호출해야 합니다.

## <a name="app-capabilities-test"></a>앱 접근 권한 값 테스트

### <a name="special-use-capabilities"></a>특수 사용 접근 권한 값

### <a name="background"></a>배경

특수 사용 접근 권한 값은 특정 시나리오를 위한 것입니다. 회사 계정만 이러한 접근 권한 값을 사용할 수 있습니다.

### <a name="test-details"></a>테스트 정보

앱이 아래 접근 권한 값 중 하나를 선언하는지 확인합니다.

-   EnterpriseAuthentication
-   SharedUserCertificates
-   DocumentsLibrary

이러한 접근 권한 값이 선언된 경우 테스트에서 사용자에게 경고를 표시합니다.

### <a name="corrective-actions"></a>수정 작업

앱에 필요하지 않은 경우 특수 사용 접근 권한 값을 제거하는 것이 좋습니다. 또한 이 접근 권한 값의 사용에는 추가 온보딩 정책 검토가 적용됩니다.

## <a name="windows-runtime-metadata-validation"></a>Windows 런타임 메타데이터 유효성 검사

### <a name="background"></a>배경

앱에 포함된 구성 요소가 UWP 형식 시스템을 준수하는지 확인합니다.

### <a name="test-details"></a>테스트 정보

패키지의 **.winmd** 파일이 UWP 규칙을 준수하는지 확인합니다.

### <a name="corrective-actions"></a>수정 작업

-   **ExclusiveTo 특성 테스트:** UWP 클래스가 다른 클래스에 ExclusiveTo로 표시된 인터페이스를 구현하지 않는지 확인합니다.
-   **형식 위치 테스트:** 모든 UWP 형식에 대한 메타데이터가 앱 패키지에서 네임스페이스와 일치하는 가장 긴 이름을 가진 winmd 파일에 있는지 확인합니다.
-   **형식 이름 대/소문자 구분 테스트:** 앱 패키지 내에서 모든 UWP 형식의 이름이 대/소문자를 구분하지 않는 고유한 이름인지 확인합니다. 또한 UWP 이름이 앱 패키지 내에서 네임스페이스 이름으로도 사용되지 않았는지 확인합니다.
-   **형식 이름 수정 테스트:** 글로벌 네임스페이스 또는 Windows 최상위 네임스페이스에 UWP 형식이 없는지 확인합니다.
-   **일반 메타데이터 수정 테스트:** 사용 중인 형식을 생성하는 컴파일러가 최신 UWP 사양으로 업데이트되었는지 확인합니다.
-   **속성 테스트:** UWP 클래스의 모든 속성에 get 메서드가 있는지 확인합니다(set 메서드는 옵션임). UWP 형식의 모든 속성에 대해 get 메서드 반환 값 유형이 set 메서드 입력 매개 변수 유형과 일치하는지 확인합니다.

## <a name="package-sanity-tests"></a>패키지 온전성 테스트

### <a name="platform-appropriate-files-test"></a>플랫폼에 적절한 파일 테스트

혼합된 이진 파일을 설치하는 앱은 사용자 프로세서 아키텍처에 따라 크래시가 발생하거나 올바르게 실행되지 않을 수 있습니다.

### <a name="background"></a>배경

이 테스트는 앱 패키지의 바이너리에서 아키텍처 충돌을 확인합니다. 앱 패키지는 매니페스트에 지정된 프로세서 아키텍처에 사용할 수 없는 바이너리를 포함하면 안 됩니다. 지원되지 않는 바이너리를 포함하면 앱 크래시가 발생하거나 불필요하게 앱 패키지 크기가 늘어날 수 있습니다.

### <a name="test-details"></a>테스트 정보

앱 패키지 매니페스트 프로세서 아키텍처 선언과 상호 참조될 때 PE 헤더에서 각 파일의 "비트 수"가 적절한지 확인합니다.

### <a name="corrective-action"></a>수정 작업

앱 패키지가 앱 매니페스트에 지정된 아키텍처에서 지원되는 파일만 포함하고 있는지 확인하려면 다음 지침을 따르세요.

-   앱의 대상 프로세서 아키텍처가 중립 프로세서 종류인 경우 앱 패키지는 x86, x64 또는 ARM 바이너리나 이미지 형식 파일을 포함할 수 없습니다.

-   앱의 대상 프로세서 아키텍처가 x86 프로세서 종류인 경우 앱 패키지는 x86 바이너리나 이미지 형식 파일만 포함해야 합니다. x64 또는 ARM 바이너리나 이미지 형식이 포함된 패키지는 테스트에 실패합니다.

-   앱의 대상 프로세서 아키텍처가 x64 프로세서 종류인 경우 앱 패키지는 x64 바이너리나 이미지 형식 파일을 포함해야 합니다. 이 경우에는 패키지가 x86 파일도 포함할 수 있지만, 기본 앱 환경은 x64 바이너리를 이용해야 합니다.

    하지만 패키지에 ARM 바이너리나 이미지 형식 파일이 포함되었거나 x86 바이너리나 이미지 형식 파일만 포함된 경우 테스트에 실패합니다.

-   앱의 대상 프로세서 아키텍처가 ARM 프로세서 종류인 경우 앱 패키지는 ARM 바이너리나 이미지 형식 파일만 포함해야 합니다. x64 또는 x86 바이너리나 이미지 형식 파일이 포함된 패키지는 테스트에 실패합니다.

### <a name="supported-directory-structure-test"></a>지원되는 디렉터리 구조 테스트

응용 프로그램이 MAX\-PATH보다 긴 하위 디렉터리를 설치 과정에서 만들지 않는지 확인합니다.

### <a name="background"></a>배경

OS 구성 요소(Trident, WWAHost 등 포함)는 내부적으로 파일 시스템 경로가 MAX\-PATH로 제한되며 이보다 긴 경로에서는 제대로 작동하지 않습니다.

### <a name="test-details"></a>테스트 정보

앱 설치 디렉터리 내의 경로가 MAX\-PATH를 초과하지 않는지 확인합니다.

### <a name="corrective-action"></a>수정 작업

더 짧은 디렉터리 구조 및/또는 파일 이름을 사용합니다.

## <a name="resource-usage-test"></a>리소스 사용 테스트

### <a name="winjs-background-task-test"></a>WinJS 백그라운드 작업 테스트

WinJS 백그라운드 작업 테스트는 앱이 배터리를 소모하지 않도록 JavaScript 앱에 적절한 close 문이 있는지 확인합니다.

### <a name="background"></a>배경

JavaScript 백그라운드 작업이 포함된 앱은 백그라운드 작업의 마지막 문으로 Close()를 호출해야 합니다. 앱이 호출하지 않을 경우 시스템이 연결된 대기 상태로 돌아갈 수 없어 배터리가 고갈될 수 있습니다.

### <a name="test-details"></a>테스트 정보

매니페스트에 지정된 백그라운드 작업 파일이 앱에 없을 경우 테스트를 통과합니다. 그렇지 않으면 테스트에서 앱 패키지에 지정된 JavaScript 백그라운드 작업 파일을 구문 분석하고 Close() 문을 찾습니다. Close() 문이 있으면 테스트를 통과하고, 그렇지 않으면 테스트에 실패합니다.

### <a name="corrective-action"></a>수정 작업

Close()를 올바르게 호출하도록 백그라운드 JavaScript 코드를 업데이트합니다.


## <a name="related-topics"></a>관련 항목

* [Windows 데스크톱 브리지 앱 테스트](windows-desktop-bridge-app-tests.md)
* [Microsoft Store 정책](https://msdn.microsoft.com/library/windows/apps/Dn764944)
 
