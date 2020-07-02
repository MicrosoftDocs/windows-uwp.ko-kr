---
Description: 확장을 사용하여 미리 정의된 방법으로 패키지 데스크톱 앱과 Windows 10을 통합할 수 있습니다.
title: 데스크톱 브리지를 사용하여 기존 데스크톱 앱 현대화
ms.date: 04/18/2018
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 0a8cedac-172a-4efd-8b6b-67fd3667df34
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.openlocfilehash: d9f5ca95678a8b31ed53cfdf2c4e6433bca504c8
ms.sourcegitcommit: 4df8c04fc6c22ec76cdb7bb26f327182f2dacafa
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/24/2020
ms.locfileid: "85334452"
---
# <a name="integrate-your-desktop-app-with-windows-10-and-uwp"></a>데스크톱 앱을 Windows 10 및 UWP와 통합

데스크톱 앱에 [패키지 ID](modernize-packaged-apps.md)가 있는 경우 [패키지 매니페스트](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/schema-root)의 미리 정의된 확장을 사용하여 앱을 Windows 10과 통합할 수 있습니다.

예를 들어 확장을 사용하여 방화벽 예외를 만들거나, 앱을 파일 형식의 기본 애플리케이션으로 설정하거나, 시작 타일이 앱을 가리키도록 지정할 수 있습니다. 확장을 사용하려면 앱의 패키지 매니페스트 파일에 약간의 XML을 추가하기만 하면 됩니다. 코드는 필요 없습니다.

이 문서에서는 이러한 확장과 확장을 통해 수행할 수 있는 작업을 설명합니다.

> [!NOTE]
> 이 문서에서 설명하는 기능을 사용하려면 [데스크톱 앱을 MSIX 패키지로 패키징](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-root)하거나 [스파스 패키지를 사용하여 앱 ID를 부여](grant-identity-to-nonpackaged-apps.md)하는 두 가지 방법 중 하나로 데스크톱 앱의 [패키지 ID](modernize-packaged-apps.md)를 준비해야 합니다.

## <a name="transition-users-to-your-app"></a>사용자를 나의 앱으로 전환

사용자가 나의 패키지된 앱으로 전환하도록 도와주세요.

* [기존의 [시작] 타일 및 작업 표시줄 단추가 나의 패키지된 앱을 가리키도록 지정](#point)
* [패키지된 애플리케이션이 데스크톱 앱 대신 파일을 열도록 지정](#make)
* [패키지된 애플리케이션을 파일 형식 세트와 연결](#associate)
* [특정 형식의 파일 상황에 맞는 메뉴에 옵션 추가](#add)
* [URL을 사용하여 특정 형식의 파일을 직접 열기](#open)

<a id="point"></a>

### <a name="point-existing-start-tiles-and-taskbar-buttons-to-your-packaged-app"></a>기존의 [시작] 타일 및 작업 표시줄 단추가 나의 패키지된 앱을 가리키도록 지정

사용자가 데스크톱 애플리케이션을 작업 표시줄 또는 시작 메뉴에 고정시켰을 수 있습니다. 이러한 바로 가기가 새 패키지된 앱을 가리키도록 지정할 수 있습니다.

#### <a name="xml-namespace"></a>XML 네임스페이스

`http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities/3`

#### <a name="elements-and-attributes-of-this-extension"></a>이 확장의 요소 및 특성

```XML
<Extension Category="windows.desktopAppMigration">
    <DesktopAppMigration>
        <DesktopApp AumId="[your_app_aumid]" />
        <DesktopApp ShortcutPath="[path]" />
    </DesktopAppMigration>
</Extension>
```

전체 스키마 참조는 [여기](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-rescap3-desktopappmigration)서 찾을 수 있습니다.

|이름 | 설명 |
|-------|-------------|
|범주 |항상 ``windows.desktopAppMigration``입니다.
|AumID |패키지된 앱의 애플리케이션 사용자 모델 ID입니다. |
|ShortcutPath |데스크톱 버전의 앱을 시작하는 .lnk 파일의 경로입니다. |

#### <a name="example"></a>예제

```XML
<Package
  xmlns:rescap3="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities/3"
  IgnorableNamespaces="rescap3">
  <Applications>
    <Application>
      <Extensions>
        <rescap3:Extension Category="windows.desktopAppMigration">
          <rescap3:DesktopAppMigration>
            <rescap3:DesktopApp AumId="[your_app_aumid]" />
            <rescap3:DesktopApp ShortcutPath="%USERPROFILE%\Desktop\[my_app].lnk" />
            <rescap3:DesktopApp ShortcutPath="%APPDATA%\Microsoft\Windows\Start Menu\Programs\[my_app].lnk" />
            <rescap3:DesktopApp ShortcutPath="%PROGRAMDATA%\Microsoft\Windows\Start Menu\Programs\[my_app_folder]\[my_app].lnk"/>
         </rescap3:DesktopAppMigration>
        </rescap3:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

#### <a name="related-sample"></a>관련 샘플

[transition/migration/uninstallation 경로가 포함된 WPF 사진 뷰어](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/DesktopAppTransition)

<a id="make"></a>

### <a name="make-your-packaged-application-open-files-instead-of-your-desktop-app"></a>패키지된 애플리케이션이 데스크톱 앱 대신 파일을 열도록 지정

특정 파일 형식에서는 사용자가 기본적으로 데스크톱 버전의 앱 대신 새 패키지된 애플리케이션을 열도록 지정할 수 있습니다.

이렇게 하려면 파일 연결을 상속하려는 각 애플리케이션의 [프로그래밍 ID(ProgID)](https://docs.microsoft.com/windows/desktop/shell/fa-progids)를 지정합니다.

#### <a name="xml-namespaces"></a>XML 네임스페이스

* `http://schemas.microsoft.com/appx/manifest/uap/windows10/3`
* `http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities/3`

#### <a name="elements-and-attributes-of-this-extension"></a>이 확장의 요소 및 특성

```XML
<Extension Category="windows.fileTypeAssociation">
    <FileTypeAssociation Name="[Name]">
         <MigrationProgIds>
            <MigrationProgId>"[ProgID]"</MigrationProgId>
        </MigrationProgIds>
    </FileTypeAssociation>
</Extension>
```

전체 스키마 참조는 [여기](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation)서 찾을 수 있습니다.

|이름 |설명 |
|-------|-------------|
|범주 |항상 ``windows.fileTypeAssociation``입니다.
|이름 |파일 형식 연결의 이름입니다. 이 이름은 파일 형식을 구성하고 그룹화하는 데 사용할 수 있습니다. 이름은 공백 없이 모두 소문자여야 합니다. |
|MigrationProgId |파일 연결을 상속하려는 데스크톱 애플리케이션의 애플리케이션, 구성 요소 및 버전을 설명하는 [프로그래밍 ID(ProgID)](https://docs.microsoft.com/windows/desktop/shell/fa-progids)입니다.|

#### <a name="example"></a>예제

```XML
<Package
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  xmlns:rescap3="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities/3"
  IgnorableNamespaces="uap3, rescap3">
  <Applications>
    <Application>
      <Extensions>
        <uap:Extension Category="windows.fileTypeAssociation">
          <uap3:FileTypeAssociation Name="myfiletypes">
            <rescap3:MigrationProgIds>
              <rescap3:MigrationProgId>Foo.Bar.1</rescap3:MigrationProgId>
              <rescap3:MigrationProgId>Foo.Bar.2</rescap3:MigrationProgId>
            </rescap3:MigrationProgIds>
          </uap3:FileTypeAssociation>
        </uap:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

#### <a name="related-sample"></a>관련 샘플

[transition/migration/uninstallation 경로가 포함된 WPF 사진 뷰어](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/DesktopAppTransition)

<a id="associate"></a>

### <a name="associate-your-packaged-application-with-a-set-of-file-types"></a>패키지된 애플리케이션을 파일 형식 세트와 연결

패키지된 애플리케이션을 파일 형식 확장과 연결할 수 있습니다. 사용자가 파일을 마우스 오른쪽 단추로 클릭하고 **연결 프로그램** 옵션을 선택하면 애플리케이션이 제안 목록에 표시됩니다.

#### <a name="xml-namespaces"></a>XML 네임스페이스

* `http://schemas.microsoft.com/appx/manifest/uap/windows10`
* `http://schemas.microsoft.com/appx/manifest/uap/windows10/3`

#### <a name="elements-and-attributes-of-this-extension"></a>이 확장의 요소 및 특성

```XML
<Extension Category="windows.fileTypeAssociation">
    <FileTypeAssociation Name="[Name]">
        <SupportedFileTypes>
            <FileType>"[file extension]"</FileType>
        </SupportedFileTypes>
    </FileTypeAssociation>
</Extension>
```

전체 스키마 참조는 [여기](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation)서 찾을 수 있습니다.

|이름 |설명 |
|-------|-------------|
|범주 |항상 ``windows.fileTypeAssociation``입니다.
|이름 | 파일 형식 연결의 이름입니다. 이 이름은 파일 형식을 구성하고 그룹화하는 데 사용할 수 있습니다. 이름은 공백 없이 모두 소문자여야 합니다.   |
|FileType |앱에서 지원하는 파일 확장명입니다. |

#### <a name="example"></a>예제

```XML
<Package
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  IgnorableNamespaces="uap, uap3">
  <Applications>
    <Application>
      <Extensions>
        <uap:Extension Category="windows.fileTypeAssociation">
          <uap3:FileTypeAssociation Name="mediafiles">
            <uap:SupportedFileTypes>
            <uap:FileType>.avi</uap:FileType>
            </uap:SupportedFileTypes>
          </uap3:FileTypeAssociation>
        </uap:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

#### <a name="related-sample"></a>관련 샘플

[transition/migration/uninstallation 경로가 포함된 WPF 사진 뷰어](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/DesktopAppTransition)

<a id="add"></a>

### <a name="add-options-to-the-context-menus-of-files-that-have-a-certain-file-type"></a>특정 형식의 파일 상황에 맞는 메뉴에 옵션 추가

대부분의 경우 사용자는 파일을 마우스 오른쪽 단추로 클릭해서 엽니다. 사용자가 파일을 마우스 오른쪽 단추로 클릭하면 다양한 옵션이 표시됩니다.

해당 메뉴에 옵션을 추가할 수 있습니다. 이러한 옵션은 인쇄, 편집, 파일 미리 보기처럼 파일과 상호 작용하는 방법을 사용자에게 제공합니다.

#### <a name="xml-namespaces"></a>XML 네임스페이스

* `http://schemas.microsoft.com/appx/manifest/uap/windows10`
* `http://schemas.microsoft.com/appx/manifest/uap/windows10/2`
* `http://schemas.microsoft.com/appx/manifest/uap/windows10/3`

#### <a name="elements-and-attributes-of-this-extension"></a>이 확장의 요소 및 특성

```XML
<Extension Category="windows.fileTypeAssociation">
    <FileTypeAssociation Name="[Name]">
        <SupportedVerbs>
           <Verb Id="[ID]" Extended="[Extended]" Parameters="[parameters]">"[verb label]"</Verb>
        </SupportedVerbs>
    </FileTypeAssociation>
</Extension>
```

전체 스키마 참조는 [여기](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation)서 찾을 수 있습니다.

|이름 |설명 |
|-------|-------------|
|범주 | 항상 ``windows.fileTypeAssociation``입니다.
|이름 |파일 형식 연결의 이름입니다. 이 이름은 파일 형식을 구성하고 그룹화하는 데 사용할 수 있습니다. 이름은 공백 없이 모두 소문자여야 합니다. |
|동사 |파일 탐색기 상황에 맞는 메뉴에 표시되는 이름입니다. 이 문자열은 ```ms-resource```를 사용하여 지역화할 수 있습니다.|
|Id |동사의 고유 ID입니다. 애플리케이션이 UWP 앱인 경우 사용자의 선택을 적절하게 처리할 수 있도록 이 ID가 활성화 이벤트 인수의 일부로 앱에 전달됩니다. 애플리케이션이 완전히 신뢰할 수 있는 패키지된 앱인 경우 앱은 이 ID 대신 매개 변수를 받습니다(다음 항목 참조). |
|매개 변수 |동사와 연관된 인수 매개 변수 및 값의 목록입니다. 애플리케이션이 완전히 신뢰할 수 있는 패키지된 앱인 경우 애플리케이션이 활성화될 때 이러한 매개 변수가 애플리케이션에 이벤트 인수로 전달됩니다. 다양한 활성화 동사에 따라 애플리케이션의 동작을 사용자 지정할 수 있습니다. 변수가 파일 경로를 포함할 수 있는 경우에는 매개 변수 값을 따옴표로 묶습니다. 이렇게 해야 경로에 공백이 포함된 경우에 발생하는 모든 문제를 방지할 수 있습니다. 애플리케이션이 UWP 앱인 경우에는 매개 변수를 전달할 수 없습니다. 앱은 그 대신 해당 ID를 수신합니다(이전 항목 참조).|
|확장 |사용자가 파일을 마우스 오른쪽 단추로 클릭하기 전에 **Shift** 키를 길게 눌러 상황에 맞는 메뉴를 표시하는 경우에만 동사 메뉴가 표시되도록 지정합니다. 이 특성은 선택 사항이며, 목록에 없으면 기본값은 **False**입니다(예: 항상 동사를 표시). 각 동사에 대해 개별적으로 이 동작을 지정합니다(항상 **False**인 "열기"는 예외).|

#### <a name="example"></a>예제

```XML
<Package
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:uap2="http://schemas.microsoft.com/appx/manifest/uap/windows10/2"
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"

  IgnorableNamespaces="uap, uap2, uap3">
  <Applications>
    <Application>
      <Extensions>
        <uap:Extension Category="windows.fileTypeAssociation">
          <uap3:FileTypeAssociation Name="myfiletypes">
            <uap2:SupportedVerbs>
              <uap3:Verb Id="Edit" Parameters="/e &quot;%1&quot;">Edit</uap3:Verb>
              <uap3:Verb Id="Print" Extended="true" Parameters="/p &quot;%1&quot;">Print</uap3:Verb>
            </uap2:SupportedVerbs>
          </uap3:FileTypeAssociation>
        </uap:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

#### <a name="related-sample"></a>관련 샘플

[transition/migration/uninstallation 경로가 포함된 WPF 사진 뷰어](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/DesktopAppTransition)

<a id="open"></a>

### <a name="open-certain-types-of-files-directly-by-using-a-url"></a>URL을 사용하여 특정 형식의 파일을 직접 열기

특정 파일 형식에서는 사용자가 기본적으로 데스크톱 버전의 앱 대신 새 패키지된 애플리케이션을 열도록 지정할 수 있습니다.

#### <a name="xml-namespaces"></a>XML 네임스페이스

* `http://schemas.microsoft.com/appx/manifest/uap/windows10`
* `http://schemas.microsoft.com/appx/manifest/uap/windows10/3`

#### <a name="elements-and-attributes-of-this-extension"></a>이 확장의 요소 및 특성

```XML
<Extension Category="windows.fileTypeAssociation">
    <FileTypeAssociation Name="[Name]" UseUrl="true" Parameters="%1">
        <SupportedFileTypes>
            <FileType>"[FileExtension]"</FileType>
        </SupportedFileTypes>
    </FileTypeAssociation>
</Extension>
```

전체 스키마 참조는 [여기](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation)서 찾을 수 있습니다.

|이름 |설명 |
|-------|-------------|
|범주 |항상 ``windows.fileTypeAssociation``입니다.
|이름 |파일 형식 연결의 이름입니다. 이 이름은 파일 형식을 구성하고 그룹화하는 데 사용할 수 있습니다. 이름은 공백 없이 모두 소문자여야 합니다. |
|UseUrl |URL 대상에서 직접 파일을 열 것인지 여부를 나타냅니다. 이 값을 설정하지 않으면 애플리케이션이 URL을 사용하여 파일을 열려고 시도할 때 시스템에서 먼저 파일을 로컬로 다운로드합니다. |
|매개 변수 | 선택적 매개 변수입니다. |
|FileType |관련 파일 확장명입니다. |

#### <a name="example"></a>예제

```XML
<Package
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  IgnorableNamespaces="uap, uap3">
  <Applications>
      <Application>
        <Extensions>
          <uap:Extension Category="windows.fileTypeAssociation">
            <uap3:FileTypeAssociation Name="myfiletypes" UseUrl="true" Parameters="%1">
              <uap:SupportedFileTypes>
                <uap:FileType>.txt</uap:FileType>
                <uap:FileType>.doc</uap:FileType>
              </uap:SupportedFileTypes>
            </uap3:FileTypeAssociation>
          </uap:Extension>
        </Extensions>
      </Application>
    </Applications>
</Package>
```

## <a name="perform-setup-tasks"></a>설치 작업 수행

* [앱의 예외 방화벽 만들기](#rules)
* [패키지의 원하는 폴더에 DLL 파일 배치](#load-paths)

<a id="rules"></a>

### <a name="create-firewall-exception-for-your-app"></a>앱의 예외 방화벽 만들기

애플리케이션에서 포트를 통해 통신해야 하는 경우 애플리케이션을 방화벽 예외 목록에 추가하면 됩니다.

#### <a name="xml-namespace"></a>XML 네임스페이스

`http://schemas.microsoft.com/appx/manifest/desktop/windows10/2`

#### <a name="elements-and-attributes-of-this-extension"></a>이 확장의 요소 및 특성

```XML
<Extension Category="windows.firewallRules">
  <FirewallRules Executable="[executable file name]">
    <Rule
      Direction="[Direction]"
      IPProtocol="[Protocol]"
      LocalPortMin="[LocalPortMin]"
      LocalPortMax="LocalPortMax"
      RemotePortMin="RemotePortMin"
      RemotePortMax="RemotePortMax"
      Profile="[Profile]"/>
  </FirewallRules>
</Extension>
```

전체 스키마 참조는 [여기](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-desktop2-firewallrules)서 찾을 수 있습니다.

|이름 |설명 |
|-------|-------------|
|범주 |항상 ``windows.firewallRules``입니다.|
|실행 파일 |방화벽 예외 목록에 추가하려는 실행 파일의 이름입니다. |
|방향 |인바운드 규칙인지 아니면 아웃바운드 규칙인지 나타냅니다. |
|IPProtocol |통신 프로토콜 |
|LocalPortMin |로컬 포트 번호의 범위 내에 있는 하위 포트 번호입니다. |
|LocalPortMax |로컬 포트 번호의 범위 내에 있는 최상위 포트 번호입니다 |
|RemotePortMax |원격 포트 번호의 범위 내에 있는 하위 포트 번호입니다. |
|RemotePortMax |원격 포트 번호의 범위 내에 있는 최상위 포트 번호입니다. |
|프로필 |네트워크 종류 |

#### <a name="example"></a>예제

```XML
<Package
  xmlns:desktop2="http://schemas.microsoft.com/appx/manifest/desktop/windows10/2"
  IgnorableNamespaces="desktop2">
  <Extensions>
    <desktop2:Extension Category="windows.firewallRules">
      <desktop2:FirewallRules Executable="Contoso.exe">
          <desktop2:Rule Direction="in" IPProtocol="TCP" Profile="all"/>
          <desktop2:Rule Direction="in" IPProtocol="UDP" LocalPortMin="1337" LocalPortMax="1338" Profile="domain"/>
          <desktop2:Rule Direction="in" IPProtocol="UDP" LocalPortMin="1337" LocalPortMax="1338" Profile="public"/>
          <desktop2:Rule Direction="out" IPProtocol="UDP" LocalPortMin="1339" LocalPortMax="1340" RemotePortMin="15"
                         RemotePortMax="19" Profile="domainAndPrivate"/>
          <desktop2:Rule Direction="out" IPProtocol="GRE" Profile="private"/>
      </desktop2:FirewallRules>
  </desktop2:Extension>
</Extensions>
</Package>
```

<a id="load-paths"></a>

### <a name="place-your-dll-files-into-any-folder-of-the-package"></a>패키지의 원하는 폴더에 DLL 파일 배치

이러한 폴더를 식별할 수 있도록 확장을 사용합니다. 이 방법으로 시스템에서는 사용자가 배치하는 파일을 찾아 로드할 수 있습니다. 이 확장이 _%PATH%_ 환경 변수를 대체한다고 생각하시면 됩니다.

이 확장을 사용하지 않으면 시스템에서는 프로세스의 패키지 종속성 그래프, 패키지 루트 폴더, 시스템 디렉터리( _%SystemRoot%\system32_)를 순서대로 검색합니다. 자세한 정보는 [Windows 앱의 검색 순서](https://docs.microsoft.com/windows/desktop/Dlls/dynamic-link-library-search-order)를 참조하세요.

각 패키지는 이러한 확장 중 하나만 포함할 수 있습니다. 따라서 이 중 하나를 주 패키지에 추가한 다음, 하나를 각 [선택적 패키지 및 관련 세트](/windows/msix/package/optional-packages)에 추가할 수 있습니다.

#### <a name="xml-namespace"></a>XML 네임스페이스

`http://schemas.microsoft.com/appx/manifest/uap/windows10/6`

#### <a name="elements-and-attributes-of-this-extension"></a>이 확장의 요소 및 특성

앱 매니페스트의 패키지 수준에서 이 확장을 선언합니다.

```XML
<Extension Category="windows.loaderSearchPathOverride">
  <LoaderSearchPathOverride>
    <LoaderSearchPathEntry FolderPath="[path]"/>
  </LoaderSearchPathOverride>
</Extension>

```

|이름 | 설명 |
|-------|-------------|
|범주 |항상 ``windows.loaderSearchPathOverride``입니다.
|FolderPath | dll 파일을 포함하는 폴더의 경로입니다. 패키지의 루트 폴더와 관련된 경로를 지정합니다. 한 확장에서 최대 5개의 경로를 지정할 수 있습니다. 시스템이 패키지의 루트 폴더에서 파일을 검색하려는 경우 이러한 경로 중 하나에 대해 빈 문자열을 사용합니다. 중복 경로를 포함하면 안 되며, 경로가 선행 또는 후행 슬래시 또는 백슬래시를 포함하지 않도록 해야 합니다. <br><br> 시스템은 하위 폴더를 검색하지 않으므로 시스템에서 로드하려는 DLL 파일을 포함하는 각 폴더를 명시적으로 나열해야 합니다.|

#### <a name="example"></a>예제

```XML
<Package
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/6"
  IgnorableNamespaces="uap6">
  ...
    <Extensions>
      <uap6:Extension Category="windows.loaderSearchPathOverride">
        <uap6:LoaderSearchPathOverride>
          <uap6:LoaderSearchPathEntry FolderPath=""/>
          <uap6:LoaderSearchPathEntry FolderPath="folder1/subfolder1"/>
          <uap6:LoaderSearchPathEntry FolderPath="folder2/subfolder2"/>
        </uap6:LoaderSearchPathOverride>
      </uap6:Extension>
    </Extensions>
...
</Package>
```

## <a name="integrate-with-file-explorer"></a>파일 탐색기와 통합

사용자가 파일을 구성하고 익숙한 방식으로 상호 작용하는 데 도움이 됩니다.

* [사용자가 여러 파일을 선택하여 동시에 열 때 애플리케이션의 동작 정의](#define)
* [파일 탐색기 내의 미리 보기 이미지에 파일 내용 표시](#show)
* [파일 탐색기의 미리 보기 창에 파일 내용 표시](#preview)
* [사용자가 파일 탐색기에서 [종류] 열을 사용하여 파일을 그룹화할 수 있도록 설정](#enable)
* [검색, 색인, 속성 대화 상자, 세부 정보 창에서 파일 속성을 사용할 수 있도록 설정](#make-file-properties)
* [파일 형식에 대한 상황에 맞는 메뉴 처리기 지정](#context-menu)
* [클라우드 서비스의 파일을 파일 탐색기에 표시](#cloud-files)

<a id="define"></a>

### <a name="define-how-your-application-behaves-when-users-select-and-open-multiple-files-at-the-same-time"></a>사용자가 여러 파일을 선택하여 동시에 열 때 애플리케이션의 동작 정의

사용자가 여러 파일을 동시에 열 때 애플리케이션의 동작을 지정합니다.

#### <a name="xml-namespaces"></a>XML 네임스페이스

* `http://schemas.microsoft.com/appx/manifest/uap/windows10`
* `http://schemas.microsoft.com/appx/manifest/uap/windows10/2`
* `http://schemas.microsoft.com/appx/manifest/uap/windows10/3`

#### <a name="elements-and-attributes-of-this-extension"></a>이 확장의 요소 및 특성

```XML
<Extension Category="windows.fileTypeAssociation">
    <FileTypeAssociation Name="[Name]" MultiSelectModel="[SelectionModel]">
        <SupportedVerbs>
            <Verb Id="Edit" MultiSelectModel="[SelectionModel]">Edit</Verb>
        </SupportedVerbs>
        <SupportedFileTypes>
            <FileType>"[FileExtension]"</FileType>
        </SupportedFileTypes>
</Extension>
```

전체 스키마 참조는 [여기](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation)서 찾을 수 있습니다.

|이름 |설명 |
|-------|-------------|
|범주 |항상 ``windows.fileTypeAssociation``입니다.
|이름 |파일 형식 연결의 이름입니다. 이 이름은 파일 형식을 구성하고 그룹화하는 데 사용할 수 있습니다. 이름은 공백 없이 모두 소문자여야 합니다. |
|MultiSelectModel |아래 참조 |
|FileType |관련 파일 확장명입니다. |

**MultiSelectModel**

패키지된 데스크톱 앱은 일반 데스크톱 앱과 동일한 세 가지 옵션을 제공합니다.

* ``Player``: 애플리케이션이 한 번 활성화됩니다. 선택한 모든 파일이 애플리케이션에 인수 매개 변수로 전달됩니다.
* ``Single``: 처음 선택한 파일에 대해 애플리케이션이 한 번 활성화됩니다. 다른 파일은 무시됩니다.
* ``Document``: 애플리케이션의 새로운 별도 인스턴스가 선택한 각 파일에 대해 활성화됩니다.

 다른 파일 형식 및 작업에 대해 다른 기본 설정을 설정할 수 있습니다. 예를 들어 *Document* 모드로 *문서*를, *Player* 모드로 *이미지*를 열 수 있습니다.

#### <a name="example"></a>예제

```XML
<Package
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:uap2="http://schemas.microsoft.com/appx/manifest/uap/windows10/2"
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  IgnorableNamespaces="uap, uap2, uap3">
  <Applications>
    <Application>
      <Extensions>
        <uap:Extension Category="windows.fileTypeAssociation">
          <uap3:FileTypeAssociation Name="myfiletypes" MultiSelectModel="Document">
            <uap2:SupportedVerbs>
              <uap3:Verb Id="Edit" MultiSelectModel="Player">Edit</uap3:Verb>
              <uap3:Verb Id="Preview" MultiSelectModel="Document">Preview</uap3:Verb>
            </uap2:SupportedVerbs>
            <uap:SupportedFileTypes>
              <uap:FileType>.txt</uap:FileType>
            </uap:SupportedFileTypes>
        </uap:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

사용자가 15개 이하의 파일을 열 경우 **MultiSelectModel** 특성에 대한 기본 설정은 *Player*입니다. 그렇지 않은 경우에는 *Document*가 기본 설정입니다. UWP 앱은 항상 *Player*로 시작됩니다.

<a id="show"></a>

### <a name="show-file-contents-in-a-thumbnail-image-within-file-explorer"></a>파일 탐색기 내의 미리 보기 이미지에 파일 내용 표시

파일의 아이콘이 중간, 대형 또는 초대형 크기로 표시될 때 사용자가 파일 내용의 미리 보기 이미지를 볼 수 있도록 합니다.

#### <a name="xml-namespace"></a>XML 네임스페이스

* `http://schemas.microsoft.com/appx/manifest/uap/windows10`
* `http://schemas.microsoft.com/appx/manifest/uap/windows10/2`
* `http://schemas.microsoft.com/appx/manifest/uap/windows10/3`
* `http://schemas.microsoft.com/appx/manifest/desktop/windows10/2`

#### <a name="elements-and-attributes-of-this-extension"></a>이 확장의 요소 및 특성

```XML
<Extension Category="windows.fileTypeAssociation">
    <FileTypeAssociation Name="[Name]">
        <SupportedFileTypes>
            <FileType>"[FileExtension]"</FileType>
        </SupportedFileTypes>
        <ThumbnailHandler
            Clsid  ="[Clsid  ]" />
    </FileTypeAssociation>
</Extension>
```

전체 스키마 참조는 [여기](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation)서 찾을 수 있습니다.

|이름 |설명 |
|-------|-------------|
|범주 |항상 ``windows.fileTypeAssociation``입니다.
|이름 |파일 형식 연결의 이름입니다. 이 이름은 파일 형식을 구성하고 그룹화하는 데 사용할 수 있습니다. 이름은 공백 없이 모두 소문자여야 합니다. |
|FileType |관련 파일 확장명입니다. |
|Clsid   |앱의 클래스 ID입니다. |

#### <a name="example"></a>예제

```XML
<Package
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:uap2="http://schemas.microsoft.com/appx/manifest/uap/windows10/2"
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  xmlns:desktop2="http://schemas.microsoft.com/appx/manifest/desktop/windows10/2"
  IgnorableNamespaces="uap, uap2, uap3, desktop2">
  <Applications>
    <Application>
      <Extensions>
        <uap:Extension Category="windows.fileTypeAssociation">
          <uap3:FileTypeAssociation Name="myfiletypes">
            <uap2:SupportedFileTypes>
              <uap:FileType>.bar</uap:FileType>
            </uap2:SupportedFileTypes>
            <desktop2:ThumbnailHandler
              Clsid  ="20000000-0000-0000-0000-000000000001"  />
            </uap3:FileTypeAssociation>
         </uap::Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

<a id="preview"></a>

### <a name="show-file-contents-in-the-preview-pane-of-file-explorer"></a>파일 탐색기의 미리 보기 창에 파일 내용 표시

사용자가 파일 탐색기의 미리 보기 창에서 파일 내용을 미리 볼 수 있도록 합니다.

#### <a name="xml-namespace"></a>XML 네임스페이스

* `http://schemas.microsoft.com/appx/manifest/uap/windows10`
* `http://schemas.microsoft.com/appx/manifest/uap/windows10/2`
* `http://schemas.microsoft.com/appx/manifest/uap/windows10/3`
* `http://schemas.microsoft.com/appx/manifest/desktop/windows10/2`

#### <a name="elements-and-attributes-of-this-extension"></a>이 확장의 요소 및 특성

```XML
<Extension Category="windows.fileTypeAssociation">
    <FileTypeAssociation Name="[Name]">
        <SupportedFileTypes>
            <FileType>"[FileExtension]"</FileType>
        </SupportedFileTypes>
        <DesktopPreviewHandler Clsid  ="[Clsid  ]" />
    </FileTypeAssociation>
</Extension>
```

전체 스키마 참조는 [여기](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation)서 찾을 수 있습니다.

|이름 |설명 |
|-------|-------------|
|범주 |항상 ``windows.fileTypeAssociation``입니다.
|이름 |파일 형식 연결의 이름입니다. 이 이름은 파일 형식을 구성하고 그룹화하는 데 사용할 수 있습니다. 이름은 공백 없이 모두 소문자여야 합니다. |
|FileType |관련 파일 확장명입니다. |
|Clsid   |앱의 클래스 ID입니다. |

#### <a name="example"></a>예제

```XML
<Package
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:uap2="http://schemas.microsoft.com/appx/manifest/uap/windows10/2"
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  xmlns:desktop2="http://schemas.microsoft.com/appx/manifest/desktop/windows10/2"
  IgnorableNamespaces="uap, uap2, uap3, desktop2">
  <Applications>
    <Application>
      <Extensions>
        <uap:Extension Category="windows.fileTypeAssociation">
          <uap3:FileTypeAssociation Name="myfiletypes">
            <uap2SupportedFileTypes>
              <uap:FileType>.bar</uap:FileType>
                </uap2SupportedFileTypes>
              <desktop2:DesktopPreviewHandler Clsid ="20000000-0000-0000-0000-000000000001" />
           </uap3:FileTypeAssociation>
        </uap:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

<a id="enable"></a>

### <a name="enable-users-to-group-files-by-using-the-kind-column-in-file-explorer"></a>사용자가 파일 탐색기에서 [종류] 열을 사용하여 파일을 그룹화할 수 있도록 설정

파일 형식에 대한 하나 이상의 미리 정의된 값을 **종류** 필드와 연결할 수 있습니다.

파일 탐색기에서 사용자는 이 필드를 사용하여 파일을 그룹화할 수 있습니다. 또한 시스템 구성 요소는 색인 같은 다양한 용도로 이 필드를 사용합니다.

**종류** 필드와 이 필드에서 사용할 수 있는 값에 대한 자세한 내용은 [종류 이름 사용하기](https://docs.microsoft.com/windows/desktop/properties/building-property-handlers-user-friendly-kind-names)를 참조하세요.

#### <a name="xml-namespaces"></a>XML 네임스페이스

* `http://schemas.microsoft.com/appx/manifest/uap/windows10`
* `http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities/3`

#### <a name="elements-and-attributes-of-this-extension"></a>이 확장의 요소 및 특성

```XML
<Extension Category="windows.fileTypeAssociation">
    <FileTypeAssociation Name="[Name]">
        <SupportedFileTypes>
            <FileType>"[FileExtension]"</FileType>
        </SupportedFileTypes>
        <KindMap>
            <Kind value="[KindValue]">
        </KindMap>
    </FileTypeAssociation>
</Extension>
```

전체 스키마 참조는 [여기](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation)서 찾을 수 있습니다.

|이름 |설명 |
|-------|-------------|
|범주 |항상 ``windows.fileTypeAssociation``입니다.
|이름 |파일 형식 연결의 이름입니다. 이 이름은 파일 형식을 구성하고 그룹화하는 데 사용할 수 있습니다. 이름은 공백 없이 모두 소문자여야 합니다. |
|FileType |관련 파일 확장명입니다. |
|value |유효한 [종류 값](https://docs.microsoft.com/windows/desktop/properties/building-property-handlers-user-friendly-kind-names)입니다. |

#### <a name="example"></a>예제

```XML
<Package
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities/3"
  IgnorableNamespaces="uap, rescap">
  <Applications>
    <Application>
      <Extensions>
        <uap:Extension Category="windows.fileTypeAssociation">
           <uap:FileTypeAssociation Name="mediafiles">
             <uap:SupportedFileTypes>
               <uap:FileType>.m4a</uap:FileType>
               <uap:FileType>.mta</uap:FileType>
             </uap:SupportedFileTypes>
             <rescap:KindMap>
               <rescap:Kind value="Item">
               <rescap:Kind value="Communications">
               <rescap:Kind value="Task">
             </rescap:KindMap>
          </uap:FileTypeAssociation>
      </uap:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

<a id="make-file-properties"></a>

### <a name="make-file-properties-available-to-search-index-property-dialogs-and-the-details-pane"></a>검색, 색인, 속성 대화 상자, 세부 정보 창에서 파일 속성을 사용할 수 있도록 설정

#### <a name="xml-namespace"></a>XML 네임스페이스

* `http://schemas.microsoft.com/appx/manifest/uap/windows10`
* `http://schemas.microsoft.com/appx/manifest/uap/windows10/3`
* `http://schemas.microsoft.com/appx/manifest/desktop/windows10/2`

#### <a name="elements-and-attributes-of-this-extension"></a>이 확장의 요소 및 특성

```XML
<uap:Extension Category="windows.fileTypeAssociation">
    <uap:FileTypeAssociation Name="[Name]">
        <SupportedFileTypes>
            <FileType>.bar</FileType>
        </SupportedFileTypes>
        <DesktopPropertyHandler Clsid ="[Clsid]"/>
    </uap:FileTypeAssociation>
</uap:Extension>
```

전체 스키마 참조는 [여기](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation)서 찾을 수 있습니다.

|이름 |설명 |
|-------|-------------|
|범주 |항상 ``windows.fileTypeAssociation``입니다.
|이름 |파일 형식 연결의 이름입니다. 이 이름은 파일 형식을 구성하고 그룹화하는 데 사용할 수 있습니다. 이름은 공백 없이 모두 소문자여야 합니다. |
|FileType |관련 파일 확장명입니다. |
|Clsid  |앱의 클래스 ID입니다. |

#### <a name="example"></a>예제

```XML
<Package
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  xmlns:desktop2="http://schemas.microsoft.com/appx/manifest/desktop/windows10/2"
  IgnorableNamespaces="uap, uap3, desktop2">
  <Applications>
    <Application>
      <Extensions>
        <uap:Extension Category="windows.fileTypeAssociation">
          <uap3:FileTypeAssociation Name="myfiletypes">
            <uap:SupportedFileTypes>
              <uap:FileType>.bar</uap:FileType>
            </uap:SupportedFileTypes>
            <desktop2:DesktopPropertyHandler Clsid ="20000000-0000-0000-0000-000000000001"/>
          </uap3:FileTypeAssociation>
        </uap:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

<a id="context-menu"></a>

### <a name="specify-a-context-menu-handler-for-a-file-type"></a>파일 형식에 대한 상황에 맞는 메뉴 처리기 지정

데스크톱 애플리케이션에서 [상황에 맞는 메뉴 처리기](https://docs.microsoft.com/windows/desktop/shell/context-menu-handlers)를 정의하는 경우 이 확장을 사용하여 메뉴 처리기를 등록합니다.

#### <a name="xml-namespaces"></a>XML 네임스페이스

* `http://schemas.microsoft.com/appx/manifest/foundation/windows10`
* `http://schemas.microsoft.com/appx/manifest/desktop/windows10/4`

#### <a name="elements-and-attributes-of-this-extension"></a>이 확장의 요소 및 특성

```XML
<Extensions>
    <com:Extension Category="windows.comServer">
        <com:ComServer>
            <com:SurrogateServer AppId="[AppID]" DisplayName="[DisplayName]">
                <com:Class Id="[Clsid]" Path="[Path]" ThreadingModel="[Model]"/>
            </com:SurrogateServer>
        </com:ComServer>
    </com:Extension>
    <desktop4:Extension Category="windows.fileExplorerContextMenus">
        <desktop4:FileExplorerContextMenus>
            <desktop4:ItemType Type="[Type]">
                <desktop4:Verb Id="[ID]" Clsid="[Clsid]" />
            </desktop4:ItemType>
        </desktop4:FileExplorerContextMenus>
    </desktop4:Extension>
</Extensions>
```

[com:ComServer](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-com-comserver) 및 [desktop4:FileExplorerContextMenus](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-desktop4-fileexplorercontextmenus)에서 전체 스키마 참조를 찾아보세요.

#### <a name="instructions"></a>지침

상황에 맞는 메뉴 처리기를 등록하려면 다음 지침을 따르세요.

1. 데스크톱 애플리케이션에서 [IExplorerCommand](https://docs.microsoft.com/windows/desktop/api/shobjidl_core/nn-shobjidl_core-iexplorercommand) 또는 [IExplorerCommandState](https://docs.microsoft.com/windows/desktop/api/shobjidl_core/nn-shobjidl_core-iexplorercommandstate) 인터페이스를 구현하여 [상황에 맞는 메뉴 처리기](https://docs.microsoft.com/windows/desktop/shell/context-menu-handlers)를 구현합니다. 샘플은 [ExplorerCommandVerb](https://github.com/microsoft/Windows-classic-samples/tree/master/Samples/Win7Samples/winui/shell/appshellintegration/ExplorerCommandVerb) 코드 샘플을 참조하세요. 각 구현 개체의 클래스 GUID를 정의해야 합니다. 예를 들어 다음 코드는 [IExplorerCommand](https://docs.microsoft.com/windows/desktop/api/shobjidl_core/nn-shobjidl_core-iexplorercommand) 구현의 클래스 ID를 정의합니다.

    ```cpp
    class __declspec(uuid("d0c8bceb-28eb-49ae-bc68-454ae84d6264")) CExplorerCommandVerb;
    ```

2. 패키지 매니페스트에서 상황에 맞는 메뉴 처리기 구현의 클래스 ID를 사용하여 COM 서로게이트 서버를 등록하는 [com:ComServer](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-com-comserver) 애플리케이션 확장을 지정합니다.

    ```xml
    <com:Extension Category="windows.comServer">
        <com:ComServer>
            <com:SurrogateServer AppId="d0c8bceb-28eb-49ae-bc68-454ae84d6264" DisplayName="ContosoHandler">
                <com:Class Id="d0c8bceb-28eb-49ae-bc68-454ae84d6264" Path="ExplorerCommandVerb.dll" ThreadingModel="STA"/>
            </com:SurrogateServer>
        </com:ComServer>
    </com:Extension>
    ```

2. 패키지 매니페스트에서 상황에 맞는 메뉴 처리기 구현을 등록하는 [desktop4:FileExplorerContextMenus](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-desktop4-fileexplorercontextmenus) 애플리케이션 확장을 지정합니다.

    ```xml
    <desktop4:Extension Category="windows.fileExplorerContextMenus">
        <desktop4:FileExplorerContextMenus>
            <desktop4:ItemType Type=".rar">
                <desktop4:Verb Id="Command1" Clsid="d0c8bceb-28eb-49ae-bc68-454ae84d6264" />
            </desktop4:ItemType>
        </desktop4:FileExplorerContextMenus>
    </desktop4:Extension>
    ```

#### <a name="example"></a>예제

```XML
<Package
  xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
  xmlns:desktop4="http://schemas.microsoft.com/appx/manifest/desktop/windows10/4"
  IgnorableNamespaces="desktop4">
  <Applications>
    <Application>
      <Extensions>
        <com:Extension Category="windows.comServer">
          <com:ComServer>
            <com:SurrogateServer AppId="d0c8bceb-28eb-49ae-bc68-454ae84d6264" DisplayName="ContosoHandler"">
              <com:Class Id="Id="d0c8bceb-28eb-49ae-bc68-454ae84d6264" Path="ExplorerCommandVerb.dll" ThreadingModel="STA"/>
            </com:SurrogateServer>
          </com:ComServer>
        </com:Extension>
        <desktop4:Extension Category="windows.fileExplorerContextMenus">
          <desktop4:FileExplorerContextMenus>
            <desktop4:ItemType Type=".contoso">
              <desktop4:Verb Id="Command1" Clsid="d0c8bceb-28eb-49ae-bc68-454ae84d6264" />
            </desktop4:ItemType>
          </desktop4:FileExplorerContextMenus>
        </desktop4:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

<a id="cloud-files"></a>

### <a name="make-files-from-your-cloud-service-appear-in-file-explorer"></a>클라우드 서비스의 파일을 파일 탐색기에 표시

애플리케이션에서 구현하는 처리기를 등록합니다. 또한 사용자가 파일 탐색기에서 클라우드 기반 파일을 마우스 오른쪽 단추로 클릭하면 나타나는 상황에 맞는 메뉴 옵션을 추가할 수 있습니다.

#### <a name="xml-namespace"></a>XML 네임스페이스

* `http://schemas.microsoft.com/appx/manifest/desktop/windows10`

#### <a name="elements-and-attributes-of-this-extension"></a>이 확장의 요소 및 특성

```XML
<Extension Category="windows.cloudfiles" >
    <CloudFiles IconResource="[Icon]">
        <CustomStateHandler Clsid ="[Clsid]"/>
        <ThumbnailProviderHandler Clsid ="[Clsid]"/>
        <ExtendedPropertyhandler Clsid ="[Clsid]"/>
        <CloudFilesContextMenus>
            <Verb Id ="Command3" Clsid= "[GUID]">[Verb Label]</Verb>
        </CloudFilesContextMenus>
    </CloudFiles>
</Extension>

```

|이름 |설명 |
|-------|-------------|
|범주 |항상 ``windows.cloudfiles``입니다.
|iconResource |클라우드 파일 공급자 서비스를 나타내는 아이콘입니다. 이 아이콘은 파일 탐색기의 탐색 창에 표시됩니다.  사용자가 이 아이콘을 선택하면 클라우드 서비스의 파일이 표시됩니다. |
|CustomStateHandler Clsid |CustomStateHandler를 구현하는 애플리케이션의 클래스 ID입니다. 시스템에서는 이 클래스 ID를 사용하여 클라우드 파일에 대한 사용자 지정 상태 및 열을 요청합니다. |
|ThumbnailProviderHandler Clsid |ThumbnailProviderHandler를 구현하는 애플리케이션의 클래스 ID입니다. 시스템에서는 이 클래스 ID를 사용하여 클라우드 파일에 대한 미리 보기 이미지를 요청합니다. |
|ExtendedPropertyHandler Clsid |ExtendedPropertyHandler를 구현하는 애플리케이션의 클래스 ID입니다.  시스템에서는 이 클래스 ID를 사용하여 클라우드 파일에 대한 확장된 속성을 요청합니다. |
|동사 |클라우드 서비스에서 제공하는 파일의 파일 탐색기 상황에 맞는 메뉴에 표시되는 이름입니다. |
|Id |동사의 고유 ID입니다. |

#### <a name="example"></a>예제

```XML
<Package
    xmlns:desktop="http://schemas.microsoft.com/appx/manifest/desktop/windows10"
    IgnorableNamespaces="desktop">
  <Applications>
    <Application>
      <Extensions>
        <Extension Category="windows.cloudfiles" >
            <CloudFiles IconResource="images\Wide310x150Logo.png">
                <CustomStateHandler Clsid ="20000000-0000-0000-0000-000000000001"/>
                <ThumbnailProviderHandler Clsid ="20000000-0000-0000-0000-000000000001"/>
                <ExtendedPropertyhandler Clsid ="20000000-0000-0000-0000-000000000001"/>
                <desktop:CloudFilesContextMenus>
                    <desktop:Verb Id ="keep" Clsid=
                       "20000000-0000-0000-0000-000000000001">
                       Always keep on this device</desktop:Verb>
                </desktop:CloudFilesContextMenus>
            </CloudFiles>
          </Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

<a id="start"></a>

## <a name="start-your-application-in-different-ways"></a>다른 방법으로 애플리케이션을 시작합니다.

* [프로토콜을 사용하여 애플리케이션 시작](#protocol)
* [별칭을 사용하여 애플리케이션 시작](#alias)
* [사용자가 Windows에 로그인할 때 실행 파일 시작](#executable)
* [사용자가 디바이스를 PC에 연결할 때 애플리케이션을 시작할 수 있도록 설정](#autoplay)
* [Microsoft Store에서 업데이트를 받은 후 자동으로 다시 시작](#updates)

<a id="protocol"></a>

### <a name="start-your-application-by-using-a-protocol"></a>프로토콜을 사용하여 애플리케이션 시작

프로토콜 연결을 사용하면 다른 프로그램과 시스템 구성 요소를 패키지된 앱과 상호 운영할 수 있습니다. 프로토콜을 사용하여 패키지된 애플리케이션을 시작하면 해당 활성화 이벤트 인수에 전달할 특정 매개 변수를 지정할 수 있으므로 앱이 적절하게 동작할 수 있습니다. 매개 변수는 완전히 신뢰할 수 있는 패키지된 앱에서만 지원됩니다. UWP 앱은 매개 변수를 사용할 수 없습니다.

#### <a name="xml-namespace"></a>XML 네임스페이스

`http://schemas.microsoft.com/appx/manifest/uap/windows10/3`

#### <a name="elements-and-attributes-of-this-extension"></a>이 확장의 요소 및 특성

```XML
<Extension
    Category="windows.protocol">
  <Protocol
      Name="[Protocol name]"
      Parameters="[Parameters]" />
</Extension>
```

전체 스키마 참조는 [여기](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap-protocol)서 찾을 수 있습니다.

|이름 |설명 |
|-------|-------------|
|범주 |항상 ``windows.protocol``입니다.
|이름 |프로토콜 이름입니다. |
|매개 변수 |애플리케이션이 활성화될 때 애플리케이션에 이벤트 인수로 전달할 매개 변수 및 값의 목록입니다. 변수가 파일 경로를 포함할 수 있는 경우에는 매개 변수 값을 따옴표로 묶습니다. 이렇게 해야 경로에 공백이 포함된 경우에 발생하는 모든 문제를 방지할 수 있습니다. |

### <a name="example"></a>예제

```XML
<Package
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  xmlns:desktop="http://schemas.microsoft.com/appx/manifest/desktop/windows10"
  IgnorableNamespaces="uap3, desktop">
  <Applications>
    <Application>
      <Extensions>
        <uap3:Extension
          Category="windows.protocol">
          <uap3:Protocol
            Name="myapp-cmd"
            Parameters="/p &quot;%1&quot;" />
        </uap3:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

<a id="alias"></a>

### <a name="start-your-application-by-using-an-alias"></a>별칭을 사용하여 애플리케이션 시작

사용자 및 기타 프로세스는 앱의 전체 경로를 지정할 필요 없이 별칭을 사용하여 애플리케이션을 시작할 수 있습니다. 이 별칭 이름을 지정할 수 있습니다.

#### <a name="xml-namespaces"></a>XML 네임스페이스

* `http://schemas.microsoft.com/appx/manifest/uap/windows10/3`
* `http://schemas.microsoft.com/appx/manifest/desktop/windows10`

#### <a name="elements-and-attributes-of-this-extension"></a>이 확장의 요소 및 특성

```XML
<Extension
    Category="windows.appExecutionAlias"
    Executable="[ExecutableName]"
    EntryPoint="Windows.FullTrustApplication">
    <AppExecutionAlias>
        <desktop:ExecutionAlias Alias="[AliasName]" />
    </AppExecutionAlias>
</Extension>
```

|이름 |설명 |
|-------|-------------|
|범주 |항상 ``windows.appExecutionAlias``입니다.
|실행 파일 |별칭이 호출될 때 시작할 실행 파일의 상대 경로입니다. |
|별칭 |앱의 간단한 이름입니다. 항상 확장명이 ".exe"로 끝나야 합니다. 패키지의 각 애플리케이션에 대해 하나의 앱 실행 별칭만 지정할 수 있습니다. 여러 앱이 같은 별칭으로 등록되는 경우 시스템은 다른 앱에서 재정의할 가능성이 없는 고유한 별칭을 선택하기 위해 마지막에 등록된 항목을 호출합니다.
|

#### <a name="example"></a>예제

```XML
<Package
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  IgnorableNamespaces="uap3">
  <Applications>
    <Application>
      <Extensions>
         <uap3:Extension
                Category="windows.appExecutionAlias"
                Executable="exes\launcher.exe"
                EntryPoint="Windows.FullTrustApplication">
            <uap3:AppExecutionAlias>
                <desktop:ExecutionAlias Alias="Contoso.exe" />
            </uap3:AppExecutionAlias>
        </uap3:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

전체 스키마 참조는 [여기](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation)서 찾을 수 있습니다.

<a id="executable"></a>

### <a name="start-an-executable-file-when-users-log-into-windows"></a>사용자가 Windows에 로그인할 때 실행 파일 시작

시작 작업을 통해 사용자가 로그온할 때마다 애플리케이션에서 실행 파일을 자동으로 실행하도록 할 수 있습니다.

> [!NOTE]
> 사용자가 애플리케이션을 한 번 이상 시작해야만 이 시작 작업이 등록됩니다.

애플리케이션에서 시작 작업을 여러 개 선언할 수 있습니다. 각 작업은 독립적으로 시작됩니다. 모든 시작 작업은 작업 관리자의 **시작** 탭에 나타나며 앱 매니페스트에 지정된 이름과 해당 앱 아이콘이 함께 표시됩니다. 작업 관리자는 작업의 시작 영향을 자동으로 분석합니다.

사용자는 작업 관리자를 사용하여 수동으로 앱의 시작 작업을 비활성화할 수 있습니다. 사용자가 작업을 비활성화하면 프로그래밍 방식으로 다시 활성화할 수 없습니다.

#### <a name="xml-namespace"></a>XML 네임스페이스

`http://schemas.microsoft.com/appx/manifest/desktop/windows10`

#### <a name="elements-and-attributes-of-this-extension"></a>이 확장의 요소 및 특성

```XML
<Extension
    Category="windows.startupTask"
    Executable="[ExecutableName]"
    EntryPoint="Windows.FullTrustApplication">
  <StartupTask
      TaskId="[TaskID]"
      Enabled="true"
      DisplayName="[DisplayName]" />
</Extension>
```

|이름 |설명 |
|-------|-------------|
|범주 |항상 ``windows.startupTask``입니다.|
|실행 파일 |시작하려는 실행 파일의 상대 경로입니다. |
|TaskId |작업의 고유 식별자입니다. 이 식별자를 사용하여 애플리케이션에서 [Windows.ApplicationModel.StartupTask](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.StartupTask) 클래스의 API를 호출하여 시작 작업을 프로그래밍 방식으로 사용하거나 사용하지 않을 수 있습니다. |
|사용 |작업이 처음에 활성화 또는 비활성화 상태로 시작할 것인지를 나타냅니다. 사용하는 작업은 다음번에 사용자가 로그온할 때 실행됩니다(사용자가 사용하지 않도록 설정하지 않는 한). |
|DisplayName |작업 관리자에 표시되는 작업의 이름입니다. ```ms-resource```를 사용하여 이 문자열을 지역화할 수 있습니다. |

#### <a name="example"></a>예제

```XML
<Package
  xmlns:desktop="http://schemas.microsoft.com/appx/manifest/desktop/windows10"
  IgnorableNamespaces="desktop">
  <Applications>
    <Application>
      <Extensions>
        <desktop:Extension
          Category="windows.startupTask"
          Executable="bin\MyStartupTask.exe"
          EntryPoint="Windows.FullTrustApplication">
        <desktop:StartupTask
          TaskId="MyStartupTask"
          Enabled="true"
          DisplayName="My App Service" />
        </desktop:Extension>
      </Extensions>
    </Application>
  </Applications>
 </Package>
```

<a id="autoplay"></a>

### <a name="enable-users-to-start-your-application-when-they-connect-a-device-to-their-pc"></a>사용자가 디바이스를 PC에 연결할 때 애플리케이션을 시작할 수 있도록 설정

자동 실행을 사용하면 사용자가 디바이스를 PC에 연결할 때 애플리케이션을 옵션으로 제공할 수 있습니다.

#### <a name="xml-namespace"></a>XML 네임스페이스

`http://schemas.microsoft.com/appx/manifest/desktop/windows10/3`

#### <a name="elements-and-attributes-of-this-extension"></a>이 확장의 요소 및 특성

```XML
<Extension Category="windows.autoPlayHandler">
  <AutoPlayHandler>
    <InvokeAction ActionDisplayName="[action string]" ProviderDisplayName="[name of your app/service]">
      <Content ContentEvent="[Content event]" Verb="[any string]" DropTargetHandler="[Clsid]" />
      <Content ContentEvent="[Content event]" Verb="[any string]" Parameters="[Initialization parameter]"/>
      <Device DeviceEvent="[Device event]" HWEventHandler="[Clsid]" InitCmdLine="[Initialization parameter]"/>
    </InvokeAction>
  </AutoPlayHandler>
```

|이름 |설명 |
|-------|-------------|
|범주 |항상 ``windows.autoPlayHandler``입니다.
|ActionDisplayName |사용자가 PC에 연결하는 디바이스로 수행할 수 있는 작업을 나타내는 문자열(예: "파일 가져오기" 또는 "비디오 재생")입니다. |
|ProviderDisplayName | 애플리케이션이나 서비스를 나타내는 문자열(예: "Contoso 비디오 플레이어")입니다. |
|ContentEvent |사용자에게 ``ActionDisplayName`` 및 ``ProviderDisplayName``을 알리는 메시지에 표시되는 콘텐츠 이벤트의 이름입니다. 콘텐츠 이벤트는 카메라 메모리 카드, 썸 드라이브(thumb drive) 또는 DVD 같은 볼륨 디바이스가 PC에 삽입될 때 발생합니다. 이러한 이벤트의 전체 목록은 [여기](https://docs.microsoft.com/windows/uwp/launch-resume/auto-launching-with-autoplay#autoplay-event-reference)서 찾을 수 있습니다.  |
|동사 |동사 설정은 선택한 옵션에 대해 애플리케이션에 전달된 값을 식별합니다. 자동 실행 이벤트에 대해 여러 개의 시작 작업을 지정하고 동사 설정을 사용하여 사용자가 앱에 대해 선택한 옵션을 확인할 수 있습니다. 앱에 전달된 시작 이벤트 인수의 동사 속성을 확인하여 사용자가 선택한 옵션을 알 수 있습니다. [동사] 설정에는 예약된 [open]을 제외한 모든 값을 사용할 수 있습니다. |
|DropTargetHandler |[IDropTarget](https://docs.microsoft.com/dotnet/api/microsoft.visualstudio.ole.interop.idroptarget?view=visualstudiosdk-2017)을 구현하는 애플리케이션의 클래스 ID입니다. 이동식 미디어의 파일은 [IDropTarget](https://docs.microsoft.com/dotnet/api/microsoft.visualstudio.ole.interop.idroptarget?view=visualstudiosdk-2017) 구현의 [Drop](https://docs.microsoft.com/dotnet/api/microsoft.visualstudio.ole.interop.idroptarget.drop?view=visualstudiosdk-2017#Microsoft_VisualStudio_OLE_Interop_IDropTarget_Drop_Microsoft_VisualStudio_OLE_Interop_IDataObject_System_UInt32_Microsoft_VisualStudio_OLE_Interop_POINTL_System_UInt32__) 메서드에 전달됩니다.  |
|매개 변수 |모든 콘텐츠 이벤트에 대해 [IDropTarget](https://docs.microsoft.com/dotnet/api/microsoft.visualstudio.ole.interop.idroptarget?view=visualstudiosdk-2017) 인터페이스를 구현할 필요가 없습니다. 모든 콘텐츠 이벤트에 대해 [IDropTarget](https://docs.microsoft.com/dotnet/api/microsoft.visualstudio.ole.interop.idroptarget?view=visualstudiosdk-2017) 인터페이스를 구현하는 대신 명령줄 매개 변수를 제공할 수 있습니다. 이러한 이벤트의 경우 자동 실행이 해당 명령줄 매개 변수를 사용하여 애플리케이션을 시작합니다. 앱의 초기화 코드에서 매개 변수를 분석하여 이 매개 변수가 자동 실행에서 시작되었는지 확인한 다음, 사용자 지정 구현을 제공할 수 있습니다. |
|DeviceEvent |사용자에게 ``ActionDisplayName`` 및 ``ProviderDisplayName``을 알리는 메시지에 표시되는 디바이스 이벤트의 이름입니다. 디바이스 이벤트는 디바이스가 PC에 연결될 때 발생합니다. 디바이스 이벤트는 ``WPD`` 문자열로 시작하며 [여기](https://docs.microsoft.com/windows/uwp/launch-resume/auto-launching-with-autoplay#autoplay-event-reference)서 찾을 수 있습니다. |
|HWEventHandler |[IHWEventHandler](https://docs.microsoft.com/windows/desktop/api/shobjidl/nn-shobjidl-ihweventhandler)를 구현하는 애플리케이션의 클래스 ID입니다. |
|InitCmdLine |[IHWEventHandler](https://docs.microsoft.com/windows/desktop/api/shobjidl/nn-shobjidl-ihweventhandler) 인터페이스의 [Initialize](https://docs.microsoft.com/windows/desktop/api/shobjidl/nf-shobjidl-ihweventhandler-initialize) 메서드에 전달하려는 문자열 매개 변수입니다. |

### <a name="example"></a>예제

```XML
<Package
  xmlns:desktop3="http://schemas.microsoft.com/appx/manifest/desktop/windows10/3"
  IgnorableNamespaces="desktop3">
  <Applications>
    <Application>
      <Extensions>
        <desktop3:Extension Category="windows.autoPlayHandler">
          <desktop3:AutoPlayHandler>
            <desktop3:InvokeAction ActionDisplayName="Import my files" ProviderDisplayName="ms-resource:AutoPlayDisplayName">
              <desktop3:Content ContentEvent="ShowPicturesOnArrival" Verb="show" DropTargetHandler="CD041BAE-0DEA-4472-9B7B-C98043D26EA8"/>
              <desktop3:Content ContentEvent="PlayVideoFilesOnArrival" Verb="play" Parameters="%1" />
              <desktop3:Device DeviceEvent="WPD\ImageSource" HWEventHandler="CD041BAE-0DEA-4472-9B7B-C98043D26EA8" InitCmdLine="/autoplay"/>
            </desktop3:InvokeAction>
          </desktop3:AutoPlayHandler>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

<a id="updates"></a>

### <a name="restart-automatically-after-receiving-an-update-from-the-microsoft-store"></a>Microsoft Store에서 업데이트를 받은 후 자동으로 다시 시작

사용자가 업데이트를 설치할 때 애플리케이션이 열려 있으면 애플리케이션을 닫습니다.

업데이트 완료 후 애플리케이션을 다시 시작하려면 다시 시작하기를 원하는 모든 프로세스에서 [RegisterApplicationRestart](https://docs.microsoft.com/windows/desktop/api/winbase/nf-winbase-registerapplicationrestart) 기능을 호출합니다.

애플리케이션의 각 활성 창은 [WM_QUERYENDSESSION](https://docs.microsoft.com/windows/desktop/Shutdown/wm-queryendsession) 메시지를 수신합니다. 이 시점에서 애플리케이션은 필요한 경우 명령줄을 업데이트할 [RegisterApplicationRestart](https://docs.microsoft.com/windows/desktop/api/winbase/nf-winbase-registerapplicationrestart) 기능을 호출합니다.

애플리케이션의 각 창에서 [WM_ENDSESSION](https://docs.microsoft.com/windows/desktop/Shutdown/wm-endsession) 메시지를 수신하면 애플리케이션은 데이터를 저장한 후 종료됩니다.

>[!NOTE]
> 또한 활성 창은 애플리케이션이 [WM ENDSESSION](https://docs.microsoft.com/windows/desktop/Shutdown/wm-endsession)을 처리하지 않는 경우에도 [WM_CLOSE](https://docs.microsoft.com/windows/desktop/winmsg/wm-close) 메시지를 수신합니다.

이때 애플리케이션에서 30초 내에 프로세스를 종료하지 않으면 플랫폼에서 강제로 프로세스를 종료합니다.

업데이트가 완료되면 애플리케이션이 다시 시작됩니다.

## <a name="work-with-other-applications"></a>다른 애플리케이션 작업

다른 앱과 통합하고, 다른 프로세스를 시작하거나 정보를 공유합니다.

* [애플리케이션이 인쇄를 지원하는 애플리케이션에 인쇄 대상으로 표시되도록 설정](#printing)
* [다른 Windows 애플리케이션과 글꼴 공유](#fonts)
* [UWP(유니버설 Windows 플랫폼) 앱에서 Win32 프로세스 시작](#win32-process)

<a id="printing"></a>

### <a name="make-your-application-appear-as-the-print-target-in-applications-that-support-printing"></a>애플리케이션이 인쇄를 지원하는 애플리케이션에 인쇄 대상으로 표시되도록 설정

메모장 같은 다른 애플리케이션에서 데이터를 인쇄하고 싶은 경우 사용자는 앱을 가용 인쇄 대상 목록에 인쇄 대상으로 표시할 수 있습니다.

XPS(XML Paper Specification) 형식으로 인쇄 데이터를 수신하려면 애플리케이션을 수정해야 합니다.

#### <a name="xml-namespaces"></a>XML 네임스페이스

`http://schemas.microsoft.com/appx/manifest/desktop/windows10/2`

#### <a name="elements-and-attributes-of-this-extension"></a>이 확장의 요소 및 특성

```XML
<Extension Category="windows.appPrinter">
    <AppPrinter
        DisplayName="[DisplayName]"
        Parameters="[Parameters]" />
</Extension>
```

전체 스키마 참조는 [여기](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-desktop2-appprinter)서 찾을 수 있습니다.

|이름 |설명 |
|-------|-------------|
|범주 |항상 ``windows.appPrinter``입니다.
|DisplayName |앱의 인쇄 대상 목록에 표시할 이름입니다. |
|매개 변수 |애플리케이션이 요청을 적절히 처리하는 데 필요한 모든 매개 변수입니다. |

#### <a name="example"></a>예제

```XML
<Package
  xmlns:desktop2="http://schemas.microsoft.com/appx/manifest/desktop/windows10/2"
  IgnorableNamespaces="desktop2">
  <Applications>
  <Application>
    <Extensions>
      <desktop2:Extension Category="windows.appPrinter">
        <desktop2:AppPrinter
          DisplayName="Send to Contoso"
          Parameters="/insertdoc %1" />
      </desktop2:Extension>
    </Extensions>
  </Application>
</Applications>
</Package>
```

이 확장을 사용하는 샘플은 [여기](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/PrintToPDF)서 찾을 수 있습니다.

<a id="fonts"></a>

### <a name="share-fonts-with-other-windows-applications"></a>다른 Windows 애플리케이션과 글꼴 공유

사용자 지정 글꼴을 다른 Windows 애플리케이션과 공유할 수 있습니다.

#### <a name="xml-namespaces"></a>XML 네임스페이스

`http://schemas.microsoft.com/appx/manifest/desktop/windows10/2`

#### <a name="elements-and-attributes-of-this-extension"></a>이 확장의 요소 및 특성

```XML
<Extension Category="windows.sharedFonts">
    <SharedFonts>
      <Font File="[FontFile]" />
    </SharedFonts>
  </Extension>
```

전체 스키마 참조는 [여기](/uwp/schemas/appxpackage/uapmanifestschema/element-uap4-sharedfonts)서 찾을 수 있습니다.

|이름 |설명 |
|-------|-------------|
|범주 |항상 ``windows.sharedFonts``입니다.
|파일 |공유하려는 글꼴이 포함된 파일입니다. |

#### <a name="example"></a>예제

```XML
<Package
  xmlns:uap4="http://schemas.microsoft.com/appx/manifest/uap/windows10/4"
  IgnorableNamespaces="uap4">
  <Applications>
    <Application>
      <Extensions>
        <uap4:Extension Category="windows.sharedFonts">
          <uap4:SharedFonts>
            <uap4:Font File="Fonts\JustRealize.ttf" />
            <uap4:Font File="Fonts\JustRealizeBold.ttf" />
          </uap4:SharedFonts>
        </uap4:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

<a id="win32-process"></a>

### <a name="start-a-win32-process-from-a-universal-windows-platform-uwp-app"></a>UWP(유니버설 Windows 플랫폼) 앱에서 Win32 프로세스 시작

완전 신뢰 모드에서 실행되는 Win32 프로세스를 시작합니다.

#### <a name="xml-namespaces"></a>XML 네임스페이스

`http://schemas.microsoft.com/appx/manifest/desktop/windows10`

#### <a name="elements-and-attributes-of-this-extension"></a>이 확장의 요소 및 특성

```XML
<Extension Category="windows.fullTrustProcess" Executable="[executable file]">
  <FullTrustProcess>
    <ParameterGroup GroupId="[GroupID]" Parameters="[Parameters]"/>
  </FullTrustProcess>
</Extension>
```

|이름 |설명 |
|-------|-------------|
|범주 |항상 ``windows.fullTrustProcess``입니다.
|GroupID |실행 파일에 전달할 매개 변수 세트를 식별하는 문자열입니다. |
|매개 변수 |실행 파일에 전달하려는 매개 변수입니다. |

#### <a name="example"></a>예제

```XML
<Package xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
         xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
         xmlns:rescap=
"http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities"
         xmlns:desktop="http://schemas.microsoft.com/appx/manifest/desktop/windows10">
  ...
  <Capabilities>
      <rescap:Capability Name="runFullTrust"/>
  </Capabilities>
  <Applications>
    <Application>
      <Extensions>
          <desktop:Extension Category="windows.fullTrustProcess" Executable="fulltrustprocess.exe">
              <desktop:FullTrustProcess>
                  <desktop:ParameterGroup GroupId="SyncGroup" Parameters="/Sync"/>
                  <desktop:ParameterGroup GroupId="OtherGroup" Parameters="/Other"/>
              </desktop:FullTrustProcess>
           </desktop:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

이러한 확장은 모든 디바이스에서 실행되는 유니버설 Windows 플랫폼 사용자 인터페이스를 만들고 싶으면서도 Win32 애플리케이션의 구성 요소가 완전 신뢰 모드에서 계속 실행되도록 하고 싶은 경우에 유용하게 사용할 수 있습니다.

Win32 앱을 위한 Windows 앱 패키지를 만든 후 UWP 앱의 패키지 파일에 이 확장을 추가하기만 하면 됩니다. 이 확장은 Windows 앱 패키지의 실행 파일을 시작하려 한다는 것을 나타냅니다.  UWP 앱과 Win32 앱 간에 통신을 원하는 경우에는 하나 이상의 [앱 서비스](/windows/uwp/launch-resume/app-services)를 설정할 수 있습니다. 이 시나리오에 대한 자세한 내용은 [여기](https://blogs.msdn.microsoft.com/appconsult/2016/12/19/desktop-bridge-the-migrate-phase-invoking-a-win32-process-from-a-uwp-app/)를 참조하세요.

## <a name="next-steps"></a>다음 단계

질문이 있으세요? Stack Overflow에서 질문하세요. 저희 팀은 이러한 [태그](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge)를 모니터링합니다. [여기](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D)에서 문의할 수도 있습니다.
