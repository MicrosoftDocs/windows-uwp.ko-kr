---
description: 이 항목에서는 MakePri.exe XML 구성 파일의 스키마에 대해 설명 합니다.
title: MakePri.exe 구성 파일
template: detail.hbs
ms.date: 10/18/2017
ms.topic: article
keywords: Windows 10, UWP, 리소스, 이미지, 자산, MRT, 한정자
ms.localizationpriority: medium
ms.openlocfilehash: 5427927322f61f44cf3a8b53112f5f7811520290
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93031726"
---
# <a name="makepriexe-configuration-file"></a>MakePri.exe 구성 파일

이 항목에서는 [MakePri.exe](compile-resources-manually-with-makepri.md) XML 구성 파일의 스키마에 대해 설명 합니다. PRI 구성 파일이 라고도 합니다. MakePri.exe 도구에는 초기화 된 새 PRI 구성 파일을 만드는 데 사용할 수 있는 [createconfig 명령이](makepri-exe-command-options.md#createconfig-command) 있습니다.

> [!NOTE]
> MakePri.exe은 Windows 소프트웨어 개발 키트를 설치 하는 동안 **UWP 관리 앱에 대 한 Windows SDK** 옵션을 선택 하면 설치 됩니다. 경로 뿐만 아니라 `%WindowsSdkDir%bin\<WindowsTargetPlatformVersion>\x64\makepri.exe` 다른 아키텍처에 대해 이름이 지정 된 폴더에도 설치 됩니다. 예: `C:\Program Files (x86)\Windows Kits\10\bin\10.0.17713.0\x64\makepri.exe`.

PRI 구성 파일은 인덱싱되는 리소스 및 방법을 제어 합니다. 구성 XML은 다음 스키마를 준수 해야 합니다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<xs:schema attributeFormDefault="unqualified" elementFormDefault="qualified" xmlns:xs="http://www.w3.org/2001/XMLSchema">
  <xs:element name="resources">
    <xs:complexType>
      <xs:sequence>
        <xs:element name="packaging" maxOccurs="1" minOccurs="0">
          <xs:complexType>
            <xs:sequence>
              <xs:element name="autoResourcePackage" maxOccurs="unbounded" minOccurs="0">
                <xs:complexType>
                  <xs:attribute name="qualifier" type="xs:string" use="required" />
                </xs:complexType>
              </xs:element>
              <xs:element name="resourcePackage" maxOccurs="unbounded" minOccurs="0">
                <xs:complexType>
                  <xs:sequence>
                    <xs:element name="qualifierSet" maxOccurs="unbounded" minOccurs="0">
                      <xs:complexType>
                        <xs:attribute name="definition" type="xs:string" use="required" />
                      </xs:complexType>
                    </xs:element>
                  </xs:sequence>
                  <xs:attribute name="name" type="xs:string" use="required" />
                </xs:complexType>
              </xs:element>
            </xs:sequence>
          </xs:complexType>
        </xs:element>
        <xs:element maxOccurs="unbounded" name="index">
          <xs:complexType>
            <xs:sequence>
              <xs:element name="qualifiers" minOccurs="0" maxOccurs="unbounded">
                <xs:complexType>
                  <xs:sequence>
                    <xs:element minOccurs="1" maxOccurs="unbounded" name="qualifier">
                      <xs:complexType>
                        <xs:attribute name="name" type="xs:string" use="required" />
                        <xs:attribute name="value" type="xs:string" use="required" />
                      </xs:complexType>
                    </xs:element>
                  </xs:sequence>
                </xs:complexType>
              </xs:element>
              <xs:element name="default" minOccurs="0" maxOccurs="unbounded">
                <xs:complexType>
                  <xs:sequence>
                    <xs:element minOccurs="1" maxOccurs="unbounded" name="qualifier">
                      <xs:complexType>
                        <xs:attribute name="name" type="xs:string" use="required" />
                        <xs:attribute name="value" type="xs:string" use="required" />
                      </xs:complexType>
                    </xs:element>
                  </xs:sequence>
                </xs:complexType>
              </xs:element>
              <xs:element name="indexer-config" minOccurs="0" maxOccurs="unbounded">
                <xs:complexType>
                  <xs:sequence>
                    <xs:any minOccurs="0" maxOccurs="unbounded" processContents="skip"/>
                  </xs:sequence>
                  <xs:attribute name="type" type="xs:string" use="required" />
                  <xs:anyAttribute processContents="skip"/>
                </xs:complexType>
              </xs:element>
            </xs:sequence>
            <xs:attribute name="root" type="xs:string" use="required" />
            <xs:attribute name="startIndexAt" type="xs:string" use="required" />
          </xs:complexType>
        </xs:element>
      </xs:sequence>
      <xs:attribute name="isDeploymentMergeable" type="xs:boolean" use="optional" />
      <xs:attribute name="majorVersion" type="xs:positiveInteger" use="optional" />
      <xs:attribute name="targetOsVersion" type="xs:string" use="optional" />
    </xs:complexType>
  </xs:element>
</xs:schema>
```

- `default`요소는 런타임 컨텍스트가 리소스 후보와 일치 하지 않는 경우 리소스를 확인 하는 데 사용 해야 하는 컨텍스트 (언어, 배율, 대비 등)를 지정 합니다. 이 컨텍스트는 빌드 시에 지정 되 고 변경 되지 않으므로 한정자가 만들어질 때 리소스가이 컨텍스트로 확인 됩니다. 일치 하는 점수는 빌드 시에 저장 됩니다. 모든 한정자에는 지정 된 일부 값이 있어야 합니다. 리소스를 선택 하는 방법에 대 한 자세한 내용은 [Resourcecontext](resource-management-system.md#resourcecontext) 를 참조 하세요.
- `index`요소는 자산에 대해 수행 되는 불연속 인덱싱 패스를 정의 합니다. 각 인덱싱 패스는 사용할 [형식 특정 인덱서](makepri-exe-format-specific-indexers.md) 및 인덱싱할 리소스를 결정 합니다.
- `qualifiers`요소는 다른 리소스가 상속 하는 첫 번째 파일이 나 폴더에 대 한 초기 한정자를 설정 합니다. 각 한정자 요소에는 올바른 이름과 값이 있어야 합니다 ( [언어, 크기 조정, 고대비 및 기타 한정자에 대 한 리소스](tailor-resources-lang-scale-contrast.md)조정 참조).
- `root`특성은 인덱스 패스에 대 한 물리적 파일의 루트 경로입니다. 상대 경로 이거나 절대 경로일 수 있습니다. 상대 경로 이면 명령줄에 제공 하는 프로젝트 루트에 추가 됩니다. 절대 uri 인 경우 인덱스 pass root로 직접 사용 됩니다. 백슬래시 또는 슬래시를 사용할 수 있습니다. 후행 슬래시가 잘립니다. 인덱스 패스의 루트는 모든 리소스가 상대 경로로 간주 되는 폴더를 결정 합니다.
- `startIndexAt`특성은 인덱싱에 사용 되는 초기 초기값 파일 또는 폴더입니다. 인덱스 패스 루트를 기준으로 합니다. 빈 값은 index pass root 폴더를 가정 합니다.

## <a name="default-pri-config-file"></a>기본 PRI 구성 파일

MakePri.exe [createconfig 명령이](makepri-exe-command-options.md#createconfig-command) 실행 될 때 초기화 된 새 PRI 구성 파일을 생성 합니다.

```xml
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<resources targetOsVersion="10.0.0" majorVersion="1">
  <packaging>
    <autoResourcePackage qualifier="Language"/>
    <autoResourcePackage qualifier="Scale"/>
    <autoResourcePackage qualifier="DXFeatureLevel"/>
  </packaging>
  <index root="\" startIndexAt="\">
    <default>
      <qualifier name="Language" value="en-US"/>
      <qualifier name="Contrast" value="standard"/>
      <qualifier name="Scale" value="100"/>
      <qualifier name="HomeRegion" value="001"/>
      <qualifier name="TargetSize" value="256"/>
      <qualifier name="LayoutDirection" value="LTR"/>
      <qualifier name="Theme" value="dark"/>
      <qualifier name="AlternateForm" value=""/>
      <qualifier name="DXFeatureLevel" value="DX9"/>
      <qualifier name="Configuration" value=""/>
      <qualifier name="DeviceFamily" value="Universal"/>
      <qualifier name="Custom" value=""/>
    </default>
    <indexer-config type="folder" foldernameAsQualifier="true" filenameAsQualifier="true" qualifierDelimiter="."/>
    <indexer-config type="resw" convertDotsToSlashes="true" initialPath=""/>
    <indexer-config type="resjson" initialPath=""/>
    <indexer-config type="PRI"/>
  </index>
  <!--<index startIndexAt="Start Index Here" root="Root Here">-->
  <!--        <indexer-config type="resfiles" qualifierDelimiter="."/>-->
  <!--        <indexer-config type="priinfo" emitStrings="true" emitPaths="true" emitEmbeddedData="true"/>-->
  <!--</index>-->
</resources>
```

## <a name="packaging-element"></a>패키징 요소

`packaging`요소는 PRI 분할 정보를 정의 합니다. 요소에 대 한 스키마는 `packaging` 자동 (특정 차원에 대 한 지원 `autoResourcePackage` ) 및 수동 구성 모두에 대해 정의 됩니다.

이 예에서는 특정 차원을 따라를 사용 하는 방법을 보여 줍니다 `autoResourcePackage` .

```xml
    <packaging>
        <autoResourcePackage qualifier="Language"/>
        <autoResourcePackage qualifier="Scale"/>
        <autoResourcePackage qualifier="DXFeatureLevel"/>
    </packaging>
```

이 예에서는 수동을 사용 하는 방법을 보여 줍니다 `resourcePackage` .

```xml
  <packaging>
    <resourcePackage name="Germany">
      <qualifierSet definition="lang-de-de"/>
      <qualifierSet definition="lang-es-es"/>
    </resourcePackage>  
    <resourcePackage name="France">
      <qualifierSet definition="lang-fr-fr"/>
    </resourcePackage>  
    <resourcePackage name="HighRes1">
      <qualifierSet definition="scale-200"/>
    </resourcePackage>
    <resourcePackage name="HighRes2">
      <qualifierSet definition="scale-400"/>
    </resourcePackage>
  </packaging>
```

MakePri.exe는 특정 차원과 함께 리소스 PRI 파일 생성을 명시적으로 차단 하지 않습니다. 특정 차원 집합에 따른 제한은 MakeAppx.exe 또는 파이프라인의 다른 도구에 의해 외부적으로 정의 및 구현 됩니다.

모든 노드 뒤에 있는 요소를 구문 분석 MakePri.exe 모든 `packaging` `index` 기본 한정자를 채웁니다. MakePri.exe은 이러한 데이터 구조에서 구문 분석 된 정보를 수집 합니다.

```csharp
enum ResourcePackageMode
{
    None,
    AutoPackQualifier,
    ManualPack
}

ResourcePackageMode eResourcePackageMode;
list<string> RPQualifierList; // To store AutoResourcePackage Qualifiers
map<string, list<string>> RPNameToQSIMap; // To store ResourcePackage name to QualifierSet list mapping.
```

## <a name="resourcesisdeploymentmergeable-attribute"></a>resources@isDeploymentMergeable 특성

이 특성은 다음을 야기 하는 PRI 파일에 플래그를 설정 합니다.

- 이 PRI 파일을 병합할 수 있는지를 식별 하는 배포 병합.
- 이 플래그가 설정 되 고 리소스 관리자가 파일을 사용 하 여 초기화 된 경우 GetFullyQualifiedReference는 오류를 반환 합니다.

이 특성의 기본값은 `true`입니다. MakePri.exe은 Windows 10을 대상으로 하는 경우에만 PRI에서 플래그를 설정 합니다.

Windows 10을 대상으로 하는 `isDeploymentMergeable` `true` 경우 리소스 팩 만들기에 대해를 생략 하거나 명시적으로 설정 하는 것이 좋습니다.

`isDeploymentMergeable` `makepri dump` 옵션을 사용 하 여를 실행 하는 경우 MakePri.exe은 덤프 파일에 값을 추가 합니다 `/dt detailed` .

```xml
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<PriInfo>
    <PriHeader>
        ...
        <IsDeploymentMergeable>true</IsDeploymentMergeable>
        ...
    </PriHeader>
  ...
</PriInfo>
```

## <a name="resourcesmajorversion-attribute"></a>resources@majorVersion 특성

이 특성의 기본값은 1입니다. 명시적 값을 제공 하는 경우에도 사용 되지 않는 명령줄 옵션을 사용 하 여 `/VersionMajor(vma)` MakePri.exe 도구를 사용 하면 구성 파일의 값이 우선 적용 됩니다.

예를 들면 다음과 같습니다.

```xml
<resources majorVersion="2">
  <packaging ... />
  <index root="\" startIndexAt="\">
    ...
  </index>
</resources>
```

## <a name="resourcestargetosversion-attribute"></a>resources@targetOsVersion 특성

대상 운영 체제 버전을 나타냅니다. 다음 표에서는 지원 되는 값을 보여 줍니다. 기본값은 6.3.0입니다.

| 값 | 의미 |
| ----- | ------- |
| 10.0.0 | Windows 10 |
| 6.3.0 (기본값) | Windows 8.1 |
| 6.2.1 | Windows 8 |

예를 들면 다음과 같습니다.

```xml
<resources targetOsVersion="10.0.0">
  <packaging ... />
  <index root="\" startIndexAt="\">
    ...
  </index>
</resources>
```

**참고** Windows는 PRI 파일에 대해 이전 버전과 호환 됩니다. 하지만 항상 호환 되는 것은 아닙니다.

`targetOsVersion` `makepri dump` 옵션을 사용 하 여를 실행 하는 경우 MakePri.exe은 덤프 파일에 값을 추가 합니다 `/dt detailed` .

```xml
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<PriInfo>
    <PriHeader>
        ...
        <TargetOS version="10.0.0"/>
        ...
    </PriHeader>
  ...
</PriInfo>
```

## <a name="validation-error-messages"></a>유효성 검사 오류 메시지

다음은 몇 가지 예제 오류 조건 및 해당 오류 메시지입니다.

| 조건 | 심각도 | 메시지 |
| --------- | -------- | ------- |
| 지원 되는 값 중 하나가 아닌 targetOsVersion이 지정 되었습니다. | Error | 구성이 잘못 되었습니다. 잘못 된 targetOsVersion이 지정 되었습니다. |
| "6.2.1"의 targetOsVersion이 지정 되 고 `packaging` 요소가 있습니다. | Error | 구성이 잘못 되었습니다. ' 패키징 ' 노드는이 targetOsVersion에서 지원 되지 않습니다. |
| 구성에 둘 이상의 모드가 있습니다. 예를 들어 Manual 및 AutoResourcePackage를 지정 합니다. | Error | 구성이 잘못 되었습니다. ' 패키징을 ' 노드에는 작업 모드를 두 개 이상 사용할 수 없습니다. |
| 기본 한정자는 리소스 패키지 아래에 나열 됩니다. | Error | 잘못 된 구성: <Qualifiername> = <QualifierValue> 은 기본 한정자 이며 해당 후보를 리소스 패키지에 추가할 수 없습니다. |
| AutoResourcePackage 한정자에 여러 개의 한정자가 포함 되어 있습니다. 예를 들어 language_scale 합니다. | Error | 구성이 잘못 되었습니다. 한정자가 여러 AutoResourcePackage 지원 되지 않습니다. |
| ResourcePackage QualifierSet에는 여러 개의 한정자가 포함 되어 있습니다. 예: 언어-en-us-us_scale-100 | Error | 구성이 잘못 되었습니다. 한정자가 여러 QualifierSet 지원 되지 않습니다. |
| 중복 된 resourcepack 이름이 발견 되었습니다. | Error | 구성이 잘못 되었습니다. 리소스 팩 이름이 중복 되었습니다 <rpname> . |
| 두 리소스 패키지에 동일한 한정자 집합이 정의 되어 있습니다. | Error | 구성이 잘못 되었습니다. QualifierSet ""의 인스턴스가 여러 개 있습니다 <qualifier tags> . |
| ' ResourcePackage ' 노드에 대해 나열 된 QualifierSet에 대 한 후보를 찾을 수 없습니다. | 경고 | 구성이 잘못 되었습니다 .에 대 한 후보를 찾을 수 없습니다 <Resource Package Name> . |
| ' AutoResourcePackage ' 노드에 나열 된 한정자가 없습니다. | 경고 | 구성이 잘못 되었습니다. 한정자에 대 한 후보를 찾을 수 없습니다 <qualifier name> . 리소스 패키지가 생성 되지 않았습니다. |
| 모드를 찾을 수 없습니다. 즉, 빈 ' 패키징 ' 노드가 있습니다. | 경고 | 구성이 잘못 되었습니다. 패키징 모드가 지정 되지 않았습니다. |

## <a name="related-topics"></a>관련 항목

* [MakePri.exe를 사용하여 수동으로 리소스 컴파일](compile-resources-manually-with-makepri.md)
* [MakePri.exe 명령줄 옵션 &mdash; createconfig 명령](makepri-exe-command-options.md#createconfig-command)
* [언어, 배율, 고대비 및 기타 한정자에 맞게 리소스 조정](tailor-resources-lang-scale-contrast.md)
* [리소스 관리 시스템 &mdash; resourcecontext](resource-management-system.md#resourcecontext)
