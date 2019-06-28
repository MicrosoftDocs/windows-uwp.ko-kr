---
description: 이 자습서에는 UWP XAML 사용자 인터페이스를 추가, MSIX 패키지 만들기 및 WPF 앱에 다른 최신 구성 요소를 통합 하는 방법을 보여 줍니다.
title: 패키지 및 MSIX를 사용 하 여 배포
ms.topic: article
ms.date: 06/27/2019
ms.author: mcleans
author: mcleanbyron
keywords: windows 10, uwp, windows forms, wpf, xaml 제도
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: d11ef296b690297d33ebd5d366c2594f70b6d10b
ms.sourcegitcommit: 1eec0e4fd8a5ba82803fdce6e23fcd01b9488523
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/27/2019
ms.locfileid: "67420101"
---
# <a name="part-5-package-and-deploy-with-msix"></a>5부: 패키지 및 MSIX를 사용 하 여 배포

Contoso Expenses 라는 샘플 WPF 데스크톱 앱을 현대화 하는 방법에 설명 하는 자습서의 마지막 부분입니다. 자습서, 필수 구성 요소 및 샘플 앱을 다운로드 하기 위한 지침의 개요를 참조 하세요. [자습서: WPF 앱을 현대화](modernize-wpf-tutorial.md)합니다. 이 문서에서는 이미 완료 했다고 가정 [4 부](modernize-wpf-tutorial-4.md)합니다.

[4 부](modernize-wpf-tutorial-4.md) 앱에서 사용할 수 있습니다 알림 API를 비롯 한 일부 WinRT Api 패키지 id를 요구 하는 것이 알아보았습니다. 사용 하 여 Contoso 비용 패키징 패키지 id를 가져올 수 있습니다 [MSIX](https://docs.microsoft.com/windows/msix), 패키지 및 Windows 응용 프로그램을 배포 하려면 Windows 10에 도입 된 패키징 형식을 합니다. MSIX 모두 개발자와 IT 전문가 비롯 한 테이블에 이점이 있습니다.

- 네트워크 사용량 및 저장소 공간을 최적화 합니다.
- 정리 완료 덕분 앱이 실행 되는 간단한 컨테이너를 제거 합니다. 시스템에 없는 레지스트리 키 및 임시 파일은 남아 있습니다.
- 응용 프로그램 업데이트 및 사용자 지정에서 OS 업데이트를 분리합니다.
- 설치를 간소화, 업데이트 및 프로세스를 제거 합니다. 

이 자습서의이 부분에서는 Contoso expenses MSIX 패키지에 패키지 하는 방법을 배웁니다.

## <a name="package-the-application"></a>응용 프로그램 패키지

Visual Studio 2019 Windows 응용 프로그램 패키징 프로젝트를 사용 하 여 데스크톱 응용 프로그램을 패키지 하는 쉬운 방법을 제공 합니다. 

1. **솔루션 탐색기**를 마우스 오른쪽 단추로 클릭 합니다 **ContosoExpenses** 솔루션 선택 **-> 새 프로젝트 추가**합니다.

    ![새 프로젝트 추가](images/wpf-modernize-tutorial/AddNewProject.png)

3. **새 프로젝트를 추가** 대화 상자에서 검색 `packaging`를 선택 합니다 **Windows 응용 프로그램 패키징 프로젝트** 프로젝트 템플릿을 C# 범주 및 클릭 **다음** .

    ![Windows 응용 프로그램 패키징 프로젝트](images/wpf-modernize-tutorial/WAP.png)

4. 새 프로젝트의 이름을 `ContosoExpenses.Package` 누릅니다 **만들기**합니다.

5. 선택 **Windows 10 버전 1903 (10.0; 18362 빌드)** 둘 다에 대 한는 **대상 버전** 하 고 **최소 버전** 클릭 **확인**합니다.

    합니다 **ContosoExpenses.Package** 프로젝트를 추가 하는 **ContosoExpenses** 솔루션입니다. 이 프로젝트에는 [패키지 매니페스트](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/schema-root)를 설명 하는 응용 프로그램 및 프로그램 메뉴에 있는 아이콘 및 시작 화면에서 타일을 같은 항목에 사용 되는 일부 기본 자산입니다. 그러나 UWP 프로젝트와 달리 패키징 프로젝트 코드를 포함 하지 않습니다. 기존 데스크톱 앱을 패키지 하기 위한 것입니다.

6. 에 **ContosoExpenses.Package** 프로젝트를 마우스 오른쪽 단추로 클릭 합니다 **응용 프로그램** 노드 선택 **참조를 추가**. 이 노드는 솔루션의 응용 프로그램 패키지에 포함 됩니다 지정 합니다.

7. 프로젝트 목록에서 선택 **ContosoExpenses.Core** 누릅니다 **확인**합니다.

8. 확장 합니다 **응용 프로그램** 노드 있는지 확인 합니다는 **ContosoExpense.Core** 프로젝트를 참조 하 고 굵게 강조 표시 된. 이 해당 사용할 시작 지점으로 패키지에 대 한 것을 의미 합니다.

9. 마우스 오른쪽 단추로 클릭 합니다 **ContosoExpenses.Package** 프로젝트를 선택 **시작 프로젝트로 설정**합니다.

10. 키를 눌러 **F5** 디버거에서 패키지 된 앱을 시작 합니다.

이 시점에서 나타내는 앱 패키지를 실행 하는 이제 몇 가지 변경 사항을 확인할 수 있습니다.

- 시작 메뉴 또는 작업 표시줄에서 아이콘은 이제 기본 자산에 포함 된 모든 **Windows 응용 프로그램 패키징 프로젝트**합니다.
- 마우스 오른쪽 단추로 클릭할 경우 합니다 **ContosoExpense.Package** 시작 메뉴에 나열 된 응용 프로그램을 보면와 같은 Microsoft Store 다운로드 한 앱에 대 한 일반적으로 예약 된 옵션 **앱 설정**하십시오 **속도 및 검토** 하 고 **공유**.

    ![시작 메뉴에서 ContosoExpenses](images/wpf-modernize-tutorial/StartMenu.png)

- 앱을 제거 하려는 경우 단추로 **ContosoExpense.Package** 시작 메뉴에서 선택한 **제거**합니다. 앱 즉시 제거 됩니다, 시스템에 남은 모든 종료 하지 않고 있습니다.

## <a name="test-the-notification"></a>알림 테스트

끝에 작동 하지는 알림 시나리오를 테스트할 수 MSIX 사용 하 여 Contoso expenses를 패키지화 했으므로 [4 부](modernize-wpf-tutorial-4.md)합니다.

1. Contoso 비용 앱에서 직원 목록에서 선택 하 고 클릭 합니다 **새 비용 추가** 단추입니다. 
2. 폼 및 키를 눌러 모든 필드를 완성 **저장할**합니다.
3. OS 알림을 표시 되는지 확인 합니다.

![알림 메시지](images/wpf-modernize-tutorial/ToastNotification.png)
