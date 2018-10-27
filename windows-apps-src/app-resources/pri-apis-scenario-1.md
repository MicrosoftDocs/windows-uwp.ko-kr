---
author: stevewhims
Description: In this scenario, we'll make a new app to represent our custom build system. We'll create a resource indexer and add strings and other kinds of resources to it. Then we'll generate and dump a PRI file.
title: 시나리오 1 문자열 리소스와 자산 파일에서 PRI 파일 생성
template: detail.hbs
ms.author: stwhi
ms.date: 05/07/2018
ms.topic: article
keywords: Windows 10, uwp, 리소스, 이미지, 자산, MRT, 한정자
ms.localizationpriority: medium
ms.openlocfilehash: 7555f4a61f7798fa32d137928cde8c042a7fcdfc
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/26/2018
ms.locfileid: "5693874"
---
# <a name="scenario-1-generate-a-pri-file-from-string-resources-and-asset-files"></a>시나리오 1: 문자열 리소스와 자산 파일에서 PRI 파일 생성
이 시나리오에서는 [패키지 리소스 인덱싱(PRI) API](https://msdn.microsoft.com/library/windows/desktop/mt845690)를 사용하여 사용자 지정 빌드 시스템을 나타내는 새로운 앱을 만듭니다. 이 사용자 지정 빌드 시스템의 용도는 대상 UWP 앱용 PRI 파일을 만드는 것입니다. 따라서, 이 연습의 일부로, 대상 UWP 앱의 리소스를 나타내는 몇 가지 샘플 리소스 파일(문자열 및 다른 종류의 리소스가 들어 있음)을 만들겠습니다.

## <a name="new-project"></a>새 프로젝트
먼저 Microsoft Visual Studio에서 새 프로젝트를 만듭니다. **Visual C++ Windows 콘솔 응용 프로그램** 프로젝트를 만들고 이름을 *CBSConsoleApp*("사용자 지정 빌드 시스템 콘솔 앱")으로 지정합니다.

**솔루션 플랫폼** 드롭다운에서 *x64*를 선택합니다.

## <a name="headers-static-library-and-dll"></a>헤더, 정적 라이브러리 및 dll
PRI API는 MrmResourceIndexer.h 헤더 파일(`%ProgramFiles(x86)%\Windows Kits\10\Include\<WindowsTargetPlatformVersion>\um\`에 설치되어 있음)에 선언되어 있습니다. 파일 `CBSConsoleApp.cpp`를 열고 필요한 몇 가지 다른 헤더와 함께 헤더를 포함시킵니다.

```cppwinrt
#include <string>
#include <windows.h>
#include <MrmResourceIndexer.h>
```

API는 MrmSupport.dll에 구현됩니다. 이 파일은 정적 라이브러리 MrmSupport.lib에 연결하여 액세스할 수 있습니다. 프로젝트 **속성**을 열고, **링커** > **입력**을 클릭하고, **AdditionalDependencies**를 편집하고, `MrmSupport.lib`를 추가합니다.

솔루션을 작성한 다음 `C:\Program Files (x86)\Windows Kits\10\bin\<WindowsTargetPlatformVersion>\x64\`에서 `MrmSupport.dll`을 빌드 출력 폴더(보통 `C:\Users\%USERNAME%\source\repos\CBSConsoleApp\x64\Debug\`)에 복사합니다.

나중에 필요하게 되므로 다음과 같은 도우미 함수를 `CBSConsoleApp.cpp`에 추가합니다.

```cppwinrt
inline void ThrowIfFailed(HRESULT hr)
{
    if (FAILED(hr))
    {
        // Set a breakpoint on this line to catch Win32 API errors.
        throw new std::exception();
    }
}
```

`main()` 함수에 COM을 초기화하고 초기화 해제하는 호출을 추가합니다.

```cppwinrt
int main()
{
    ::ThrowIfFailed(::CoInitializeEx(nullptr, COINIT_MULTITHREADED));
    
    // More code will go here.
    
    ::CoUninitialize();
}
```

## <a name="resource-files-belonging-to-the-target-uwp-app"></a>대상 UWP 앱에 속하는 리소스 파일
이제, 대상 UWP 앱의 리소스를 나타내는 몇 가지 샘플 리소스 파일(문자열 및 다른 종류의 리소스가 들어 있음)이 필요합니다. 물론 이러한 파일은 파일 시스템의 어느 곳에나 있을 수 있습니다. 그러나 이 연습에서는 CBSConsoleApp의 프로젝트 폴더에 저장하여 모든 파일이 한 곳에 있도록 하는 것이 편합니다. 이러한 리소스 파일은 파일 시스템에만 추가해야 하며, CBSConsoleApp 프로젝트에는 추가하지 마세요.

`CBSConsoleApp.vcxproj`가 들어 있는 동일한 폴더 내에, `UWPAppProjectRootFolder`라는 새로운 하위 폴더를 추가합니다. 해당 새 하위 폴더 내에 이러한 샘플 리소스 파일을 만듭니다.

### <a name="uwpappprojectrootfoldersample-imagepng"></a>\UWPAppProjectRootFolder\sample-image.png
이 파일에는 PNG 이미지를 포함할 수 있습니다.

### <a name="uwpappprojectrootfolderresourcesresw"></a>\UWPAppProjectRootFolder\resources.resw
```xml
<?xml version="1.0"?>
<root>
    <data name="LocalizedString1">
        <value>LocalizedString1-neutral</value>
    </data>
    <data name="LocalizedString2">
        <value>LocalizedString2-neutral</value>
    </data>
    <data name="NeutralOnlyString">
        <value>NeutralOnlyString-neutral</value>
    </data>
</root>
```

### <a name="uwpappprojectrootfolderde-deresourcesresw"></a>\UWPAppProjectRootFolder\de-DE\resources.resw
```xml
<?xml version="1.0"?>
<root>
    <data name="LocalizedString2">
        <value>LocalizedString2-de-DE</value>
    </data>
</root>
```

### <a name="uwpappprojectrootfolderen-usresourcesresw"></a>\UWPAppProjectRootFolder\en-US\resources.resw
```xml
<?xml version="1.0"?>
<root>
    <data name="LocalizedString1">
        <value>LocalizedString1-en-US</value>
    </data>
    <data name="EnOnlyString">
        <value>EnOnlyString-en-US</value>
    </data>
</root>
```

## <a name="index-the-resources-and-create-a-pri-file"></a>리소스를 인덱싱하고 PRI 파일을 만듭니다.
`main()` 함수에서, COM을 초기화하도록 호출하기 전에, 필요한 몇 가지 문자열을 선언하고, PRI 파일을 생성할 출력 폴더도 만듭니다.

```cppwinrt
std::wstring projectRootFolderUWPApp{ L"UWPAppProjectRootFolder" };
std::wstring generatedPRIsFolder{ projectRootFolderUWPApp + L"\\Generated PRIs" };
std::wstring filePathPRI{ generatedPRIsFolder + L"\\resources.pri" };
std::wstring filePathPRIDumpBasic{ generatedPRIsFolder + L"\\resources-pri-dump-basic.xml" };

::CreateDirectory(generatedPRIsFolder.c_str(), nullptr);
```

COM을 초기화하는 호출 직후, 리소스 인덱서 핸들을 선언한 다음, [**MrmCreateResourceIndexer**]()를 호출하여 리소스 인덱서를 만듭니다.

```cppwinrt
MrmResourceIndexerHandle indexer;
::ThrowIfFailed(::MrmCreateResourceIndexer(
    L"OurUWPApp",
    projectRootFolderUWPApp.c_str(),
    MrmPlatformVersion::MrmPlatformVersion_Windows10_0_0_0,
    L"language-en_scale-100_contrast-standard",
    &indexer));
```

다음은 **MrmCreateResourceIndexer**로 전달되고 있는 인수에 대한 설명입니다.

- 대상 UWP 앱의 패키지 패밀리 이름입니다. 이 이름은 나중에 이 리소스 인덱서로부터 PRI 파일을 생성할 때 리소스 맵 이름으로 사용됩니다.
- 대상 UWP 앱의 프로젝트 루트입니다. 즉, 리소스 파일의 경로입니다. 동일한 리소스 인덱서를 연속으로 API 호출 시 루트에 대한 상대 경로를 지정할 수 있도록 이를 지정합니다.
- 대상으로 지정하고자 하는 Windows 버전입니다.
- 기본 리소스 한정자의 목록입니다.
- 함수에서 리소스 인덱서 핸들을 설정할 수 있도록 해주는 리소스 인덱서 핸들에 대한 포인터입니다.

다음 단계는 방금 만든 리소스 인덱서에 리소스를 추가하는 것입니다. `resources.resw` 대상 UWP 앱에 대한 중간 문자열을 포함하는 리소스 파일(.resw)입니다. 내용을 확인하려면 위로 스크롤합니다(이 항목에서). `de-DE\resources.resw` 독일어 문자열을 포함하며 `en-US\resources.resw`는 영어 문자열을 포함합니다. 리소스 파일 내의 문자열 리소스를 리소스 인덱서에 추가하려면 [**MrmIndexResourceContainerAutoQualifiers**]()를 호출합니다. 셋째, 리소스 인덱서에 대한 중간 이미지 리소스를 포함하는 파일에 대해 [**MrmIndexFile**]() 함수를 호출합니다.

```cppwinrt
::ThrowIfFailed(::MrmIndexResourceContainerAutoQualifiers(indexer, L"resources.resw"));
::ThrowIfFailed(::MrmIndexResourceContainerAutoQualifiers(indexer, L"de-DE\\resources.resw"));
::ThrowIfFailed(::MrmIndexResourceContainerAutoQualifiers(indexer, L"en-US\\resources.resw"));
::ThrowIfFailed(::MrmIndexFile(indexer, L"ms-resource:///Files/sample-image.png", L"sample-image.png", L""));
```

**MrmIndexFile** 호출에서, 값 L"ms-resource:///Files/sample-image.png"는 리소스 URI입니다. 첫 번째 경로 세그먼트는 "파일"이며, 나중에 이 리소스 인덱서로부터 PRI 파일을 생성할 때 리소스 맵 하위 트리 이름으로 사용됩니다.

리소스 파일에 대한 리소스 인덱서를 간단히 살펴봤으므로, 이제 [**MrmCreateResourceFile**]() 함수를 호출하여 디스크에 PRI 파일을 생성할 차례입니다.

```cppwinrt
::ThrowIfFailed(::MrmCreateResourceFile(indexer, MrmPackagingModeStandaloneFile, MrmPackagingOptionsNone, generatedPRIsFolder.c_str()));
```

이 시점에서는 `resources.pri`라는 PRI 파일이 `Generated PRIs`라는 폴더 내에 생성되어 있습니다. 이제 리소스 인덱서 사용을 마쳤으므로 [**MrmDestroyIndexerAndMessages**]()를 호출하여 핸들을 폐기하고 할당되어 있는 컴퓨터 리소스를 해제합니다.

```cppwinrt
::ThrowIfFailed(::MrmDestroyIndexerAndMessages(indexer));
```

PRI 파일은 이진 파일이므로 해당 XML 파일에 이진 PRI 파일을 덤프하면 방금 생성한 항목을 보다 쉽게 확인할 수 있습니다. 이를 위해서는 [**MrmDumpPriFile**]()만 호출하면 됩니다.

```cppwinrt
::ThrowIfFailed(::MrmDumpPriFile(filePathPRI.c_str(), nullptr, MrmDumpType::MrmDumpType_Basic, filePathPRIDumpBasic.c_str()));
```

다음은 **MrmDumpPriFile**로 전달되고 있는 인수에 대한 설명입니다.

- 덤프할 PRI 파일에 대한 경로입니다. 이 호출에서는 리소스 인덱서를 사용하고 있지 않으므로(방금 폐기함) 전체 파일 경로를 지정해야 합니다.
- 스키마 파일이 없습니다. 스키마가 무엇인지에 대해서는 이 항목의 뒷부분에서 설명합니다.
- 여기에서는 기본 정보만 다룹니다.
- 만들려는 XML 파일의 경로입니다.

여기서 XML로 덤프되는 PRI 파일에 들어 있는 내용입니다.

```xml
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<PriInfo>
    <ResourceMap name="OurUWPApp" version="1.0" primary="true">
        <Qualifiers>
            <Language>en-US,de-DE</Language>
        </Qualifiers>
        <ResourceMapSubtree name="Files">
            <NamedResource name="sample-image.png" uri="ms-resource://OurUWPApp/Files/sample-image.png">
                <Candidate type="Path">
                    <Value>sample-image.png</Value>
                </Candidate>
            </NamedResource>
        </ResourceMapSubtree>
        <ResourceMapSubtree name="resources">
            <NamedResource name="EnOnlyString" uri="ms-resource://OurUWPApp/resources/EnOnlyString">
                <Candidate qualifiers="Language-en-US" isDefault="true" type="String">
                    <Value>EnOnlyString-en-US</Value>
                </Candidate>
            </NamedResource>
            <NamedResource name="LocalizedString1" uri="ms-resource://OurUWPApp/resources/LocalizedString1">
                <Candidate qualifiers="Language-en-US" isDefault="true" type="String">
                    <Value>LocalizedString1-en-US</Value>
                </Candidate>
                <Candidate type="String">
                    <Value>LocalizedString1-neutral</Value>
                </Candidate>
            </NamedResource>
            <NamedResource name="LocalizedString2" uri="ms-resource://OurUWPApp/resources/LocalizedString2">
                <Candidate qualifiers="Language-de-DE" type="String">
                    <Value>LocalizedString2-de-DE</Value>
                </Candidate>
                <Candidate type="String">
                    <Value>LocalizedString2-neutral</Value>
                </Candidate>
            </NamedResource>
            <NamedResource name="NeutralOnlyString" uri="ms-resource://OurUWPApp/resources/NeutralOnlyString">
                <Candidate type="String">
                    <Value>NeutralOnlyString-neutral</Value>
                </Candidate>
            </NamedResource>
        </ResourceMapSubtree>
    </ResourceMap>
</PriInfo>
```

이 정보는 리소스 맵으로 시작하며 대상 UWP 앱의 패키지 패밀리 이름으로 이름이 지정되어 있습니다. 리소스 맵으로 묶여 있는 내용은 두 개의 리소스 맵 하위 트리입니다(인덱싱된 파일 리소스용 트리 하나와 문자열 리소스용 다른 트리 하나). 패키지 패밀리 이름이 모든 리소스 URI에 삽입되는 과정을 확인해 보세요.

첫 번째 문자열 리소스는 `en-US\resources.resw`의 *EnOnlyString*이며, 하나의 후보만 가지고 있습니다(*language-en-US* 한정자와 일치함). 그 다음으로 `resources.resw` 및 `en-US\resources.resw` 모두의 *LocalizedString1*이 옵니다. 그 결과, *language-en-US*과 일치하는 것 하나와 모든 컨텍스트와 일치하는 대체 중간 후보 등 두 개의 후보를 갖게 됩니다. 마찬가지로, *LocalizedString2*에도 *language-de-DE*와 중간의 두 후보가 있게 됩니다. 마지막으로, *NeutralOnlyString*은 중간 형식으로만 존재합니다. 지역화용이 아님을 알 수 있도록 이름도 그렇게 지정했습니다.

## <a name="summary"></a>요약
이 시나리오에서는 [패키지 리소스 인덱싱(PRI) API](https://msdn.microsoft.com/library/windows/desktop/mt845690)를 사용하여 리소스 인덱서를 만드는 방법을 살펴봤습니다. 그리고 리소스 인덱서에 문자열 리소스와 자산 파일을 추가했습니다. 그런 다음, 리소스 인덱서를 사용하여 이진 PRI 파일을 생성했습니다. 마지막으로 XML 형식으로 이진 PRI 파일을 덤프하여 원하는 정보가 들어 있는지를 확인했습니다.

## <a name="important-apis"></a>중요 API
* [패키지 리소스 인덱싱(PRI) 참조](https://msdn.microsoft.com/library/windows/desktop/mt845690)

## <a name="related-topics"></a>관련 항목
* [패키지 리소스 인덱싱(PRI) API 및 사용자 지정 빌드 시스템](pri-apis-custom-build-systems.md)
