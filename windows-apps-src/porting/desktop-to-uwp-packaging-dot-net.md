---
author: normesta
Description: This guide explains how to configure your Visual Studio Solution to edit, debug, and package desktop application.
Search.Product: eADQiWindows 10XVcnh
title: Visual Studio를 사용 하 여 데스크톱 응용 프로그램 패키지
ms.author: normesta
ms.date: 08/30/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: 807a99a7-d285-46e7-af6a-7214da908907
ms.localizationpriority: medium
ms.openlocfilehash: 2c9b7a30a50c26d2dbdaf6df04e85549addaf181
ms.sourcegitcommit: 1938851dc132c60348f9722daf994b86f2ead09e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/02/2018
ms.locfileid: "4259812"
---
# <a name="package-a-desktop-application-by-using-visual-studio"></a>Visual Studio를 사용 하 여 데스크톱 응용 프로그램 패키지

Visual Studio를 사용하여 데스크톱 앱용 패키지를 만들 수 있습니다. 그런 다음 이 패키지를 Microsoft Store에 게시하거나, 하나 이상의 PC에서 사이드로드할 수 있습니다.

최신 버전의 Visual Studio는 앱을 패키징하는 데 필수적이었던 수동 단계를 모두 제거하는 패키징 프로젝트의 새 버전을 제공합니다. 패키징 프로젝트를 추가하고 데스크톱 프로젝트를 참조한 다음 F5 키를 눌러 앱을 디버깅하면 됩니다. 수동 조정이 필요하지 않습니다. 이 새로운 간소화된 환경은 이전 버전의 Visual Studio에서 사용할 수 있는 환경에 대해 상당히 향상되었습니다.

>[!IMPORTANT]
>데스크톱 응용 프로그램용 Windows 앱 패키지를 만들 수 있습니다 (데스크톱 브리지도 알려진 Windows 10, 버전 1607에에서 도입 그렇지 및 Windows 10 1 주년 업데이트 (10.0;를 대상으로 하는 프로젝트 에서만 사용할 수 있습니다 빌드 14393) 또는 Visual Studio의 최신 릴리스 합니다.

## <a name="first-prepare-your-application"></a>첫 번째, 응용 프로그램 준비

이 가이드를 검토 하 여 응용 프로그램에 대 한 패키지 만들기를 시작 하기 전에: [데스크톱 응용 프로그램 패키지를 준비](desktop-to-uwp-prepare.md)합니다.

<a id="new-packaging-project"/>

## <a name="create-a-package"></a>패키지 만들기

1. Visual Studio에서 데스크톱 응용 프로그램 프로젝트가 포함된 솔루션을 엽니다.

2. 솔루션에 **Windows 응용 프로그램 패키징 프로젝트** 프로젝트를 추가합니다.

   여기에 코드를 추가할 필요가 없습니다. 패키지를 생성할 코드가 있습니다. 이 프로젝트를 '패키징 프로젝트'로 부를 것입니다.

   ![패키징 프로젝트](images/desktop-to-uwp/packaging-project.png)

   >[!NOTE]
   >이 프로젝트는 Visual Studio 2017 버전 15.5 이상에서만 나타납니다.

3. 이 프로젝트 **대상 버전**을 원하는 버전으로 설정할 수 있지만 **최소 버전**은 **Windows 10 1주년 업데이트**으로 설정해야 합니다.

   ![패키징 버전 선택기 대화 상자](images/desktop-to-uwp/packaging-version.png)

4. 패키징 프로젝트에서 **응용 프로그램** 폴더를 마우스 오른쪽 단추로 클릭하고 **참조 추가**를 선택합니다.

   ![프로젝트 참조 추가](images/desktop-to-uwp/add-project-reference.png)

5. 데스크톱 응용 프로그램 프로젝트를 선택하고 **확인** 버튼을 선택합니다.

   ![데스크톱 프로젝트](images/desktop-to-uwp/reference-project.png)

   패키지에 여러 데스크톱 응용 프로그램을 포함할 수 있지만 사용자가 앱 타일을 선택할 때 이 중 하나만 시작할 수 있습니다. **응용 프로그램** 노드에서 사용자가 앱의 타일을 선택할 때 시작할 응용 프로그램을 마우스 오른쪽 단추로 클릭한 다음 **진입점으로 설정**을 선택합니다.

   ![진입점 설정](images/desktop-to-uwp/entry-point-set.png)

6. 오류가 표시되지 않도록 패키징 프로젝트를 빌드합니다.

7. [앱 패키지 만들기](../packaging/packaging-uwp-apps.md) 마법사를 사용해 appxupload 파일을 생성합니다.

   Microsoft Store에 직접 해당 파일을 업로드할 수 있습니다.

**비디오**

&nbsp;
> [!VIDEO https://www.youtube.com/embed/fJkbYPyd08w]

## <a name="next-steps"></a>다음 단계

**질문에 대한 답변 찾기**

질문이 있으세요? Stack Overflow에서 질문해 주세요. 저희 팀은 이러한 [태그](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge)를 모니터링합니다. [여기](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D)에서 Microsoft에 문의할 수도 있습니다.

**피드백 제공 또는 기능 제안**

[UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial)를 참조하세요.

**실행, 디버그 또는 데스크톱 응용 프로그램을 테스트 합니다.**

[실행, 디버그 및 패키지 된 데스크톱 응용 프로그램 테스트](desktop-to-uwp-debug.md) 를 참조 하세요.

**UWP Api를 추가 하 여 데스크톱 응용 프로그램 향상**

[Windows 10용 데스크톱 응용 프로그램 개선](desktop-to-uwp-enhance.md) 참조

**UWP 프로젝트와 Windows 런타임 구성 요소를 추가 하 여 데스크톱 응용 프로그램 확장**

[최신 UWP 구성 요소로 데스크톱 응용 프로그램 확장](desktop-to-uwp-extend.md) 참조.

**앱 배포**

[패키지로 만든된 데스크톱 응용 프로그램 배포](desktop-to-uwp-distribute.md) 를 참조 하세요.
