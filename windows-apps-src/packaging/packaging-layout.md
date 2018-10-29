---
author: laurenhughes
title: 패키징 레이아웃으로 패키지 만들기
description: 패키징 레이아웃은 앱 패키징 구조를 설명하는 단일 문서입니다. 앱(기본 및 선택)의 번들, 번들의 패키지, 패키지의 파일을 지정합니다.
ms.author: lahugh
ms.date: 04/30/2018
ms.topic: article
keywords: Windows 10, 패키징, 패키지 레이아웃, 자산 패키지
ms.localizationpriority: medium
ms.openlocfilehash: 9342b4ce35cb50037813ed2210e2d7246411ad92
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2018
ms.locfileid: "5769159"
---
# <a name="package-creation-with-the-packaging-layout"></a>패키징 레이아웃으로 패키지 만들기  

자산 패키지의 도입으로 개발자는 이제 도구를 더 많은 패키지 유형뿐만 아닌 더 많은 패키지를 빌드하는 도구를 갖추게 되었습니다. 앱의 크기가 커지고 더 복잡해짐에 따라 종종 더 많은 패키지로 구성되며 이러한 패키지 관리의 어려움이 증가하고 있습니다(특히 Visual Studio 외부에서 빌드하고 매핑 파일을 사용하는 경우). 앱의 패키징 구조 관리를 간소화하기 위해 MakeAppx.exe에서 지원하는 패키징 레이아웃을 사용할 수 있습니다. 

패키징 레이아웃은 앱 패키징 구조를 설명하는 단일 XML 문서입니다. 앱(기본 및 선택)의 번들, 번들의 패키지, 패키지의 파일을 지정합니다. 다른 폴더, 드라이브, 네트워크 위치에서 파일을 선택할 수 있습니다. 파일을 선택하거나 제외하는 데 와일드카드를 사용할 수 있습니다.

앱에 대한 레이아웃을 패키징하면 MakeAppx.exe와 단일 명령줄 호출에서 앱에 대한 모든 패키지를 만드는 데 사용됩니다. 패키징 레이아웃은 배포 요구에 맞게 패키지 구조를 변경할 수 있도록 편집할 수 있습니다. 


## <a name="simple-packaging-layout-example"></a>단순한 패키징 레이아웃 예

단순한 패키징 레이아웃의 모습의 예는 다음과 같습니다.

```xml
<PackagingLayout xmlns="http://schemas.microsoft.com/appx/makeappx/2017">
  <PackageFamily ID="MyGame" FlatBundle="true" ManifestPath="C:\mygame\appxmanifest.xml" ResourceManager="false">
    
    <!-- x64 code package-->
    <Package ID="x64" ProcessorArchitecture="x64">
      <Files>
        <File DestinationPath="*" SourcePath="C:\mygame\*"/>
        <File ExcludePath="*C:\mygame\*.txt"/>
      </Files>
    </Package>
    
    <!-- Media asset package -->
    <AssetPackage ID="Media" AllowExecution="false">
      <Files>
        <File DestinationPath="Media\**" SourcePath="C:\mygame\media\**"/>
      </Files>
    </AssetPackage>

  </PackageFamily>
</PackagingLayout>
```

이 예를 살펴 작동 방식을 파악해 보겠습니다.

### <a name="packagefamily"></a>PackageFamily
이 패키징 레이아웃 x64를 사용 하 여 단일 플랫 앱 번들 파일을 만듭니다 아키텍처 패키지와 "미디어" 자산 패키지로 합니다. 

**PackageFamily** 요소는 앱 번들을 정의하는 데 사용됩니다. 번들에 대한 **AppxManifest**를 제공하기 위해 **ManifestPath** 특성을 사용해야 하며 **AppxManifest**는 번들의 아키텍처 패키지에 대한 **AppxManifest**애 해당해야 합니다. **ID** 특성도 제공해야 합니다. 이는 패키지를 생성하는 동안 MakeAppx.exe에 사용되기 때문에 원하는 경우 이 패키지만 만들 수 있으며 결과 패키지의 파일 이름이 됩니다. **FlatBundle** 특성은 만들려는 유형의 번들을 설명하는 데 사용되면 플랫 번들의 경우 **true**(여기에서 자세한 내용을 알아볼 수 있음), 클래식 번들의 경우 **false**입니다. **ResourceManager** 특성은 이 번들 내 리소스 패키지가 파일에 액세스하기 위해 MRT를 사용할지 여부를 지정하는 데 사용됩니다. 기본적으로 **true**지만 Windows 10 버전을 1803에서는 아직 준비가 되지 않았으므로 이 특성은 **false**로 설정해야 합니다.


### <a name="package-and-assetpackage"></a>패키지 및 AssetPackage
**PackageFamily** 내에서 앱 번들이 포함 또는 참조하는 패키지가 정의됩니다. 여기서 아키텍처 패키지(주 패키지라고도 함)는 **패키지** 요소로 정의되며 자산 패키지는 **AssetPackage** 요소로 정의됩니다. 아키텍처 패키지는 패키지가 "x64", "x86", "arm" 또는 "중립" 중에 어떤 아키텍처용인지 지정해야 합니다. (선택 사항)**ManifestPath** 특성을 다시 사용하여 특별히 이 패키지에 대한 **AppxManifest**를 직접 제공할 수 있습니다. **AppxManifest**를 제공하지 않으면 **PackageFamily**에 대해 제공되는 **AppxManifest**에서 자동으로 생성됩니다. 

기본적으로 **AppxManifest**는 번들 내 모든 패키지에 대해 생성됩니다. 자산 패키지의 경우 **AllowExecution** 특성을 설정할 수도 있습니다. 이를 **false**로 설정하면 실행할 필요가 없는 패키지에 게시 프로세스를 차단하는 바이러스 검사가 없으므로 앱 게시 시간을 줄일 수 있습니다([자산 패키지 소개](asset-packages.md)에서 이에 대해 자세히 알아볼 수 있습니다). 

### <a name="files"></a>파일
각 패키지 정의 내에서 **File** 요소를 사용하여 이 패키지에 포함되는 파일을 선택합니다. **SourcePath** 특성에 파일이 로컬로 위치합니다. 다른 폴더(상대 경로를 제공하여), 다른 드라이브(절대 경로를 제공하여), 또는 네트워크 공유(`\\myshare\myapp\*` 등을 제공하여)에서 파일을 선택할 수 있습니다. **DestinationPath**는 패키지 루트를 기준으로 패키지 내 파일이 끝나는 위치입니다. 다른 두 특성 대신 **ExcludePath**를 사용하여 같은 패키지 내 다른 **File** 요소의 **SourcePath** 특성에 의해 선택된 파일에서 제외할 파일을 선택할 수 있습니다.

각 **File** 요소는 와일드카드를 사용하여 여러 파일을 선택하는 데 사용할 수 있습니다. 일반적으로 단일 와일드카드(`*`)는 몇 번이든 경로 내 어디에서나 사용할 수 있습니다. 그러나 단일 와일드카드는 폴더 내에 있는 파일과만 일치하며 모든 하위 폴더에 있는 파일과는 일치하지 않습니다. 예를 들어, `C:\MyGame\*\*`은 **SourcePath**에서 사용하여 `C:\MyGame\Audios\UI.mp3` 및 `C:\MyGame\Videos\intro.mp4` 파일을 선택할 수 있지만 `C:\MyGame\Audios\Level1\warp.mp3`를 선택할 수 없습니다. 더블 와일드카드(`**`)는 반복적으로 어느 것이든 일치하기 위해 폴더 또는 파일 위치에서 사용할 수 있습니다(하지만 부분 이름 옆에 있을 수는 없음). 예를 들어, `C:\MyGame\**\Level1\**`는 `C:\MyGame\Audios\Level1\warp.mp3` 및 `C:\MyGame\Videos\Bonus\Level1\DLC1\intro.mp4`를 선택할 수 있습니다. 와일드카드가 원본과 대상 간에 여러 장소에서 사용되는 경우 패키징 과정의 일환으로 파일 이름을 직접 변경하기 위해 와일드카드를 사용할 수도 있습니다. 예를 들어, **SourcePath**에 `C:\MyGame\Audios\*`를 가지고 **DestinationPath**에 대해 `Sound\copy_*`를 가지면 `C:\MyGame\Audios\UI.mp3`를 선택하고 패키지에 `Sound\copy_UI.mp3`로 표시할 수 있습니다. 일반적으로 단일 와일드카드 및 이중 와일드카드의 수는 단일 **File** 요소의 **SourcePath** 및 **DestinationPath**와 동일해야 합니다.


## <a name="advanced-packaging-layout-example"></a>고급 패키징 레이아웃 예

좀 더 복잡 한 패키징 레이아웃의 예는 다음과 같습니다.

```xml
<PackagingLayout xmlns="http://schemas.microsoft.com/appx/makeappx/2017">
  <!-- Main game -->
  <PackageFamily ID="MyGame" FlatBundle="true" ManifestPath="C:\mygame\appxmanifest.xml" ResourceManager="false">
    
    <!-- x64 code package-->
    <Package ID="x64" ProcessorArchitecture="x64">
      <Files>
        <File DestinationPath="*" SourcePath="C:\mygame\*"/>
        <File ExcludePath="*C:\mygame\*.txt"/>
      </Files>
    </Package>

    <!-- Media asset package -->
    <AssetPackage ID="Media" AllowExecution="false">
      <Files>
        <File DestinationPath="Media\**" SourcePath="C:\mygame\media\**"/>
      </Files>
    </AssetPackage>
    
    <!-- English resource package -->
    <ResourcePackage ID="en">
      <Files>
        <File DestinationPath="english\**" SourcePath="C:\mygame\english\**"/>
      </Files>
      <Resources Default="true">
        <Resource Language="en"/>
      </Resources>
    </ResourcePackage>

    <!-- French resource package -->
    <ResourcePackage ID="fr">
      <Files>
        <File DestinationPath="french\**" SourcePath="C:\mygame\french\**"/>
      </Files>
      <Resources>
        <Resource Language="fr"/>
      </Resources>
    </ResourcePackage>
  </PackageFamily>

  <!-- DLC in the related set -->
  <PackageFamily ID="DLC" Optional="true" ManifestPath="C:\DLC\appxmanifest.xml">
    <Package ID="DLC.x86" Architecture="x86">
      <Files>
        <File DestinationPath="**" SourcePath="C:\DLC\**"/>
      </Files>
    </Package>
  </PackageFamily>

  <!-- DLC not part of the related set -->
  <PackageFamily ID="Themes" Optional="true" RelatedSet="false" ManifestPath="C:\themes\appxmanifest.xml">
    <Package ID="Themes.main" Architecture="neutral">
      <Files>
        <File DestinationPath="**" SourcePath="C:\themes\**"/>
      </Files>
    </Package>
  </PackageFamily>

  <!-- Existing packages that need to be included/referenced in the bundle -->
  <PrebuiltPackage Path="C:\prebuilt\DLC2.appxbundle" />

</PackagingLayout>
```

이 예는 **ResourcePackage** 및 **Optional** 요소가 추가된 단순한 예와 다릅니다.

리소스 패키지는 **ResourcePackage** 요소로 지정할 수 있습니다. **ResourcePackage** 내에서 **Resources** 요소는 리소스 팩의 리소스 한정자를 지정하는 데 사용해야 합니다. 리소스 한정자는 리소스 팩에서 지원되는 리소스입니다. 여기에서 정의된 리소스 팩이 두 개 있으며 각각 영어 및 프랑스어와 관련된 파일이 포함된다는 것을 알 수 있습니다. 리소스 팩은 하나 이상의 한정자를 가질 수 있으며 **Resources** 내 다른 **Resource** 요소를 추가하여 이 작업을 수행할 수 있습니다. 리소스 차원에 대한 기본 리소스는 차원이 있는지 지정해야 합니다(언어, 크기, dxfl 등의 차원). 여기에서 영어가 기본 언어임을 확인할 수 있으며 이는 프랑스어 시스템 언어가 설정되지 않은 사용자에 대해 영어 리소스 팩을 다운로드하여 영어로 표시할 것임을 의미합니다.


선택적 패키지 각각에는 고유한 패키지 패밀리 이름이 있으며 **PackageFamily** 요소로 정의해야 합니다. **선택적** 특성은 **true**로 지정해야 합니다. **RelatedSet** 특성은 선택적 패키지가 관련 집합 내에 있는지 여부, 즉 선택적 패키지가 주 패키지 내에서 업데이트되어야 하는지 여부를 지정하는 데 사용됩니다(기본적으로 true입니다).

**PrebuiltPackage** 요소 포함 되거나 빌드될 앱 번들 파일에서 참조 될 패키징 레이아웃에 정의 되지 않은 패키지를 추가 하려면 사용 됩니다. 이 경우 다른 DLC 선택적 패키지가 여기에 포함 되어 있으므로 기본 앱 번들 파일 참조 하 고 관련된 집합의 일부로 가질 수 있습니다.


## <a name="build-app-packages-with-a-packaging-layout-and-makeappxexe"></a>패키징 레이아웃과 MakeAppx.exe로 앱 패키지 빌드
앱에 대한 패키징 레이아웃이 있으면 MakeAppx.exe를 사용하여 앱 패키지 빌드를 시작할 수 있습니다. 패키징 레이아웃에 정의된 모든 패키지를 빌드하려면 다음 명령을 사용합니다.

``` example 
MakeAppx.exe build /f PackagingLayout.xml /op OutputPackages\
```

그러나 앱을 업데이트하고 일부 패키지에 변경된 파일이 모두 포함되지 않는 경우 변경된 패키지만 빌드할 수 있습니다. 이 페이지에서 단순한 패키징 레이아웃 예를 사용하여 x64 아키텍처 패키지를 빌드하면 명령은 다음과 같은 모습입니다.

``` example 
MakeAppx.exe build /f PackagingLayout.xml /id "x64" /ip PreviousVersion\ /op OutputPackages\ /iv
```

`/id` 플래그를 사용하여 레이아웃의 **ID** 특성에 해당하는 패키징 레이아웃에서 빌드할 패키지를 선택할 수 있습니다. 이 경우 `/ip`는 패키지의 이전 버전의 위치를 나타내는 데 사용됩니다. 앱 번들 파일 여전히 **미디어** 패키지의 이전 버전을 참조 해야 하기 때문에 이전 버전을 제공 해야 합니다. `/iv` 플래그는 빌드되는 패키지의 버전을 자동으로 증분하는 데 사용됩니다(**AppxManifest**에서 버전을 변경하는 대신). 또한 `/pv` 및 `/bv` 스위치는 직접 패키지 버전(만들 모든 패키지에 대한)과 번들 버전(만들 모든 번들에 대한)을 제공하는 데 사용할 수 있습니다.
고급 패키징 레이아웃 예를 사용하여 **테마** 선택적 번들 및 이를 참조하는 **Themes.main**만 빌드하려는 경우 이 명령을 사용해야 합니다.

``` example 
MakeAppx.exe build /f PackagingLayout.xml /id "Themes" /op OutputPackages\ /bc /nbp
```

`/bc` 플래그는 **테마** 번들의 자식도 빌드해야 함을 나타내는 데 사용됩니다(이 경우 **Themes.main**이 빌드됩니다). `/nbp` 플래그는 **테마** 번들의 부모를 빌드해서는 안 됨을 나타내는 데 사용됩니다. 선택적 앱 번들인 **테마**의 부모는 기본 앱 번들 **MyGame**입니다. 선택적 패키지는 관련 집합에 있을 때 (주 패키지 및 선택적 패키지 간에 버전을 보장하기 위해) 기본 앱 번들에서도 참조되기 때문에 일반적으로 관련 집합의 선택적 패키지에 대해 선택적 패키지를 설치하기 위해 기본 앱 번들도 구축해야 합니다. 다음 다이어그램에서는 패키지 간 부모 자식 관계를 보여 줍니다.

![패키지 레이아웃 다이어그램](images/packaging-layout.png)
