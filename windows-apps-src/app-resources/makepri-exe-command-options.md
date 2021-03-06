---
description: MakePri.exe에는 createconfig, dump, new, resourcepack 및 버전의 명령 집합이 있습니다. 이 항목에서는 사용에 대해 자세히 설명 합니다.
title: MakePri.exe 명령줄 옵션
template: detail.hbs
ms.date: 04/10/2018
ms.topic: article
keywords: Windows 10, UWP, 리소스, 이미지, 자산, MRT, 한정자
ms.localizationpriority: medium
ms.openlocfilehash: 7443efbb227bf3f9ea64db58902ebeb67b02f676
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93031736"
---
# <a name="makepriexe-command-line-options"></a>MakePri.exe 명령줄 옵션

[MakePri.exe](compile-resources-manually-with-makepri.md) 에는,,, 및 명령 집합이 있습니다 `createconfig` `dump` `new` `resourcepack` `versioned` . 이 항목에서는 사용을 위한 명령줄 옵션에 대해 자세히 설명 합니다.

> [!NOTE]
> MakePri.exe은 Windows 소프트웨어 개발 키트를 설치 하는 동안 **UWP 관리 앱에 대 한 Windows SDK** 옵션을 선택 하면 설치 됩니다. 경로 뿐만 아니라 `%WindowsSdkDir%bin\<WindowsTargetPlatformVersion>\x64\makepri.exe` 다른 아키텍처에 대해 이름이 지정 된 폴더에도 설치 됩니다. 예: `C:\Program Files (x86)\Windows Kits\10\bin\10.0.17713.0\x64\makepri.exe`.

## <a name="getting-help-from-the-command-line"></a>명령줄에서 도움말 보기

`MakePri.exe help`또는 `MakePri.exe /?` 를 실행 하 여 MakePri.exe에서 사용할 수 있는 명령을 볼 수 있습니다. 명령에 대 한 `MakePri.exe <command> /?` 세부 정보를 확인 하 고 매우 드문 경우에도 `MakePri.exe <command> <option>` 옵션에 대 한 세부 정보를 볼 수 있습니다.

## <a name="makepri-commands"></a>MakePri 명령

```console
C:\>makepri help

Usage:
------
    MakePri.exe <command> [options]

Example:
--------
    MakePri.exe new /cf C:\MyApp\priconfig.xml /pr C:\MyApp\src\ /in PackageName

Description:
------------
    Creates, dumps, and performs utility functions on a PRI file. A PRI file is 
    an index of application resources, such as strings and image files.

Command Options:
----------------
    MakePri.exe createconfig   Creates a PRI config file for use with other
                               commands
    MakePri.exe dump           Dumps the contents of a PRI file
    MakePri.exe new            Creates a new PRI file from scratch
    MakePri.exe resourcepack   Creates a PRI file that contains additional
                               resource variants for a base PRI file
    MakePri.exe versioned      Creates a PRI file based on a previous version

Help:
-----
    MakePri.exe help           Show this help page
    MakePri.exe <command> /?   Shows detailed help for <command>

    For example,
    MakePri.exe createconfig /?
```

## <a name="createconfig-command"></a>Createconfig 명령

이 `createconfig` 명령은 지정 하는 한정자 기본값을 정의 하는 초기화 된 새 PRI 구성 파일을 만듭니다. `MakePri.exe createconfig /?`이 명령에 대 한 자세한 도움말을 보려면를 실행 합니다.

```console
C:\>makepri createconfig /?

Usage:
------
    MakePri.exe createconfig /cf <config file destination> /dq
    <default qualifiers> [options]

Example:
--------
    MakePri.exe createconfig /cf C:\MyApp\priconfig.xml /dq lang-en-US /o /pv 10.0.0

Description:
------------
    Creates a PRI configuration file at <config file destination> with default 
    qualifiers specified by <default qualifiers>. Multiple qualifiers are separated 
    by underscores (_)

Required Parameters:
--------------------
    /ConfigXml(cf)    : <FILEPATH> Configuration file output location
    /Default(dq)      : <QUALIFIERS> The default qualifiers to set in the
                        configuration file. A language qualifier is required

Options:
--------
    /ExtensionDll(ex) : <FILEPATH> Location of the Resource Management System
                        environment extension DLL. This DLL must be signed by
                        a Microsoft-issued certificate. Default is an empty path
                        (no DLL will be used)
    /Overwrite(o)     : Overwrite an existing output file of the same name
                        without prompting
    /Platform(pv)     : <VERSION> Platform version to use for generated
                        configuration file

    FILEPATH          - a path to a file, either relative to the current
                        directory or absolute
    QUALIFIERS        - a valid qualifier token
                        (for example, lang-en-US_scale-100_contrast-high)

Help:
-----
    /Help(h, ?)       : Display the usage help text
```

## <a name="dump-command"></a>Dump 명령

`dump`명령은 지정 된 PRI 파일의 모든 리소스 목록을 포함 하는 덤프 된 xml 파일을 출력 합니다. `MakePri.exe dump /?`이 명령에 대 한 자세한 도움말을 보려면를 실행 합니다.

> [!NOTE]
> 스키마 없는 리소스 팩은 PRI 구성 파일에서 *omitSchemaFromResourcePacks* 스위치를 사용 하 여 만든 것입니다. 스키마 없는 리소스 팩을 덤프 하려면 스위치를 사용 `/es <main_package_PRI_file>` 합니다. 주 파일을 지정 하지 않으면 " *패키지의 리소스 pri가 손상 되었으므로 암호화에 실패 했습니다 (오류 PRI222:0xdef0000f-지정 되지 않은 오류 발생)* " 라는 오류 메시지가 표시 됩니다.

```console
C:\>makepri dump /?

Usage:
------
    MakePri.exe dump [options]

Example:
--------
    MakePri.exe dump /if C:\MyApp\resources.pri /of C:\resources.pri.xml

Description:
------------
    Outputs a dumped xml file at <output file> containing a list of all 
    resources in <index file>.

Options:
--------
    /DumpType(dt)       : <STRING> Format of the dumped file, default is
                          Basic
    /ExtensionDll(ex)   : <FILEPATH> Location of the Resource Management System
                          environment extension DLL. This DLL must be signed by a
                          Microsoft-issued certificate. Default is an empty path
                          (no DLL will be used)
    /ExternalSchema(es) : <FILEPATH> Location of the external schema file
    /IndexFile(if)      : <FILEPATH> Location of the PRI file to dump from.
                          Default is .\resources.pri
    /OutputFile(of)     : <FILEPATH> Output location of the dump file, default
                          is .\[indexfile].xml
    /OutputOptions(oo)  : <OPTIONS> Options to provide detailed control over
                          contents of XML output files.
    /Overwrite(o)       : Overwrite an existing output file of the same name
                          without prompting
    /Verbose(v)         : Causes verbose messages to be output to the console

    Dump Type:
        Either 'Basic', 'Detailed', 'Schema', or 'Summary'

    FILEPATH            - a path to a file, either relative to the current
                          directory or absolute
Help:
-----
    /Help(h, ?)         : Display the usage help text
```

## <a name="new-command"></a>새 명령

`new`명령은 구성 파일의 지시에 따라 프로젝트의 파일을 인덱싱하여 새 PRI 파일을 만듭니다. `MakePri.exe new /?`이 명령에 대 한 자세한 도움말을 보려면를 실행 합니다.

```console
C:\>makepri new /?

Usage:
------
    MakePri.exe new /cf <config file> /pr <project root> [options]

Example:
--------
    MakePri.exe new /cf C:\MyApp\priconfig.xml /pr C:\MyApp\src\ 
    /mn C:\MyApp\AppXManifest.xml /o /of C:\MyApp\src\resources.pri

Description:
------------
    Creates a PRI file at <output file> by indexing all files in
    <project root> and its subdirectories as directed by <config file>. The
    index will be assigned <index name> to reference resources in the app

Required Parameters:
--------------------
    /ConfigXml(cf)    : <FILEPATH> Configuration file location. Use the
                        'Makepri.exe createconfig' command to generate one
    /ProjectRoot(pr)  : <FOLDERPATH> Root location of project files

Options:
--------
    /AutoMerge(am)    : This flag is not recommended for normal use with .appx
                        packages. It causes Makepri.exe to set the auto
                        merge flag within the PRI file. Default is not set.
    /ExtensionDll(ex) : <FILEPATH> Location of the Resource Management System
                        environment extension DLL. This DLL must be signed by
                        a Microsoft-issued certificate. Default is an empty path
                        (no DLL will be used)
    /IndexLog(il)     : <FILEPATH> XML Log of indexed resources, no file
                        generated by default
    /IndexName(in)    : <STRING> Name for the generated index of resources.
                        Typically matches the .appx package name, class library
                        simple name, etc. May be supplied via the
                        [manifest] parameter.
    /IndexOptions(io) : <OPTIONS> Options to provide detailed control over
                        behavior of resource indexers.
    /Manifest(mn)     : <FILEPATH> Location of the application or component's
                        manifest. This parameter is ignored if [indexname]
                        is given. Default is [projectroot]\AppXManifest.xml
    /MappingFile(mf)  : <MAPPINGFILETYPE> Generate a mapping file in the given
                        file format.
    /OutputFile(of)   : <FILEPATH> Output location of PRI file, default is
                        .\resources.pri
    /Overwrite(o)     : Overwrite an existing output file of the same name
                        without prompting
    /ReverseMap(rm)   : Generate a reverse mapping section in the PRI file
                        which can be used for debugging purposes.
    /SchemaFile(sf)   : <FILEPATH> Output location of XML resource schema
                        description.
    /Verbose(v)       : Causes verbose messages to be output to the console
    /VersionMajor(vma): <INTEGER> [Deprecated] Major version number for
                        index, default is 1

    FOLDERPATH        - a path to a folder
    FILEPATH          - a path to a file, either relative to the current
                        directory or absolute
    MAPPINGFILETYPE   - Supported File type(s): 'AppX'

Help:
-----
    /Help(h, ?)        : Display the usage help text
```

## <a name="resourcepack-command"></a>ResourcePack 명령

`resourcepack`명령은 구성 파일의 지시에 따라 프로젝트의 파일을 인덱싱하여 새 PRI 파일을 만듭니다. 리소스 팩 PRI 파일은 기존 PRI 파일에 이미 지정 된 리소스의 추가 변형만 포함 합니다. `MakePri.exe resourcepack /?`이 명령에 대 한 자세한 도움말을 보려면를 실행 합니다.

```console
C:\>makepri resourcepack /?

Usage:
------
    MakePri.exe resourcepack /pr <project root> /cf <config file> [options]

Example:
--------
    MakePri.exe resourcepack /cf C:\MyAppEs\priconfig.xml /pr C:\MyAppEs\src\ 
    /if C:\MyApp\1.2\resources.pri /o /of C:\MyAppEs\resources.pri

Description:
------------
    Creates a PRI file at <output file> by indexing all files in 
    <project root> and its subdirectories as directed by <config file>. A 
    resource pack PRI file contains only additional variants of resources 
    already specified in <index file>.

Required Parameters:
--------------------
    /ConfigXml(cf)    : <FILEPATH> Configuration file location. Use
                        'Makepri.exe createconfig' command to generate one
    /ProjectRoot(pr)  : <FOLDERPATH> Root location of project files

Options:
--------
    /AutoMerge(am)    : This flag is not recommended for normal use with .appx
                        packages. It causes Makepri.exe to set the auto
                        merge flag within the PRI file. By default it is set
                        to same as the base PRI file.
    /ExtensionDll(ex) : <FILEPATH> Location of the Resource Management System
                        environment extension DLL. This DLL must be signed by
                        a Microsoft-issued certificate. Default is an empty path
                        (no DLL will be used)
    /IndexFile(if)    : <FILEPATH> Location of the base PRI or XML schema file.
                        Default is <ProjectRoot>\resources.pri
    /IndexLog(il)     : <FILEPATH> XML Log of indexed resources, no file
                        generated by default
    /IndexOptions(io) : <OPTIONS> Options to provide detailed control over
                        behavior of resource indexers.
    /MappingFile(mf)  : <MAPPINGFILETYPE> Generate a mapping file in the given
                        file format.
    /OutputFile(of)   : <FILEPATH> Output location of PRI file, default is
                        .\resources.pri
    /Overwrite(o)     : Overwrite an existing output file of the same name
                        without prompting
    /ReverseMap(rm)   : Generate a reverse mapping section in the PRI file
                        which can be used for debugging purposes.
    /SchemaFile(sf)   : <FILEPATH> Output location of XML resource schema
                        description.
    /Verbose(v)       : Causes verbose messages to be output to the console

    FOLDERPATH        - a path to a folder
    FILEPATH          - a path to a file, either relative to the current
                        directory or absolute
    MAPPINGFILETYPE   - Supported File type(s): 'AppX'

Help:
-----
    /Help(h, ?)        : Display the usage help text
```

## <a name="versioned-command"></a>버전이 있는 명령

이 `versioned` 명령은 구성 파일의 지시에 따라 프로젝트의 파일을 인덱싱하여 버전이 지정 된 PRI 파일을 만듭니다. `MakePri.exe versioned /?`이 명령에 대 한 자세한 도움말을 보려면를 실행 합니다.

```console
C:\>makepri versioned /?

Usage:
------
    MakePri.exe versioned /cf <config file> /pr <project root> [options]

Example:
--------
    MakePri.exe versioned /cf C:\MyApp\priconfig.xml /pr C:\MyApp\src 
    /if C:\MyApp\1.2\resources.pri /o /of C:\MyApp\src\resources.pri /o

Description:
------------
    Creates a versioned PRI file at <output file> by indexing all files in 
    <project root> and its subdirectories as directed by <config file>.

Required Parameters:
--------------------
    /ConfigXml(cf)    : <FILEPATH> Configuration file location. Use
                        'Makepri.exe createconfig' command to generate one
    /ProjectRoot(pr)  : <FOLDERPATH> Root location of project files

Options:
--------
    /AutoMerge(am)    : This flag is not recommended for normal use with .appx
                        packages. It causes Makepri.exe to set the auto
                        merge flag within the PRI file. By default it is set
                        to same as the base PRI file.
    /ExtensionDll(ex) : <FILEPATH> Location of the Resource Management System
                        environment extension DLL. This DLL must be signed by
                        a Microsoft-issued certificate. Default is an empty path
                        (no DLL will be used)
    /IndexFile(if)    : <FILEPATH> Location of the base PRI or XML schema file
                        to version from. Default is <ProjectRoot>\resources.pri
    /IndexLog(il)     : <FILEPATH> XML Log of indexed resources, no file
                        generated by default
    /IndexOptions(io) : <OPTIONS> Options to provide detailed control over
                        behavior of resource indexers.
    /MappingFile(mf)  : <MAPPINGFILETYPE> Generate a mapping file in the given
                        file format.
    /OutputFile(of)   : <FILEPATH> Output location of PRI file, default is
                        [current directory]\resources.pri
    /Overwrite(o)     : Overwrite an existing output file of the same name
                        without prompting
    /ReverseMap(rm)   : Generate a reverse mapping section in the PRI file
                        which can be used for debugging purposes.
    /SchemaFile(sf)   : <FILEPATH> Output location of XML resource schema
                        description.
    /Verbose(v)       : Causes verbose messages to be output to the console

    FOLDERPATH        - a path to a folder
    FILEPATH          - a path to a file, either relative to the current
                        directory or absolute
    MAPPINGFILETYPE   - Supported File type(s): 'AppX'

Help:
-----
    /Help(h, ?)        : Display the usage help text
```

## <a name="47extensiondllex"></a>&#47;ExtensionDll (예:)

확장 dll 옵션 (/ex)은,,, 및와 함께 사용 `createconfig` `dump` 하 여 `new` `resourcepack` `versioned` 리소스 관리 시스템 환경 확장 dll의 위치를 지정 합니다.

## <a name="logging47metadata-file"></a>&#47;메타 데이터 파일 로깅

MakePri는 인덱서 메타 데이터 파일에서 리소스 팩과 관련 된 정보를 포함할 수 있습니다. 다음은 `resources.pri` 리소스 PRI 파일 및에 대 한 로그 파일의 예 `german.pri` 입니다 `highresolution.pri` .

```xml
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<root>
  <package filename="resources.pri">
    <instance itemname="Files\logo.jpg" qualifiers="Scale-100" src="" type="Path">
      <value>logo.scale-100.jpg</value>
    </instance>
    <instance itemname="resources\string2" qualifiers="Language-en-us" src="C:\Users\alias\Desktop\repro\app4\project\en-us\resources.resw" type="String">
      <value>value2</value>
    </instance>
    <instance itemname="resources\string1" qualifiers="Language-en-us" src="C:\Users\alias\Desktop\repro\app4\project\en-us\resources.resw" type="String">
      <value>value1</value>
    </instance>
  </package>
  <package filename="german.pri">
    <instance itemname="resources\string2" qualifiers="Language-de-de" src="C:\Users\alias\Desktop\repro\app4\project\de-de\resources.resw" type="String">
      <value>value2</value>
    </instance>
    <instance itemname="resources\string1" qualifiers="Language-de-de" src="C:\Users\alias\Desktop\repro\app4\project\de-de\resources.resw" type="String">
      <value>value1</value>
    </instance>
  </package>
  <package filename="highresolution.pri">
    <instance itemname="Files\logo.jpg" qualifiers="Scale-200" src="" type="Path">
      <value>logo.scale-200.jpg</value>
    </instance>
  </package>
</root>
```

## <a name="47indexfileif-option"></a>&#47;IndexFile (if) 옵션

인덱스 파일 옵션 (/if)을, 및와 함께 사용 `dump` `resourcepack` 하 여 `versioned` 입력 PRI 파일을 지정할 수 있습니다.

`resourcepack`및의 `versioned` 경우 (인 경우) 파일에 대 한 입력 매개 변수로 PRI 파일을 제공 하는 대신 스키마 파일을 제공할 수 있습니다.

```console
/IndexFile(if) <FILEPATH>
```

**FILEPATH** 는 입력 pri 파일 또는 pri 스키마 파일의 위치를 지정 하는 토큰입니다.

## <a name="47indexoptionsio-option"></a>&#47;IndexOptions (io) 옵션

인덱스 옵션 옵션 (/sio)을, 및와 함께 사용 하 여 `new` `resourcepack` `versioned` 리소스 인덱서의 동작에 대 한 자세한 제어를 제공 하는 옵션을 지정할 수 있습니다. 인덱스 옵션은 기본적으로 사용 되지 않습니다.

```console
/IndexOptions(io) <OPTIONS>
```

**옵션** 은 다음 옵션으로 구성 된 쉼표로 구분 된 목록입니다.

- +/-HiddenFiles (hf). 숨겨진 파일 및 폴더를 인덱싱하고 (+) 또는 무시 합니다.
- +/-LinkedFiles (lf). 연결 된 파일 및 폴더를 인덱싱하고 (+) 또는 무시 합니다.

## <a name="47mappingfilemf-option"></a>Mf (&#47;MappingFile) 옵션

매핑 파일 옵션 (/smf) `new` 을, 및와 함께 사용 `resourcepack` 하 여 `versioned` 매핑 파일을 생성 합니다. [MakeAppx.exe](/windows/msix/package/create-app-package-with-makeappx-tool) 는 매핑 파일을 사용 하 여 앱 패키지를 생성 합니다.

```console
/MappingFile(mf) <MAPPINGFILETYPE>
```

**MAPPINGFILETYPE** 는 매핑 파일의 형식을 지정 하는 토큰입니다. 유일 하 게 지원 되는 형식은 `appx` 입니다.

```console
/mf appx
```

이는 주 매핑 파일의 예제 내용입니다.

```console
"ResourceDimensions"                   "language-de-de"
```

리소스 팩 매핑 파일의 예제 내용입니다.

```console
"ResourceId"                           "Resources184.la5decaf08"
"ResourceDimensions"                   "language-de-de"
```

## <a name="output-summary"></a>출력 요약

리소스 팩이 생성 된 경우 MakePRI.exe의 출력 요약은 보다 자세한 형식입니다. 예를 들면 다음과 같습니다.

```console
Index Pass Completed: ResourcePackTests\TestApp_ResourcePack
Language Qualifiers: fr-FR, de-DE

Finished building
Version: 1.0
Resource Map Name: AppTest
Named Resources: 11

Resource PRI: fr-FR.pri
Version: 1.0
Resource Candidates: 4
Language: fr-FR

Resource PRI: de-DE.pri
Version: 1.0
Resource Candidates: 4
Language: de-DE

Output File(s) at TempTestResults
Successfully Completed
```

## <a name="47overwriteo-option"></a>&#47;Overwrite (o) 옵션

오버 쓰기 옵션 (/o)이 제공 되지 않은 경우 지정 된 출력 파일이 이미 있으면 MakePri.exe를 덮어쓰기 전에 확인이 필요 합니다.

```console
Following file(s) already exist at output location:
<file(s)>
Overwrite these file(s)? [Y]es (any other key to cancel):
```

## <a name="47outputfileof-option"></a>&#47;OutputFile (of) 옵션

출력 파일 옵션 (//)을,, 및와 함께 사용 하 여 `dump` `new` `resourcepack` `versioned` 출력 위치와 생성할 PRI 파일의 이름을 지정할 수 있습니다. MakePri.exe 둘 이상의 리소스 PRI 파일을 생성 하는 경우 대상 파일의 부모 폴더에 배치 합니다. 예를 들어를 지정 하는 경우 `/of MyParentFolder\TargetFile.pri` MakePri.exe는 `TargetFile.language-en.pri` 와 함께 및을 생성 `TargetFile.scale-100.pri` `TargetFile.pri` `ParentFolder` 합니다.

다음은 오류 조건 및 해당 오류 메시지의 예입니다.

| 오류 조건 | 오류 메시지 |
| --------------- | ------------- |
| 출력 파일 이름은 구성의 리소스 팩 이름 중 하 나와 동일 합니다. | 구성이 잘못 되었습니다. 리소스 팩 이름은 <resource pack name> outputfilename> <출력 파일과 동일할 수 없습니다. |

## <a name="reversemaprm-option"></a>/ReverseMap (rm) 옵션

역방향 매핑 옵션 (/srm)을, 및와 함께 사용 하면 `new` `resourcepack` `versioned` PRI 파일에 디버깅에 사용할 수 있는 역방향 매핑 섹션을 생성할 수 있습니다.

## <a name="47schemafilesf-option"></a>&#47;SchemaFile (sf) 옵션

스키마 파일 옵션 (/ssf)을, 및와 함께 사용 하 여 `new` `resourcepack` 지정 된 `versioned` 위치에 스키마 파일을 작성 합니다.

`resourcepack`및의 `versioned` 경우 (인 경우) 파일에 대 한 입력 매개 변수로 PRI 파일을 제공 하는 대신 스키마 파일을 제공할 수 있습니다.

```console
/SchemaFile(sf) <FILEPATH>
```

**FILEPATH** 는 스키마 파일을 쓸 위치를 지정 하는 토큰입니다.

다음은 스키마 파일의 예입니다.

```xml
<PriInfo>
    <ResourceMap name="IndexName" resourceVersion="1.0"> 
        <ResourceMapSubtree name="Resources" index="1">
            <NamedResource name="String1" index="1"/>
            <NamedResource name="String2" index="1"/>
        </ResourceMapSubtree>
        <ResourceMapSubtree name="Files" index="2">
            <NamedResource name="logo.png" index="2"/>
            <ResourceMapSubtree name="images" index="3">
                <NamedResource name="success.png" index="3"/>
                <NamedResource name="error.png" index="3"/>
            </ResourceMapSubtree>
        </ResourceMapSubtree>
    </ResourceMap>
</PriInfo>
```

## <a name="47versionmajorvma-is-deprecated"></a>Vma (&#47;VersionMajor)는 사용 되지 않습니다.

주 버전 (/vma) 옵션 (명령의 경우)은 사용 되지 않으며이 옵션을 `new` 사용 하면이 경고 메시지가 발생 합니다.

```console
'VersionMajor (vma)' input parameter has been deprecated. Please specify major version in the configuration file using 'majorVersion' attribute on 'resources' node.
```

주 버전 번호를 제공 하려면 [resources@majorVersion](makepri-exe-configuration.md) 구성 파일에서 특성을 사용 합니다.

## <a name="related-topics"></a>관련 항목

* [MakePri.exe](compile-resources-manually-with-makepri.md)
