---
author: stevewhims
Description: This topic describes the format-specific indexers used by the MakePri.exe tool to generate its index of resources.
title: MakePri.exe 형식별 인덱서
template: detail.hbs
ms.author: stwhi
ms.date: 10/18/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, 리소스, 이미지, 자산, MRT, 한정자
ms.localizationpriority: medium
ms.openlocfilehash: 8ec6b2a31f4f577de30dac1c96a411c6aee6e9dc
ms.sourcegitcommit: e16c9845b52d5bd43fc02bbe92296a9682d96926
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/19/2018
ms.locfileid: "4946688"
---
# <a name="makepriexe-format-specific-indexers"></a>MakePri.exe 형식별 인덱서

이 항목에서는 리소스 인덱스를 생성하기 위해 [MakePri.exe](compile-resources-manually-with-makepri.md) 도구에 의해 사용되는 형식별 인덱서에 대해 설명합니다.

> [!NOTE]
> MakePri.exe는 Windows 소프트웨어 개발 키트를 설치 하는 동안 **Windows SDK UWP 관리 되는 앱에 대 한** 옵션을 선택 하면 설치 됩니다. 경로에 설치 된 `%WindowsSdkDir%bin\<WindowsTargetPlatformVersion>\x64\makepri.exe` 메모가 다른 아키텍처에 대 한 명명 된 폴더에 따라 합니다. 예를 들면 `C:\Program Files (x86)\Windows Kits\10\bin\10.0.17713.0\x64\makepri.exe`입니다.

MakePri.exe는 일반적으로 `new`, `versioned`, 또는 `resourcepack` 명령과 함께 사용됩니다. [MakePri.exe 명령줄 옵션](makepri-exe-command-options.md)을 참조하세요. 이러한 경우 원본 파일을 인덱싱하여 리소스 인덱스를 생성합니다. MakePri.exe는 다른 소스 리소스 파일 또는 리소스 컨테이너를 읽기 위해 다양한 개별 인덱서를 사용합니다. 가장 단순한 인덱서는 폴더 인덱서이며 `.jpg`또는 `.png` 이미지 등의 폴더의 콘텐츠를 인덱싱합니다.

[MakePri.exe 구성 파일](makepri-exe-configuration.md)의 `<index>`요소 내에 `<indexer-config>` 요소를 지정하여 형식별 인덱서를 식별합니다. `type` 특성은 사용되는 형식별 인덱서를 식별합니다.

일반적으로 색인하는 동안 마주치는 리소스 컨테이너는 인덱스 자체에 추가되는 대신 콘텐츠가 인덱싱됩니다. 예를 들어 폴더 인덱서가 찾는 `.resjson` 폴더는 `.resjson` 인덱서에 의해 추가로 인덱싱될 수 있으며 이 경우 `.resjson` 파일 자체는 인덱스에 표시되지 않습니다. **참고** 이를 위해서는 해당 컨테이너와 관련된 `<indexer-config>` 요소가 구성 파일에 포함되어야 합니다.

일반적으로 폴더 또는 파일과 같이 이를 포함하는 엔터티에서 찾을 수 있는 한정자는 해당 폴더 또는 `.resw` 파일 내에 있는 모든 리소스에 적용됩니다. 예를 들면 폴더 내의 파일, 또는 `.resw` 파일 내의 문자열입니다.

## <a name="folder"></a>폴더

폴더 인덱서는 FOLDER의 `type` 특성에 의해 식별됩니다. 폴더의 콘텐츠를 인덱싱하며 폴더 및 파일 이름에서 리소스 한정자를 결정합니다. 구성 요소는 다음 스키마를 준수합니다.

```xml
<xs:schema attributeFormDefault=\"unqualified\" elementFormDefault=\"qualified\" xmlns:xs=\"http://www.w3.org/2001/XMLSchema\">\
    <xs:simpleType name=\"ExclusionTypeList\">\
        <xs:restriction base=\"xs:string\">\
            <xs:enumeration value=\"path\"/>\
            <xs:enumeration value=\"extension\"/>\
            <xs:enumeration value=\"name\"/>\
            <xs:enumeration value=\"tree\"/>\
        </xs:restriction>\
    </xs:simpleType>\
    <xs:complexType name=\"FolderExclusionType\">\
        <xs:attribute name=\"type\" type=\"ExclusionTypeList\" use=\"required\"/>\
        <xs:attribute name=\"value\" type=\"xs:string\" use=\"required\"/>\
        <xs:attribute name=\"doNotTraverse\" type=\"xs:boolean\" use=\"required\"/>\
        <xs:attribute name=\"doNotIndex\" type=\"xs:boolean\" use=\"required\"/>\
    </xs:complexType>\
    <xs:simpleType name=\"IndexerConfigFolderType\">\
        <xs:restriction base=\"xs:string\">\
            <xs:pattern value=\"((f|F)(o|O)(l|L)(d|D)(e|E)(r|R))\"/>\
        </xs:restriction>\
    </xs:simpleType>\
    <xs:element name=\"indexer-config\">\
        <xs:complexType>\
            <xs:sequence>\
                <xs:element name=\"exclude\" type=\"FolderExclusionType\" minOccurs=\"0\" maxOccurs=\"unbounded\"/>\
            </xs:sequence>\
            <xs:attribute name=\"type\" type=\"IndexerConfigFolderType\" use=\"required\"/>\
            <xs:attribute name=\"foldernameAsQualifier\" type=\"xs:boolean\" use=\"required\"/>\
            <xs:attribute name=\"filenameAsQualifier\" type=\"xs:boolean\" use=\"required\"/>\
            <xs:attribute name=\"qualifierDelimiter\" type=\"xs:string\" use=\"required\"/>\
        </xs:complexType>\
    </xs:element>\
</xs:schema>
```

`qualifierDelimiter` 특성은 문자를 지정하고 그 이후에 한정자는 파일 이름에 지정됩니다(확장은 무시됨). 기본값은 "."입니다.

## <a name="pri"></a>PRI

PRI 인덱서는 PRI의 `type` 특성에 의해 식별됩니다. PRI 파일의 콘텐츠를 인덱싱합니다. 일반적으로 다른 어셈블리, DLL, SDK 또는 클래스 라이브러리에 포함된 리소스를 앱의 PRI에 인덱싱할 때 사용합니다.

PRI 파일에 포함된 모든 리소스 이름, 한정자, 값은 새 PRI 파일에 직접 유지됩니다. 그러나 최상위 수준 리소스 맵은 최종 PRI에 유지되지 않습니다. 리소스 맵은 병합됩니다.

```xml
<xs:schema id=\"prifile\" xmlns:xs=\"http://www.w3.org/2001/XMLSchema\" elementFormDefault=\"qualified\">\
    <xs:simpleType name=\"IndexerConfigPriType\">\
        <xs:restriction base=\"xs:string\">\
            <xs:pattern value=\"((p|P)(r|R)(i|I))\"/>\
        </xs:restriction>\
    </xs:simpleType>\
    <xs:element name=\"indexer-config\">\
        <xs:complexType>\
            <xs:attribute name=\"type\" type=\"IndexerConfigPriType\" use=\"required\"/>\
        </xs:complexType>\
    </xs:element>\
</xs:schema>\
```

## <a name="priinfo"></a>PriInfo

PriInfo 인덱서는 PRIINFO의 `type` 특성에 의해 식별됩니다. 세부 덤프 파일의 콘텐츠를 인덱싱합니다. `makepri dump`를 `/dt detailed` 옵션으로 실행하여 세부 덤프 파일을 생성합니다. 인덱서에 대한 구성 요소는 다음 스키마를 준수합니다.

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

이 구성 요소를 사용하여 선택적 특성으로 PriInfo 인덱서의 동작을 구성할 수 있습니다. 기본값 `emitStrings` 및 `emitPaths`는 `true`입니다. `emitStrings`가 `true`인 경우 `type` 특성이 "String"으로 설정된 리소스 후보는 인덱스에 포함되고 그렇지 않으면 제외됩니다. 'emitPaths'가 `true`인 경우 `type` 특성이 "Path"로 설정된 리소스 후보는 인덱스에 포함되고 그렇지 않으면 제외됩니다.

String 리소스 유형을 포함하지만 Path 리소스 유형은 건너뛰는 구성 예는 다음과 같습니다.

```xml
<indexer-config type="priinfo" emitStrings="true" emitPaths="false" />
```

인덱스되려면 덤프 파일은 `.pri.xml` 확장명으로 끝나야 하며 다음 스키마를 따라야 합니다.

```xml
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema" elementFormDefault="qualified" >\
  <xs:simpleType name="candidateType">\
    <xs:restriction base="xs:string">\
      <xs:pattern value="Path|String"/>\
    </xs:restriction>\
  </xs:simpleType>\
  <xs:complexType name="scopeType">\
    <xs:sequence>\
      <xs:element name="ResourceMapSubtree" type="scopeType" minOccurs="0" maxOccurs="unbounded"/>\
      <xs:element name="NamedResource" minOccurs="0" maxOccurs="unbounded">\
        <xs:complexType>\
          <xs:sequence>\
            <xs:element name="Decision" minOccurs="0" maxOccurs="unbounded">\
              <xs:complexType>\
                <xs:sequence>\
                  <xs:any processContents="skip" minOccurs="0" maxOccurs="unbounded"/>\
                </xs:sequence>\
                <xs:anyAttribute processContents="skip" />\
              </xs:complexType>\
            </xs:element>\
            <xs:element name="Candidate" minOccurs="0" maxOccurs="unbounded">\
              <xs:complexType>\
                <xs:sequence>\
                  <xs:element name="QualifierSet" minOccurs="0" maxOccurs="unbounded">\
                    <xs:complexType>\
                      <xs:sequence>\
                        <xs:element name="Qualifier" minOccurs="0" maxOccurs="unbounded">\
                          <xs:complexType>\
                            <xs:attribute name="name" type="xs:string" use="required" />\
                            <xs:attribute name="value" type="xs:string" use="required" />\
                            <xs:attribute name="priority" type="xs:integer" use="required" />\
                            <xs:attribute name="scoreAsDefault" type="xs:decimal" use="required" />\
                            <xs:attribute name="index" type="xs:integer" use="required" />\
                          </xs:complexType>\
                        </xs:element>\
                      </xs:sequence>\
                      <xs:anyAttribute processContents="skip" />\
                    </xs:complexType>\
                  </xs:element>\
                  <xs:element name="Value" type="xs:string"  minOccurs="0" maxOccurs="unbounded"/>\
                </xs:sequence>\
                <xs:attribute name="type" type="candidateType" use="required" />\
              </xs:complexType>\
            </xs:element>\
          </xs:sequence>\
          <xs:attribute name="name" use="required" type="xs:string" />\
          <xs:anyAttribute processContents="skip" />\
        </xs:complexType>\
      </xs:element>\
    </xs:sequence>\
    <xs:attribute name="name" use="required" type="xs:string" />\
    <xs:anyAttribute processContents="skip" />\
  </xs:complexType>\
  <xs:element name="PriInfo">\
    <xs:complexType>\
      <xs:sequence>\
        <xs:element name="PriHeader" >\
          <xs:complexType>\
            <xs:sequence>\
              <xs:any minOccurs ="0" maxOccurs="unbounded" processContents="skip" />\
            </xs:sequence>\
            <xs:anyAttribute processContents="skip" />\
          </xs:complexType>\
        </xs:element>\
        <xs:element name="QualifierInfo">\
          <xs:complexType>\
            <xs:sequence>\
              <xs:any minOccurs="0" maxOccurs="unbounded" processContents="skip" />\
            </xs:sequence>\
          </xs:complexType>\
        </xs:element>\
        <xs:element name="ResourceMap">\
          <xs:complexType>\
            <xs:sequence>\
              <xs:element name="VersionInfo">\
                <xs:complexType>\
                  <xs:anyAttribute processContents="skip" />\
                </xs:complexType>\
              </xs:element>\
              <xs:element minOccurs="0" maxOccurs="unbounded" name="ResourceMapSubtree" type="scopeType" />\
            </xs:sequence>\
            <xs:attribute name="name" type="xs:string" use="required" />\
            <xs:anyAttribute processContents="skip" />\
          </xs:complexType>\
        </xs:element>\
      </xs:sequence>\
    </xs:complexType>\
  </xs:element>\
</xs:schema>
```

MakePri.exe는 '기본', '자세히', '스키마' 및 '요약' 덤프 유형을 지원합니다. MakePri.exe를 구성하여 PriInfo 인덱서가 읽을 수 있는 덤프 유형을 내보내려면 `dump` 명령을 사용할 때 "/DumpType Detailed"를 포함합니다.

MakePri.exe는 `.pri.xml` 파일의 여러 요소를 건너뜁니다. 이러한 요소는 인덱싱하는 동안 계산되거나 MakePri.exe 구성 파일에서 지정됩니다. 덤프 파일에 포함된 리소스 이름, 한정자, 값은 새 PRI 파일에 직접 유지됩니다. 그러나 최상위 수준 리소스 맵은 최종 PRI에 유지되지 않습니다. 리소스 맵은 인덱싱의 일환으로 병합됩니다.

다음은 덤프 파일의 유효한 문자열 유형 리소스의 예입니다.

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

다음은 덤프 파일의 두 후보가 있는 유효한 경로 유형 리소스의 예입니다.

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

ResFiles 인덱서는 RESFILES의 `type` 특성에 의해 식별됩니다. `.resfiles` 파일의 콘텐츠를 인덱싱합니다. 구성 요소는 다음 스키마를 준수합니다.

```xml
<xs:schema id=\"resx\" xmlns:xs=\"http://www.w3.org/2001/XMLSchema\" elementFormDefault=\"qualified\">\
    <xs:simpleType name=\"IndexerConfigResFilesType\">\
        <xs:restriction base=\"xs:string\">\
            <xs:pattern value=\"((r|R)(e|E)(s|S)(f|F)(i|I)(l|L)(e|E)(s|S))\"/>\
        </xs:restriction>\
    </xs:simpleType>\
    <xs:element name=\"indexer-config\">\
        <xs:complexType>\
            <xs:attribute name=\"type\" type=\"IndexerConfigResFilesType\" use=\"required\"/>\
            <xs:attribute name=\"qualifierDelimiter\" type=\"xs:string\" use=\"required\"/>\
        </xs:complexType>\
    </xs:element>\
</xs:schema>\
```

`.resfiles` 파일은 파일 경로의 단순 목록을 포함하는 텍스트 파일입니다. `.resfiles` 파일에는 "//" 설명이 포함될 수 있습니다. 예를 들면 다음과 같습니다.

```
Strings\component1\fr\elements.resjson
Images\logo.scale-100.png
Images\logo.scale-140.png
Images\logo.scale-180.png
```

## <a name="resjson"></a>ResJSON

ResJSON 인덱서는 RESJSON의 `type` 특성에 의해 식별됩니다. 문자열 리소스 파일인 `.resjson` 파일의 콘텐츠를 인덱싱합니다. 구성 요소는 다음 스키마를 준수합니다.

```xml
<xs:schema id=\"resjson\" xmlns:xs=\"http://www.w3.org/2001/XMLSchema\" elementFormDefault=\"qualified\">\
    <xs:simpleType name=\"IndexerConfigResJsonType\">\
        <xs:restriction base=\"xs:string\">\
            <xs:pattern value=\"((r|R)(e|E)(s|S)(j|J)(s|S)(o|O)(n|N))\"/>\
        </xs:restriction>\
    </xs:simpleType>\
    <xs:element name=\"indexer-config\">\
        <xs:complexType>\
            <xs:attribute name=\"type\" type=\"IndexerConfigResJsonType\" use=\"required\"/>\
            <xs:attribute name=\"initialPath\" type=\"xs:string\" use=\"optional\"/>\
        </xs:complexType>\
    </xs:element>\
</xs:schema>\
```

`.resjson` 파일에는 JSON 텍스트가 포함됩니다([JSON(JavaScript Object Notation)에 대한 응용 프로그램/json 미디어 유형](http://www.ietf.org/rfc/rfc4627.txt) 참조). 파일에는 계층적 속성이 있는 JSON 개체가 있어야 합니다. 각 속성은 다른 JSON 개체 또는 문자열 값이어야 합니다.

밑줄("_")로 시작하는 이름을 가진 JSON 속성은 최종 PRI 파일로 컴파일되지는 않지만 로그 파일에서 유지됩니다.

파일에는 구문 분석을 수행하는 동안 무시되는 "//" 설명이 포함될 수도 있습니다.

`initialPath` 특성은 이 초기 경로를 리소스의 이름 앞에 추가하여 모든 리소스를 초기 경로 아래에 배치합니다. 일반적으로 클래스 라이브러리 리소스를 인덱싱할 때 사용합니다. 기본값은 blank입니다.

## <a name="resw"></a>ResW

ResW 인덱서는 RESW의 `type` 특성에 의해 식별됩니다. 문자열 리소스 파일인 `.resw` 파일의 콘텐츠를 인덱싱합니다. 구성 요소는 다음 스키마를 준수합니다.

```xml
<xs:schema id=\"resw\" xmlns:xs=\"http://www.w3.org/2001/XMLSchema\" elementFormDefault=\"qualified\">\
    <xs:simpleType name=\"IndexerConfigResxType\">\
        <xs:restriction base=\"xs:string\">\
            <xs:pattern value=\"((r|R)(e|E)(s|S)(w|W))\"/>\
        </xs:restriction>\
    </xs:simpleType>\
    <xs:element name=\"indexer-config\">\
        <xs:complexType>\
            <xs:attribute name=\"type\" type=\"IndexerConfigResxType\" use=\"required\"/>\
            <xs:attribute name=\"convertDotsToSlashes\" type=\"xs:boolean\" use=\"required\"/>\
            <xs:attribute name=\"initialPath\" type=\"xs:string\" use=\"optional\"/>\
        </xs:complexType>\
    </xs:element>\
</xs:schema>\
```

`.resw` 파일은 다음 스키마에 맞는 XML 파일입니다.

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

마침표 기호가 "[" 및 "]" 사이에 오는 경우를 제외하고 `convertDotsToSlashes` 특성은 리소스 이름(데이터 요소 이름 특성)에서 찾을 수 있는 모든 마침표(".")를 슬래시("/")로 변환합니다.

`initialPath` 특성은 이 초기 경로를 리소스의 이름 앞에 추가하여 모든 리소스를 초기 경로 아래에 배치합니다. 일반적으로 클래스 라이브러리 리소스를 인덱싱할 때 사용합니다. 기본값은 blank입니다.

## <a name="related-topics"></a>관련 항목

* [MakePri.exe를 사용하여 수동으로 리소스 컴파일](compile-resources-manually-with-makepri.md)
* [MakePri.exe 명령줄 옵션](makepri-exe-command-options.md)
* [MakePri.exe 구성 파일](makepri-exe-configuration.md)
* [JSON(JavaScript Object Notation)에 대한 응용 프로그램/json 미디어 유형](http://www.ietf.org/rfc/rfc4627.txt)