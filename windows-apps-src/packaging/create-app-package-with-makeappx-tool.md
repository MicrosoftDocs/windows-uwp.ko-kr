---
title: MakeAppx.exe 도구를 사용하여 앱 패키지 만들기
description: MakeAppx.exe는 앱 패키지와 번들을 만들고, 암호화 및 암호 해독하고, 파일을 추출합니다.
ms.date: 06/21/2018
ms.topic: article
keywords: windows 10, uwp, 패키징
ms.assetid: 7c1c3355-8bf7-4c9f-b13b-2b9874b7c63c
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: dc109fe2e684dd3bc1fef62cece5cac3ab50d246
ms.sourcegitcommit: 681c70f964210ab49ac5d06357ae96505bb78741
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/26/2018
ms.locfileid: "7697186"
---
# <a name="create-an-app-package-with-the-makeappxexe-tool"></a>MakeAppx.exe 도구를 사용하여 앱 패키지 만들기


**MakeAppx.exe**는 앱 패키지와 앱 패키지 번들을 둘 다 만듭니다. 또한 **MakeAppx.exe**는 앱 패키지 또는 번들에서 파일을 추출하고 앱 패키지와 번들을 암호화 또는 암호 해독합니다. 이 도구는 Windows 10 SDK에 포함되어 있으며 명령 프롬프트 또는 스크립트 파일에서 사용할 수 있습니다.

> [!IMPORTANT] 
> Visual Studio를 사용하여 앱을 개발하는 경우 Visual Studio 마법사를 사용하여 앱 패키지를 만드는 것이 좋습니다. 자세한 내용은 [Visual Studio를 사용하여 UWP 앱 패키징](https://msdn.microsoft.com/windows/uwp/packaging/packaging-uwp-apps)을 참조하세요.

**MakeAppx.exe**는 .appxupload 파일을 만들지 않습니다. .Appxupload 파일은 Visual Studio 패키징 프로세스의 일부로 생성 하 고 다른 두 개의 파일이 포함:.msix 또는.appx 및.appxsym 이라는 합니다. .Appxsym 파일은 파트너 센터에서 [크래시 분석](../publish/health-report.md) 사용 되는 앱의 공용 기호를 포함 하는 압축된.pdb 파일입니다. 일반적인 .appx 파일도 제출할 수 있지만 충돌 분석이나 디버깅 정보를 사용할 수 없습니다. Microsoft Store에 패키지를 제출하는 방법에 대한 자세한 내용은 [앱 패키지 업로드](../publish/upload-app-packages.md)를 참조하세요. 

 가장 최신 버전의 Windows 10에서이 도구에 대 한 업데이트.appx 패키지 사용 영향을 주지 않습니다. 이 도구를 사용 하 여.appx 패키지를 사용 하 여 계속 하거나 아래 설명 된 대로.msix 패키지에 대 한 도구를 지 원하는 사용할 수 있습니다.

.appxupload 파일을 수동으로 만들려면
- .msix 및.appxsym은 폴더에 배치
- 폴더를 압축합니다.
- 압축 폴더 확장명을 .zip에서 .appxupload로 변경합니다.

## <a name="using-makeappxexe"></a>MakeAppx.exe 사용

SDK 설치 경로에 따라 Windows 10 PC에서 **MakeAppx.exe**가 있는 위치는 다음과 같습니다.
- x86: C:\Program Files (x86) \Windows Kits\10\bin\\&lt;빌드 번호&gt;\x86\makeappx.exe
- x64: C:\Program Files (x86) \Windows Kits\10\bin\\&lt;빌드 번호&gt;\x64\makeappx.exe

이 도구의 ARM 버전은 없습니다.

### <a name="makeappxexe-syntax-and-options"></a>MakeAppx.exe 구문 및 옵션

일반적인 **MakeAppx.exe** 구문은 다음과 같습니다.

``` Usage
MakeAppx <command> [options]      
```

다음 표에서는 **MakeAppx.exe**에 대한 명령을 설명합니다.

| **명령**   | **설명**                       |
|---------------|---------------------------------------|
| pack          | 패키지를 만듭니다.                    |
| unpack        | 지정한 패키지의 모든 파일을 지정한 출력 디렉터리에 추출합니다. |
| bundle        | 번들을 만듭니다.                     |
| unbundle      | 지정한 출력 경로 아래의 하위 디렉터리(번들 전체 이름에 따라 이름이 지정됨)에 모든 패키지의 압축을 풉니다. |
| encrypt       | 입력 패키지/번들에서 암호화된 앱 패키지 또는 번들을 지정한 출력 패키지/번들에 만듭니다. |
| decrypt       | 입력 앱 패키지/번들에서 암호 해독된 앱 패키지 또는 번들을 지정한 출력 패키지/번들에 만듭니다. |


모든 명령에 적용되는 옵션 목록은 다음과 같습니다.

| **옵션**    | **설명**                       |
|---------------|---------------------------------------|
| /d            | 입력, 출력 또는 콘텐츠 디렉터리를 지정합니다. |
| /l            | 지역화된 패키지에 사용됩니다. 기본 유효성 검사는 지역화된 패키지에서 작동하지 않습니다. 이 옵션 모든 유효성 검사의 해제를 요구하지 않고 해당 특정 유효성 검사만 사용하지 않도록 설정합니다. |
| /kf           | 지정한 키 파일의 키를 사용하여 패키지 또는 번들을 암호화하거나 암호를 해독합니다. /kt와 함께 사용할 수 없습니다. |
| /kt           | 전역 테스트 키를 사용하여 패키지 또는 번들을 암호화하거나 암호를 해독합니다. /kf와 함께 사용할 수 없습니다. |
| /no           | 있는 경우 출력 파일을 덮어쓰지 않도록 합니다. 이 옵션이나 /o 옵션을 지정하지 않으면 파일을 덮어쓸지 여부를 묻는 메시지가 사용자에게 표시됩니다. |
| /nv           | 의미 체계 유효성 검사를 건너뜁니다. 이 옵션을 지정하지 않으면 도구에서 패키지의 전체 유효성 검사를 수행합니다. |
| /o            | 있는 경우 출력 파일을 덮어씁니다. 이 옵션이나 /no 옵션을 지정하지 않으면 파일을 덮어쓸지 여부를 묻는 메시지가 사용자에게 표시됩니다. |
| /p            | 앱 패키지 또는 번들을 지정합니다.  |
| /v            | 콘솔에 자세한 정보 로깅이 출력되도록 합니다. |
| /?            | 도움말 텍스트를 표시합니다.                   |


다음 목록에는 가능한 인수가 포함되어 있습니다.

| **인수**                          | **설명**                       |
|---------------------------------------|---------------------------------------|
| &lt;출력 패키지 이름&gt;           | 생성되는 패키지 이름입니다. 추가 된.msix 또는.appx 파일 이름입니다. |
| &lt;암호화된 출력 패키지 이름&gt; | 생성되는 암호화된 패키지 이름입니다. 추가 된.emsix 또는.eappx 파일 이름입니다. |
| &lt;입력 패키지 이름&gt;            | 패키지 이름입니다. 추가 된.msix 또는.appx 파일 이름입니다. |
| &lt;암호화된 입력 패키지 이름&gt;  | 암호화된 패키지 이름입니다. 추가 된.emsix 또는.eappx 파일 이름입니다. |
| &lt;출력 번들 이름&gt;            | 생성되는 번들 이름입니다. 추가 된.msixbundle 또는.appxbundle 파일 이름입니다. |
| &lt;암호화된 출력 번들 이름&gt;  | 생성되는 암호화된 번들 이름입니다. 추가 된.emsixbundle 또는.eappxbundle 파일 이름입니다. |
| &lt;입력 번들 이름&gt;             | 번들 이름입니다. 추가 된.msixbundle 또는.appxbundle 파일 이름입니다. |
| &lt;암호화된 입력 번들 이름&gt;   | 암호화된 번들 이름입니다. 추가 된.emsixbundle 또는.eappxbundle 파일 이름입니다. |
| &lt;콘텐츠 디렉터리&gt;             | 앱 패키지 또는 번들 콘텐츠 경로입니다. |
| &lt;매핑 파일&gt;                  | 패키지 원본 및 대상을 지정하는 파일 이름입니다. |
| &lt;출력 디렉터리&gt;              | 출력 패키지 및 번들의 디렉터리 경로입니다. |
| &lt;키 파일&gt;                      | 암호화 또는 암호 해독에 사용되는 키가 포함된 파일 이름입니다. |
| &lt;알고리즘 ID&gt;                  | 블록 맵을 만들 때 사용되는 알고리즘입니다. 유효한 알고리즘에는 SHA256(기본값), SHA384, SHA512 등이 있습니다. |


### <a name="create-an-app-package"></a>앱 패키지 만들기

앱 패키지는.msix 또는.appx 패키지 파일에 패키징된 앱 파일의 전체 집합입니다. **pack** 명령을 사용하여 앱 패키지를 만들려면 패키지 위치에 대한 매핑 파일이나 콘텐츠 디렉터리를 제공해야 합니다. 패키지를 만드는 동안 암호화할 수도 있습니다. 패키지를 암호화하려는 경우 /ep를 사용하고 키 파일(/kf) 또는 전역 테스트 키(/kt)를 사용 중인지 지정해야 합니다. 암호화된 패키지를 만드는 방법에 대한 자세한 내용은 [패키지나 번들 암호화 또는 암호 해독](#encrypt-or-decrypt-a-package-or-bundle)을 참조하세요.

**pack** 명령과 관련된 옵션은 다음과 같습니다.

| **옵션**    | **설명**                       |
|---------------|---------------------------------------|
| /f            | 매핑 파일을 지정합니다.           |
| /h            | 블록 맵을 만들 때 사용할 해시 알고리즘을 지정합니다. pack 명령에만 사용할 수 있습니다. 유효한 알고리즘에는 SHA256(기본값), SHA384, SHA512 등이 있습니다. |
| /m            | 출력 앱 패키지 또는 리소스 패키지 매니페스트를 생성하기 위한 기초로 사용할 입력 앱 매니페스트의 경로를 지정합니다.  이 옵션을 사용하는 경우 /f도 사용하고 매핑 파일에 [ResourceMetadata] 섹션을 포함하여 생성된 매니페스트에 포함될 리소스 차원을 지정해야 합니다.|
| /nc           | 패키지 파일이 압축되지 않도록 합니다. 기본적으로 검색된 파일 형식에 따라 파일이 압축됩니다. |
| /r            | 리소스 패키지를 빌드합니다. /m과 함께 사용해야 하 고 /l 옵션 사용을 암시합니다. |  


다음 사용 예제에서는 **pack** 명령의 가능한 몇 가지 구문 옵션을 보여 줍니다.

``` syntax 
MakeAppx pack [options] /d <content directory> /p <output package name>
MakeAppx pack [options] /f <mapping file> /p <output package name>
MakeAppx pack [options] /m <app package manifest> /f <mapping file> /p <output package name>
MakeAppx pack [options] /r /m <app package manifest> /f <mapping file> /p <output package name>
MakeAppx pack [options] /d <content directory> /ep <encrypted output package name> /kf <key file>
MakeAppx pack [options] /d <content directory> /ep <encrypted output package name> /kt

```
다음은 **pack** 명령의 명령줄 예제를 보여 줍니다.

``` examples
MakeAppx pack /v /h SHA256 /d "C:\My Files" /p MyPackage.msix
MakeAppx pack /v /o /f MyMapping.txt /p MyPackage.msix
MakeAppx pack /m "MyApp\AppxManifest.xml" /f MyMapping.txt /p AppPackage.msix
MakeAppx pack /r /m "MyApp\AppxManifest.xml" /f MyMapping.txt /p ResourcePackage.msix
MakeAppx pack /v /h SHA256 /d "C:\My Files" /ep MyPackage.emsix /kf MyKeyFile.txt
MakeAppx pack /v /h SHA256 /d "C:\My Files" /ep MyPackage.emsix /kt
```

### <a name="create-an-app-bundle"></a>앱 번들 만들기

앱 번들은 앱 패키지와 유사하지만 번들은 사용자가 다운로드하는 앱 크기를 줄일 수 있습니다. 앱 번들은 언어별 자산, 다양한 이미지 스케일 자산, 특정 Microsoft DirectX 버전에 적용되는 리소스 등에 유용합니다. 암호화된 앱 패키지 만들기와 마찬가지로, 앱 번들을 만드는 동안 암호화할 수도 있습니다. 앱 번들을 암호화하려면 /ep 옵션을 사용하고 키 파일(/kf) 또는 전역 테스트 키(/kt)를 사용 중인지 지정합니다. 암호화된 번들을 만드는 방법에 대한 자세한 내용은 [패키지나 번들 암호화 또는 암호 해독](#encrypt-or-decrypt-a-package-or-bundle)을 참조하세요.

**bundle** 명령과 관련된 옵션은 다음과 같습니다.

| **옵션**    | **설명**                       |
|---------------|---------------------------------------|
| /bv           | 번들의 버전 번호를 지정합니다. 버전 번호는 &lt;주&gt;.&lt;부&gt;.&lt;빌드&gt;.&lt;수정 버전&gt; 형식의 마침표로 구분된 네 부분으로 구성되어야 합니다. |
| /f            | 매핑 파일을 지정합니다.           |

번들 버전을 지정하지 않거나 "0.0.0.0"으로 설정하면 현재 날짜와 시간을 사용하여 번들이 생성됩니다.

다음 사용 예제에서는 **bundle** 명령의 가능한 몇 가지 구문 옵션을 보여 줍니다.

``` syntax
MakeAppx bundle [options] /d <content directory> /p <output bundle name>
MakeAppx bundle [options] /f <mapping file> /p <output bundle name>
MakeAppx bundle [options] /d <content directory> /ep <encrypted output bundle name> /kf MyKeyFile.txt
MakeAppx bundle [options] /f <mapping file> /ep <encrypted output bundle name> /kt
```
다음 블록에는 **bundle** 명령의 예가 포함되어 있습니다.

``` examples
MakeAppx bundle /v /d "C:\My Files" /p MyBundle.msixbundle
MakeAppx bundle /v /o /bv 1.0.1.2096 /f MyMapping.txt /p MyBundle.msixbundle
MakeAppx bundle /v /o /bv 1.0.1.2096 /f MyMapping.txt /ep MyBundle.emsixbundle /kf MyKeyFile.txt
MakeAppx bundle /v /o /bv 1.0.1.2096 /f MyMapping.txt /ep MyBundle.emsixbundle /kt
```

### <a name="extract-files-from-a-package-or-bundle"></a>패키지나 번들에서 파일 추출

앱 패키지 및 번들을 만드는 것뿐 아니라 **MakeAppx.exe**에서 기존 패키지의 압축을 풀거나 번들을 취소할 수도 있습니다. 압축을 푼 파일의 대상으로 콘텐츠 디렉터리를 제공해야 합니다. 암호화된 패키지나 번들에서 파일을 추출하려는 경우 /ep 옵션을 사용하고 키 파일(/kf) 또는 전역 테스트 키(/kt)를 사용하여 암호를 해독해야 하는지를 지정하여 파일 암호를 해독하는 동시에 추출할 수 있습니다. 패키지나 번들의 암호를 해독하는 방법에 대한 자세한 내용은 [패키지나 번들 암호화 또는 암호 해독](#encrypt-or-decrypt-a-package-or-bundle)을 참조하세요.

**unpack** 및 **unbundle** 명령과 관련된 옵션은 다음과 같습니다.

| **옵션**    | **설명**                       |
|---------------|---------------------------------------|
| /nd           | 패키지/번들의 압축을 풀거나 번들을 취소할 때 암호를 해독하지 않습니다. |
| /pfn          | 지정한 출력 경로 아래의 하위 디렉터리(패키지 또는 번들 전체 이름에 따라 이름이 지정됨)에 모든 파일의 압축을 풉니다/번들을 취소합니다. |

다음 사용 예제에서는 **unpack** 및 **unbundle** 명령의 가능한 몇 가지 구문 옵션을 보여 줍니다.

``` syntax
MakeAppx unpack [options] /p <input package name> /d <output directory>
MakeAppx unpack [options] /ep <encrypted input package name> /d <output directory> /kf <key file>
MakeAppx unpack [options] /ep <encrypted input package name> /d <output directory> /kt

MakeAppx unbundle [options] /p <input bundle name> /d <output directory>
MakeAppx unbundle [options] /ep <encrypted input bundle name> /d <output directory> /kf <key file>
MakeAppx unbundle [options] /ep <encrypted input bundle name> /d <output directory> /kt
```

다음 블록에는 **unpack** 및 **unbundle** 명령의 예가 포함되어 있습니다.

``` examples
MakeAppx unpack /v /p MyPackage.msix /d "C:\My Files"
MakeAppx unpack /v /ep MyPackage.emsix /d "C:\My Files" /kf MyKeyFile.txt
MakeAppx unpack /v /ep MyPackage.emsix /d "C:\My Files" /kt

MakeAppx unbundle /v /p MyBundle.msixbundle /d "C:\My Files"
MakeAppx unbundle /v /ep MyBundle.emsixbundle /d "C:\My Files" /kf MyKeyFile.txt
MakeAppx unbundle /v /ep MyBundle.emsixbundle /d "C:\My Files" /kt
```

### <a name="encrypt-or-decrypt-a-package-or-bundle"></a>패키지나 번들 암호화 또는 암호 해독

**MakeAppx.exe** 도구는 기존 패키지 또는 번들을 암호화하거나 암호를 해독할 수도 있습니다. 패키지 이름, 출력 패키지 이름, 암호화 또는 암호 해독 시 키 파일(/kf) 또는 전역 테스트 키(/kt)를 사용해야 하는지 여부를 제공해야 합니다.

Visual Studio 패키징 마법사에서는 암호화 및 암호 해독을 사용할 수 없습니다. 

**encrypt** 및 **decrypt** 명령과 관련된 옵션은 다음과 같습니다.

| **옵션**    | **설명**                       |
|---------------|---------------------------------------|
| /ep           | 암호화된 앱 패키지 또는 번들을 지정합니다. |

다음 사용 예제에서는 **encrypt** 및 **decrypt** 명령의 가능한 몇 가지 구문 옵션을 보여 줍니다.

``` syntax
MakeAppx encrypt [options] /p <package name> /ep <output package name> /kf <key file>
MakeAppx encrypt [options] /p <package name> /ep <output package name> /kt

MakeAppx decrypt [options] /ep <package name> /p <output package name> /kf <key file>
MakeAppx decrypt [options] /ep <package name> /p <output package name> /kt
```

다음 블록에는 **encrypt** 및 **decrypt** 명령의 예가 포함되어 있습니다.

``` examples
MakeAppx.exe encrypt /p MyPackage.msix /ep MyEncryptedPackage.emsix /kt
MakeAppx.exe encrypt /p MyPackage.msix /ep MyEncryptedPackage.emsix /kf MyKeyFile.txt

MakeAppx.exe decrypt /p MyPackage.msix /ep MyEncryptedPackage.emsix /kt
MakeAppx.exe decrypt p MyPackage.msix /ep MyEncryptedPackage.emsix /kf MyKeyFile.txt
```

## <a name="key-files"></a>키 파일

키 파일은 "[Keys]" 문자열이 포함된 줄로 시작하고 각 패키지의 암호화에 사용할 키를 설명하는 줄이 뒤에 와야 합니다. 각 키는 따옴표로 묶인 한 쌍의 문자열로 표시되며 공백이나 탭으로 구분됩니다. 첫 번째 문자열은 base64로 인코드된 32바이트 키 ID를 나타내고, 두 번째 문자열은 base64로 인코드된 32바이트 암호화 키를 나타냅니다. 키 파일은 간단한 텍스트 파일이어야 합니다.

키 파일의 예는 다음과 같습니다.

``` Example
[Keys]
"OWVwSzliRGY1VWt1ODk4N1Q4R2Vqc04zMzIzNnlUREU="    "MjNFTlFhZGRGZEY2YnVxMTBocjd6THdOdk9pZkpvelc="
```

## <a name="mapping-files"></a>파일 매핑
매핑 파일은 "[Files]" 문자열이 포함된 줄로 시작하고 패키지에 추가할 파일을 설명하는 줄이 뒤에 와야 합니다. 각 파일은 따옴표로 묶인 한 쌍의 경로로 설명되며 공백이나 탭으로 구분됩니다. 각 파일은 해당 원본(디스크)과 대상(패키지)을 나타냅니다. 매핑 파일을 간단한 텍스트 파일이어야 합니다.

매핑 파일의 예(/m 옵션 제외)는 다음과 같습니다.

``` Example
[Files]
"C:\MyApp\StartPage.html"               "default.html"
"C:\Program Files (x86)\example.txt"    "misc\example.txt"
"\\MyServer\path\icon.png"              "icon.png"
"my app files\readme.txt"               "my app files\readme.txt"
"CustomManifest.xml"                    "AppxManifest.xml"
``` 

매핑 파일을 사용할 때 /m 옵션을 사용할지 여부를 선택할 수 있습니다. /m 옵션을 사용하면 사용자가 생성된 매니페스트에 포함할 매핑 파일의 리소스 메타데이터를 지정할 수 있습니다. /m 옵션을 사용하는 경우 매핑 파일에 "[ResourceMetadata]" 줄로 시작하고 "ResourceDimensions" 및 "ResourceId"를 지정하는 줄이 오는 섹션이 포함되어야 합니다. 앱 패키지에 여러 "ResourceDimensions"를 포함할 수 있지만 "ResourceId"는 하나만 포함할 수 있습니다.

매핑 파일의 예(/m 옵션 포함)는 다음과 같습니다.

``` Example
[ResourceMetadata]
"ResourceDimensions"                    "language-en-us"
"ResourceId"                            "English"

[Files]
"images\en-us\logo.png"                 "en-us\logo.png"
"en-us.pri"                             "resources.pri"
```

## <a name="semantic-validation-performed-by-makeappxexe"></a>MakeAppx.exe에서 수행하는 의미 체계 유효성 검사

**MakeAppx.exe**는 가장 일반적인 배포 오류를 catch하고 앱 패키지가 유효한지 확인할 수 있도록 설계된 제한된 의미 체계 유효성 검사를 수행합니다. **MakeAppx.exe**를 사용하는 동안 유효성 검사를 건너뛰려는 경우 /nv 옵션을 참조하세요. 

이 유효성 검사는 다음을 확인합니다.
- 패키지 매니페스트에서 참조된 모든 파일이 앱 패키지에 포함되어 있습니다.
- 응용 프로그램에 두 개의 동일한 키가 없습니다.
- 응용 프로그램이 이 목록에서 금지된 프로토콜(SMB, FILE, MS-WWA-WEB, MS-WWA)에 등록하지 않습니다. 

일반적인 오류만 catch하도록 설계되었기 때문에 전체 의미 체계 유효성 검사는 아닙니다. **MakeAppx.exe**에서 빌드된 패키지는 설치 가능한 패키지가 아닐 수도 있습니다.
