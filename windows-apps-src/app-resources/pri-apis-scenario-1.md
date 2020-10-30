---
description: 이 시나리오에서는 사용자 지정 빌드 시스템을 나타내는 새 앱을 만듭니다. 리소스 인덱서를 만들고 문자열 및 다른 종류의 리소스를 추가 합니다. 그런 다음, PRI 파일을 생성 하 고 덤프 합니다.
title: 시나리오 1 문자열 리소스 및 자산 파일에서 PRI 파일 생성
template: detail.hbs
ms.date: 05/07/2018
ms.topic: article
keywords: Windows 10, UWP, 리소스, 이미지, 자산, MRT, 한정자
ms.localizationpriority: medium
ms.openlocfilehash: 44f4b8297cc1a34a378af137f75babca64e4edf2
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93031676"
---
# <a name="scenario-1-generate-a-pri-file-from-string-resources-and-asset-files"></a>시나리오 1: 문자열 리소스 및 자산 파일에서 PRI 파일 생성
이 시나리오에서는 [PRI (패키지 리소스 인덱싱) api](/windows/desktop/menurc/pri-indexing-reference) 를 사용 하 여 사용자 지정 빌드 시스템을 나타내는 새 앱을 만듭니다. 이 사용자 지정 빌드 시스템의 목적은 대상 UWP 앱에 대 한 PRI 파일을 만드는 것입니다. 따라서이 연습의 일부로, 대상 UWP 앱의 리소스를 나타내기 위해 일부 샘플 리소스 파일 (문자열 및 다른 종류의 리소스 포함)을 만듭니다.

## <a name="new-project"></a>새 프로젝트
먼저 Microsoft Visual Studio에서 새 프로젝트를 만듭니다. **Visual C++ Windows 콘솔 응용 프로그램** 프로젝트를 만들고 이름을 *CBSConsoleApp* ("사용자 지정 빌드 시스템 콘솔 앱"의 경우)로 지정 합니다.

**솔루션 플랫폼** 드롭다운에서 *x64* 를 선택 합니다.

## <a name="headers-static-library-and-dll"></a>헤더, 정적 라이브러리 및 dll
PRI Api는에 설치 되는 MrmResourceIndexer 헤더 파일에 선언 됩니다 `%ProgramFiles(x86)%\Windows Kits\10\Include\<WindowsTargetPlatformVersion>\um\` . 파일을 열고 `CBSConsoleApp.cpp` 필요한 다른 헤더와 함께 헤더를 포함 합니다.

```cppwinrt
#include <string>
#include <windows.h>
#include <MrmResourceIndexer.h>
```

Api는 정적 라이브러리 MrmSupport에 연결 하 여 액세스 하는 MrmSupport.dll에서 구현 됩니다. 프로젝트의 **속성** 을 열고 **링커**  >  **입력** , **additionaldependencies** 편집 및 추가를 차례로 클릭 `MrmSupport.lib` 합니다.

솔루션을 빌드한 다음 `MrmSupport.dll` 에서 `C:\Program Files (x86)\Windows Kits\10\bin\<WindowsTargetPlatformVersion>\x64\` 빌드 출력 폴더 (아마도)로 복사 `C:\Users\%USERNAME%\source\repos\CBSConsoleApp\x64\Debug\` 합니다.

다음 도우미 함수를에 추가 `CBSConsoleApp.cpp` 합니다 .이 함수는 필요 하기 때문입니다.

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

함수에서 `main()` 호출을 추가 하 여 COM을 초기화 하 고 초기화를 취소 합니다.

```cppwinrt
int main()
{
    ::ThrowIfFailed(::CoInitializeEx(nullptr, COINIT_MULTITHREADED));
    
    // More code will go here.
    
    ::CoUninitialize();
}
```

## <a name="resource-files-belonging-to-the-target-uwp-app"></a>대상 UWP 앱에 속한 리소스 파일
이제 대상 UWP 앱의 리소스를 나타내는 몇 가지 샘플 리소스 파일 (문자열 및 다른 종류의 리소스 포함)이 필요 합니다. 물론 파일 시스템의 어느 위치에 나 배치할 수 있습니다. 그러나이 연습에서는 모든 항목이 한 곳에 있도록 CBSConsoleApp의 프로젝트 폴더에 배치 하는 것이 편리 합니다. 이러한 리소스 파일을 파일 시스템에 추가 하기만 하면 됩니다. CBSConsoleApp 프로젝트에 추가 하지 마세요.

을 포함 하는 동일한 폴더 내에 `CBSConsoleApp.vcxproj` 라는 새 하위 폴더를 추가 `UWPAppProjectRootFolder` 합니다. 해당 새 하위 폴더 내에서 이러한 샘플 리소스 파일을 만듭니다.

### <a name="uwpappprojectrootfoldersample-imagepng"></a>\UWPAppProjectRootFolder\sample-image.png
이 파일은 PNG 이미지를 포함할 수 있습니다.

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
함수에서 `main()` COM 초기화를 호출 하기 전에 필요한 일부 문자열을 선언 하 고 PRI 파일을 생성할 출력 폴더도 만들어야 합니다.

```cppwinrt
std::wstring projectRootFolderUWPApp{ L"UWPAppProjectRootFolder" };
std::wstring generatedPRIsFolder{ projectRootFolderUWPApp + L"\\Generated PRIs" };
std::wstring filePathPRI{ generatedPRIsFolder + L"\\resources.pri" };
std::wstring filePathPRIDumpBasic{ generatedPRIsFolder + L"\\resources-pri-dump-basic.xml" };

::CreateDirectory(generatedPRIsFolder.c_str(), nullptr);
```

COM 초기화를 호출 하는 즉시 리소스 인덱서 핸들을 선언 하 고 [**MrmCreateResourceIndexer**](/windows/desktop/menurc/mrmcreateresourceindexer) 를 호출 하 여 리소스 인덱서를 만듭니다.

```cppwinrt
MrmResourceIndexerHandle indexer;
::ThrowIfFailed(::MrmCreateResourceIndexer(
    L"OurUWPApp",
    projectRootFolderUWPApp.c_str(),
    MrmPlatformVersion::MrmPlatformVersion_Windows10_0_0_0,
    L"language-en_scale-100_contrast-standard",
    &indexer));
```

**MrmCreateResourceIndexer** 에 전달 되는 인수에 대 한 설명은 다음과 같습니다.

- 나중에이 리소스 인덱서에서 PRI 파일을 생성할 때 리소스 맵 이름으로 사용 되는 대상 UWP 앱의 패키지 패밀리 이름입니다.
- 대상 UWP 앱의 프로젝트 루트입니다. 즉, 리소스 파일에 대 한 경로입니다. 동일한 리소스 인덱서에 대 한 후속 API 호출에서 해당 루트에 상대적인 경로를 지정할 수 있도록이를 지정 합니다.
- 대상으로 지정할 Windows 버전입니다.
- 기본 리소스 한정자의 목록입니다.
- 함수에서 설정할 수 있도록 리소스 인덱서 핸들에 대 한 포인터입니다.

다음 단계는 방금 만든 리소스 인덱서에 리소스를 추가 하는 것입니다. `resources.resw` 는 대상 UWP 앱에 대 한 중립 문자열을 포함 하는 리소스 파일 (. resw)입니다. 내용을 보려면이 항목에서 위로 스크롤합니다. `de-DE\resources.resw` 에는 독일어 문자열과 영어 문자열이 포함 되어 있습니다 `en-US\resources.resw` . 리소스 파일 내의 문자열 리소스를 리소스 인덱서에 추가 하려면 [**MrmIndexResourceContainerAutoQualifiers**](/windows/desktop/menurc/mrmindexresourcecontainerautoqualifiers)를 호출 합니다. Thirdly, [**MrmIndexFile**](/windows/desktop/menurc/mrmindexfile) 함수를 리소스 인덱서에 중립 이미지 리소스를 포함 하는 파일로 호출 합니다.

```cppwinrt
::ThrowIfFailed(::MrmIndexResourceContainerAutoQualifiers(indexer, L"resources.resw"));
::ThrowIfFailed(::MrmIndexResourceContainerAutoQualifiers(indexer, L"de-DE\\resources.resw"));
::ThrowIfFailed(::MrmIndexResourceContainerAutoQualifiers(indexer, L"en-US\\resources.resw"));
::ThrowIfFailed(::MrmIndexFile(indexer, L"ms-resource:///Files/sample-image.png", L"sample-image.png", L""));
```

**MrmIndexFile** 에 대 한 호출에서 값 L "ms-resource://Hfile/sample-image.png"는 리소스 uri입니다. 첫 번째 경로 세그먼트는 "Files" 이며, 나중에이 리소스 인덱서에서 PRI 파일을 생성할 때 리소스 맵 하위 트리 이름으로 사용 됩니다.

리소스 파일에 대 한 리소스 인덱서를 briefed, [**MrmCreateResourceFile**](/windows/desktop/menurc/mrmcreateresourcefile) 함수를 호출 하 여 디스크에서 PRI 파일을 생성 해야 합니다.

```cppwinrt
::ThrowIfFailed(::MrmCreateResourceFile(indexer, MrmPackagingModeStandaloneFile, MrmPackagingOptionsNone, generatedPRIsFolder.c_str()));
```

이 시점에서 라는 `resources.pri` 폴더 내에 라는 PRI 파일이 생성 되었습니다 `Generated PRIs` . 이제 리소스 인덱서를 사용 하 여 작업을 완료 했으므로 [**MrmDestroyIndexerAndMessages**](/windows/desktop/menurc/mrmdestroyindexerandmessages) 를 호출 하 여 핸들을 삭제 하 고 할당 된 모든 컴퓨터 리소스를 해제 합니다.

```cppwinrt
::ThrowIfFailed(::MrmDestroyIndexerAndMessages(indexer));
```

PRI 파일은 이진 파일 이므로 이진 PRI 파일을 해당 XML로 덤프할 경우 방금 생성 한 항목을 쉽게 볼 수 있습니다. [**MrmDumpPriFile**](/windows/desktop/menurc/mrmdumpprifile) 에 대 한 호출은이 작업만 수행 합니다.

```cppwinrt
::ThrowIfFailed(::MrmDumpPriFile(filePathPRI.c_str(), nullptr, MrmDumpType::MrmDumpType_Basic, filePathPRIDumpBasic.c_str()));
```

**MrmDumpPriFile** 에 전달 되는 인수에 대 한 설명은 다음과 같습니다.

- 덤프할 PRI 파일의 경로입니다. 이 호출에서 리소스 인덱서를 사용 하 고 있지 않기 때문에 전체 파일 경로를 지정 해야 합니다.
- 스키마 파일이 없습니다. 이 항목의 뒷부분에서 스키마의 용도에 대해 설명 합니다.
- 기본 정보만 있으면 됩니다.
- 만들 XML 파일의 경로입니다.

여기에는 여기에 포함 된 XML로 덤프 된 PRI 파일이 포함 됩니다.

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

이 정보는 대상 UWP 앱의 패키지 패밀리 이름으로 이름이 지정 된 리소스 맵으로 시작 합니다. 리소스 맵으로 묶인 두 개의 리소스 맵 하위 트리는 인덱싱된 파일 리소스에 대 한 것이 고 다른 하나는 문자열 리소스에 대 한 것입니다. 패키지 제품군 이름이 모든 리소스 Uri에 삽입 된 방식을 확인 합니다.

첫 번째 문자열 리소스는의 *Enonly 문자열이* `en-US\resources.resw` 고 *언어-en-us* 한정자와 일치 하는 후보가 하나 뿐입니다. 다음에는 및에서 *LocalizedString1* 제공 `resources.resw` `en-US\resources.resw` 됩니다. 따라서 두 가지 후보가 있습니다. 하나는 일치 하는 *언어-en-us* 이 고 모든 컨텍스트와 일치 하는 대체 (fallback) 중립 후보입니다. 마찬가지로 *LocalizedString2* 에는 두 가지 후보가 있습니다. *언어 de-de* 및 중립입니다. 마지막으로 *NeutralOnlyString* 는 중립 형식 으로만 존재 합니다. 이 이름을 사용 하 여 지역화할 수 없는 것으로 분명 하 게 지정 했습니다.

## <a name="summary"></a>요약
이 시나리오에서는 [PRI (패키지 리소스 인덱싱) api](/windows/desktop/menurc/pri-indexing-reference) 를 사용 하 여 리소스 인덱서를 만드는 방법을 살펴보았습니다. 리소스 인덱서에 문자열 리소스와 자산 파일을 추가 했습니다. 그런 다음 리소스 인덱서를 사용 하 여 이진 PRI 파일을 생성 했습니다. 마지막으로 XML 형식으로 이진 PRI 파일을 덤프 했으므로 예상한 정보가 포함 되어 있는지 확인할 수 있습니다.

## <a name="important-apis"></a>중요 API
* [PRI (패키지 리소스 인덱싱) 참조](/windows/desktop/menurc/pri-indexing-reference)

## <a name="related-topics"></a>관련 항목
* [PRI(패키지 리소스 인덱싱) API 및 사용자 지정 빌드 시스템](pri-apis-custom-build-systems.md)
