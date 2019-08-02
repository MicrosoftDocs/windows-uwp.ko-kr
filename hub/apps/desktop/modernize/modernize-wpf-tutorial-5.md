---
description: 이 자습서에서는 UWP XAML 사용자 인터페이스를 추가 하 고, MSIX 개의 패키지를 만들고, 기타 최신 구성 요소를 WPF 앱에 통합 하는 방법을 보여 줍니다.
title: MSIX로 패키징 및 배포
ms.topic: article
ms.date: 06/27/2019
ms.author: mcleans
author: mcleanbyron
keywords: windows 10, uwp, windows forms, wpf, xaml 제도
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: 961157bc3d3429b56d3da24a46d71cbb5b84e7a3
ms.sourcegitcommit: 3cc6eb3bab78f7e68c37226c40410ebca73f82a9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/02/2019
ms.locfileid: "68729495"
---
# <a name="part-5-package-and-deploy-with-msix"></a>5부: MSIX로 패키징 및 배포

Contoso 지출 이라는 샘플 WPF 데스크톱 앱을 현대화 하는 방법을 보여 주는 자습서의 마지막 부분입니다. 샘플 앱을 다운로드 하기 위한 자습서, 필수 구성 요소 및 지침에 대 한 개요를 [보려면 자습서: WPF 앱](modernize-wpf-tutorial.md)을 현대화 합니다. 이 문서에서는 [4 부](modernize-wpf-tutorial-4.md)를 이미 완료 했다고 가정 합니다.

[4 부](modernize-wpf-tutorial-4.md) 에서는 알림 API를 비롯 한 일부 WinRT api를 앱에서 사용 하려면 패키지 id가 필요 하다는 것을 배웠습니다. Windows 응용 프로그램을 패키지 하 고 배포 하기 위해 Windows 10에 도입 된 패키징 형식인 [Msix](https://docs.microsoft.com/windows/msix)을 사용 하 여 패키지 id를 가져올 수 있습니다. MSIX은 개발자와 IT 전문가를 위해 다음을 포함 하 여 테이블에 대 한 이점을 제공 합니다.

- 네트워크 사용량 및 저장소 공간을 최적화 합니다.
- 앱이 실행 되는 경량 컨테이너 덕분에 clean 제거를 완료 합니다. 레지스트리 키와 임시 파일은 시스템에 남아 있지 않습니다.
- 응용 프로그램 업데이트 및 사용자 지정에서 OS 업데이트를 분리 합니다.
- 설치, 업데이트 및 제거 프로세스를 간소화 합니다. 

자습서의이 부분에서는 MSIX 패키지에서 Contoso 지출 앱을 패키징하는 방법에 대해 알아봅니다.

## <a name="package-the-application"></a>응용 프로그램 패키지

Visual Studio 2019은 Windows 응용 프로그램 패키징 프로젝트를 사용 하 여 데스크톱 응용 프로그램을 패키지 하는 쉬운 방법을 제공 합니다. 

1. **솔루션 탐색기**에서 **ContosoExpenses** 솔루션을 마우스 오른쪽 단추로 클릭 하 고 **추가-> 새 프로젝트**를 선택 합니다.

    ![새 프로젝트 추가](images/wpf-modernize-tutorial/AddNewProject.png)

3. **새 프로젝트 추가** 대화 상자에서 `packaging`를 검색 하 고, C# 범주에서 **Windows 응용 프로그램 패키징 프로젝트** 프로젝트 템플릿을 선택 하 고, **다음**을 클릭 합니다.

    ![Windows 애플리케이션 패키징 프로젝트](images/wpf-modernize-tutorial/WAP.png)

4. 새 프로젝트 `ContosoExpenses.Package` 의 이름을로 하 고 **만들기**를 클릭 합니다.

5. **Windows 10, 버전 1903 (10.0;)을 선택 합니다. 빌드 18362)** 를 선택 하 **고** **확인**을 클릭 합니다.

    **ContosoExpenses** 프로젝트가 **ContosoExpenses** 솔루션에 추가 됩니다. 이 프로젝트에는 응용 프로그램을 설명 하는 [패키지 매니페스트와](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/schema-root)프로그램 메뉴의 아이콘, 시작 화면에 있는 타일 등의 항목에 사용 되는 일부 기본 자산이 포함 됩니다. 그러나 UWP 프로젝트와 달리 패키징 프로젝트에는 코드가 포함 되지 않습니다. 용도는 기존 데스크톱 앱을 패키징하는 것입니다.

6. **ContosoExpenses** 프로젝트에서 **응용 프로그램** 노드를 마우스 오른쪽 단추로 클릭 하 고 **참조 추가**를 선택 합니다. 이 노드는 패키지에 포함 될 솔루션의 응용 프로그램을 지정 합니다.

7. 프로젝트 목록에서 **ContosoExpenses** 를 선택 하 고 **확인**을 클릭 합니다.

8. **응용 프로그램** 노드를 확장 하 고 **ContosoExpense** 프로젝트가 참조 되 고 굵은 글꼴로 강조 표시 되는지 확인 합니다. 즉, 패키지의 시작 지점으로 사용 됩니다.

9. **ContosoExpenses** 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 **시작 프로젝트로 설정**을 선택 합니다.

10. 솔루션 탐색기에서 **ContosoExpenses** 프로젝트 노드를 마우스 오른쪽 단추로 클릭 하 고 **프로젝트 파일 편집**을 선택 합니다.

11. 파일에서 `<Import Project="$(WapProjPath)\Microsoft.DesktopBridge.targets" />` 요소를 찾습니다.

12. 이 요소를 다음 XML로 바꿉니다.

    ``` xml
    <ItemGroup>
        <SDKReference Include="Microsoft.VCLibs,Version=14.0">
        <TargetedSDKConfiguration Condition="'$(Configuration)'!='Debug'">Retail</TargetedSDKConfiguration>
        <TargetedSDKConfiguration Condition="'$(Configuration)'=='Debug'">Debug</TargetedSDKConfiguration>
        <TargetedSDKArchitecture>$(PlatformShortName)</TargetedSDKArchitecture>
        <Implicit>true</Implicit>
        </SDKReference>
    </ItemGroup>
    <Import Project="$(WapProjPath)\Microsoft.DesktopBridge.targets" />
    <Target Name="_StompSourceProjectForWapProject" BeforeTargets="_ConvertItems">
        <ItemGroup>
        <_TemporaryFilteredWapProjOutput Include="@(_FilteredNonWapProjProjectOutput)" />
        <_FilteredNonWapProjProjectOutput Remove="@(_TemporaryFilteredWapProjOutput)" />
        <_FilteredNonWapProjProjectOutput Include="@(_TemporaryFilteredWapProjOutput)">
            <SourceProject>
            </SourceProject>
        </_FilteredNonWapProjProjectOutput>
        </ItemGroup>
    </Target>
    ```

13. 프로젝트 파일을 저장하고 닫습니다.

14. **F5** 키를 눌러 디버거에서 패키지 된 앱을 시작 합니다.

이제 앱이 패키지로 실행 중임을 나타내는 몇 가지 변경 내용이 있습니다.

- 작업 표시줄 또는 시작 메뉴의 아이콘이 이제 모든 **Windows 응용 프로그램 패키징 프로젝트**에 포함 된 기본 자산입니다.
- 시작 메뉴에 나열 된 ContosoExpense 응용 프로그램을 마우스 오른쪽 단추로 클릭 하면 **앱 설정**, **평가** 및 공유와 같이 일반적으로 Microsoft Store에서 다운로드 한 앱 용으로 예약 된 옵션이 표시 됩니다 **.** .

    ![시작 메뉴의 ContosoExpenses](images/wpf-modernize-tutorial/StartMenu.png)

- 앱을 제거 하려는 경우 시작 메뉴에서 **ContosoExpense** 를 마우스 오른쪽 단추로 클릭 하 고 **제거**를 선택할 수 있습니다. 시스템에 남아 있는 상태를 유지 하지 않고 앱이 즉시 제거 됩니다.

## <a name="test-the-notification"></a>알림 테스트

이제 MSIX을 사용 하 여 Contoso 지출 앱을 패키지 했으므로 [4 부](modernize-wpf-tutorial-4.md)의 끝에서 작동 하지 않는 알림 시나리오를 테스트할 수 있습니다.

1. Contoso expense 앱의 목록에서 직원을 선택 하 고 **새 지출 추가** 단추를 클릭 합니다. 
2. 양식의 모든 필드를 완료 하 고 **저장**을 누릅니다.
3. OS 알림이 표시 되는지 확인 합니다.

![알림 메시지](images/wpf-modernize-tutorial/ToastNotification.png)
