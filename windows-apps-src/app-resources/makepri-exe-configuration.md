---
author: stevewhims
Description: This topic describes the schema of the MakePri.exe XML configuration file.
title: MakePri.exe 구성 파일
template: detail.hbs
ms.author: stwhi
ms.date: 10/18/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, 리소스, 이미지, 자산, MRT, 한정자
ms.localizationpriority: medium
ms.openlocfilehash: 512880b7a7ea955a45697762cbbdb7f74ac70102
ms.sourcegitcommit: e16c9845b52d5bd43fc02bbe92296a9682d96926
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/19/2018
ms.locfileid: "4951156"
---
# <a name="makepriexe-configuration-file"></a>MakePri.exe 구성 파일

이 항목은 PRI 구성 파일이라고도 하는 [MakePri.exe](compile-resources-manually-with-makepri.md) XML 구성 파일의 스키마에 대해 설명합니다. MakePri.exe 도구에는 초기화된 새 PRI 구성 파일을 만드는 데 사용할 수 있는 [createconfig 명령](makepri-exe-command-options.md#createconfig-command)이 있습니다.

> [!NOTE]
> MakePri.exe는 Windows 소프트웨어 개발 키트를 설치 하는 동안 **Windows SDK UWP 관리 되는 앱에 대 한** 옵션을 선택 하면 설치 됩니다. 경로에 설치 된 `%WindowsSdkDir%bin\<WindowsTargetPlatformVersion>\x64\makepri.exe` 메모가 다른 아키텍처에 대 한 명명 된 폴더에 따라 합니다. 예를 들면 `C:\Program Files (x86)\Windows Kits\10\bin\10.0.17713.0\x64\makepri.exe`입니다.

PRI 구성 파일은 인덱싱하는 리소스와 그 방법을 제어합니다. 구성 XML은 다음 스키마를 준수해야 합니다.

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

- `default` 요소는 런타임 컨텍스트가 모든 리소스 후보와 일치하지 않는 경우 리소스를 확인하는 데 사용해야 할 컨텍스트(언어, 배율, 고대비 등)를 지정합니다. 이 컨텍스트는 빌드 시 지정되고 변경되지 않기 때문에 리소스는 한정자가 만들어지는 이 컨텍스트에서 확인됩니다. 일치하는 점수는 빌드 시에 저장됩니다. 모든 한정자에는 몇 가지 값이 지정되어야 합니다. 리소스를 선택하는 방법 대한 자세한 내용은 [ResourceContext](resource-management-system.md#resourcecontext)를 참조하세요.
- `index` 요소는 자산을 통해 완료된 개별 인덱싱 전달을 정의합니다. 각 인덱싱 전달은 사용할 [형식별 인덱서](makepri-exe-format-specific-indexers.md)와 인덱싱할 리소스를 결정합니다.
- `qualifiers` 요소는 기타 리소스가 상속하는 첫 번째 파일 또는 폴더에 대한 초기 한정자를 설정합니다. 각 한정자 요소는 유효한 이름과 값을 지녀야 합니다([언어, 배율, 고대비 및 기타 한정자에 맞게 리소스 조정](tailor-resources-lang-scale-contrast.md) 참조).
- `root` 특성은 인덱스 전달에 대한 실제 파일의 경로 루트입니다. 상대 또는 절대 경로일 수 있습니다. 상대 경로인 경우 명령줄에 제공하는 프로젝트 루트에 추가됩니다. 절대 경로인 경우 직접 인덱스 전달 루트로 사용됩니다. 슬래시 또는 백슬래시를 사용할 수 있습니다. 후행 슬래시는 잘립니다. 인덱스 전달 루트는 모든 리소스가 상대 경로로 간주되는 폴더를 결정합니다.
- `startIndexAt` 특성은 인덱싱에 사용되는 초기 시드 파일 또는 폴더입니다. 인덱스 전달 루트에 상대적입니다. 빈 값은 인덱스 전달 루트 폴더로 간주합니다.

## <a name="default-pri-config-file"></a>기본 PRI 구성 파일

MakePri.exe는[createconfig 명령](makepri-exe-command-options.md#createconfig-command)이 실행될 때 이 초기화된 새 PRI 구성 파일을 생성합니다.

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

## <a name="packaging-element"></a>Packaging 요소

`packaging` 요소는 PRI 분할 정보를 정의합니다. `packaging` 요소에 대한 스키마는 자동(특정 차원과 함께 `autoResourcePackage` 지원) 및 수동 구성 둘 다에 대해 정의됩니다.

이 예제는 특정 차원과 함께 `autoResourcePackage`를 사용하는 방법을 보여줍니다.

```xml
    <packaging>
        <autoResourcePackage qualifier="Language"/>
        <autoResourcePackage qualifier="Scale"/>
        <autoResourcePackage qualifier="DXFeatureLevel"/>
    </packaging>
```

이 예제에서는 수동 `resourcePackage`를 사용하는 방법을 보여줍니다.

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

MakePri.exe는 모든 특정 차원과 함께 리소스 PRI 파일의 생성을 명시적으로 차단하지 않습니다. 제한은 특정 차원 집합과 함께 정의되고 MakeAppx.exe 또는 파이프라인의 기타 도구에 의해 외부에서 구현됩니다.

MakePri.exe는 모든 `index` 노드 이후의 `packaging` 요소를 구문 분석하여 모든 기본 한정자를 채울 수 있습니다. MakePri.exe는 이러한 데이터 구조의 구문 분석된 정보를 수집합니다.

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

이 특성은 배포 병합을 촉진하는 플래그를 PRI 파일에 설정하여

- PRI 파일이 병합할 수 있는 배포를 파악합니다.
- 이 플래그가 설정되고 리소스 관리자가 파일로 초기화된 경우 오류를 반환할 GetFullyQualifiedReference.

이 특성의 기본값은 `true`입니다. MakePri.exe는 Windows 10을 대상으로 하는 경우에만 PRI에 플래그를 설정합니다.

Windows 10을 대상으로 하는 경우 리소스 팩에 대한 `isDeploymentMergeable`를 생략(또는 명시적으로 `true`로 설정)하는 것이 좋습니다.

MakePri.exe는 `makepri dump`가 `/dt detailed` 옵션으로 실행되는 경우 덤프 파일에 `isDeploymentMergeable` 값을 추가합니다.

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

이 특성의 기본값은 1입니다. 명시적 값을 제공하고 MakePri.exe 도구에 더 이상 사용되지 않는 `/VersionMajor(vma)` 명령줄 옵션을 사용한 경우 구성 파일의 값이 우선합니다.

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

대상 운영 체제 버전을 나타냅니다. 다음 표는 지원되는 값을 나열합니다. 기본값은 6.3.0입니다.

| 값 | 의미 |
| ----- | ------- |
| 10.0.0 | Windows10 |
| 6.3.0(기본값) | Windows 8.1 |
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

**참고** Windows는 PRI 파일에 대해 이전 버전과 호환되지만 항상 이후 버전과 호환되지는 않습니다.

MakePri.exe는 `makepri dump`가 `/dt detailed` 옵션으로 실행되는 경우 덤프 파일에 `targetOsVersion` 값을 추가합니다.

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

다음은 몇 가지 오류 조건의 예와 해당 오류 메시지입니다.

| 조건 | 심각도 | 메시지 |
| --------- | -------- | ------- |
| 지원되는 값 중 하나 이외의 targetOsVersion이 지정됩니다. | 오류 | 잘못된 구성: 지정한 targetOsVersion이 잘못되었습니다. |
| "6.2.1"의 targetOsVersion이 지정되었으며 `packaging` 요소가 있습니다. | 오류 | 잘못된 구성: 'packaging' 노드가 이 targetOsVersion에 대해 지원되지 않습니다. |
| 구성에 둘 이상의 모드를 찾았습니다. 예를 들어 Manual 및 AutoResourcePackage가 지정되었습니다. | 오류 | 잘못된 구성: 'packaging' 노드는 두 개 이상의 작업 모드를 가질 수 없습니다. |
| 기본 한정자는 리소스 패키지 아래에 표시됩니다. | 오류 | 잘못된 구성: <Qualifiername>=<QualifierValue>는 기본 한정자이며 리소스 패키지에 해당 후보를 추가할 수 없습니다. |
| AutoResourcePackage 한정자에는 여러 한정자가 포함되어 있습니다. 예: language_scale | 오류 | 잘못된 구성: 여러 한정자가 있는 AutoResourcePackage는 지원되지 않습니다. |
| ResourcePackage QualifierSet에는 여러 한정자가 포함되어 있습니다. 예: language-en-us_scale-100 | 오류 | 잘못된 구성: 여러 한정자가 있는 QualifierSet는 지원되지 않습니다. |
| 중복된 리소스 팩 이름이 있습니다. | 오류 | 잘못된 구성: 중복된 리소스 팩 이름 <rpname>입니다. |
| 두 가지 리소스 패키지에 동일한 한정자 집합이 정의되었습니다. | 오류 | 잘못된 구성: 여러 QualifierSet "<qualifier tags>" 인스턴스가 있습니다. |
| 'ResourcePackage' 노드에 나열된 QualifierSet에 대한 후보를 찾을 수 없습니다. | 경고 | 잘못된 구성: <Resource Package Name>에 대한 후보를 찾을 수 없습니다. |
| 'AutoResourcePackage' 노드 아래에 나열된 한정자에 대한 후보를 찾을 수 없습니다. | 경고 | 잘못된 구성: <qualifier name> 한정자에 대한 후보를 찾을 수 없습니다. 리소스 패키지가 생성되지 않았습니다. |
| 어떤 모드도 찾을 수 없습니다. 즉, 'packaging' 노드가 비어 있습니다. | 경고 | 잘못된 구성: 패키징 모드가 지정되지 않았습니다. |

## <a name="related-topics"></a>관련 항목

* [MakePri.exe를 사용하여 수동으로 리소스 컴파일](compile-resources-manually-with-makepri.md)
* [MakePri.exe 명령줄 옵션&mdash;createconfig 명령](makepri-exe-command-options.md#createconfig-command)
* [언어, 배율, 고대비 및 기타 한정자에 맞게 리소스 조정](tailor-resources-lang-scale-contrast.md)
* [리소스 관리 정책&mdash;ResourceContext](resource-management-system.md#resourcecontext)