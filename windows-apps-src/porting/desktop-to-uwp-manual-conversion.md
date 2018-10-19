---
author: normesta
Description: Shows how to manually package a Windows desktop application (like Win32, WPF, and Windows Forms) for Windows 10.
Search.Product: eADQiWindows 10XVcnh
title: 응용 프로그램을 수동으로 패키징 (데스크톱 브리지)
ms.author: normesta
ms.date: 05/18/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: e8c2a803-9803-47c5-b117-73c4af52c5b6
ms.localizationpriority: medium
ms.openlocfilehash: 9f14e7f8747639ef139e774416e09af954211940
ms.sourcegitcommit: 310a4555fedd4246188a98b31f6c094abb33ec60
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/19/2018
ms.locfileid: "5132942"
---
# <a name="package-a-desktop-application-manually"></a>데스크톱 응용 프로그램을 수동으로 패키징

이 항목에서는 Visual Studio 또는 Desktop App Converter (DAC) 같은 도구를 사용 하지 않고 응용 프로그램을 패키징하는 방법을 보여 줍니다.

앱을 수동으로 패키징하려면 패키지 매니페스트 파일을 만들고 명령줄 도구를 실행하여 Windows 앱 패키지를 생성합니다.

프로세스를 보다 세부적으로 제어 하 고 xcopy 명령을 사용 하 여 응용 프로그램을 설치 하거나 앱의 설치 관리자가 시스템에 익숙한 경우 수동 패키징을 고려 하십시오.

설치 관리자가 시스템을 어떻게 변경했는지 확실히 모르는 경우나 패키지 매니페스트를 생성하기 위해 자동화된 도구를 사용한 경우에는 [이러한](desktop-to-uwp-root.md#convert) 옵션을 고려하십시오.

>[!IMPORTANT]
>데스크톱 응용 프로그램용 Windows 앱 패키지를 만들 수 있습니다 (데스크톱 브리지도 알려진 Windows 10, 버전 1607에서에서 도입 된 그렇지 하며 Windows 10 1 주년 업데이트 (10.0; 대상으로 하는 프로젝트에만 사용할 수 있습니다 빌드 14393) 또는 Visual Studio의 최신 릴리스 합니다.

## <a name="first-prepare-your-application"></a>첫 번째, 응용 프로그램 준비

응용 프로그램에 대 한 패키지 만들기를 시작 하기 전에이 가이드를 검토: [데스크톱 응용 프로그램 패키지를 준비](desktop-to-uwp-prepare.md)합니다.

## <a name="create-a-package-manifest"></a>패키지 매니페스트 만들기

파일을 만들고 이름을 **appxmanifest.xml**로 지정한 다음, 여기에 이 XML를 추가합니다.

이것은 패키지에 필요한 요소와 특성이 포함된 기본 템플릿입니다. 다음 섹션에서는 여기에 값을 추가해볼 것입니다.

```XML
<?xml version="1.0" encoding="utf-8"?>
<Package
    xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities">
  <Identity Name="" Version="" Publisher="" ProcessorArchitecture="" />
    <Properties>
       <DisplayName></DisplayName>
       <PublisherDisplayName></PublisherDisplayName>
             <Description></Description>
      <Logo></Logo>
    </Properties>
    <Resources>
      <Resource Language="" />
    </Resources>
      <Dependencies>
      <TargetDeviceFamily Name="Windows.Desktop" MinVersion="" MaxVersionTested="" />
      </Dependencies>
      <Capabilities>
        <rescap:Capability Name="runFullTrust"/>
      </Capabilities>
    <Applications>
      <Application Id="" Executable="" EntryPoint="Windows.FullTrustApplication">
        <uap:VisualElements DisplayName="" Description=""   Square150x150Logo=""
                   Square44x44Logo=""   BackgroundColor="" />
      </Application>
     </Applications>
  </Package>
```

## <a name="fill-in-the-package-level-elements-of-your-file"></a>파일의 패키지 수준 요소를 입력합니다.

이 템플릿에 패키지를 설명하는 정보를 입력합니다.

### <a name="identity-information"></a>ID 정보

여기에는 특성에 대한 자리 표시자 텍스트가 포함된 **ID** 요소가 예로 나와 있습니다. ``ProcessorArchitecture`` 특성을 ``x64``또는 ``x86``으로 설정할 수 있습니다.

```XML
<Identity Name="MyCompany.MySuite.MyApp"
          Version="1.0.0.0"
          Publisher="CN=MyCompany, O=MyCompany, L=MyCity, S=MyState, C=MyCountry"
                ProcessorArchitecture="x64">
```
> [!NOTE]
> Windows 스토어에서 응용 프로그램 이름, 예약한 경우 Windows 개발자 센터 대시보드를 사용 하 여 이름과 게시자를 얻을 수 있습니다. 테스트용 로드에는 다른 시스템에 응용 프로그램에 하려는 경우에 한 해 고유한 이름을 선택 인증서의 이름과 일치 하는 게시자 이름이 앱 로그인에 사용으로 제공할 수 있습니다.

### <a name="properties"></a>특성

[특성](https://docs.microsoft.com/uwp/schemas/appxpackage/appxmanifestschema/element-properties) 요소는 세 개의 필수 자식 요소를 가지고 있습니다. 여기에는 요소에 대한 자리 표시자 텍스트가 포함된 **특성** 요소가 예로 나와 있습니다. **DisplayName** 스토어에 업로드 된 앱에 대해 스토어에서 예약 하는 응용 프로그램의 이름입니다.

```XML
<Properties>
  <DisplayName>MyApp</DisplayName>
  <PublisherDisplayName>MyCompany</PublisherDisplayName>
  <Logo>images\icon.png</Logo>
</Properties>
```

### <a name="resources"></a>리소스

여기에는 [리소스](https://docs.microsoft.com/uwp/schemas/appxpackage/appxmanifestschema/element-resources) 노드의 예가 나와 있습니다.

```XML
<Resources>
  <Resource Language="en-us" />
</Resources>
```
### <a name="dependencies"></a>종속성

항상 설정에 대 한 패키지를 만드는 데스크톱 앱의 경우는 ``Name`` 특성을 ``Windows.Desktop``.

```XML
<Dependencies>
<TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.14316.0" MaxVersionTested="10.0.15063.0" />
</Dependencies>
```

### <a name="capabilities"></a>접근 권한 값
추가 해야에 대 한 패키지를 생성 하는 데스크톱 앱의 ``runFullTrust`` 접근 권한 값입니다.

```XML
<Capabilities>
  <rescap:Capability Name="runFullTrust"/>
</Capabilities>
```
## <a name="fill-in-the-application-level-elements"></a>응용 프로그램 수준에서 요소 입력

이 템플릿에 앱을 설명하는 정보를 입력합니다.

### <a name="application-element"></a>응용 프로그램 요소

패키지를 생성 하는 데스크톱 앱의 ``EntryPoint`` 응용 프로그램 요소의 특성은 항상 ``Windows.FullTrustApplication``.

```XML
<Applications>
  <Application Id="MyApp"     
        Executable="MyApp.exe" EntryPoint="Windows.FullTrustApplication">
   </Application>
</Applications>
```

### <a name="visual-elements"></a>시각적 요소

여기에는 [VisualElements](https://docs.microsoft.com/uwp/schemas/appxpackage/appxmanifestschema/element-visualelements) 노드가 예로 나와 있습니다.

```XML
<uap:VisualElements
    BackgroundColor="#464646"
    DisplayName="My App"
    Square150x150Logo="images\icon.png"
    Square44x44Logo="images\small_icon.png"
    Description="A useful description" />
```
<a id="target-based-assets" />

## <a name="optional-add-target-based-unplated-assets"></a>(선택 사항)대상 기반의 판이 없는 자산을 추가

대상 기반 자산은 아이콘 및 타일을 위한 것으로 Windows 작업 표시줄, 작업 보기, Alt + Tab, 끌기 도우미 및 시작 타일의 오른쪽 아래 모서리에 표시됩니다. 자세한 내용은 [여기](https://docs.microsoft.com/windows/uwp/shell/tiles-and-notifications/app-assets#target-based-assets)를 참조하세요.

1. 올바른 44x44 이미지를 가져와서 이미지가 들어 있는 폴더(즉, "자산" 폴더)에 복사합니다.

2. 각 44x44 이미지의 복사본을 동일한 폴더에서 만들고 파일 이름에 **.targetsize-44_altform-unplated**를 추가합니다. 아이콘마다 각각 특정 방식으로 이름이 지정된 두 복사본이 있어야 합니다. 예를 들어, 프로세스를 완료한 후에는 자산 폴더에 **MYAPP_44x44.png** 및 **MYAPP_44x44.targetsize-44_altform-unplated.png**이 포함될 수 있습니다.

   > [!NOTE]
   > 이 예제에서 **MYAPP_44x44.png**라는 아이콘은 Windows 앱 패키지의 ``Square44x44Logo`` 로고 특성에서 참조하게 될 아이콘입니다.

3.  Windows 앱 패키지에서는 생성 중인 모든 아이콘에 대한 ``BackgroundColor``를 투명으로 설정합니다.

4. 새로운 PRI(Package Resource Index) 파일을 생성하려면 다음 하위 섹션을 진행합니다.

<a id="make-pri" />

### <a name="generate-a-package-resource-index-pri-file"></a>PRI(Package Resource Index) 파일 생성

위의 섹션에 설명 된 대로 대상 기반 자산을 만들 경우 수정 응용 프로그램의 시각적 자산 패키지를 만든 후, 새 PRI 파일을 생성 해야 합니다.

1.  **VS 2017용 개발자 명령 프롬프트**를 엽니다.

    ![개발자 명령 프롬프트](images/desktop-to-uwp/developer-command-prompt.png)

2.  디렉터리를 패키지의 루트 폴더로 변경한 다음 ``makepri createconfig /cf priconfig.xml /dq en-US`` 명령을 실행하여 priconfig.xml 파일을 만듭니다.

5.  ``makepri new /pr <PHYSICAL_PATH_TO_FOLDER> /cf <PHYSICAL_PATH_TO_FOLDER>\priconfig.xml`` 명령을 사용하여 resources.pri 파일을 만듭니다.

    예를 들어, 응용 프로그램에 대 한 명령은 다음과 같습니다: ``makepri new /pr c:\MYAPP /cf c:\MYAPP\priconfig.xml``.

6.  다음 단계의 지침에 따라 Windows 앱 패키지를 패키징합니다.

<a id="make-appx" />

## <a name="generate-a-windows-app-package"></a>Windows 앱 패키지 생성

**MakeAppx.exe**를 사용하여 프로젝트의 Windows 앱 패키지를 생성합니다. Windows 10 SDK와 함께 포함되어 있습니다. Visual Studio가 설치되어 있는 경우, 해당 Visual Studio 버전용 개발자 명령 프롬프트를 통해 쉽게 액세스할 수 있습니다.

[MakeAppx.exe 도구를 사용하여 앱 패키지 만들기](https://docs.microsoft.com/windows/uwp/packaging/create-app-package-with-makeappx-tool)를 참조하세요.

## <a name="run-the-packaged-app"></a>패키지로 만든 앱 실행

이렇게 하면 응용 프로그램 인증서를 얻어서 로그인 할 필요 없이 로컬 테스트를 실행할 수 있습니다. 이 PowerShell cmdlet을 실행하면 됩니다.

```Add-AppxPackage –Register AppxManifest.xml```

앱의 .exe 또는 .dll 파일을 업데이트하려면 패키지의 기존 파일을 새 파일로 바꾸고, AppxManifest.xml에서 버전 번호를 늘린 다음, 위의 명령을 다시 실행합니다.

> [!NOTE]
> 패키지 된 응용 프로그램이 항상 대화형 사용자로 실행 하 고에 패키지 된 응용 프로그램을 설치할 수 있는 모든 드라이브는 NTFS 형식으로 포맷 되어야 합니다.

## <a name="next-steps"></a>다음 단계

**질문에 대한 답변 찾기**

질문이 있으세요? Stack Overflow에서 질문해 주세요. 저희 팀이 이 [태그](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge)를 모니터링합니다. [여기](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D)에서 문의할 수도 있습니다.

**피드백 제공 또는 기능 제안**

[UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial)를 참조하세요.

**단계별로 코드 실행 / 문제를 찾아서 해결**

[실행, 디버그 및 패키지 된 데스크톱 응용 프로그램 테스트](desktop-to-uwp-debug.md) 를 참조 하세요.

**응용 프로그램에 서명 하 고 배포**

[패키지로 만든된 데스크톱 응용 프로그램 배포](desktop-to-uwp-distribute.md) 를 참조 하세요.
