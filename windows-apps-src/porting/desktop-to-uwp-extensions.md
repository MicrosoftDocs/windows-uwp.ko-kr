---
Description: 확장을 사용하여 미리 정의된 방법으로 패키지 데스크톱 앱과 Windows 10을 통합할 수 있습니다.
Search.Product: eADQiWindows 10XVcnh
title: Windows 10과 앱 통합(데스크톱 브리지)
ms.date: 04/18/2018
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 0a8cedac-172a-4efd-8b6b-67fd3667df34
ms.localizationpriority: medium
ms.openlocfilehash: 8e1f40d527d4bc7b5773b885f35ed87c3866449e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57613828"
---
# <a name="integrate-your-packaged-desktop-application-with-windows-10"></a>Windows 10을 사용 하 여 패키지에 포함 된 데스크톱 응용 프로그램 통합

확장 사용 미리 정의 된 방식으로 Windows 10을 사용 하 여 패키지에 포함 된 데스크톱 응용 프로그램을 통합 합니다.

예를 들어, 방화벽 예외를 만들기, 응용 프로그램 파일 형식의 경우 기본 응용 프로그램 및 시작 타일을 앱의 패키지 버전 가리킵니다 확장을 사용 합니다. 확장을 사용하려면 앱의 패키지 매니페스트 파일에 일부 XML을 추가하기만 하면 됩니다. 코드가 필요하지 않습니다.

여기서는 이러한 확장과 이를 통해 수행할 수 있는 작업을 설명합니다.

## <a name="transition-users-to-your-app"></a>사용자를 앱으로 전환

사용자의 패키지 앱 전환을 돕습니다.

* [패키지 된 앱에 기존 시작 타일 및 작업 표시줄 단추를 가리키고](#point)
* [데스크톱 앱 대신 파일을 열고 패키지에 포함 된 응용 프로그램](#make)
* [패키지 응용 프로그램 파일 형식의 집합을 사용 하 여 연결](#associate)
* [특정 파일 형식이 있는 파일의 상황에 맞는 메뉴 옵션 추가](#add)
* [직접 URL을 사용 하 여 특정 형식의 파일을 열려면](#open)

<a id="point" />

### <a name="point-existing-start-tiles-and-taskbar-buttons-to-your-packaged-app"></a>기존의 시작 타일 및 작업 표시줄 단추가 패키지 앱을 가리키도록 지정

사용자가 데스크톱 응용 프로그램을 작업 표시줄 또는 시작 메뉴에 고정시켰을 수 있습니다. 이러한 바로 가기가 새 패키지 앱을 가리키도록 지정할 수 있습니다.

#### <a name="xml-namespace"></a>XML 네임스페이스

http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities/3

#### <a name="elements-and-attributes-of-this-extension"></a>이 확장의 요소 및 특성

```XML
<Extension Category="windows.desktopAppMigration">
    <DesktopAppMigration>
        <DesktopApp AumId="[your_app_aumid]" />
        <DesktopApp ShortcutPath="[path]" />
    </DesktopAppMigration>
</Extension>

```

전체 스키마 참조는 [여기](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-rescap3-desktopappmigration)에서 찾으세요.

|이름 | 설명 |
|-------|-------------|
|범주 |항상 ``windows.desktopAppMigration``입니다.
|AumID |패키지 앱의 응용 프로그램 사용자 모델 ID입니다. |
|ShortcutPath |데스크톱 버전의 앱을 시작하는 .lnk 파일에 대한 경로입니다. |

#### <a name="example"></a>예

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

[전환/마이그레이션/제거를 사용 하 여 WPF 사진 뷰어](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/DesktopAppTransition)

<a id="make" />

### <a name="make-your-packaged-application-open-files-instead-of-your-desktop-app"></a>데스크톱 앱 대신 파일을 열고 패키지에 포함 된 응용 프로그램

있는지 사용자가 연 데스크톱 버전의 앱을 여는 대신 파일의 특정 형식에 대해 기본적으로 새 패키지에 포함 된 응용 프로그램을 만들 수 있습니다.

이를 위해 파일 연결을 상속하려는 각 애플리케이션의 [프로그래밍 ID (ProgID)](https://msdn.microsoft.com/library/windows/desktop/cc144152.aspx)를 지정합니다.

#### <a name="xml-namespaces"></a>XML 네임스페이스

* http://schemas.microsoft.com/appx/manifest/uap/windows10/3
* http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities/3

#### <a name="elements-and-attributes-of-this-extension"></a>이 확장의 요소 및 특성

```XML
<Extension Category="windows.fileTypeAssociation">
<FileTypeAssociation Name="[AppID]">
         <MigrationProgIds>
            <MigrationProgId>"[ProgID]"</MigrationProgId>
        </MigrationProgIds>
    </FileTypeAssociation>
</Extension>
```

전체 스키마 참조는 [여기](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation)에서 찾으세요.

|이름 |설명 |
|-------|-------------|
|범주 |항상 ``windows.fileTypeAssociation``입니다.
|이름 |앱에 대한 고유 ID입니다. 이 ID는 파일 형식 연결과 관련된 해시된 [프로그래밍 ID (ProgID)](https://msdn.microsoft.com/library/windows/desktop/cc144152.aspx)를 생성하는 데 내부적으로 사용됩니다. 앱의 이후 버전에서 변경을 관리하는 데 이 ID를 사용할 수 있습니다. |
|MigrationProgId |합니다 [ProgID (프로그래밍 식별자)](https://msdn.microsoft.com/library/windows/desktop/cc144152.aspx) 응용 프로그램, 구성 요소 및 파일 연결을 상속 하려는 데스크톱 응용 프로그램의 버전을 설명 하는 합니다.|

#### <a name="example"></a>예

```XML
<Package
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  xmlns:rescap3="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities/3"
  IgnorableNamespaces="uap3, rescap3">
  <Applications>
    <Application>
      <Extensions>
        <uap:Extension Category="windows.fileTypeAssociation">
          <uap3:FileTypeAssociation Name="Contoso">
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

[전환/마이그레이션/제거를 사용 하 여 WPF 사진 뷰어](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/DesktopAppTransition)

<a id="associate" />

### <a name="associate-your-packaged-application-with-a-set-of-file-types"></a>패키지 응용 프로그램 파일 형식의 집합을 사용 하 여 연결

패키지 응용 프로그램 파일 형식 확장명 연관 수 있습니다. 사용자 파일을 마우스 오른쪽 단추로 클릭 한 다음를 선택 하는 경우는 **열리고** 옵션을 응용 프로그램 제안 목록에 나타납니다.

#### <a name="xml-namespace"></a>XML 네임스페이스

* http://schemas.microsoft.com/appx/manifest/uap/windows10
* http://schemas.microsoft.com/appx/manifest/uap/windows10/3

#### <a name="elements-and-attributes-of-this-extension"></a>이 확장의 요소 및 특성

```XML
<Extension Category="windows.fileTypeAssociation">
    <FileTypeAssociation Name="[AppID]">
        <SupportedFileTypes>
            <FileType>"[file extension]"</FileType>
        </SupportedFileTypes>
    </FileTypeAssociation>
</Extension>
```

전체 스키마 참조는 [여기](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation)에서 찾으세요.

|이름 |설명 |
|-------|-------------|
|범주 |항상 ``windows.fileTypeAssociation``입니다.
|이름 |앱에 대한 고유 ID입니다. 이 ID는 파일 형식 연결과 관련된 해시된 [프로그래밍 ID (ProgID)](https://msdn.microsoft.com/library/windows/desktop/cc144152.aspx)를 생성하는 데 내부적으로 사용됩니다. 앱의 이후 버전에서 변경을 관리하는 데 이 ID를 사용할 수 있습니다.   |
|FileType |앱에서 지원하는 파일 확장명입니다. |

#### <a name="example"></a>예

```XML
<Package
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  IgnorableNamespaces="uap, uap3">
  <Applications>
    <Application>
      <Extensions>
        <uap:Extension Category="windows.fileTypeAssociation">
          <uap3:FileTypeAssociation Name="Contoso">
            <uap:SupportedFileTypes>
            <uap:FileType>.txt</uap:FileType>
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

[전환/마이그레이션/제거를 사용 하 여 WPF 사진 뷰어](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/DesktopAppTransition)

<a id="add" />

### <a name="add-options-to-the-context-menus-of-files-that-have-a-certain-file-type"></a>특정 파일 형식을 가진 파일의 컨텍스트 메뉴에 옵션 추가

대부분의 경우, 사용자는 파일을 마우스 오른쪽 단추로 클릭해서 엽니다. 사용자가 파일을 마우스 오른쪽 단추로 클릭하면 다양한 옵션이 표시됩니다.

해당 메뉴에 옵션을 추가할 수 있습니다. 이들 옵션은 인쇄, 편집, 파일 미리 보기 같이 파일과 상호 작용하는 방법들을 제공합니다.

#### <a name="xml-namespaces"></a>XML 네임스페이스

* http://schemas.microsoft.com/appx/manifest/uap/windows10
* http://schemas.microsoft.com/appx/manifest/uap/windows10/2
* http://schemas.microsoft.com/appx/manifest/uap/windows10/3

#### <a name="elements-and-attributes-of-this-extension"></a>이 확장의 요소 및 특성

```XML
<Extension Category="windows.fileTypeAssociation">
    <FileTypeAssociation Name="[AppID]">
        <SupportedVerbs>
           <Verb Id="[ID]" Extended="[Extended]" Parameters="[parameters]">"[verb label]"</Verb>
        </SupportedVerbs>
    </FileTypeAssociation>
</Extension>
```

전체 스키마 참조는 [여기](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation)에서 찾으세요.

|이름 |설명 |
|-------|-------------|
|범주 | 항상 ``windows.fileTypeAssociation``입니다.
|이름 |앱에 대한 고유 ID입니다. |
|동사 |파일 탐색기 컨텍스트 메뉴에 표시되는 이름입니다. 이 문자열은 ```ms-resource```를 사용하여 지역화할 수 있습니다.|
|Id |동사의 고유 ID입니다. 응용 프로그램을 UWP 앱 인 경우이 전달 됩니다 일부로 활성화 이벤트 인수입니다. 해당 앱에 사용자가 선택한 적절 하 게 처리할 수 있도록 합니다. 응용 프로그램이 완전 신뢰 패키지 된 앱 인 경우 수신한 매개 변수 대신 (다음 글머리 기호 참조). |
|매개 변수 |동사와 연관된 인수 매개 변수 및 값의 목록입니다. 응용 프로그램이 완전 신뢰 패키지 된 앱 인 경우 응용 프로그램이 활성화 될 때 이러한 매개 변수는 이벤트 인수입니다. 응용 프로그램 전달 됩니다. 다른 활성화 동사에 따라 응용 프로그램의 동작을 사용자 지정할 수 있습니다. 변수가 파일 경로를 포함할 수 있는 경우에는 매개 변수 값을 따옴표로 묶습니다. 이렇게 해야 경로에 공백이 포함된 경우에 발생하는 모든 문제를 방지할 수 있습니다. 응용 프로그램을 UWP 앱 인 경우에 매개 변수를 전달할 수 없습니다. 대신에 앱은 ID를 수신합니다 (이전 항목 참조).|
|확장 |사용자가 파일을 마우스 오른쪽 단추로 클릭하기 전에 **Shift** 키를 눌러서 컨텍스트 메뉴를 표시하는 경우에만 동사 메뉴가 표시되도록 지정합니다. 이 특성은 선택적으로 사용할 수 있으며, 목록에 없는 경우 기본적으로 **False**로 설정됩니다. 각 동사에 대해 개별적으로 이 동작을 지정합니다(항상 **False**인 "열기"는 예외).|

#### <a name="example"></a>예

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
          <uap3:FileTypeAssociation Name="Contoso">
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

[전환/마이그레이션/제거를 사용 하 여 WPF 사진 뷰어](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/DesktopAppTransition)

<a id="open" />

### <a name="open-certain-types-of-files-directly-by-using-a-url"></a>URL을 사용하여 특정 형식의 파일을 직접 열기

있는지 사용자가 연 데스크톱 버전의 앱을 여는 대신 파일의 특정 형식에 대해 기본적으로 새 패키지에 포함 된 응용 프로그램을 만들 수 있습니다.

#### <a name="xml-namespaces"></a>XML 네임스페이스

* http://schemas.microsoft.com/appx/manifest/uap/windows10
* http://schemas.microsoft.com/appx/manifest/uap/windows10/3"

#### <a name="elements-and-attributes-of-this-extension"></a>이 확장의 요소 및 특성

```XML
<Extension Category="windows.fileTypeAssociation">
    <FileTypeAssociation Name="[AppID]" UseUrl="true" Parameters="%1">
        <SupportedFileTypes>
            <FileType>"[FileExtension]"</FileType>
        </SupportedFileTypes>
    </FileTypeAssociation>
</Extension>
```

전체 스키마 참조는 [여기](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation)에서 찾으세요.

|이름 |설명 |
|-------|-------------|
|범주 |항상 ``windows.fileTypeAssociation``입니다.
|이름 |앱에 대한 고유 ID입니다. |
|UseUrl |URL 대상에서 직접 파일을 열 것인지 여부를 나타냅니다. 이 값을 설정 하지 않으면 파일을 로컬 URL 발생 하는 첫 번째 다운로드 파일 시스템에서 사용 하 여 응용 프로그램에서 시도 합니다. |
|매개 변수 |선택적 매개 변수입니다. |
|FileType |관련 파일 확장명입니다. |

#### <a name="example"></a>예

```XML
<Package
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  IgnorableNamespaces="uap, uap3">
  <Applications>
      <Application>
        <Extensions>
          <uap:Extension Category="windows.fileTypeAssociation">
            <uap3:FileTypeAssociation Name="documenttypes" UseUrl="true" Parameters="%1">
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

## <a name="perform-setup-tasks"></a>설정 작업을 수행

* [앱에 대 한 방화벽 예외를 만들려면](#rules)
* [패키지의 모든 폴더에 DLL 파일을 저장 합니다.](#load-paths)

<a id="rules" />

### <a name="create-firewall-exception-for-your-app"></a>앱에 대한 예외 방화벽 만들기

응용 프로그램 포트를 통한 통신에 필요한 경우 방화벽 예외 목록에 응용 프로그램을 추가할 수 있습니다.

#### <a name="xml-namespace"></a>XML 네임스페이스

http://schemas.microsoft.com/appx/manifest/desktop/windows10/2

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

전체 스키마 참조는 [여기](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-desktop2-firewallrules)에서 찾으세요.

|이름 |설명 |
|-------|-------------|
|범주 |항상 ``windows.firewallRules``|
|실행 파일 |방화벽 예외 목록에 추가하고자 하는 실행 파일의 이름 |
|방향 |인바운드 규칙인지 아웃바운드 규칙인지를 표시 |
|IPProtocol |통신 프로토콜 |
|LocalPortMin |로컬 포트 번호의 범위 내에 있는 하위 포트 번호입니다. |
|LocalPortMax |로컬 포트 번호의 범위 내에 있는 최상위 포트 번호입니다 |
|RemotePortMax |원격 포트 번호의 범위 내에 있는 하위 포트 번호입니다. |
|RemotePortMax |원격 포트 번호의 범위 내에 있는 최상위 포트 번호입니다 |
|프로필 |네트워크 종류 |

#### <a name="example"></a>예

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

<a id="load-paths" />

### <a name="place-your-dll-files-into-any-folder-of-the-package"></a>패키지의 원하는 폴더에 DLL 파일 배치

해당 폴더를 식별하기 위해 확장을 사용합니다. 이렇게 시스템은 사용자가 안에 배치한 파일을 찾아 로드할 수 있습니다. 이를 _%PATH%_ 환경 변수를 대체하는 확장으로 생각하세요.

이 확장을 사용하지 않는 경우 시스템은 프로세스의 패키지 종속성 그래프, 패키지 루트 폴더, 그리고 시스템 디렉터리(_%SystemRoot%\system32_)를 순서대로 검색합니다. 자세한 내용은 [Windows 앱의 검색 순서](https://msdn.microsoft.com/library/windows/desktop/ms682586.aspx#_search_order_for_windows_store_apps)를 참조하세요.

각 패키지에는 이 확장 중 하나만 포함될 수 있습니다. 따라서 이 중 하나를 주 패키지에 추가한 다음 하나를 각 [선택적 패키지 및 관련 집합](https://docs.microsoft.com/windows/uwp/packaging/optional-packages)에 추가할 수 있습니다.

#### <a name="xml-namespace"></a>XML 네임스페이스

http://schemas.microsoft.com/appx/manifest/uap/windows10/6

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
|FolderPath | DLL 파일을 포함하는 폴더의 경로. 패키지의 루트 폴더와 관련된 경로를 지정합니다. 한 확장에서 최대 5개의 경로를 지정할 수 있습니다. 시스템이 패키지의 루트 폴더에서 파일을 검색하려는 경우 이러한 경로 중 하나에 대해 빈 문자열을 사용합니다. 중복 경로를 포함해서는 안되며 경로가 선행 또는 후행 슬래시 또는 백슬래시를 포함하지 않는지 확인하세요. <br><br> 시스템은 하위 폴더를 검색하지 않으므로 시스템에서 로드하기 원하는 DLL 파일을 포함하는 각 폴더를 명시적으로 나열하도록 하세요.|

#### <a name="example"></a>예

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

## <a name="integrate-with-file-explorer"></a>파일 탐색기와 통합합니다.

사용자가 파일을 구성하고 친숙한 방식으로 상호 작용하는 데 도움이 됩니다.

* [사용자가 선택 하 고 동시에 여러 파일을 열 때 응용 프로그램의 동작 방식을 정의 합니다.](#define)
* [파일 탐색기 내에서 축소판 그림 이미지의 파일 내용 표시](#show)
* [파일 탐색기의 미리 보기 창에서 파일 콘텐츠를 보여 줍니다.](#preview)
* [파일 탐색기의 유형 열을 사용 하 여 사용자가 그룹 파일을 사용 하도록 설정](#enable)
* [검색, 인덱스, 속성 대화 상자 및 세부 정보 창에 파일 속성을 사용할 수 있도록](#make-file-properties)
* [클라우드 서비스에서 파일을 파일 탐색기에 표시](#cloud-files)

<a id="define" />

### <a name="define-how-your-application-behaves-when-users-select-and-open-multiple-files-at-the-same-time"></a>사용자가 선택 하 고 동시에 여러 파일을 열 때 응용 프로그램의 동작 방식을 정의 합니다.

사용자가 여러 파일을 동시에 열면 응용 프로그램의 동작을 지정 합니다.

#### <a name="xml-namespaces"></a>XML 네임스페이스

* http://schemas.microsoft.com/appx/manifest/uap/windows10
* http://schemas.microsoft.com/appx/manifest/uap/windows10/2
* http://schemas.microsoft.com/appx/manifest/uap/windows10/3

#### <a name="elements-and-attributes-of-this-extension"></a>이 확장의 요소 및 특성

```XML
<Extension Category="windows.fileTypeAssociation">
    <FileTypeAssociation Name="[AppID]" MultiSelectModel="[SelectionModel]">
        <SupportedVerbs>
            <Verb Id="Edit" MultiSelectModel="[SelectionModel]">Edit</Verb>
        </SupportedVerbs>
        <SupportedFileTypes>
            <FileType>"[FileExtension]"</FileType>
        </SupportedFileTypes>
</Extension>
```

전체 스키마 참조는 [여기](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation)에서 찾으세요.

|이름 |설명 |
|-------|-------------|
|범주 |항상 ``windows.fileTypeAssociation``입니다.
|이름 |앱에 대한 고유 ID입니다. |
|MultiSelectModel |아래 참조 |
|FileType |관련 파일 확장명입니다. |

**MultSelectModel**

패키지 데스크톱 앱에는 일반 데스크톱 앱과 동일한 세 가지 옵션이 있습니다.

* ``Player``: 응용 프로그램에 한 번 활성화 됩니다. 선택한 파일의 모든 인수 매개 변수로 응용 프로그램에 전달 됩니다.
* ``Single``: 응용 프로그램에 첫 번째로 선택한 파일에 한 번씩 활성화 됩니다. 다른 파일은 무시됩니다.
* ``Document``: 응용 프로그램의 별도 새 인스턴스를 선택한 각 파일에 대 한 활성화 됩니다.

 다른 파일 형식 및 작업에 대해 다른 기본 설정을 설정할 수 있습니다. 예를 들어 *Document* 모드로 *문서*를, *Player* 모드로 *이미지*를 열 수 있습니다.

#### <a name="example"></a>예

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
          <uap3:FileTypeAssociation Name="myapp" MultiSelectModel="Document">
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

사용자가 15개 이하의 파일을 열 경우, **MultiSelectModel** 특성에 대한 기본 설정은 *Player*입니다. 그렇지 않은 경우에는 *Document*가 기본 설정입니다. UWP 앱은 항상 *Player*로 시작됩니다.

<a id="show" />

### <a name="show-file-contents-in-a-thumbnail-image-within-file-explorer"></a>파일 탐색기 내에서 미리 보기 이미지에 파일 내용을 보여 줍니다.

사용자가 해당 파일의 아이콘이 중간, 대형, 초대형 크기로 표시될 때 파일 내용에 대한 미리 보기 이미지를 볼 수 있도록 합니다.

#### <a name="xml-namespace"></a>XML 네임스페이스

* http://schemas.microsoft.com/appx/manifest/uap/windows10
* http://schemas.microsoft.com/appx/manifest/uap/windows10/2
* http://schemas.microsoft.com/appx/manifest/uap/windows10/3
* http://schemas.microsoft.com/appx/manifest/desktop/windows10/2

#### <a name="elements-and-attributes-of-this-extension"></a>이 확장의 요소 및 특성

```XML
<Extension Category="windows.fileTypeAssociation">
    <FileTypeAssociation Name="[AppID]">
        <SupportedFileTypes>
            <FileType>"[FileExtension]"</FileType>
        </SupportedFileTypes>
        <ThumbnailHandler
            Clsid  ="[Clsid  ]" />
    </FileTypeAssociation>
</Extension>
```

전체 스키마 참조는 [여기](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation)에서 찾으세요.

|이름 |설명 |
|-------|-------------|
|범주 |항상 ``windows.fileTypeAssociation``입니다.
|이름 |앱에 대한 고유 ID입니다. |
|FileType |관련 파일 확장명입니다. |
|Clsid   |앱의 클래스 ID입니다. |

#### <a name="example"></a>예

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
          <uap3:FileTypeAssociation Name="Contoso">
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

<a id="preview" />

### <a name="show-file-contents-in-the-preview-pane-of-file-explorer"></a>파일 탐색기의 미리 보기 창에 파일 내용을 보여 줍니다.

사용자가 파일 탐색기의 미리 보기 창에서 파일 내용을 미리 볼 수 있도록 합니다.

#### <a name="xml-namespace"></a>XML 네임스페이스

* http://schemas.microsoft.com/appx/manifest/uap/windows10
* http://schemas.microsoft.com/appx/manifest/uap/windows10/2
* http://schemas.microsoft.com/appx/manifest/uap/windows10/3
* http://schemas.microsoft.com/appx/manifest/desktop/windows10/2

#### <a name="elements-and-attributes-of-this-extension"></a>이 확장의 요소 및 특성

```XML
<Extension Category="windows.fileTypeAssociation">
    <FileTypeAssociation Name="[AppID]">
        <SupportedFileTypes>
            <FileType>"[FileExtension]"</FileType>
        </SupportedFileTypes>
        <DesktopPreviewHandler Clsid  ="[Clsid  ]" />
    </FileTypeAssociation>
</Extension>
```

전체 스키마 참조는 [여기](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation)에서 찾으세요.

|이름 |설명 |
|-------|-------------|
|범주 |항상 ``windows.fileTypeAssociation``입니다.
|이름 |앱에 대한 고유 ID입니다. |
|FileType |관련 파일 확장명입니다. |
|Clsid   |앱의 클래스 ID입니다. |

#### <a name="example"></a>예

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
          <uap3:FileTypeAssociation Name="Contoso">
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

<a id="enable" />

### <a name="enable-users-to-group-files-by-using-the-kind-column-in-file-explorer"></a>사용자가 파일 탐색기에서 "종류(Kind)" 열을 사용하여 파일을 그룹화하도록 합니다.

파일 형식에 대한 하나 이상의 미리 정의된 값을 **종류** 필드와 연결할 수 있습니다.

파일 탐색기에서 사용자는 이 필드를 사용하여 해당 파일을 그룹화할 수 있습니다. 또한 시스템 구성 요소는 색인 같은 다양한 목적을 위해 이 필드를 사용합니다.

**종류** 필드와 이 필드에서 사용할 수 있는 값에 대한 자세한 내용은 [종류 이름 사용하기](https://msdn.microsoft.com/library/windows/desktop/cc144136.aspx)을 참조하세요.

#### <a name="xml-namespaces"></a>XML 네임스페이스

* http://schemas.microsoft.com/appx/manifest/uap/windows10
* http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities/3

#### <a name="elements-and-attributes-of-this-extension"></a>이 확장의 요소 및 특성

```XML
<Extension Category="windows.fileTypeAssociation">
    <FileTypeAssociation Name="[AppID]">
        <SupportedFileTypes>
            <FileType>"[FileExtension]"</FileType>
        </SupportedFileTypes>
        <KindMap>
            <Kind value="[KindValue]">
        </KindMap>
    </FileTypeAssociation>
</Extension>
```

전체 스키마 참조는 [여기](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation)에서 찾으세요.

|이름 |설명 |
|-------|-------------|
|범주 |항상 ``windows.fileTypeAssociation``입니다.
|이름 |앱에 대한 고유 ID입니다. |
|FileType |관련 파일 확장명입니다. |
|value |유효한 [종류 값](https://msdn.microsoft.com/en-us/library/windows/desktop/cc144136.aspx#kind_hierarchy)입니다. |

#### <a name="example"></a>예

```XML
<Package
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities/3"
  IgnorableNamespaces="uap, rescap">
  <Applications>
    <Application>
      <Extensions>
        <uap:Extension Category="windows.fileTypeAssociation">
           <uap:FileTypeAssociation Name="Contoso">
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

<a id="make-file-properties" />

### <a name="make-file-properties-available-to-search-index-property-dialogs-and-the-details-pane"></a>검색, 색인, 속성 대화 상자, 세부 정보 창에서 파일 속성을 사용할 수 있도록 합니다.

#### <a name="xml-namespace"></a>XML 네임스페이스

* http://schemas.microsoft.com/appx/manifest/uap/windows10
* http://schemas.microsoft.com/appx/manifest/uap/windows10/3
* http://schemas.microsoft.com/appx/manifest/desktop/windows10/2

#### <a name="elements-and-attributes-of-this-extension"></a>이 확장의 요소 및 특성

```XML
<uap:Extension Category="windows.fileTypeAssociation">
    <uap:FileTypeAssociation Name="[AppID]">
        <SupportedFileTypes>
            <FileType>.bar</FileType>
        </SupportedFileTypes>
        <DesktopPropertyHandler Clsid ="[Clsid]"/>
    </uap:FileTypeAssociation>
</uap:Extension>
```

전체 스키마 참조는 [여기](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation)에서 찾으세요.

|이름 |설명 |
|-------|-------------|
|범주 |항상 ``windows.fileTypeAssociation``입니다.
|이름 |앱에 대한 고유 ID입니다. |
|FileType |관련 파일 확장명입니다. |
|Clsid  |앱의 클래스 ID입니다. |

#### <a name="example"></a>예

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
          <uap3:FileTypeAssociation Name="Contoso">
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

<a id="cloud-files" />

### <a name="make-files-from-your-cloud-service-appear-in-file-explorer"></a>클라우드 서비스의 파일을 파일 탐색기에 표시

응용 프로그램에서 구현한 처리기를 등록합니다. 또한 사용자가 파일 탐색기에서 클라우드 기반 파일을 마우스 오른쪽 단추로 클릭하면 나타나는 컨텍스트 메뉴 옵션을 추가할 수 있습니다.

#### <a name="xml-namespace"></a>XML 네임스페이스

* http://schemas.microsoft.com/appx/manifest/desktop/windows10

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
|iconResource |클라우드 파일 공급자 서비스를 나타내는 아이콘. 이 아이콘은 파일 탐색기의 탐색 창에 표시됩니다.  사용자는 이 아이콘을 클라우드 서비스의 파일을 표시하려면 이 아이콘을 선택합니다. |
|CustomStateHandler Clsid |클래스 ID를 CustomStateHandler를 구현 하는 응용 프로그램입니다. 시스템은 이 클래스 ID를 사용하여 클라우드 파일에 대한 사용자 지정 상태 및 열을 요청합니다. |
|ThumbnailProviderHandler Clsid |클래스 ID를 ThumbnailProviderHandler를 구현 하는 응용 프로그램입니다. 시스템은 이 클래스 ID를 사용하여 클라우드 파일에 대한 미리 보기 이미지를 요청합니다. |
|ExtendedPropertyHandler Clsid |클래스 ID를 ExtendedPropertyHandler를 구현 하는 응용 프로그램입니다.  시스템은 이 클래스 ID를 사용하여 클라우드 파일에 대한 확장된 속성을 요청합니다. |
|동사 |클라우드 서비스에서 제공되는 파일에 대해 파일 탐색기 컨텍스트 메뉴에 표시되는 이름. |
|Id |동사의 고유 ID입니다. |

#### <a name="example"></a>예

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

<a id="start" />

## <a name="start-your-application-in-different-ways"></a>다른 방법으로 응용 프로그램을 시작 합니다.

* [프로토콜을 사용 하 여 응용 프로그램을 시작 합니다.](#protocol)
* [별칭을 사용 하 여 응용 프로그램을 시작 합니다.](#alias)
* [사용자가 Windows에 로그인 하는 경우 실행 파일 시작](#executable)
* [사용자가 자신의 PC에 장치를 연결할 때 응용 프로그램을 시작할 수 있게](#autoplay)
* [Microsoft Store 업데이트를 받은 후 자동으로 다시 시작](#updates)

<a id="protocol" />

### <a name="start-your-application-by-using-a-protocol"></a>프로토콜을 사용 하 여 응용 프로그램을 시작 합니다.

프로토콜 연결을 사용하면 다른 프로그램 및 시스템 구성 요소들을 패키지 앱에서 상호 운영할 수 있습니다. 패키지 응용 프로그램 프로토콜을 사용 하 여 시작 되 면 정품 인증 이벤트 인수를 전달할 적절 하 게 동작할 수 있습니다 특정 매개 변수를 지정할 수 있습니다. 매개 변수는 완전 신뢰 패키지 앱에서만 지원됩니다. UWP 앱은 매개 변수를 사용할 수 없습니다.

#### <a name="xml-namespace"></a>XML 네임스페이스

http://schemas.microsoft.com/appx/manifest/uap/windows10/3

#### <a name="elements-and-attributes-of-this-extension"></a>이 확장의 요소 및 특성

```XML
<Extension
    Category="windows.protocol">
  <Protocol
      Name="[Protocol name]"
      Parameters="[Parameters]" />
</Extension>
```

전체 스키마 참조는 [여기](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap-protocol)에서 찾으세요.

|이름 |설명 |
|-------|-------------|
|범주 |항상 ``windows.protocol``입니다.
|이름 |프로토콜 이름입니다. |
|매개 변수 |매개 변수 및 응용 프로그램 활성화 되 면 응용 프로그램으로 이벤트 인수를 전달 하는 값의 목록입니다. 변수가 파일 경로를 포함할 수 있는 경우에는 매개 변수 값을 따옴표로 묶습니다. 이렇게 해야 경로에 공백이 포함된 경우에 발생하는 모든 문제를 방지할 수 있습니다. |

### <a name="example"></a>예

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

<a id="alias" />

### <a name="start-your-application-by-using-an-alias"></a>별칭을 사용 하 여 응용 프로그램을 시작 합니다.

사용자 및 다른 프로세스를 앱에 전체 경로 지정 하지 않고도 응용 프로그램을 시작 하려면 별칭을 사용 하 합니다. 별칭 이름을 지정할 수 있습니다.

#### <a name="xml-namespaces"></a>XML 네임스페이스

* http://schemas.microsoft.com/appx/manifest/uap/windows10/3
* http://schemas.microsoft.com/appx/manifest/desktop/windows10

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
|실행 파일 |별칭이 호출될 때 시작할 실행 파일에 대한 상대 경로입니다. |
|별칭 |앱에 대한 간단한 이름입니다. 항상 확장명이 ".exe"로 끝나야 합니다. 패키지의 각 응용 프로그램에 대해 하나의 앱 실행 별칭만 지정할 수 있습니다. 여러 앱이 같은 별칭으로 등록되는 경우 시스템은 다른 앱에서 재정의할 가능성이 없는 고유한 별칭을 선택하기 위해 마지막에 등록된 항목을 호출합니다.
|

#### <a name="example"></a>예

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
 
...
</Package>
```

전체 스키마 참조는 [여기](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation)에서 찾으세요.

<a id="executable" />

### <a name="start-an-executable-file-when-users-log-into-windows"></a>사용자가 Windows에 로그인할 때 실행 파일 시작

시작 작업에는 응용 프로그램 사용자가 로그온 할 때마다 실행 파일을 자동으로 실행을 허용 합니다.

> [!NOTE]
> 사용자는이 시작 작업을 등록 하려면 적어도 한 번에 응용 프로그램을 시작 해야 합니다.

응용 프로그램 시작 작업을 여러 개를 선언할 수 있습니다. 각 작업은 독립적으로 시작됩니다. 모든 시작 작업은 작업 관리자의 **시작** 탭에 나타나며 앱 매니페스트에 지정된 이름과 해당 앱 아이콘이 함께 표시됩니다. 작업 관리자는 작업의 시작 영향을 자동으로 분석합니다.

사용자는 작업 관리자를 사용하여 수동으로 앱의 시작 작업을 비활성화할 수 있습니다. 사용자가 작업을 비활성화하면 프로그래밍 방식으로 다시 활성화할 수 없습니다.

#### <a name="xml-namespace"></a>XML 네임스페이스

http://schemas.microsoft.com/appx/manifest/desktop/windows10

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
|실행 파일 |시작하려는 실행 파일에 대한 상대 경로입니다. |
|TaskId |작업의 고유 ID입니다. 이 식별자를 사용 하 여 응용 프로그램 수는에서 Api를 호출 합니다 [Windows.ApplicationModel.StartupTask](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.StartupTask) 클래스를 사용 하도록 설정 하는 프로그래밍 방식으로 또는 시작 작업을 사용 하지 않도록 설정 합니다. |
|Enabled |작업이 처음에 활성화 또는 비활성화 상태로 시작할 것인지를 나타냅니다. 사용하는 작업은 다음번에 사용자가 로그온할 때 실행됩니다(사용자가 사용하지 않도록 설정하지 않는 한). |
|DisplayName |작업 관리자에 표시되는 작업의 이름입니다. ```ms-resource```를 사용해 이 문자열을 지역화할 수 있습니다. |

#### <a name="example"></a>예

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

<a id="autoplay" />

### <a name="enable-users-to-start-your-application-when-they-connect-a-device-to-their-pc"></a>사용자가 자신의 PC에 장치를 연결할 때 응용 프로그램을 시작할 수 있게

사용자가 PC에 장치를 연결 하는 경우 자동 실행 옵션으로 응용 프로그램을 표시할 수 있습니다.

#### <a name="xml-namespace"></a>XML 네임스페이스

http://schemas.microsoft.com/appx/manifest/desktop/windows10/3

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
|ActionDisplayName |사용자 PC에 연결 하는 장치를 사용 하 여 수행할 수 있는 작업을 나타내는 문자열입니다 (예: "파일 가져오기" 또는 "비디오 재생"). |
|ProviderDisplayName | 응용 프로그램 또는 서비스를 나타내는 문자열입니다 (예: "Contoso 비디오 플레이어")입니다. |
|ContentEvent |사용자에게 ``ActionDisplayName``및 ``ProviderDisplayName``을 알리는 메시지가 표시하는 콘텐츠 이벤트의 이름. 콘텐츠 이벤트는 카메라 메모리 카드, 썸 드라이브(thumb drive) 또는 DVD 같은 볼륨 장치가 PC에 삽입될 때 발생합니다. 이러한 이벤트의 전체 목록은 [여기](https://docs.microsoft.com/windows/uwp/launch-resume/auto-launching-with-autoplay#autoplay-event-reference)에서 찾을 수 있습니다.  |
|동사 |선택한 옵션에 대 한 응용 프로그램에 전달 되는 값을 식별 하는 동사 설정 합니다. 자동 실행 이벤트에 대해 여러 개의 시작 작업을 지정하고 동사 설정을 사용하여 사용자가 앱에 대해 선택한 옵션을 확인할 수 있습니다. 앱에 전달된 시작 이벤트 인수의 verb 속성을 확인하여 사용자가 선택한 옵션을 알 수 있습니다. 동사 설정에는 예약된 open을 제외한 모든 값을 사용할 수 있습니다. |
|DropTargetHandler |구현 하는 응용 프로그램의 클래스 ID를 [IDropTarget](https://docs.microsoft.com/dotnet/api/microsoft.visualstudio.ole.interop.idroptarget?view=visualstudiosdk-2017) 인터페이스입니다. 이동식 미디어의 파일은 [IDropTarget](https://docs.microsoft.com/dotnet/api/microsoft.visualstudio.ole.interop.idroptarget?view=visualstudiosdk-2017) 구현의 [Drop](https://docs.microsoft.com/dotnet/api/microsoft.visualstudio.ole.interop.idroptarget.drop?view=visualstudiosdk-2017#Microsoft_VisualStudio_OLE_Interop_IDropTarget_Drop_Microsoft_VisualStudio_OLE_Interop_IDataObject_System_UInt32_Microsoft_VisualStudio_OLE_Interop_POINTL_System_UInt32__) 메서드에 전달됩니다.  |
|매개 변수 |모든 콘텐츠 이벤트에 대해 [IDropTarget](https://docs.microsoft.com/dotnet/api/microsoft.visualstudio.ole.interop.idroptarget?view=visualstudiosdk-2017) 인터페이스를 구현할 필요가 없습니다. 모든 콘텐츠 이벤트에 대해 [IDropTarget](https://docs.microsoft.com/dotnet/api/microsoft.visualstudio.ole.interop.idroptarget?view=visualstudiosdk-2017) 인터페이스를 구현하는 대신 명령줄 매개 변수를 제공할 수 있습니다. 이러한 이벤트에 대 한 자동 재생 해당 명령줄 매개 변수를 사용 하 여 응용 프로그램을 시작 됩니다. 앱의 초기화 코드에서 매개 변수를 분석하여 이 매개 변수가 자동 실행에서 시작되었는지 확인한 다음 사용자 지정 구현을 제공할 수 있습니다. |
|DeviceEvent |사용자에게 ``ActionDisplayName``및 ``ProviderDisplayName``을 알리는 메시지가 표시하는 장치 이벤트의 이름. 장치 이벤트는 장치가 PC에 연결될 때 발생합니다. 장치 이벤트는 ``WPD`` 문자열로 시작하며 [여기](https://docs.microsoft.com/windows/uwp/launch-resume/auto-launching-with-autoplay#autoplay-event-reference)서 나열되어 있는 것을 찾을 수 있습니다. |
|HWEventHandler |구현 하는 응용 프로그램의 클래스 ID를 [IHWEventHandler](https://msdn.microsoft.com/library/windows/desktop/bb775492.aspx) 인터페이스입니다. |
|InitCmdLine |[IHWEventHandler](https://msdn.microsoft.com/library/windows/desktop/bb775492.aspx) 인터페이스의 [Initialize](https://msdn.microsoft.com/en-us/library/windows/desktop/bb775495.aspx) 메서드에 전달하려는 문자열 매개 변수. |

### <a name="example"></a>예

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

<a id="updates" />

### <a name="restart-automatically-after-receiving-an-update-from-the-microsoft-store"></a>Microsoft Store에서 업데이트를 받은 후 자동으로 다시 시작

사용자가 되도록 업데이트를 설치할 때 응용 프로그램이 열려 있으면 응용 프로그램을 종료 합니다.

해당 응용 프로그램 업데이트 후 다시 시작을 완료를 호출 합니다 [RegisterApplicationRestart](https://msdn.microsoft.com/library/windows/desktop/aa373347.aspx) 다시 시작 하려는 모든 프로세스에는 함수입니다.

응용 프로그램의 각 활성 창에 수신 된 [WM_QUERYENDSESSION](https://msdn.microsoft.com/library/windows/desktop/aa376890.aspx) 메시지입니다. 이 시점에서 응용 프로그램에서 호출 수를 [RegisterApplicationRestart](https://msdn.microsoft.com/library/windows/desktop/aa373347.aspx) 다시 필요한 경우 명령줄을 업데이트 하는 함수입니다.

응용 프로그램의 활성 창에 각 받으면 합니다 [WM_ENDSESSION](https://msdn.microsoft.com/library/windows/desktop/aa376889.aspx) 메시지를 응용 프로그램 데이터를 저장 하며 종료 합니다.

>[!NOTE]
활성 windows 권한도 합니다 [WM_CLOSE](https://msdn.microsoft.com/library/windows/desktop/ms632617.aspx) 응용 프로그램 처리 하지 않는 경우 메시지를 [WM_ENDSESSION](https://msdn.microsoft.com/library/windows/desktop/aa376889.aspx) 메시지입니다.

이 시점에서 응용 프로그램에 자체 프로세스를 닫으십시오 30 초 또는 플랫폼을 강제로 종료 합니다.

업데이트를 완료 한 후에 응용 프로그램 다시 시작 합니다.

## <a name="work-with-other-applications"></a>다른 응용 프로그램에서 작동합니다.

다른 앱과 통합하고, 다른 프로세스를 시작하거나 정보를 공유합니다.

* [인쇄를 지 원하는 응용 프로그램에서 인쇄 대상으로 표시 하는 응용 프로그램](#printing)
* [다른 Windows 응용 프로그램을 사용 하 여 공유 글꼴](#fonts)
* [유니버설 Windows 플랫폼 (UWP) 앱에서 Win32 프로세스를 시작 합니다.](#win32-process)

<a id="printing" />

### <a name="make-your-application-appear-as-the-print-target-in-applications-that-support-printing"></a>인쇄를 지 원하는 응용 프로그램에서 인쇄 대상으로 표시 하는 응용 프로그램

사용자를 메모장과 같은 다른 응용 프로그램에서 데이터를 인쇄 하려면 인쇄 앱의 사용 가능한 인쇄 대상 목록 대상으로 표시 하는 응용 프로그램을 만들 수 있습니다.

문서 사양 XPS (XML) 형식으로 인쇄 데이터를 받을 수 있도록 응용 프로그램을 수정 해야 합니다.

#### <a name="xml-namespaces"></a>XML 네임스페이스

http://schemas.microsoft.com/appx/manifest/desktop/windows10/2

#### <a name="elements-and-attributes-of-this-extension"></a>이 확장의 요소 및 특성

```XML
<Extension Category="windows.appPrinter">
    <AppPrinter
        DisplayName="[DisplayName]"
        Parameters="[Parameters]" />
</Extension>
```

전체 스키마 참조는 [여기](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-desktop2-appprinter)에서 찾으세요.

|이름 |설명 |
|-------|-------------|
|범주 |항상 ``windows.appPrinter``입니다.
|DisplayName |앱에 대한 인쇄 대상 목록에 표시할 이름입니다. |
|매개 변수 |응용 프로그램 요청을 올바르게 처리 하는 데 필요한 모든 매개 변수입니다. |

#### <a name="example"></a>예

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

이 확장을 사용하는 샘플은 [여기](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/PrintToPDF)를 참조하세요.

<a id="fonts" />

### <a name="share-fonts-with-other-windows-applications"></a>다른 Windows 응용 프로그램과 글꼴을 공유합니다.

사용자 지정 글꼴을 다른 Windows 응용 프로그램과 공유합니다.

#### <a name="xml-namespaces"></a>XML 네임스페이스

http://schemas.microsoft.com/appx/manifest/desktop/windows10/2

#### <a name="elements-and-attributes-of-this-extension"></a>이 확장의 요소 및 특성

```XML
<Extension Category="windows.sharedFonts">
    <SharedFonts>
      <Font File="[FontFile]" />
    </SharedFonts>
  </Extension>
```

전체 스키마 참조는 [여기](/uwp/schemas/appxpackage/uapmanifestschema/element-uap4-sharedfonts)에서 찾으세요.

|이름 |설명 |
|-------|-------------|
|범주 |항상 ``windows.sharedFonts``입니다.
|파일 |공유하려는 글꼴이 포함된 파일입니다. |

#### <a name="example"></a>예

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

<a id="win32-process" />

### <a name="start-a-win32-process-from-a-universal-windows-platform-uwp-app"></a>유니버설 Windows 플랫폼(UWP) 앱에서 Win32 프로세스를 시작합니다.

완전 신뢰 모드에서 실행되는 Win32 프로세스를 시작합니다.

#### <a name="xml-namespaces"></a>XML 네임스페이스

http://schemas.microsoft.com/appx/manifest/desktop/windows10

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
|GroupID |실행 파일에 전달할 매개 변수 집합을 식별하는 문자열입니다. |
|매개 변수 |실행 파일에 전달하려는 매개 변수입니다. |

#### <a name="example"></a>예

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

이 확장 모든 장치에서 실행 되는 유니버설 Windows 플랫폼 사용자 인터페이스를 만들려는 경우 유용 하지만 완전 신뢰에서 실행을 계속 하려면 Win32 응용 프로그램의 구성 요소를 원하는 수 있습니다.

Win32 앱에 대 한 Windows 앱 패키지를 만듭니다. 그런 다음, UWP 앱의 패키지 파일에 이 확장을 추가합니다. 이 확장은 Windows 앱 패키지에는 실행 파일을 시작 하려는 나타냅니다.  UWP 앱과 Win32 앱 간에 통신을 원하는 경우에는 하나 이상의 [앱 서비스](../launch-resume/app-services.md)를 설정할 수 있습니다. 이 시나리오에 대한 자세한 내용은 [여기](https://blogs.msdn.microsoft.com/appconsult/2016/12/19/desktop-bridge-the-migrate-phase-invoking-a-win32-process-from-a-uwp-app/)를 참조하세요.

## <a name="next-steps"></a>다음 단계

**질문에 답변**

질문이 있으세요? Stack Overflow에서 질문해 주세요. 저희 팀은 이러한 [태그](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge)를 모니터링합니다. [여기](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D)에서 Microsoft에 문의할 수도 있습니다.

**피드백 제공 또는 기능 제안**

[UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial)를 참조하세요.
