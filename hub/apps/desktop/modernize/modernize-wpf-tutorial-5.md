---
description: 이 자습서에서는 UWP XAML 사용자 인터페이스를 추가하고, MSIX 패키지를 만들고, WPF 앱에 다른 최신 구성 요소를 통합하는 방법을 보여줍니다.
title: MSIX로 패키징 및 배포
ms.topic: article
ms.date: 01/23/2020
ms.author: mcleans
author: mcleanbyron
keywords: windows 10, uwp, windows forms, wpf, xaml island
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: 27906d9d389c065ab1fdf7124151cd1915f850eb
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/29/2020
ms.locfileid: "76726016"
---
# <a name="part-5-package-and-deploy-with-msix"></a>5부: MSIX로 패키징 및 배포

Contoso 지출이라는 샘플 WPF 데스크톱 앱을 현대화하는 방법을 보여주는 자습서의 마지막 부분입니다. 자습서의 개요, 필수 구성 요소 및 샘플 앱 다운로드 지침은 [자습서: WPF 앱 현대화](modernize-wpf-tutorial.md)를 참조하세요. 이 문서에서는 [4부](modernize-wpf-tutorial-4.md)를 이미 완료했다고 가정합니다.

[4부](modernize-wpf-tutorial-4.md)에서는 알림 API를 포함한 일부 WinRT API가 앱에서 사용되기 전에 패키지 ID가 필요하다는 것을 배웠습니다. 패키지 ID는 Windows 애플리케이션을 패키지하고 배포하기 위해 Windows 10에 도입된 패키징 형식인 [MSIX](https://docs.microsoft.com/windows/msix)를 사용하여 가져올 수 있습니다. MSIX는 개발자와 IT 전문가에게 다음과 같은 이점을 제공합니다.

- 네트워크 사용량 및 스토리지 공간을 최적화합니다.
- 앱이 경량 컨테이너에서 실행되므로 앱을 완전히 제거할 수 있습니다. 레지스트리 키와 임시 파일이 시스템에 남지 않습니다.
- OS 업데이트가 애플리케이션 업데이트 및 사용자 지정과 분리됩니다.
- 설치, 업데이트 및 제거 프로세스가 간단합니다.

자습서의 이 파트에서는 MSIX 패키지에 Contoso 지출 앱을 패키징하는 방법을 알아봅니다.

## <a name="package-the-application"></a>애플리케이션 패키징

Visual Studio 2019에서는 Windows 애플리케이션 패키징 프로젝트를 사용하여 데스크톱 애플리케이션을 쉽게 패키징할 수 있습니다. 

1. **솔루션 탐색기**에서 **ContosoExpenses** 솔루션을 마우스 오른쪽 단추로 클릭하고 **추가 -> 새 프로젝트**를 선택합니다.

    ![새 프로젝트 추가](images/wpf-modernize-tutorial/AddNewProject.png)

3. **새 프로젝트 추가** 대화 상자에서 `packaging`을 검색하고, C# 범주에서 **Windows 애플리케이션 패키징 프로젝트** 프로젝트 템플릿을 선택하고, **다음**을 클릭합니다.

    ![Windows 애플리케이션 패키징 프로젝트](images/wpf-modernize-tutorial/WAP.png)

4. 새 프로젝트 이름을 `ContosoExpenses.Package`로 지정하고 **만들기**를 클릭합니다.

5. **대상 버전**과 **최소 버전**을 모두**Windows 10 버전 1903(10.0; 빌드 18362)** 으로 선택하고 **확인**을 클릭합니다.

    **ContosoExpenses.Package** 프로젝트가 **ContosoExpenses** 솔루션에 추가됩니다. 이 프로젝트에는 애플리케이션을 설명하는 [패키지 매니페스트](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/schema-root), 그리고 [프로그램] 메뉴의 아이콘 및 [시작] 화면의 타일과 같은 항목에 사용되는 몇 가지 기본 자산이 포함되어 있습니다. 그러나 UWP 프로젝트와 달리, 패키징 프로젝트에는 코드가 포함되지 않습니다. 패키징 프로젝트의 목적은 기존 데스크톱 앱을 패키징하는 것입니다.

6. **ContosoExpenses.Package** 프로젝트에서 **애플리케이션** 노드를 마우스 오른쪽 단추로 클릭하고 **참조 추가**를 선택합니다. 이 노드는 솔루션의 애플리케이션 중에서 패키지에 포함할 애플리케이션을 지정합니다.

6. 프로젝트 목록에서 **ContosoExpenses.Core**를 선택하고 **확인**을 클릭합니다.

7. **애플리케이션** 노드를 확장하고, **ContosoExpense.Core** 프로젝트가 참조되며 굵게 표시되는지 확인합니다. 이는 프로젝트가 패키지의 시작점으로 사용된다는 의미입니다.

8. **ContosoExpenses.Package** 프로젝트를 마우스 오른쪽 단추로 클릭하고 **시작 프로젝트로 설정**을 선택합니다.

9. **F5** 키를 눌러 디버거에서 패키지된 앱을 시작합니다.

이제 앱이 패키지로 실행 중임을 나타내는 몇 가지 변경 내용을 볼 수 있습니다.

- 작업 표시줄 또는 [시작] 메뉴의 아이콘은 이제 모든 **Windows 애플리케이션 패키징 프로젝트**에 포함되는 기본 자산입니다.
- [시작] 메뉴에 나열된 **ContosoExpense.Package** 애플리케이션을 마우스 오른쪽 단추로 클릭하면 일반적으로 Microsoft Store에서 다운로드한 앱용으로 예약된 옵션(예: **앱 설정**, **리뷰 남기기**, **공유**)이 표시됩니다.

    ![[시작] 메뉴의 ContosoExpenses](images/wpf-modernize-tutorial/StartMenu.png)

- 앱을 제거하려면 [시작] 메뉴에서 **ContosoExpense.Package**를 마우스 오른쪽 단추로 클릭하고 **제거**를 선택합니다. 시스템에 아무 것도 남기지 않고 앱이 즉시 제거됩니다.

## <a name="test-the-notification"></a>알림 테스트

MSIX을 사용하여 Contoso 지출 앱을 패키징했으므로, [4부](modernize-wpf-tutorial-4.md)의 끝에서 작동하지 않던 알림 시나리오를 테스트할 수 있습니다.

1. Contoso 지출 앱의 목록에서 직원을 선택한 다음, **새 지출 추가** 단추를 클릭합니다.
2. 양식의 모든 필드를 작성하고 **저장**을 누릅니다.
3. OS 알림이 표시되는지 확인합니다.

![알림 메시지](images/wpf-modernize-tutorial/ToastNotification.png)
