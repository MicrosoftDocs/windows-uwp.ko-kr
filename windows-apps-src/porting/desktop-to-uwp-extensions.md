---
author: normesta
Description: "확장을 사용하여 미리 정의된 방법으로 패키지 데스크톱 앱과 Windows 10을 통합할 수 있습니다."
Search.Product: eADQiWindows 10XVcnh
title: "Windows 10과 앱 통합(데스크톱 브리지)"
ms.author: normesta
ms.date: 05/25/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: 0a8cedac-172a-4efd-8b6b-67fd3667df34
ms.openlocfilehash: 0c3427a7b49b17fda9a3ba0680e59b134732e9fa
ms.sourcegitcommit: 38ef208ef457ce1857038c9cde3658c884d29b75
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/13/2017
---
# <a name="integrate-your-app-with-windows-10-desktop-bridge"></a>Windows 10과 앱 통합(데스크톱 브리지)

확장을 사용하여 미리 정의된 방법으로 Windows 10 앱을 통합합니다.

예를 들어, 확장을 사용하여 방화벽 예외를 만들거나, 앱을 파일 형식에 대한 기본 앱으로 설정하거나, 시작 타일이 패키지 버전의 앱을 가리키도록 지정할 수 있습니다. 확장을 사용하려면 앱의 패키지 매니페스트 파일에 일부 XML을 추가하기만 하면 됩니다. 코드가 필요하지 않습니다.

여기서는 이러한 확장과 이를 통해 수행할 수 있는 작업을 설명합니다.

## <a name="transition-users-to-your-app"></a>사용자를 앱으로 전환

사용자의 패키지 앱 전환을 돕습니다.

* [기존의 시작 타일 및 작업 표시줄 단추가 패키지 앱을 가리키도록 지정](#point)
* [패키지 앱이 데스크톱 앱 대신 파일을 열도록 명령](#make)
* [파일 형식 별로 패키지 앱을 연결합니다.](#associate)
* [특정 파일 형식을 가진 파일의 컨텍스트 메뉴에 옵션 추가](#add)
* [URL을 사용하여 특정 형식의 파일을 직접 열기](#open)

<span id="point" />
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
|범주 |항상 ``windows.desktopAppMigration`` 값입니다.
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

[전환/마이그레이션/제거와 WPF 사진 뷰어](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/DesktopAppTransition)

<span id="make" />
### <a name="make-your-packaged-app-open-files-instead-of-your-desktop-app"></a>패키지 앱이 데스크톱 앱 대신 파일을 열도록 명령

사용자가 데스크톱 버전의 앱을 여는 대신, 특정 파일 형식에서 새 패키지 앱을 기본적으로 여는지 확인합니다.

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
|범주 |항상 ``windows.fileTypeAssociation`` 값입니다.
|이름 |앱에 대한 고유 ID입니다. 이 ID는 파일 형식 연결과 관련된 해시된 [프로그래밍 ID (ProgID)](https://msdn.microsoft.com/library/windows/desktop/cc144152.aspx)를 생성하는 데 내부적으로 사용됩니다. 앱의 이후 버전에서 변경을 관리하는 데 이 ID를 사용할 수 있습니다. |
|MigrationProgId |파일 연결을 상속하려는 데스크톱 앱의 응용 프로그램, 구성 요소 및 버전을 설명하는 [프로그래밍 ID (ProgID)](https://msdn.microsoft.com/library/windows/desktop/cc144152.aspx)입니다.|

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

[전환/마이그레이션/제거와 WPF 사진 뷰어](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/DesktopAppTransition)

<span id="associate" />
### <a name="associate-your-packaged-app-with-a-set-of-file-types"></a>파일 형식 별로 패키지 앱을 연결합니다.

패키지 파일 형식 확장과 연결할 수 있습니다. 사용자가 파일을 마우스 오른쪽 단추로 클릭하고 **연결 프로그램** 옵션을 선택하면 앱이 제안 목록에 표시됩니다.

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
|범주 |항상 ``windows.fileTypeAssociation`` 값입니다.
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

[전환/마이그레이션/제거와 WPF 사진 뷰어](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/DesktopAppTransition)

<span id="add" />
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
|범주 | 항상 ``windows.fileTypeAssociation`` 값입니다.
|이름 |앱에 대한 고유 ID입니다. |
|동사 |파일 탐색기 컨텍스트 메뉴에 표시되는 이름입니다. 이 문자열은 ```ms-resource```를 사용하여 지역화할 수 있습니다.|
|Id |동사의 고유 ID입니다. 앱이 UWP 앱인 경우 사용자의 선택을 적절하게 처리할 수 있도록 이 ID가 활성화 이벤트 인수의 일부로 앱에 전달됩니다. 앱이 완전 신뢰 패키지 앱인 경우 앱은 이 ID 대신에 매개 변수를 받습니다(다음 항목 참조). |
|매개 변수 |동사와 연관된 인수 매개 변수 및 값의 목록입니다. 앱이 완전 신뢰 패키지 앱인 경우에는 앱이 활성화될 때 이러한 매개 변수들이 이벤트 인수로서 앱에 전달됩니다. 다양한 활성화 동사에 따라 앱의 동작을 사용자가 지정할 수 있습니다. 변수가 파일 경로를 포함할 수 있는 경우에는 매개 변수 값을 따옴표로 묶습니다. 이렇게 해야 경로에 공백이 포함된 경우에 발생하는 모든 문제를 방지할 수 있습니다. 앱이 UWP 앱인 경우에는 매개 변수를 전달할 수 없습니다. 대신에 앱은 ID를 수신합니다 (이전 항목 참조).|
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

[전환/마이그레이션/제거와 WPF 사진 뷰어](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/DesktopAppTransition)

<span id="open" />
### <a name="open-certain-types-of-files-directly-by-using-a-url"></a>URL을 사용하여 특정 형식의 파일을 직접 열기

사용자가 데스크톱 버전의 앱을 여는 대신, 특정 파일 형식에서 새 패키지 앱을 기본적으로 여는지 확인합니다.

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
|범주 |항상 ``windows.fileTypeAssociation`` 값입니다.
|이름 |앱에 대한 고유 ID입니다. |
|UseUrl |URL 대상에서 직접 파일을 열 것인지 여부를 나타냅니다. 이 값 설정 하지 않는 경우, 앱이 URL을 사용해 파일을 열려고 시도하면 시스템에서 먼저 파일이 로컬 다운로드되는 결과가 야기됩니다. |
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

* [앱에 대한 예외 방화벽 만들기](#rules)

<span id="rules" />
### <a name="create-firewall-exception-for-your-app"></a>앱에 대한 예외 방화벽 만들기

앱에서 포트를 통한 통신이 필요한 경우, 앱을 방화벽 예외 목록에 추가할 수 있습니다.

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

## <a name="integrate-with-file-explorer"></a>파일 탐색기와 통합합니다.

사용자가 파일을 구성하고 친숙한 방식으로 상호 작용하는 데 도움이 됩니다.

* [사용자가 여러 개의 파일을 선택해서 동시에 열 때 앱의 동작하는 방법을 정의합니다.](#define)
* [파일 탐색기 내에서 미리 보기 이미지에 파일 내용을 보여 줍니다.](#show)
* [파일 탐색기의 미리 보기 창에 파일 내용을 보여 줍니다.](#preview)
* [사용자가 파일 탐색기에서 "종류(Kind)" 열을 사용하여 파일을 그룹화하도록 합니다.](#enable)
* [검색, 색인, 속성 대화 상자, 세부 정보 창에서 파일 속성을 사용할 수 있도록 합니다.](#make-file-properties)

<span id="define" />
### <a name="define-how-your-app-behaves-when-users-select-and-open-multiple-files-at-the-same-time"></a>사용자가 여러 개의 파일을 선택해서 동시에 열 때 앱의 동작하는 방법을 정의합니다.

사용자가 여러 개의 파일을 동시에 열 때 앱이 동작하는 방법을 지정합니다.

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
|범주 |항상 ``windows.fileTypeAssociation`` 값입니다.
|이름 |앱에 대한 고유 ID입니다. |
|MultiSelectModel |아래 참조 |
|FileType |관련 파일 확장명입니다. |

**MultSelectModel**

패키지 데스크톱 앱에는 일반 데스크톱 앱과 동일한 세 가지 옵션이 있습니다.

 * ``Player``: 앱이 한 번 활성화됩니다. 선택한 파일이 모두 인수 매개 변수로서 앱에 전달됩니다.
 * ``Single``: 처음 선택한 파일에서 앱이 한 번 활성화됩니다. 다른 파일은 무시됩니다.
 * ``Document``: 선택한 각 파일에서 앱의 새로운 별도 인스턴스가 활성화됩니다.

 다른 파일 형식 및 작업에 대해 다른 기본 설정을 설정할 수 있습니다. 예를 들어 *Document* 모드로 *문서*를, *Player* 모드로 *이미지*를 열고 싶을 수 있습니다.

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

<span id="show" />
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
            Clsid  ="[Clsid  ]"
            Cutoff="[Cutoff]"
            Treatment="[Treatment]" />
    </FileTypeAssociation>
</Extension>
```

전체 스키마 참조는 [여기](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation)에서 찾으세요.

|이름 |설명 |
|-------|-------------|
|범주 |항상 ``windows.fileTypeAssociation`` 값입니다.
|이름 |앱에 대한 고유 ID입니다. |
|FileType |관련 파일 확장명입니다. |
|Clsid   |앱의 클래스 ID입니다. |
|자르기 |아래 크기에서는 미리 보기 이미지가 지원되지 않습니다. [미리 보기 캐시 및 크기 조정](https://msdn.microsoft.com/library/windows/desktop/cc144118.aspx#cache)을 참조하세요. |
|처리 |[미리 보기 장식](https://msdn.microsoft.com/library/windows/desktop/cc144118.aspx#adornments)에서 미리 보기 아이콘의 모양을 정의할 수 있습니다. |

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
              Clsid  ="20000000-0000-0000-0000-000000000001"
              Cutoff="20x20"
              Treatment="Video Sprockets" />
            </uap3:FileTypeAssociation>
         </uap::Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

<span id="preview" />
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
|범주 |항상 ``windows.fileTypeAssociation`` 값입니다.
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

<span id="enable" />
### <a name="enable-users-to-group-files-by-using-the-kind-column-in-file-explorer"></a>사용자가 파일 탐색기에서 Kind 열을 사용하여 파일을 그룹화하도록 합니다.

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
|범주 |항상 ``windows.fileTypeAssociation`` 값입니다.
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
<span id="make-file-properties" />
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
        <DesktopPropertyHandler Clsid ="[Clsid ]"/>
    </uap:FileTypeAssociation>
</uap:Extension>
```
**주요 요소 및 특성 설명**

전체 스키마 참조는 [여기](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation)에서 찾으세요.

|이름 |설명 |
|-------|-------------|
|범주 |항상 ``windows.fileTypeAssociation`` 값입니다.
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

<span id="start" />
## <a name="start-your-app-in-different-ways"></a>다른 방법으로 앱을 시작합니다.

* [프로토콜을 사용하여 앱을 시작합니다.](#protocol)
* [별칭을 사용하여 앱을 시작합니다.](#alias)
* [사용자가 Windows에 로그인할 때 실행 파일 시작](#executable)
* [Windows 스토어에서 업데이트를 받은 후 자동으로 다시 시작](#updates)

<span id="protocol" />
### <a name="start-your-app-by-using-a-protocol"></a>프로토콜을 사용하여 앱을 시작합니다.

프로토콜 연결을 사용하면 다른 프로그램 및 시스템 구성 요소들을 패키지 앱에서 상호 운영할 수 있습니다. 프로토콜을 사용하여 패키지 앱을 시작하면 해당 활성화 이벤트 인수에 전달할 특정 매개 변수를 지정할 수 있으므로 앱이 적절하게 동작할 수 있습니다. 매개 변수는 완전 신뢰 패키지 앱에서만 지원됩니다. UWP 앱은 매개 변수를 사용할 수 없습니다.  

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
|범주 |항상 ``windows.protocol`` 값입니다.
|이름 |프로토콜 이름입니다. |
|매개 변수 |앱이 활성화될 때 이벤트 인수로서 앱에 전달할 매개 변수 및 값의 목록입니다. 변수가 파일 경로를 포함할 수 있는 경우에는 매개 변수 값을 따옴표로 묶습니다. 이렇게 해야 경로에 공백이 포함된 경우에 발생하는 모든 문제를 방지할 수 있습니다. |

### <a name="example"></a>예

```XML
<Package
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  IgnorableNamespaces="uap3">
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
<span id="alias" />
### <a name="start-your-app-by-using-an-alias"></a>별칭을 사용하여 앱을 시작합니다.

사용자 및 기타 프로세스는 앱에 대한 전체 경로 지정할 필요 없이 별칭을 사용해 앱을 시작할 수 있습니다. 별칭 이름을 지정할 수 있습니다.

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
|범주 |항상 ``windows.appExecutionAlias`` 값입니다.
|실행 파일 |별칭이 호출될 때 시작할 실행 파일에 대한 상대 경로입니다. |
|별칭 |앱에 대한 간단한 이름입니다. 항상 확장명이 ".exe"로 끝나야 합니다. 패키지의 각 응용 프로그램에 대해 하나의 앱 실행 별칭만 지정할 수 있습니다. 여러 앱이 같은 별칭으로 등록되는 경우 시스템은 다른 앱에서 재정의할 가능성이 없는 고유한 별칭을 선택하기 위해 마지막에 등록된 항목을 호출합니다.
|

#### <a name="example"></a>예

```XML
<Package
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  xmlns:desktop="http://schemas.microsoft.com/appx/manifest/desktop/windows10"
  IgnorableNamespaces="uap3, desktop">
  ...
  <uap3:Extension
        Category="windows.appExecutionAlias"
        Executable="exes\launcher.exe"
        EntryPoint="Windows.FullTrustApplication">
        <uap3:AppExecutionAlias>
            <desktop:ExecutionAlias Alias="Contoso.exe" />
        </uap3:AppExecutionAlias>
  </uap3:Extension>
...
</Package>
```

전체 스키마 참조는 [여기](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation)에서 찾으세요.

|이름 |설명 |
|-------|-------------|
|범주 |항상 ``windows.fileTypeAssociation`` 값입니다.
|이름 |앱에 대한 고유 ID입니다. 이 ID는 파일 형식 연결과 관련된 해시된 [프로그래밍 ID (ProgID)](https://msdn.microsoft.com/library/windows/desktop/cc144152.aspx)를 생성하는 데 내부적으로 사용됩니다. 앱의 이후 버전에서 변경을 관리하는 데 이 ID를 사용할 수 있습니다.   |
|FileType |앱에서 지원하는 파일 확장명입니다. |

<span id="executable" />
### <a name="start-an-executable-file-when-users-log-into-windows"></a>사용자가 Windows에 로그인할 때 실행 파일 시작

시작 작업을 통해 사용자가 로그온할 때마다 앱에서 실행 파일을 자동으로 실행하도록 할 수 있습니다.

> [!NOTE]
> 사용자가 앱을 한 번 이상 시작해야만 이 시작 작업이 등록됩니다.

앱이 여러 개의 시작 작업을 선언할 수 있습니다. 각 작업은 독립적으로 시작됩니다. 모든 시작 작업은 작업 관리자의 **시작** 탭에 나타나며 앱 매니페스트에 지정된 이름과 해당 앱 아이콘이 함께 표시됩니다. 작업 관리자는 작업의 시작 영향을 자동으로 분석합니다.

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
|범주 |항상 ``windows.startupTask`` 값입니다.|
|실행 파일 |시작하려는 실행 파일에 대한 상대 경로입니다. |
|TaskId |작업의 고유 ID입니다. 이 ID를 사용하여 앱에서 [Windows.ApplicationModel.StartupTask](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.StartupTask) 클래스의 API를 호출하여 시작 작업을 프로그래밍 방식으로 활성화 또는 비활성화할 수 있습니다. |
|활성화 |작업이 처음에 활성화 또는 비활성화 상태로 시작할 것인지를 나타냅니다. 사용하는 작업은 다음번에 사용자가 로그온할 때 실행됩니다(사용자가 사용하지 않도록 설정하지 않는 한). |
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

<span id="updates" />
### <a name="restart-automatically-after-receiving-an-update-from-the-windows-store"></a>Windows 스토어에서 업데이트를 받은 후 자동으로 다시 시작

사용자가 업데이트를 설치할 때 앱이 열려 있으면 앱을 닫습니다.

업데이트 완료 후 앱을 다시 시작하려면 다시 시작하기를 원하는 모든 프로세스에서 [RegisterApplicationRestart](https://msdn.microsoft.com/library/windows/desktop/aa373347.aspx) 기능을 호출합니다.

앱의 각 활성 창은 [WM_QUERYENDSESSION](https://msdn.microsoft.com/library/windows/desktop/aa376890.aspx) 메시지를 수신합니다. 이 시점에서 앱은 필요한 경우 명령줄을 업데이트할 [RegisterApplicationRestart](https://msdn.microsoft.com/library/windows/desktop/aa373347.aspx) 기능을 호출합니다.

앱의 각 창이 [WM_ENDSESSION](https://msdn.microsoft.com/library/windows/desktop/aa376889.aspx) 메시지를 받을 때, 앱은 데이터를 저장한 후 종료됩니다.

>[!NOTE]
활성 창은 또한 앱이 [WM ENDSESSION](https://msdn.microsoft.com/library/windows/desktop/aa376889.aspx)을 처리하지 않는 경우에도, [WM_CLOSE](https://msdn.microsoft.com/library/windows/desktop/ms632617.aspx) 메시지를 수신합니다.

이때 앱은 30초 후 직접 프로세스를 닫거나, 플랫폼이 이를 강제 종료합니다.

업데이트가 완료되면 앱을 다시 시작합니다.

## <a name="work-with-other-applications"></a>다른 응용 프로그램에서 작동합니다.

다른 앱과 통합하고, 다른 프로세스를 시작하거나 정보를 공유합니다.

* [앱이 인쇄를 지원하는 응용 프로그램에 인쇄 대상으로 표시되도록 합니다.](#printing)
* [다른 Windows 응용 프로그램과 글꼴을 공유합니다.](#fonts)
* [유니버설 Windows 플랫폼(UWP) 앱에서 Win32 프로세스를 시작합니다.](#win32-process)

<span id="printing" />
### <a name="make-your-app-appear-as-the-print-target-in-applications-that-support-printing"></a>앱이 인쇄를 지원하는 응용 프로그램에 인쇄 대상으로 표시되도록 합니다.

메모장 같은 다른 앱에서 데이터를 인쇄하고 싶은 경우, 사용자는 앱을 가용 인쇄 대상 목록에 인쇄 대상으로서 표시할 수 있습니다.

XPS(XML Paper Specification) 형식으로 인쇄 데이터를 수신하려면 앱을 수정해야 합니다.

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
|범주 |항상 ``windows.appPrinter`` 값입니다.
|DisplayName |앱에 대한 인쇄 대상 목록에 표시할 이름입니다. |
|매개 변수 |앱이 요청을 적절히 처리하는 데 필요한 모든 매개 변수입니다. |

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

<span id="fonts" />
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

전체 스키마 참조는 [여기](https://review.docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap4-sharedfonts)에서 찾으세요.


|이름 |설명 |
|-------|-------------|
|범주 |항상 ``windows.sharedFonts`` 값입니다.
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
<span id="win32-process" />
### <a name="start-a-win32-process-from-a-universal-windows-platform-uwp-app"></a>유니버설 Windows 플랫폼(UWP) 앱에서 Win32 프로세스를 시작합니다.

완전 신뢰 모드에서 실행되는 Win32 프로세스를 시작합니다.

#### <a name="xml-namespaces"></a>XML 네임스페이스

http://schemas.microsoft.com/appx/manifest/desktop/windows10

#### <a name="elements-and-attributes-of-this-extension"></a>이 확장의 요소 및 특성

```XML
<xtension Category="windows.fullTrustProcess" Executable="[executable file]">
  <FullTrustProcess>
    <ParameterGroup GroupId="[GroupID]" Parameters="[Parameters]"/>
  </FullTrustProcess>
</Extension>
```

|이름 |설명 |
|-------|-------------|
|범주 |항상 ``windows.fullTrustProcess`` 값입니다.
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
이러한 확장은 모든 장치에서 실행되는 유니버설 Windows 플랫폼 사용자 인터페이스를 만들고 싶으면서도, Win32 앱의 구성 요소가 완전 신뢰 모드에서 계속 실행되도록 하고 싶은 경우에 유용할 수 있습니다.

Win32 앱용 데스크톱 브리지 패키지를 만들기만 하면 됩니다. 그런 다음, UWP 앱의 패키지 파일에 이 확장을 추가합니다. 이 확장은 데스크톱 브리지 패키지에서 실행 파일을 시작하고 싶다고 표시합니다.  UWP 앱과 Win32 앱 간에 통신을 원하는 경우에는 하나 이상의 [앱 서비스](../launch-resume/app-services.md)를 설정할 수 있습니다. 이 시나리오에 대한 자세한 내용은 [여기](https://blogs.msdn.microsoft.com/appconsult/2016/12/19/desktop-bridge-the-migrate-phase-invoking-a-win32-process-from-a-uwp-app/)를 참조하세요.

## <a name="next-steps"></a>다음 단계

**특정 질문에 대한 답변 찾기**

저희 팀은 이러한 [StackOverflow 태그](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge)를 모니터링합니다.

**이 문서에 대한 피드백 제공**

아래의 설명 섹션을 사용합니다.
