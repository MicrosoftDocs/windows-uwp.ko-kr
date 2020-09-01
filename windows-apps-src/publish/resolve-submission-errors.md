---
Description: 스토어에 앱을 제출한 후 오류가 발생 하는 경우 인증 프로세스를 계속 하려면 문제를 해결 해야 합니다.
title: 제출 오류 해결
ms.assetid: 68199E09-0C66-4EB4-BFE8-D2EEB139C4F3
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: cf2092291c4521a7ed9d32944e0ad4cb88f45a00
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89174737"
---
# <a name="resolve-submission-errors"></a>제출 오류 해결

스토어에 앱을 제출한 후 오류가 발생 하는 경우 [인증 프로세스](the-app-certification-process.md)를 계속 하려면 문제를 해결 해야 합니다. 오류 메시지는 문제가 무엇이며, 문제를 해결하기 위해 수행해야 할 수 있는 작업을 나타냅니다. 이러한 오류를 해결 하는 데 도움이 될 수 있는 몇 가지 추가 정보는 다음과 같습니다.

## <a name="uwp-apps"></a>UWP 앱

UWP 앱을 제출 하는 경우 패키지 파일이 저장소에 대해 Visual Studio에서 생성 된. msixupload 또는 .appxupload 파일이 아닌 경우 전처리 하는 동안 오류가 표시 될 수 있습니다. 앱의 패키지 파일을 만들 때 [Visual Studio를 사용 하 여 UWP 앱 패키지](/windows/msix/package/packaging-uwp-apps) 의 단계를 수행 하 고, 전송의 [패키지](upload-app-packages.md) 페이지에 있는. msixupload 또는 .appxupload 파일만 업로드 합니다. 단,.

컴파일 오류가 표시 되 면 릴리스 모드에서 응용 프로그램을 빌드할 수 있는지 확인 합니다. 자세한 내용은 [.NET 네이티브 내부 컴파일러 오류](https://github.com/dotnet/core/blob/master/Documentation/ilcRepro.md)를 참조 하세요.

## <a name="desktop-application"></a>데스크톱 애플리케이션

Win32 및 UWP 이진 파일이 포함 된 패키지를 제출 하려는 경우 Visual Studio 2017 업데이트 4 이상 버전에서 사용할 수 있는 Windows 패키징 프로젝트를 사용 하 여 해당 패키지를 만들어야 합니다. UWP 프로젝트 템플릿을 사용 하 여 패키지를 만드는 경우 해당 패키지를 스토어에 제출 하거나 다른 Pc로 테스트용으로 로드 하지 못할 수 있습니다. 패키지가 성공적으로 게시 되더라도 사용자 PC에서 예기치 않은 방식으로 작동할 수 있습니다. 자세한 내용은 [Visual Studio를 사용 하 여 앱 패키지 (데스크톱 브리지)]( /windows/msix/desktop/desktop-to-uwp-packaging-dot-net)를 참조 하세요.

## <a name="windows-phone-8x-and-earlier"></a>Windows Phone .x 및 이전 버전

> [!IMPORTANT]
> 2018 년 10 월 31 일까 지 새로 만든 제품은 Windows Phone .x 또는 이전 버전을 대상으로 하는 패키지를 포함할 수 없습니다. 자세한 내용은이 [블로그 게시물](https://blogs.windows.com/windowsdeveloper/2018/08/20/important-dates-regarding-apps-with-windows-phone-8-x-and-earlier-and-windows-8-8-1-packages-submitted-to-microsoft-store)을 참조 하세요.

전처리 하는 동안 Windows Phone 패키지에 문제가 감지 되 면 **오류 2001** 이 표시 될 수 있습니다. 대부분의 경우에는 오류를 수정 하기 위해 앱의 패키지를 다시 빌드해야 합니다. 이 작업을 완료 한 후에는 이전 패키지를 전송의 [패키지](upload-app-packages.md) 페이지에 있는 새 패키지로 대체 한 후 다시 클릭 하 여 **스토어에 제출** 합니다.

이 오류를 발생 시킬 수 있는 여러 가지 문제가 있습니다. 다음 목록을 검토 하 여 패키지에 적용할 수 있는 사항을 확인 합니다.

-   **패키지에 있는 하나 이상의 어셈블리가 잘못 난독 처리 되었습니다.** 다른 도구를 사용 하 여 난독 처리를 수행 하거나 난독 처리를 제거 합니다. 컴파일 프로세스는 난독 처리 된 어셈블리를 최적화 하지만 때때로 일부 어셈블리는 오류를 발생 시키는 지원 되지 않는 방식으로 MSIL을 수정 하는 도구를 사용 하 여 난독 처리 됩니다.
-   **앱에 있는 하나 이상의 메서드 크기가 256 KB의 IL을 초과 합니다.** 잘못 된 메서드를 더 작은 함수로 리팩터링 합니다. 어셈블리의 메서드에 대 한 MSIL 크기는 ILDASM.EXE 도구를 사용 하 여 확인할 수 있습니다.
-   **하나 이상의 어셈블리에 대 한 강력한 이름 서명 유효성 검사가 실패 했습니다.** 일반적으로이 오류는 강력한 이름 서명이 어셈블리 메타 데이터에 필요한 키와 다른 키를 사용 하 여 수행 된 경우에 발생 합니다. 올바른 키로 서명 하거나 강력한 이름 서명을 제거 하십시오.
-   **패키지에는 혼합 모드 (관리 코드 및 네이티브 코드 포함) 어셈블리가 포함 되어 있습니다.** Windows Phone에서는 혼합 모드 어셈블리가 지원 되지 않습니다. 패키지에서 혼합 모드 어셈블리를 제거 하 고 앱을 다시 전송 합니다.
-   **Windows Phone 8.1 XAP 또는 appx/.appxbundle 어셈블리가 잘못 되었습니다.** Winmd 파일에 하나 이상의 공개 진입점이 있는지 확인 합니다. 디컴파일러 응용 프로그램을 사용 하 여 코드를 검토 하 고 필요한 경우 공용 진입점을 확인할 수 있습니다.

앱을 제출한 후에 표시 될 수 있는 또 다른 오류는 **오류 1300**입니다. 이는 하나 이상의 어셈블리 (또는 전체 패키지)가 이미 미리 컴파일된 경우에 발생 합니다. 이 문제를 해결 하려면 Microsoft Visual Studio에서 앱 패키지를 다시 작성 한 다음 새로 생성 된 패키지를 제출 합니다.

## <a name="nameidentity-errors"></a>이름/id 오류

**패키지에 있는 이름이 예약 된 앱 이름 중 하나가 아님을 나타내는 오류가 표시 되는 경우 앱 이름을 예약 하거나이 언어에 맞는 올바른 앱 이름으로 패키지를 업데이트 하세요**. 패키지에 잘못 된 이름을 입력 했기 때문일 수 있습니다. 파트너 센터에서 예약 하지 않은 앱 이름을 사용 하는 경우에도이 오류가 발생할 수 있습니다. 일반적으로 다음 단계를 수행 하 여이 오류를 해결할 수 있습니다.

- 앱 **관리**에서 앱에 대 한 [앱 id](view-app-identity-details.md) 페이지로 이동 하 여 앱에 할당 된 id가 있는지 여부를 확인 합니다. 그렇지 않으면 하나를 만드는 옵션이 표시 됩니다. Id를 만들기 위해 앱의 이름을 예약 해야 합니다. 패키지에서 사용한 이름 인지 확인 합니다.
- 앱에 이미 id가 있는 경우 패키지에서 사용할 이름을 예약 해야 할 수 있습니다. **앱 관리**에서 [앱 이름 관리](manage-app-names.md)를 클릭 합니다. 사용할 이름을 입력 하 고 **앱 이름 예약**을 클릭 합니다.

> [!IMPORTANT]
>  사용 하려는 이름을 사용할 수 없는 경우 다른 앱이 해당 이름을 이미 예약한 것일 수 있습니다. 해당 이름으로 앱이 이미 게시 된 경우 또는 사용할 권한이 있다고 생각 되 면 [지원 담당자에 게 문의 하세요](https://support.microsoft.com/getsupport/hostpage.aspx?locale=EN-US&supportregion=EN-US&ccfcode=US&ln=EN-US&pesid=14654&oaspworkflow=start_1.0.0.0&tenant=store&supporttopic_L1=31762156&supporttopic_L2=31762179).  

 

 