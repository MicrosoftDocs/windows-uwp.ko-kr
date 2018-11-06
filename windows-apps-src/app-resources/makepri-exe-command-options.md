---
author: stevewhims
Description: MakePri.exe has the set of commands createconfig, dump, new, resourcepack, and versioned. This topic details their use.
title: MakePri.exe 명령줄 옵션
template: detail.hbs
ms.author: stwhi
ms.date: 04/10/2018
ms.topic: article
keywords: Windows 10, uwp, 리소스, 이미지, 자산, MRT, 한정자
ms.localizationpriority: medium
ms.openlocfilehash: c777996dceeb443c25fcf526e3a029fca00047c1
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/05/2018
ms.locfileid: "6043486"
---
# <a name="makepriexe-command-line-options"></a>MakePri.exe 명령줄 옵션

[MakePri.exe](compile-resources-manually-with-makepri.md)에는 `createconfig`, `dump`, `new`, `resourcepack` 및 `versioned` 명령 집합이 있습니다. 이 항목에서는 이들의 사용에 대한 명령줄 옵션을 자세히 설명합니다.

> [!NOTE]
> MakePri.exe는 Windows 소프트웨어 개발 키트를 설치 하는 동안 **앱을 관리 하는 UWP 용 Windows SDK** 옵션을 선택 하는 때 설치 됩니다. 경로에 설치 된 `%WindowsSdkDir%bin\<WindowsTargetPlatformVersion>\x64\makepri.exe` 메모가 다른 아키텍처에 대 한 폴더에 따라 합니다. 예를 들면 `C:\Program Files (x86)\Windows Kits\10\bin\10.0.17713.0\x64\makepri.exe`입니다.

## <a name="getting-help-from-the-command-line"></a>명령줄에서 도움말 보기

실행할 수 있습니다 `MakePri.exe help` 또는 `MakePri.exe /?` 하 여 MakePri.exe로 사용할 수 있는 명령을 봅니다. 실행할 수 있습니다 `MakePri.exe <command> /?` 명령에 대 한 하 고, 매우 드문 경우이 특성을 보려면도 `MakePri.exe <command> <option>` 하는 옵션에 대 한 세부 정보가 표시 됩니다.

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

## <a name="createconfig-command"></a>createconfig 명령

`createconfig` 명령은 사용자가 지정하는 한정자 기본값을 정의하는 초기화된 새 PRI 구성 파일을 만듭니다. `MakePri.exe createconfig /?`를 실행하여 이 명령에 대한 자세한 도움말을 봅니다.

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

## <a name="dump-command"></a>dump 명령

`dump` 명령은 지정된 PRI 파일의 모든 리소스 목록을 포함하는 덤프된 xml 파일을 출력합니다. `MakePri.exe dump /?`를 실행하여 이 명령에 대한 자세한 도움말을 봅니다.

> [!NOTE]
> 스키마가 없는 리소스 팩은 PRI config 파일에서 *omitSchemaFromResourcePacks* 스위치를 사용하여 생성한 것입니다. 스키마가 없는 리소스 팩을 덤프하려면 `/es <main_package_PRI_file>` 스위치를 사용하세요. 주 파일을 지정하지 않으면 오류 메시지 "*패키지의 resources.pri가 손상되었으므로 암호화에 실패했습니다(오류 PRI222: 0xdef0000f - 지정되지 않은 오류 발생함)*"가 표시됩니다.

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

## <a name="new-command"></a>new 명령

`new` 명령은 구성 파일의 지침대로 프로젝트에 파일을 인덱싱하여 새 PRI 파일을 만듭니다. `MakePri.exe new /?`를 실행하여 이 명령에 대한 자세한 도움말을 봅니다.

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
    /AutoMerge(am)    : This flag is not recommended for normal use with AppX
                        packages. It causes Makepri.exe to set the auto
                        merge flag within the PRI file. Default is not set.
    /ExtensionDll(ex) : <FILEPATH> Location of the Resource Management System
                        environment extension DLL. This DLL must be signed by
                        a Microsoft-issued certificate. Default is an empty path
                        (no DLL will be used)
    /IndexLog(il)     : <FILEPATH> XML Log of indexed resources, no file
                        generated by default
    /IndexName(in)    : <STRING> Name for the generated index of resources.
                        Typically matches the AppX package name, class library
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

`resourcepack` 명령은 구성 파일의 지침대로 프로젝트에 파일을 인덱싱하여 새 PRI 파일을 만듭니다. 리소스 팩 PRI 파일에는 이미 기존 PRI 파일에 지정된 리소스의 추가 변형만 포함되어 있습니다. `MakePri.exe resourcepack /?`를 실행하여 이 명령에 대한 자세한 도움말을 봅니다.

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
    /AutoMerge(am)    : This flag is not recommended for normal use with AppX
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

## <a name="versioned-command"></a>versioned 명령

`versioned` 명령은 구성 파일의 지침대로 프로젝트에 파일을 인덱싱하여 버전이 지정된 PRI 파일을 만듭니다. `MakePri.exe versioned /?`를 실행하여 이 명령에 대한 자세한 도움말을 봅니다.

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
    /AutoMerge(am)    : This flag is not recommended for normal use with AppX
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

## <a name="47extensiondllex"></a>/ExtensionDll(ex)

`createconfig`, `dump`, `new`, `resourcepack`, 및 `versioned`와 확장 DLL 옵션(/ex)을 사용하여 리소스 관리 시스템 환경 확장 DLL의 위치를 지정합니다.

## <a name="logging47metadata-file"></a>Logging/메타데이터 파일

MakePri는 인덱서 메타데이터 파일에 리소스 팩 관련 정보를 포함할 수 있습니다. 다음은 `german.pri` 및 `highresolution.pri` 리소스 PRI 파일이 있는 `resources.pri`에 대한 로그 파일의 예입니다.

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

## <a name="47indexfileif-option"></a>/IndexFile(if) 옵션

`dump`, `resourcepack`, `versioned`에 인덱스 파일 옵션(/if)을 사용하여 입력 PRI 파일을 지정할 수 있습니다.

`resourcepack` 및 `versioned`의 경우 /IndexFile(if)에 대한 입력 매개 변수로 PRI 파일을 제공하는 대신 스키마 파일을 제공할 수 있습니다.

```console
/IndexFile(if) <FILEPATH>
```

**FILEPATH**는 입력 PRI 파일이나 PRI 스키마 파일의 위치를 지정하는 토큰입니다.

## <a name="47indexoptionsio-option"></a>& #47;IndexOptions(io) 옵션

인덱스 옵션을 사용 하 여 (/ io)와 `new`, `resourcepack`, 및 `versioned` 리소스 인덱서의 동작을 보다 세부적된으로 제어를 제공 하는 옵션을 지정 합니다. 인덱스 옵션은 기본적으로 비활성화 됩니다.

```console
/IndexOptions(io) <OPTIONS>
```

**옵션** 은 다음과 같은 옵션으로 이루어진 쉼표로 구분 된 목록입니다.

- + /-HiddenFiles(hf) 합니다. (+) 인덱싱하거나 (-)를 무시 숨김 파일 및 폴더.
- + /-LinkedFiles(lf) 합니다. (+) 인덱싱하거나 (-)를 무시 파일 및 폴더를 연결 합니다.

## <a name="47mappingfilemf-option"></a>/MappingFile(mf) 옵션

`new`, `resourcepack`, 및 `versioned`와 매핑 파일 옵션(/mf)을 사용하여 매핑 파일을 생성할 수 있습니다. [MakeAppx.exe](../packaging/create-app-package-with-makeappx-tool.md)는 매핑 파일을 사용하여 앱 패키지를 생성합니다.

```console
/MappingFile(mf) <MAPPINGFILETYPE>
```

**MAPPINGFILETYPE**은 매핑 파일의 형식을 지정하는 토큰입니다. 지원되는 유효한 형식은 `appx`뿐입니다.

```console
/mf appx
```

다음은 주 매핑 파일 콘텐츠의 예입니다.

```console
"ResourceDimensions"                   "language-de-de"
```

다음은 리소스 팩 매핑 파일 콘텐츠의 예입니다.

```console
"ResourceId"                           "Resources184.la5decaf08"
"ResourceDimensions"                   "language-de-de"
```

## <a name="output-summary"></a>출력 요약

팩 리소스를 만든 경우 MakePRI.exe에서 출력 요약은 더 자세한 양식을 띱니다. 예를 들면 다음과 같습니다.

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

## <a name="47overwriteo-option"></a>/Overwrite(o) 옵션

덮어쓰기 옵션(/o)을 제공하지 않고 지정된 출력 파일 이미 있는 경우 MakePri.exe는 덮어쓰기 전에 확인이 필요합니다.

```console
Following file(s) already exist at output location:
<file(s)>
Overwrite these file(s)? [Y]es (any other key to cancel):
```

## <a name="47outputfileof-option"></a>/OutputFile(of) 옵션

`dump`, `new`, `resourcepack`, 및 `versioned`와 출력 파일 옵션(/of)을 사용하여 출력 위치와 생성할 PRI 파일의 이름을 지정할 수 있습니다. MakePri.exe가 두 개 이상의 리소스 PRI 파일을 생성하는 경우 이 파일을 대상 파일의 상위 폴더에 배치합니다. 예를 들어, 사용자가 `/of MyParentFolder\TargetFile.pri`를 지정하면 MakePri.exe는 `ParentFolder` 아래에 `TargetFile.pri`와 함께 `TargetFile.language-en.pri` 및 `TargetFile.scale-100.pri`를 생성합니다.

다음은 오류 조건의 예와 해당 오류 메시지입니다.

| 오류 조건 | 오류 메시지 |
| --------------- | ------------- |
| 출력 파일 이름은 구성의 리소스 팩 이름 중 하나와 같습니다. | 잘못된 구성: 리소스 팩 이름 <resource pack name>이(가) 출력 파일 <outputfilename.pri>와 동일해야 합니다. |

## <a name="reversemaprm-option"></a>/ReverseMap(rm) 옵션

`new`, `resourcepack`, 및 `versioned`와 역방향 맵 옵션(/rm)을 사용하여 PRI 파일에 디버깅에 사용할 수 있는 역방향 매핑 섹션을 생성할 수 있습니다.

## <a name="47schemafilesf-option"></a>/SchemaFile(sf) 옵션

`new`, `resourcepack`, 및 `versioned`와 스키마 파일 옵션(/sf)을 사용하면 지정된 위치에 스키마 파일을 작성할 수 있습니다.

`resourcepack` 및 `versioned`의 경우 /IndexFile(if)에 대한 입력 매개 변수로 PRI 파일을 제공하는 대신 스키마 파일을 제공할 수 있습니다.

```console
/SchemaFile(sf) <FILEPATH>
```

**FILEPATH**는 스키마 파일을 쓸 수 있는 위치를 지정하는 토큰입니다.

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

## <a name="47versionmajorvma-is-deprecated"></a>/VersionMajor(vma)는 더 이상 사용되지 않습니다.

(`new` 명령에 대한) 주 버전(/vma) 옵션은 더 이상 사용되지 않으며 이를 사용하면 경고 메시지가 표시됩니다.

```console
'VersionMajor (vma)' input parameter has been deprecated. Please specify major version in the configuration file using 'majorVersion' attribute on 'resources' node.
```

주 버전 번호를 제공하려면 구성 파일의 [resources@majorVersion](makepri-exe-configuration.md) 특성을 사용합니다.

## <a name="related-topics"></a>관련 항목

* [MakePri.exe](compile-resources-manually-with-makepri.md)
