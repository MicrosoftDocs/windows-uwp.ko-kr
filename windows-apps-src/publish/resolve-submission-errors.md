---
Description: 스토어에 앱을 제출한 후 오류가 발생하는 경우 오류를 해결해야만 인증 프로세스를 계속할 수 있습니다.
title: 제출 오류 해결
ms.assetid: 68199E09-0C66-4EB4-BFE8-D2EEB139C4F3
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: df7c1bbbc77374b8afb4272e1d9618c8294a4b6e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57591858"
---
# <a name="resolve-submission-errors"></a>제출 오류 해결

스토어에 앱을 제출한 후 오류가 발생하는 경우 오류를 해결해야만 [인증 프로세스](the-app-certification-process.md)를 계속할 수 있습니다. 오류 메시지는 문제가 무엇이며, 문제를 해결하기 위해 수행해야 할 수 있는 작업을 나타냅니다. 이러한 오류를 해결하는 데 도움이 될 수 있는 일부 추가 정보는 다음과 같습니다.

## <a name="uwp-apps"></a>UWP 앱

UWP 앱을 제출 하는 경우 스토어 용 Visual Studio에서 생성 된.msixupload 또는.appxupload 파일을 없으면 패키지 파일을 전처리 하는 동안 오류가 표시 될 수 있습니다. 단계를 따르는 해야 [Visual Studio를 사용 하 여 UWP 앱 패키지](../packaging/packaging-uwp-apps.md) 에서 앱의 패키지 파일 및 업로드.msixupload 또는.appxupload 파일을 만들 때 합니다 [패키지](upload-app-packages.md) 제출, 페이지 .msix/appx 또는.msixbundle/appxbundle 아닙니다.

컴파일 오류가 표시되면 릴리스 모드에서 응용 프로그램을 성공적으로 빌드할 수 있는지 확인합니다. 자세한 내용은 [.NET 네이티브 내부 컴파일러 오류](https://go.microsoft.com/fwlink/p/?LinkID=613098)를 참조하세요.

## <a name="desktop-application"></a>데스크톱 응용 프로그램

Win32와 UWP 모두 이진 파일을 포함 하는 패키지를 제출 하려는 경우 Visual Studio 2017 업데이트 4에서 사용할 수 있는 Windows 패키징 프로젝트를 사용 하 여 해당 패키지를 만들 수 있는지 확인 합니다. UWP 프로젝트 템플릿을 사용 하 여 패키지를 만들 수는 패키지 저장소 또는 테스트용으로 로드를 다른 Pc에 전송할 수 없습니다 수 있습니다. 패키지를 성공적으로 게시 하는 경우에 사용자의 PC에서 예기치 않게에서 동작할 수 있습니다. 자세한 내용은 참조 하세요. [Visual Studio (데스크톱 브리지)를 사용 하 여 응용 프로그램을 패키징할]( https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-packaging-dot-net)합니다.

## <a name="windows-phone-8x-and-earlier"></a>Windows Phone 8.x 및 이전 버전

> [!IMPORTANT]
> 2018 년 10 월 31 일부 터 새로 만든 제품 Windows Phone 대상으로 하는 패키지를 포함할 수 없습니다 8.x 또는 이전 버전입니다. 자세한 내용은 참조 [블로그 게시물](https://blogs.windows.com/buildingapps/2018/08/20/important-dates-regarding-apps-with-windows-phone-8-x-and-earlier-and-windows-8-8-1-packages-submitted-to-microsoft-store/#SzKghBbqDMlmAO4c.97)합니다.

전처리 중에Windows Phone 패키지의 문제가 감지되면 **오류 2001**이 표시될 수 있습니다. 대부분의 경우 오류를 수정하고 앱 패키지를 다시 작성해야 합니다. 이 작업이 끝나면 **스토어에 제출**을 다시 클릭하기 전에 제출의 [패키지](upload-app-packages.md) 페이지에서 이전 패키지를 새 패키지로 바꿉니다.

이 오류를 유발하는 문제는 다양합니다. 아래 목록을 검토하여 사용 중인 패키지에 해당되는 내용을 확인하세요.

-   **패키지에 하나 이상의 어셈블리는 난독 처리 하지 올바르게:** 다른 도구는 난독 처리 하는 데 사용 하 여 또는 난독 처리를 제거 합니다. 이 컴파일 프로세스는 난독 처리된 어셈블리를 최적화합니다. 그러나 오류를 유발하는 지원되지 않는 방식으로 MSIL을 수정하는 도구를 사용하여 일부 어셈블리가 난독 처리되는 경우가 있습니다.
-   **앱에서 하나 이상의 메서드의 크기 IL의 256KB를 초과합니다.** 좀 더 작은 함수로 문제의 메서드를 리팩터링 합니다. 어셈블리에 있는 메서드의 MSIL 크기는 ILDASM 도구를 사용하여 확인할 수 있습니다.
-   **하나 이상의 어셈블리에 강력한 이름 서명 유효성 검사에 실패 했습니다.** 이 오류는 일반적으로 강력한 이름 서명은 어셈블리 메타 데이터에 예상 되는 다른 키를 사용 하 여 수행 된 경우에 발생 합니다. 올바른 키로 서명하거나 강력한 이름 서명을 제거하세요.
-   **혼합 모드 (사용 하 여 관리 및 네이티브 코드) 어셈블리를 포함 하는 패키지:** Windows Phone 혼합 모드 어셈블리를 사용 하는 것이 없습니다. 패키지에서 혼합 모드 어셈블리를 제거하고 앱을 다시 제출합니다.
-   **Windows Phone 8.1 XAP 또는 appx/appxbundle 어셈블리 잘못 되었습니다.** .Winmd 파일에 공용 진입점을 하나 이상 있는지 확인 합니다. 필요한 경우 디컴파일러 응용 프로그램을 사용하여 코드를 검토하고 공용 진입점을 확인할 수 있습니다.

앱을 제출한 후 표시될 수 있는 또 다른 오류는 **오류 1300**입니다. 이 오류는 하나 이상의 어셈블리(또는 전체 패키지)가 이미 미리 컴파일되어 있을 때 발생합니다. 이 문제를 해결하려면 Microsoft Visual Studio에서 앱 패키지를 다시 작성하고 새로 생성된 패키지를 제출합니다.

## <a name="nameidentity-errors"></a>이름/ID 오류

라는 오류가 표시 **패키지에서 이름이 예약 된 앱 이름 중 하나가 되었습니다. 앱 이름을 예약 하거나이 언어에 대 한 올바른 앱 이름으로 패키지를 업데이트 하세요**, 패키지에 잘못 된 이름을 입력 한 때문일 수 있습니다. 앱 이름을 예약 하지 않은 파트너 센터에서 사용 중인 경우이 오류가 발생할 수 있습니다. 일반적으로 다음 단계에 따라 이 오류를 해결할 수 있습니다.

- 앱의 [앱 ID](view-app-identity-details.md) 페이지(**앱 관리** 아래)로 이동하여 앱에 할당된 ID가 있는지 확인합니다. 없는 경우 새로 만드는 옵션이 표시됩니다. ID를 만들려면 앱 이름을 예약해야 합니다. 패키지에 사용한 이름이 맞는지 확인합니다.
- 앱에 ID가 이미 있는 경우에도 패키지에 사용하려는 이름을 예약해야 할 수 있습니다. **앱 관리**에서 [앱 이름 관리](manage-app-names.md)를 클릭합니다. 사용할 이름을 입력하고 **앱 이름 예약**을 클릭합니다.

> [!IMPORTANT]
>  사용하려는 이름을 사용할 수 없는 경우 다른 앱에서 해당 이름을 이미 예약했을 수 있습니다. 사용자 앱이 해당 이름으로 이미 게시되었거나 해당 이름을 사용할 권리가 있다고 생각한다면 [지원 센터로 문의](https://go.microsoft.com/fwlink/p/?LinkId=331509)하세요.  

 

 




