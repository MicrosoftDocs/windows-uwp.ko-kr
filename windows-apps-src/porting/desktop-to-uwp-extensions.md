---
author: normesta
Description: "모든 UWP 앱에서 사용할 수 있는 일반 API 외에도 변환된 데스크톱 앱에서만 사용할 수 있는 일부 확장 및 API가 있습니다. 이 문서에서는 이러한 확장을 설명하고 이러한 확장을 사용하는 방법을 보여 줍니다."
Search.Product: eADQiWindows 10XVcnh
title: "데스크톱-UWP 브리지 앱 확장"
ms.author: normesta
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: 0a8cedac-172a-4efd-8b6b-67fd3667df34
ms.openlocfilehash: a3c29f0d36cec9a5816d5c20eb43a6634260cc43
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="desktop-to-uwp-bridge-app-extensions"></a>데스크톱-UWP 브리지: 앱 확장

광범위한 UWP(유니버설 Windows 플랫폼) API를 사용하여 변환된 데스크톱 응용 프로그램을 향상시킬 수 있습니다. 그러나 모든 UWP 앱에서 사용할 수 있는 일반 API 외에도 변환된 데스크톱 앱에서만 사용할 수 있는 일부 확장 및 API가 있습니다. 이러한 기능은 사용자 로그온 시 프로세스 시작 및 파일 탐색기 통합과 같은 시나리오에 초점을 맞추며 원래 데스크톱 앱과 변환된 앱 패키지 간의 전환을 부드럽게 하도록 디자인되었습니다.

이 문서에서는 이러한 확장을 설명하고 이러한 확장을 사용하는 방법을 보여 줍니다. 대부분의 경우 앱에서 사용하는 확장에 관한 선언을 포함하고 있는, 변환된 앱의 매니페스트 파일을 수동으로 수정해야 합니다. 매니페스트를 편집하려면 Visual Studio 솔루션에서 **Package.appxmanifest** 파일을 마우스 오른쪽 단추로 클릭하여 *코드 보기*를 선택합니다.

## <a name="manifest-xml-namespaces"></a>매니페스트 XML 네임스페이스

데스크톱 브리지용 앱 확장에는 몇 가지 다른 XML 네임스페이스가 필요합니다. 이러한 네임스페이스는 다음과 같이 메니페스트의 루트에 있는 `<Package>` 요소 내에 정의되어야 합니다.

```xml
<Package
  ...
  xmlns:uap2="http://schemas.microsoft.com/appx/manifest/uap/windows10/2"
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  xmlns:desktop="http://schemas.microsoft.com/appx/manifest/desktop/windows10"
  ...
  IgnorableNamespaces="uap uap2 uap3 mp rescap desktop">
```

매니페스트 파일 컴파일러 오류가 표시되면, 필요한 모든 네임스페이스를 포함시켰는지, 하위 XML 요소에 올바른 네임스페이스가 접두사로 붙었는지 확인합니다. 작동하는 전체 매니페스트 파일은 [GitHub에서 데스크톱 브리지 샘플 저장소](https://github.com/Microsoft/DesktopBridgeToUWP-Samples)에서 참조하세요.

## <a name="startup-tasks"></a>시작 작업

시작 작업을 통해 사용자가 로그온할 때마다 앱에서 실행 파일을 자동으로 실행하도록 할 수 있습니다.

시작 작업을 선언하려면 앱 매니페스트에 다음을 추가합니다.

```XML
<desktop:Extension Category="windows.startupTask" Executable="bin\MyStartupTask.exe" EntryPoint="Windows.FullTrustApplication">
    <desktop:StartupTask TaskId="MyStartupTask" Enabled="true" DisplayName="My App Service" />
</desktop:Extension>
```
- *Extension Category*에는 항상 "windows.startupTask" 값이 있어야 합니다.
- *Extension Executable*은 시작할 .exe의 상대 경로입니다.
- *Extension EntryPoint*에는 항상 "Windows.FullTrustApplication" 값이 있어야 합니다.
- *StartupTask TaskId*는 작업의 고유 식별자입니다. 이 식별자를 사용하여 앱에서 [**Windows.ApplicationModel.StartupTask**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.StartupTask) 클래스의 API를 호출하여 시작 작업을 프로그래밍 방식으로 사용하거나 사용하지 않을 수 있습니다.
- *StartupTask Enabled*는 작업이 처음 시작될 때 사용할지 아니면 사용하지 않을지 여부를 나타냅니다. 사용하는 작업은 다음번에 사용자가 로그온할 때 실행됩니다(사용자가 사용하지 않도록 설정하지 않는 한).
- *StartupTask DisplayName*은 작업 관리자에 표시되는 작업의 이름입니다. 이 문자열은 ```ms-resource```를 사용하여 지역화할 수 있습니다.

앱은 여러 시작 작업을 선언할 수 있습니다. 각 작업은 독립적으로 시작되며 실행됩니다. 모든 시작 작업은 작업 관리자의 **시작** 탭에 나타나며 앱 매니페스트에 지정된 이름 및 해당 앱 아이콘으로 표시됩니다. 작업 관리자는 작업의 시작 영향을 자동으로 분석합니다. 사용자는 작업 관리자를 사용하여 수동으로 앱의 시작 작업을 사용하지 않도록 선택할 수 있습니다. 사용자가 작업을 사용하지 않도록 설정하면 프로그래밍 방식으로 다시 사용하도록 설정할 수 없습니다.

## <a name="app-execution-alias"></a>앱 실행 별칭

앱 실행 별칭을 사용하여 앱에 키워드 이름을 지정할 수 있습니다. 사용자 또는 다른 프로세스는 이 키워드를 사용하여 전체 경로를 제공하지 않고도 예를 들어 실행 또는 명령 프롬프트에서 PATH 변수에 사용하는 것처럼 앱을 쉽게 시작할 수 있습니다. 예를 들어 별칭 "Foo"를 선언하는 경우 사용자가 cmd.exe에서 "Foo Bar.txt"를 입력하면 앱이 활성화 이벤트 인수의 일부로 "Bar.txt"에 대한 경로를 사용하여 활성화됩니다.

앱 실행 별칭을 지정하려면 앱 매니페스트에 다음을 추가합니다.

```XML
<uap3:Extension Category="windows.appExecutionAlias" Executable="exes\launcher.exe" EntryPoint="Windows.FullTrustApplication">
    <uap3:AppExecutionAlias>
        <desktop:ExecutionAlias Alias="Foo.exe" />
    </uap3:AppExecutionAlias>
</uap3:Extension>
```

- *Extension Category*에는 항상 "windows.appExecutionAlias" 값이 있어야 합니다.
- *Extension Executable*은 별칭을 호출할 때 시작할 실행 파일의 상대 경로입니다.
- *Extension EntryPont*에는 항상 "Windows.FullTrustApplication" 값이 있어야 합니다.
- *ExecutionAlias Alias*는 앱의 짧은 이름으로, 항상 확장명이 ".exe"로 끝나야 합니다.

패키지의 각 응용 프로그램에 대해 하나의 앱 실행 별칭만 지정할 수 있습니다. 여러 앱이 같은 별칭으로 등록되는 경우 시스템은 다른 앱에서 재정의할 가능성이 없는 고유한 별칭을 선택하기 위해 마지막에 등록된 항목을 호출합니다.

## <a name="protocol-associations"></a>프로토콜 연결

프로토콜 연결을 통해 변환된 앱과 다른 프로그램 또는 시스템 구성 요소 간에 interop 시나리오를 사용할 수 있습니다. 프로토콜을 사용하여 변환된 앱을 시작하면 해당 활성화 이벤트 인수에 전달할 특정 매개 변수를 지정할 수 있으므로 앱이 적절하게 동작할 수 있습니다. 매개 변수는 변환된 완전 신뢰 앱에 대해서만 지원됩니다. UWP 앱은 매개 변수를 사용할 수 없습니다.  

프로토콜 연결을 선언하려면 앱 매니페스트에 다음을 추가합니다.

```XML
<uap3:Extension Category="windows.protocol">
    <uap3:Protocol Name="myapp-cmd" Parameters="/p &quot;%1&quot;" />
</uap3:Extension>
```

- *Extension Category*에는 항상 "windows.protocol" 값이 있어야 합니다.
- *Protocol Name*은 프로토콜의 이름입니다.
- *Protocol Parameters*는 앱이 활성화될 때 이벤트 인수로 앱에 전달할 매개 변수 및 값의 목록입니다. 변수가 파일 경로를 포함할 수 있는 경우 공백을 포함하는 경로를 전달할 때 값이 구분되지 않도록 값을 따옴표로 래핑해야 합니다.

## <a name="files-and-file-explorer-integration"></a>파일 및 파일 탐색기 통합

변환된 앱에는 특정 파일 형식을 처리하도록 등록하고 파일 탐색기에 통합하기 위한 다양한 옵션이 있습니다. 이러한 등록 및 통합을 통해 사용자는 일반 워크플로의 일부로 앱에 쉽게 액세스할 수 있습니다.

시작하려면 먼저 앱 매니페스트에 다음을 추가합니다.

```XML
<uap3:Extension Category="windows.fileTypeAssociation">
    <uap3:FileTypeAssociation Name="myapp">
        ... additional elements here ...
    </uap3:FileTypeAssociation>
</uap3:Extension>
```

- *Extension Category*에는 항상 "windows.fileTypeAssociation" 값이 있어야 합니다.
- *FileTypeAssociation Name*은 고유 ID입니다. 이 ID는 파일 형식 연결과 관련된 해시된 ProgID를 생성하는 데 내부적으로 사용됩니다. 앱의 이후 버전에서 변경을 관리하는 데 이 ID를 사용할 수 있습니다. 예를 들어 파일 확장명의 아이콘을 변경하려는 경우 다른 이름이 있는 새 FileTypeAssociation으로 이동할 수 있습니다.  

다음으로, 필요한 특정 기능에 따라 이 항목에 추가 자식 요소를 추가합니다. 사용 가능한 옵션은 아래에서 설명합니다.

### <a name="supported-file-types"></a>지원되는 파일 형식

앱에서 특정 형식의 파일 열기를 지원하도록 지정할 수 있습니다. 사용자가 파일을 마우스 오른쪽 단추로 클릭하고 "다른 프로그램으로 열기"를 선택하면 앱이 제안 목록에 표시됩니다.

예:

```XML
<uap:SupportedFileTypes>
    <uap:FileType>.txt</uap:FileType>
    <uap:FileType>.avi</uap:FileType>
</uap:SupportedFileTypes>
```

- *FileType*은 앱에서 지원하는 확장명입니다.

### <a name="file-context-menu-verbs"></a>상황에 맞는 파일 메뉴 동사

일반적으로 사용자가 파일을 두 번 클릭하면 파일이 열립니다. 그러나 사용자가 파일을 마우스 오른쪽 단추로 클릭할 경우 상황에 맞는 메뉴가 "열기", "편집", "미리 보기" 또는 "인쇄"와 같이 파일을 조작할 수 있는 방법에 대한 추가 세부 정보를 제공하는 다양한 옵션("동사"라고도 함)과 함께 표시됩니다.

지원되는 파일 형식을 자동으로 지정하면 "열기" 동사가 추가됩니다. 그러나 앱에서 파일 탐색기의 상황에 맞는 메뉴에 다른 사용자 지정 동사를 추가할 수도 있습니다. 이렇게 하면 파일을 열 때 사용자의 선택에 따라 앱이 특정 방식으로 시작될 수 있습니다.

예:

```XML
<uap2:SupportedVerbs>
    <uap3:Verb Id="Edit" Parameters="/e &quot;%1&quot;">Edit</uap3:Verb>
    <uap3:Verb Id="Print" Extended="true" Parameters="/p &quot;%1&quot;">Print</uap3:Verb>
</uap2:SupportedVerbs>
```

- *Verb Id*는 동사의 고유 ID입니다. 앱이 UWP 앱인 경우 사용자의 선택을 적절하게 처리할 수 있도록 이 ID가 활성화 이벤트 인수의 일부로 앱에 전달됩니다. 앱이 변환된 완전 신뢰 앱인 경우 앱은 이 ID 대신에 매개 변수를 받습니다(다음 항목 참조).
- *Verb Parameters*는 동사와 연결된 인수 매개 변수 및 값의 목록입니다. 앱이 변환된 전체 신뢰 앱인 경우 다른 활성화 동사에 대한 동작을 사용자 지정할 수 있도록 앱이 활성화될 때 이 동사 매개 변수가 이벤트 인수로 앱에 전달됩니다. 변수가 파일 경로를 포함할 수 있는 경우 공백을 포함하는 경로를 전달할 때 값이 구분되지 않도록 값을 따옴표로 래핑해야 합니다. 앱이 UWP 앱인 경우 매개 변수를 전달할 수 없습니다. 앱은 이 매개 변수 대신에 ID를 받습니다(이전 항목 참조).
- *Verb Extended*는 사용자가 상황에 맞는 메뉴를 표시하기 위해 파일을 마우스 오른쪽 단추로 클릭하기 전에 **Shift** 키를 누르고 있는 경우에만 동사 메뉴가 표시되도록 지정합니다. 이 특성은 선택적으로 사용할 수 있으며, 목록에 없는 경우 기본적으로 *False*로 설정됩니다(예: 항상 동사 표시). 각 동사에 대해 개별적으로 이 동작을 지정합니다(항상 *False*인 "열기"는 예외).
- *Verb*는 파일 탐색기의 상황에 맞는 메뉴에 표시될 이름입니다. 이 문자열은 ```ms-resource```를 사용하여 지역화할 수 있습니다.

### <a name="shell-context-menu-verbs"></a>상황에 맞는 셸 메뉴 동사

셸 폴더의 상황에 맞는 메뉴에 항목을 추가하는 작업은 현재 지원되지 않습니다.

### <a name="multiple-selection-model"></a>다중 선택 모델

다중 선택을 통해 사용자가 동시에 여러 파일을 여는 것을 앱에서 처리하는 방법을 지정할 수 있습니다(예를 들어 파일 탐색기에서 10개의 파일을 선택하고 "열기"를 탭함).

변환된 데스크톱 앱에는 일반 데스크톱 앱과 동일한 세 가지 옵션이 있습니다.
- *Player*: 선택된 모든 파일이 인수 매개 변수로 전달될 때 한 번 앱이 활성화됩니다.
- *Single*: 앱이 첫 번째로 선택된 파일에 대해 한 번 활성화됩니다. 다른 파일은 무시됩니다.
- *Document*: 앱의 새로운 별도 인스턴스가 선택된 각 파일에 대해 활성화됩니다.

다른 파일 형식 및 작업에 대해 다른 기본 설정을 설정할 수 있습니다. 예를 들어 *Document* 모드로 *문서*를, *Player* 모드로 *이미지*를 열 수 있습니다.

앱의 동작을 설정하려면 파일 형식 및 파일 시작과 관련된 매니페스트의 요소에 *MultiSelectModel* 특성을 추가합니다.

지원되는 파일 형식에 대한 모델 설정:

```XML
<uap3:FileTypeAssociation Name="myapp" MultiSelectModel="Document">
    <uap:SupportedFileTypes>
        <uap:FileType>.txt</uap:FileType>
</uap:SupportedFileTypes>
```

동사에 대한 모델 설정:

```XML
<uap3:Verb Id="Edit" MultiSelectModel="Player">Edit</uap3:Verb>
<uap3:Verb Id="Preview" MultiSelectModel="Document">Preview</uap3:Verb>
```

앱에서 다중 선택의 선택 항목을 지정하지 않으면 사용자가 15개 이하의 파일을 여는 경우 기본값은 *Player*입니다. 그러지 않고, 앱이 변환된 앱인 경우 기본값은 *Document*입니다. UWP 앱은 항상 *Player*로 시작됩니다.

### <a name="complete-example"></a>전체 예제

다음은 위에서 설명한 파일 탐색기 관련 요소 및 여러 파일을 통합하는 전체 예제입니다.

```XML
<uap3:Extension Category="windows.fileTypeAssociation">
    <uap3:FileTypeAssociation Name="myapp" MultiSelectModel="Document">
        <uap:SupportedFileTypes>
            <uap:FileType>.txt</uap:FileType>
            <uap:FileType>.foo</uap:FileType>
    </uap:SupportedFileTypes>
    <uap2:SupportedVerbs>
            <uap3:Verb Id="Edit" Parameters="/e &quot;%1&quot;">Edit</uap3:Verb>
            <uap3:Verb Id="Print" Parameters="/p &quot;%1&quot;">Print</uap3:Verb>
    </uap2:SupportedVerbs>
    <uap:Logo>Assets\MyExtensionLogo.png</uap:Logo>
    </uap3:FileTypeAssociation>
</uap3:Extension>
```

## <a name="see-also"></a>참고 항목

- [앱 패키지 매니페스트](https://msdn.microsoft.com/library/windows/apps/br211474.aspx)
