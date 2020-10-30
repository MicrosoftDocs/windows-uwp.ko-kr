---
description: 이 항목에서는 MakePri.exe 도구에서 리소스의 인덱스를 생성 하는 데 사용 하는 형식별 인덱서를 설명 합니다.
title: MakePri.exe 형식별 인덱서
template: detail.hbs
ms.date: 10/18/2017
ms.topic: article
keywords: Windows 10, UWP, 리소스, 이미지, 자산, MRT, 한정자
ms.localizationpriority: medium
ms.openlocfilehash: 3794d369ae9d47cfc7aad1b24ca2768b04024581
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93031696"
---
# <a name="makepriexe-format-specific-indexers"></a>MakePri.exe 형식별 인덱서

이 항목에서는 [MakePri.exe](compile-resources-manually-with-makepri.md) 도구에서 리소스의 인덱스를 생성 하는 데 사용 하는 형식별 인덱서를 설명 합니다.

> [!NOTE]
> MakePri.exe은 Windows 소프트웨어 개발 키트를 설치 하는 동안 **UWP 관리 앱에 대 한 Windows SDK** 옵션을 선택 하면 설치 됩니다. 경로 뿐만 아니라 `%WindowsSdkDir%bin\<WindowsTargetPlatformVersion>\x64\makepri.exe` 다른 아키텍처에 대해 이름이 지정 된 폴더에도 설치 됩니다. 예: `C:\Program Files (x86)\Windows Kits\10\bin\10.0.17713.0\x64\makepri.exe`.

MakePri.exe은 일반적으로 `new` , 또는 명령과 함께 사용 됩니다 `versioned` `resourcepack` . [MakePri.exe 명령줄 옵션](makepri-exe-command-options.md)을 참조 하세요. 이러한 경우 원본 파일을 인덱싱하고 리소스의 인덱스를 생성 합니다. MakePri.exe는 다양 한 개별 인덱서를 사용 하 여 리소스에 대 한 다른 소스 리소스 파일이 나 컨테이너를 읽습니다. 가장 간단한 인덱서는 폴더의 콘텐츠 (예: 또는 이미지)를 인덱싱하는 폴더 인덱서입니다 `.jpg` `.png` .

`<indexer-config>` `<index>` [MakePri.exe 구성 파일](makepri-exe-configuration.md)의 요소 내에서 요소를 지정 하 여 형식별 인덱서를 식별 합니다. 특성은 사용 되는 `type` 형식별 인덱서를 식별 합니다.

인덱싱 중에 발생 하는 리소스 컨테이너는 일반적으로 인덱스 자체에 추가 되는 것이 아니라 콘텐츠를 인덱싱합니다. 예를 들어 `.resjson` 폴더 인덱서가 찾은 파일은 인덱서를 통해 추가로 인덱싱될 수 있으며, `.resjson` 이 경우 `.resjson` 파일 자체는 인덱스에 표시 되지 않습니다. **Note** 이 `<indexer-config>` 를 수행 하려면 해당 컨테이너와 연결 된 인덱서의 요소가 구성 파일에 포함 되어 있어야 합니다.

일반적으로 폴더 또는 파일과 같은 포함 하는 엔터티에 있는 한정자 &mdash; `.resw` &mdash; 는 폴더 내의 파일 또는 파일 내의 문자열과 같은 모든 리소스에 적용 됩니다 `.resw` .

## <a name="folder"></a>폴더

폴더 인덱서는 `type` 폴더의 특성으로 식별 됩니다. 폴더의 콘텐츠를 인덱싱하고 폴더 및 파일 이름에서 리소스 한정자를 결정 합니다. 해당 구성 요소는 다음 스키마를 따릅니다.

```xml
<xs:schema attributeFormDefault="unqualified" elementFormDefault="qualified" xmlns:xs="http://www.w3.org/2001/XMLSchema">
    <xs:simpleType name="ExclusionTypeList">
        <xs:restriction base="xs:string">
            <xs:enumeration value="path"/>
            <xs:enumeration value="extension"/>
            <xs:enumeration value="name"/>
            <xs:enumeration value="tree"/>
        </xs:restriction>
    </xs:simpleType>
    <xs:complexType name="FolderExclusionType">
        <xs:attribute name="type" type="ExclusionTypeList" use="required"/>
        <xs:attribute name="value" type="xs:string" use="required"/>
        <xs:attribute name="doNotTraverse" type="xs:boolean" use="required"/>
        <xs:attribute name="doNotIndex" type="xs:boolean" use="required"/>
    </xs:complexType>
    <xs:simpleType name="IndexerConfigFolderType">
        <xs:restriction base="xs:string">
            <xs:pattern value="((f|F)(o|O)(l|L)(d|D)(e|E)(r|R))"/>
        </xs:restriction>
    </xs:simpleType>
    <xs:element name="indexer-config">
        <xs:complexType>
            <xs:sequence>
                <xs:element name="exclude" type="FolderExclusionType" minOccurs="0" maxOccurs="unbounded"/>
            </xs:sequence>
            <xs:attribute name="type" type="IndexerConfigFolderType" use="required"/>
            <xs:attribute name="foldernameAsQualifier" type="xs:boolean" use="required"/>
            <xs:attribute name="filenameAsQualifier" type="xs:boolean" use="required"/>
            <xs:attribute name="qualifierDelimiter" type="xs:string" use="required"/>
        </xs:complexType>
    </xs:element>
</xs:schema>
```

`qualifierDelimiter`특성은 확장명을 무시 하 고 파일 이름에 한정자가 지정 된 후의 문자를 지정 합니다. 기본값은 "."입니다.

## <a name="pri"></a>PRI

PRI 인덱서는 `type` pri의 특성으로 식별 됩니다. PRI 파일의 콘텐츠를 인덱싱합니다. 일반적으로 다른 어셈블리, DLL, SDK 또는 클래스 라이브러리 내에 포함 된 리소스를 앱의 PRI에 인덱싱할 때 사용 합니다.

PRI 파일에 포함 된 모든 리소스 이름, 한정자 및 값은 새 PRI 파일에서 직접 유지 관리 됩니다. 그러나 최상위 리소스 맵은 최종 PRI에서 유지 되지 않습니다. 리소스 맵이 병합 됩니다.

```xml
<xs:schema id="prifile" xmlns:xs="http://www.w3.org/2001/XMLSchema" elementFormDefault="qualified">
    <xs:simpleType name="IndexerConfigPriType">
        <xs:restriction base="xs:string">
            <xs:pattern value="((p|P)(r|R)(i|I))"/>
        </xs:restriction>
    </xs:simpleType>
    <xs:element name="indexer-config">
        <xs:complexType>
            <xs:attribute name="type" type="IndexerConfigPriType" use="required"/>
        </xs:complexType>
    </xs:element>
</xs:schema>
```

## <a name="priinfo"></a>방법 정보

' 특성 정보 ' 인덱서는 ' 특성 ' 특성으로 식별 됩니다 `type` . 자세한 덤프 파일의 콘텐츠를 인덱싱합니다. 옵션으로를 실행 하 여 자세한 덤프 파일을 생성 `makepri dump` `/dt detailed` 합니다. 인덱서의 구성 요소는 다음 스키마를 따릅니다.

```xml
<xs:schema id="priinfo" xmlns:xs="http://www.w3.org/2001/XMLSchema" elementFormDefault="qualified">
  <xs:simpleType name="IndexerConfigPriInfoType">
    <xs:restriction base="xs:string">
      <xs:pattern value="((p|P)(r|R)(i|I)(i|I)(n|N)(f|F)(o|O))"/>
    </xs:restriction>
  </xs:simpleType>
  <xs:element name="indexer-config">
    <xs:complexType>
      <xs:attribute name="type" type="IndexerConfigPriInfoType" use="required"/>
      <xs:attribute name="emitStrings" type="xs:boolean" use="optional"/>
      <xs:attribute name="emitPaths" type="xs:boolean" use="optional"/>
    </xs:complexType>
  </xs:element>
</xs:schema>
```

이 구성 요소를 사용 하면 선택적 특성을 사용 하 여 대상 정보 인덱서의 동작을 구성할 수 있습니다. 및의 기본값은 `emitStrings` `emitPaths` `true` 입니다. `emitStrings`가 이면 `true` `type` 특성이 "String"으로 설정 된 리소스 후보가 인덱스에 포함 되 고, 그렇지 않으면 제외 됩니다. ' EmitPaths ' 이면 `true` `type` 특성이 "Path"로 설정 된 리소스 후보가 인덱스에 포함 되 고, 그렇지 않으면 제외 됩니다.

다음은 문자열 리소스 형식을 포함 하지만 경로 리소스 형식을 건너뛰는 구성 예제입니다.

```xml
<indexer-config type="priinfo" emitStrings="true" emitPaths="false" />
```

인덱스를 만들려면 덤프 파일이 확장으로 끝나야 `.pri.xml` 하며 다음 스키마를 준수 해야 합니다.

```xml
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema" elementFormDefault="qualified" >
  <xs:simpleType name="candidateType">
    <xs:restriction base="xs:string">
      <xs:pattern value="Path|String"/>
    </xs:restriction>
  </xs:simpleType>
  <xs:complexType name="scopeType">
    <xs:sequence>
      <xs:element name="ResourceMapSubtree" type="scopeType" minOccurs="0" maxOccurs="unbounded"/>
      <xs:element name="NamedResource" minOccurs="0" maxOccurs="unbounded">
        <xs:complexType>
          <xs:sequence>
            <xs:element name="Decision" minOccurs="0" maxOccurs="unbounded">
              <xs:complexType>
                <xs:sequence>
                  <xs:any processContents="skip" minOccurs="0" maxOccurs="unbounded"/>
                </xs:sequence>
                <xs:anyAttribute processContents="skip" />
              </xs:complexType>
            </xs:element>
            <xs:element name="Candidate" minOccurs="0" maxOccurs="unbounded">
              <xs:complexType>
                <xs:sequence>
                  <xs:element name="QualifierSet" minOccurs="0" maxOccurs="unbounded">
                    <xs:complexType>
                      <xs:sequence>
                        <xs:element name="Qualifier" minOccurs="0" maxOccurs="unbounded">
                          <xs:complexType>
                            <xs:attribute name="name" type="xs:string" use="required" />
                            <xs:attribute name="value" type="xs:string" use="required" />
                            <xs:attribute name="priority" type="xs:integer" use="required" />
                            <xs:attribute name="scoreAsDefault" type="xs:decimal" use="required" />
                            <xs:attribute name="index" type="xs:integer" use="required" />
                          </xs:complexType>
                        </xs:element>
                      </xs:sequence>
                      <xs:anyAttribute processContents="skip" />
                    </xs:complexType>
                  </xs:element>
                  <xs:element name="Value" type="xs:string"  minOccurs="0" maxOccurs="unbounded"/>
                </xs:sequence>
                <xs:attribute name="type" type="candidateType" use="required" />
              </xs:complexType>
            </xs:element>
          </xs:sequence>
          <xs:attribute name="name" use="required" type="xs:string" />
          <xs:anyAttribute processContents="skip" />
        </xs:complexType>
      </xs:element>
    </xs:sequence>
    <xs:attribute name="name" use="required" type="xs:string" />
    <xs:anyAttribute processContents="skip" />
  </xs:complexType>
  <xs:element name="PriInfo">
    <xs:complexType>
      <xs:sequence>
        <xs:element name="PriHeader" >
          <xs:complexType>
            <xs:sequence>
              <xs:any minOccurs ="0" maxOccurs="unbounded" processContents="skip" />
            </xs:sequence>
            <xs:anyAttribute processContents="skip" />
          </xs:complexType>
        </xs:element>
        <xs:element name="QualifierInfo">
          <xs:complexType>
            <xs:sequence>
              <xs:any minOccurs="0" maxOccurs="unbounded" processContents="skip" />
            </xs:sequence>
          </xs:complexType>
        </xs:element>
        <xs:element name="ResourceMap">
          <xs:complexType>
            <xs:sequence>
              <xs:element name="VersionInfo">
                <xs:complexType>
                  <xs:anyAttribute processContents="skip" />
                </xs:complexType>
              </xs:element>
              <xs:element minOccurs="0" maxOccurs="unbounded" name="ResourceMapSubtree" type="scopeType" />
            </xs:sequence>
            <xs:attribute name="name" type="xs:string" use="required" />
            <xs:anyAttribute processContents="skip" />
          </xs:complexType>
        </xs:element>
      </xs:sequence>
    </xs:complexType>
  </xs:element>
</xs:schema>
```

MakePri.exe는 ' Basic ', ' Detailed ', ' Schema ' 및 ' Summary ' 덤프 형식을 지원 합니다. 이 구성에서 사용할 수 있는 덤프 유형을 내보내도록 MakePri.exe 구성 하려면 명령을 사용 하는 경우 "/Dvtype Detailed"를 포함 `dump` 합니다.

`.pri.xml`MakePri.exe에서 파일의 여러 요소를 건너뜁니다. 이러한 요소는 인덱싱을 수행 하는 동안 계산 되거나 MakePri.exe 구성 파일에 지정 됩니다. 덤프 파일 내에 포함 된 리소스 이름, 한정자 및 값은 새 PRI 파일에서 직접 유지 관리 됩니다. 그러나 최상위 리소스 맵은 최종 PRI에서 유지 관리 되지 않습니다. 리소스 맵은 인덱싱의 일부로 병합 됩니다.

덤프 파일의 유효한 문자열 유형 리소스의 예입니다.

```xml
<NamedResource name="SampleString " index="96" uri="ms-resource://SampleApp/resources/SampleString ">
  <Decision index="2">
    <QualifierSet index="1">
      <Qualifier name="Language" value="EN-US" priority="900" scoreAsDefault="1.0" index="1"/>
    </QualifierSet>
  </Decision>
  <Candidate type="String">
    <QualifierSet index="1">
      <Qualifier name="Language" value="EN-US" priority="900" scoreAsDefault="1.0" index="1"/>
    </QualifierSet>
    <Value>A Sample String Value</Value>
  </Candidate>
</NamedResource>
```

덤프 파일의 두 후보를 사용 하는 유효한 경로 유형 리소스의 예입니다.

```xml
<NamedResource name="Sample.png" index="77" uri="ms-resource://SampleApp/Files/Images/Sample.png">
  <Decision index="2">
    <QualifierSet index="1">
      <Qualifier name="Scale" value="180" priority="500" scoreAsDefault="1.0" index="1"/>
    </QualifierSet>
    <QualifierSet index="2">
      <Qualifier name="Scale" value="140" priority="500" scoreAsDefault="0.7" index="2"/>
    </QualifierSet>
  </Decision>
  <Candidate type="Path">
    <QualifierSet index="1">
      <Qualifier name="Scale" value="180" priority="500" scoreAsDefault="1.0" index="1"/>
    </QualifierSet>
    <Value>Images\Sample.scale-180.png</Value>
  </Candidate>
  <Candidate type="Path">
    <QualifierSet index="2">
      <Qualifier name="Scale" value="140" priority="500" scoreAsDefault="1.0" index="1"/>
    </QualifierSet>
    <Value>Images\Sample.scale-140.png</Value>
  </Candidate>
</NamedResource>
```

## <a name="resfiles"></a>ResFiles

ResFiles 인덱서는 `type` resfiles 특성으로 식별 됩니다. 파일의 내용을 인덱싱합니다 `.resfiles` . 해당 구성 요소는 다음 스키마를 따릅니다.

```xml
<xs:schema id="resx" xmlns:xs="http://www.w3.org/2001/XMLSchema" elementFormDefault="qualified">
    <xs:simpleType name="IndexerConfigResFilesType">
        <xs:restriction base="xs:string">
            <xs:pattern value="((r|R)(e|E)(s|S)(f|F)(i|I)(l|L)(e|E)(s|S))"/>
        </xs:restriction>
    </xs:simpleType>
    <xs:element name="indexer-config">
        <xs:complexType>
            <xs:attribute name="type" type="IndexerConfigResFilesType" use="required"/>
            <xs:attribute name="qualifierDelimiter" type="xs:string" use="required"/>
        </xs:complexType>
    </xs:element>
</xs:schema>
```

`.resfiles`파일은 단순 파일 경로 목록을 포함 하는 텍스트 파일입니다. `.resfiles`파일은 "//" 주석을 포함할 수 있습니다. 예를 들면 다음과 같습니다.

<blockquote>
<pre>
Strings\component1\fr\elements.resjson
Images\logo.scale-100.png
Images\logo.scale-140.png
Images\logo.scale-180.png
</pre>
</blockquote>

## <a name="resjson"></a>ResJSON

ResJSON 인덱서는 `type` resjson의 특성으로 식별 됩니다. 이 `.resjson` 파일은 문자열 리소스 파일인 파일의 콘텐츠를 인덱싱합니다. 해당 구성 요소는 다음 스키마를 따릅니다.

```xml
<xs:schema id="resjson" xmlns:xs="http://www.w3.org/2001/XMLSchema" elementFormDefault="qualified">
    <xs:simpleType name="IndexerConfigResJsonType">
        <xs:restriction base="xs:string">
            <xs:pattern value="((r|R)(e|E)(s|S)(j|J)(s|S)(o|O)(n|N))"/>
        </xs:restriction>
    </xs:simpleType>
    <xs:element name="indexer-config">
        <xs:complexType>
            <xs:attribute name="type" type="IndexerConfigResJsonType" use="required"/>
            <xs:attribute name="initialPath" type="xs:string" use="optional"/>
        </xs:complexType>
    </xs:element>
</xs:schema>
```

파일에는 `.resjson` json 텍스트가 포함 되어 있습니다. json 텍스트 [는 application/Json Media Type for JAVASCRIPT OBJECT NOTATION (json)](https://www.ietf.org/rfc/rfc4627.txt)을 참조 하세요. 이 파일에는 계층적 속성이 포함 된 단일 JSON 개체가 있어야 합니다. 각 속성은 다른 JSON 개체 또는 문자열 값 이어야 합니다.

이름이 밑줄 ("_")로 시작 하는 JSON 속성은 최종 PRI 파일로 컴파일되지 않지만 로그 파일에서 유지 관리 됩니다.

이 파일에는 구문 분석 중에 무시 되는 "//" 주석이 포함 될 수도 있습니다.

`initialPath`특성은 리소스 이름 앞에 추가 하 여이 초기 경로 아래에 모든 리소스를 배치 합니다. 일반적으로 클래스 라이브러리 리소스를 인덱싱할 때이를 사용 합니다. 기본값은 없습니다.

## <a name="resw"></a>Resources.resw

ResW 인덱서는 `type` resw의 특성으로 식별 됩니다. 이 `.resw` 파일은 문자열 리소스 파일인 파일의 콘텐츠를 인덱싱합니다. 해당 구성 요소는 다음 스키마를 따릅니다.

```xml
<xs:schema id="resw" xmlns:xs="http://www.w3.org/2001/XMLSchema" elementFormDefault="qualified">
    <xs:simpleType name="IndexerConfigResxType">
        <xs:restriction base="xs:string">
            <xs:pattern value="((r|R)(e|E)(s|S)(w|W))"/>
        </xs:restriction>
    </xs:simpleType>
    <xs:element name="indexer-config">
        <xs:complexType>
            <xs:attribute name="type" type="IndexerConfigResxType" use="required"/>
            <xs:attribute name="convertDotsToSlashes" type="xs:boolean" use="required"/>
            <xs:attribute name="initialPath" type="xs:string" use="optional"/>
        </xs:complexType>
    </xs:element>
</xs:schema>
```

`.resw`파일은 다음 스키마를 따르는 XML 파일입니다.

```xml
  <xsd:schema id="root" xmlns="" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:msdata="urn:schemas-microsoft-com:xml-msdata">
    <xsd:import namespace="http://www.w3.org/XML/1998/namespace" />
    <xsd:element name="root" msdata:IsDataSet="true">
      <xsd:complexType>
        <xsd:choice maxOccurs="unbounded">
          <xsd:element name="metadata">
            <xsd:complexType>
              <xsd:sequence>
                <xsd:element name="value" type="xsd:string" minOccurs="0" />
              </xsd:sequence>
              <xsd:attribute name="name" use="required" type="xsd:string" />
              <xsd:attribute name="type" type="xsd:string" />
              <xsd:attribute name="mimetype" type="xsd:string" />
              <xsd:attribute ref="xml:space" />
            </xsd:complexType>
          </xsd:element>
          <xsd:element name="assembly">
            <xsd:complexType>
              <xsd:attribute name="alias" type="xsd:string" />
              <xsd:attribute name="name" type="xsd:string" />
            </xsd:complexType>
          </xsd:element>
          <xsd:element name="data">
            <xsd:complexType>
              <xsd:sequence>
                <xsd:element name="value" type="xsd:string" minOccurs="0" msdata:Ordinal="1" />
                <xsd:element name="comment" type="xsd:string" minOccurs="0" msdata:Ordinal="2" />
              </xsd:sequence>
              <xsd:attribute name="name" type="xsd:string" use="required" msdata:Ordinal="1" />
              <xsd:attribute name="type" type="xsd:string" msdata:Ordinal="3" />
              <xsd:attribute name="mimetype" type="xsd:string" msdata:Ordinal="4" />
              <xsd:attribute ref="xml:space" />
            </xsd:complexType>
          </xsd:element>
          <xsd:element name="resheader">
            <xsd:complexType>
              <xsd:sequence>
                <xsd:element name="value" type="xsd:string" minOccurs="0" msdata:Ordinal="1" />
              </xsd:sequence>
              <xsd:attribute name="name" type="xsd:string" use="required" />
            </xsd:complexType>
          </xsd:element>
        </xsd:choice>
      </xsd:complexType>
    </xsd:element>
  </xsd:schema>
```

`convertDotsToSlashes`특성은 점 문자가 "[" 및 "]" 사이에 있는 경우를 제외 하 고 리소스 이름 (데이터 요소 이름 특성)에 있는 모든 점 (".") 문자를 슬래시 "/"로 변환 합니다.

`initialPath`특성은 리소스 이름 앞에 추가 하 여이 초기 경로 아래에 모든 리소스를 배치 합니다. 일반적으로 클래스 라이브러리 리소스를 인덱싱할 때이를 사용 합니다. 기본값은 없습니다.

## <a name="related-topics"></a>관련 항목

* [MakePri.exe를 사용하여 수동으로 리소스 컴파일](compile-resources-manually-with-makepri.md)
* [MakePri.exe 명령줄 옵션](makepri-exe-command-options.md)
* [MakePri.exe 구성 파일](makepri-exe-configuration.md)
* [JavaScript Object Notation에 대 한 application/json 미디어 유형 (JSON)](https://www.ietf.org/rfc/rfc4627.txt)
