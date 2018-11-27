---
title: 자산 패키지 소개
description: 자산 패키지는 아키텍처 패키지 전체에서 중복된 파일에 대한 필요성을 효율적으로 제거하는 응용 프로그램의 일반적인 파일에 대한 중앙 집중식 위치 역할을 수행할 패키지의 유형입니다.
ms.date: 09/30/2018
ms.topic: article
keywords: Windows 10, 패키징, 패키지 레이아웃, 자산 패키지
ms.localizationpriority: medium
ms.openlocfilehash: b7ae65d13f92f5ab28f2f5eda468032bb7f83793
ms.sourcegitcommit: 681c70f964210ab49ac5d06357ae96505bb78741
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/27/2018
ms.locfileid: "7718103"
---
# <a name="introduction-to-asset-packages"></a>자산 패키지 소개

> [!IMPORTANT]
> Microsoft Store에 앱을 제출하고자 하는 경우 [Windows 개발자 지원](https://developer.microsoft.com/windows/support)에 문의하여 자산 패키지에 대한 승인을 얻어야 합니다.

자산 패키지는 아키텍처 패키지 전체에서 중복된 파일에 대한 필요성을 효율적으로 제거하는 응용 프로그램의 일반적인 파일에 대한 중앙 집중식 위치 역할을 수행할 패키지의 유형입니다. 자산 패키지는 앱을 실행하기 위해 필요한 정적 콘텐츠를 포함하도록 설계되었다는 점에서 리소스 패키지와 비슷하지만 모든 자산 패키지는 사용자의 시스템 아키텍처, 언어, 또는 디스플레이 배율과 관계없이 항상 다운로드된다는 점에서 다릅니다.

![자산 패키지 번들 다이어그램](images/primary-bundle.png)

자산 패키지는 모든 아키텍처, 언어 및 배율과 관련 없는 파일을 포함하기 때문에 자산 패키지를 활용하면 이러한 파일이 더 이상 중복되지 않으므로 전반적인 패키지 앱의 크기가 줄어들며, 사용자는 대용량 앱의 로컬 개발 디스크 공간을 관리하고 앱 패키지의 일반적인 관리를 수행할 수 있습니다. 

### <a name="how-do-asset-packages-affect-publishing"></a>자산 패키지가 게시에 어떤 영향을 미치나요?
자산 패키지의 가장 큰 이점은 패키지 앱의 크기 축소입니다. 작은 앱 패키지를 사용하면 Microsoft Store가 파일을 덜 처리할 수 있기 때문에 앱의 게시 프로세스 속도가 빨라집니다. 그러나 이 외에도 자산 패키지의 중요한 혜택이 있습니다.

자산 패키지를 만들 때 패키지를 실행하도록 할지 여부를 지정할 수 있습니다. 자산 패키지는 아키텍처와 독립적인 파일만 포함해야 하므로 일반적으로 .dll 또는 .exe 파일을 포함할 수 없으며, 따라서 자산 패키지의 경우 일반적으로 실행할 필요가 없습니다. 이러한 차이점의 중요성은 게시 프로세스 동안 모든 실행 패키지를 검사하여 맬웨어가 포함되어 있지 않은지 확인해야 하며 큰 패키지의 경우 이 검사 프로세스는 더 오래 걸립니다. 하지만 패키지가 실행할 수 없도록 지정된 경우 앱의 설치는 이 패키지에 포함된 파일을 실행할 수 없는지 확인합니다. 이를 통해 전체 패키지 검사에 대한 필요성을 제거하고 앱을 게시하는 동안(또한 업데이트 시) 맬웨어 검사 시간을 크게 줄입니다. 따라서 자산 패키지를 사용하는 앱에 대한 게시가 더 빨라집니다. Note이 게시의 이점을 얻으려면가 스토어 각.appx 또는.msix 패키지 파일을 병렬로 처리할 수 있도록 해당 [플랫 번들 앱 패키지](flat-bundles.md) 를 사용 해야 합니다. 


### <a name="should-i-use-asset-packages"></a>자산 패키지를 사용해야 하나요?
자산 패키지를 활용하기 위해 앱의 파일 구조를 업데이트하면 패키지 크기를 크게 줄이고 개발 반복을 줄이는 등 실질적인 혜택 얻을 수 있습니다. 아키텍처 패키지가 모두 공통적으로 상당한 양의 파일을 포함하거나 앱의 대부분이 실행할 수 없는 파일로 구성된 경우 자산 패키지를 사용하도록 전환하는 데 시간을 오래 투자하는 것이 좋습니다.

그러나 자산 패키지가 앱 콘텐츠 옵션을 제공하기 위한 수단이 되지 않도록 주의하세요. 자산 패키지 파일은 선택 사항이 아니며 대상 장치의 아키텍처, 언어 또는 배율에 관계 없이 **항상** 다운로드해야 합니다. [선택적 패키지](optional-packages.md)를 사용하여 앱이 지원하기 원하는 모든 선택적 콘텐츠를 구현해야 합니다. 


### <a name="how-to-create-an-asset-package"></a>자산 패키지를 만드는 방법
자산 패키지를 만드는 가장 쉬운 방법은 패키징 레이아웃을 사용하는 것입니다. 그러나 MakeAppx.exe를 사용하여 수동으로 자산 패키지를 만들 수도 있습니다. 자산 패키지에 포함할 파일을 지정하려면 "매핑 파일"을 만들어야 합니다. 이 예에서는 자산 패키지의 유일한 파일이 "Video.mp4"이지만 자산 패키지의 모든 파일이 여기에 나열됩니다. **ResourceMetadata**의 **ResourceDimensions** 지정자는 자산 패키지에 대해 생략됩니다(리소스 패키지에 대한 매핑 파일과 달리).

```example 
[ResourceMetadata]
"ResourceId"        "Videos"

[Files]
"Video.mp4"         "Video.mp4"
```

이 명령을 사용하여 MakeAppx.exe로 자산 패키지를 만듭니다. 

```syntax 
MakeAppx.exe pack /r /m AppxManifest.xml /f MappingFile.txt /p Videos.appx

...

MakeAppx.exe pack /r /m AppxManifest.xml /f MappingFile.txt /p Videos.msix

```
여기에서 AppxManifest에서 참조되는 모든 파일(로고 파일)은 자산 패키지로 이동할 수 없습니다. 이러한 파일은 아키텍처 패키지에서 중복되어야 합니다. 자산 패키지는 resources.pri도 포함하지 않아야 합니다. MRT는 자산 패키지 파일에 액세스하는 데 사용할 수 없습니다. 자산 패키지 파일에 액세스하는 방법 및 자산 패키지를 사용하려면 NTFS 드라이브에 앱을 설치해야 하는 이유에 대해 자세히 알아보려면 [자산 패키지 및 패키지 접기를 사용하여 개발](Package-Folding.md)을 참조하세요.

자산 패키지를 실행하도록 할지 여부를 제어하기 위해 AppxManifest의 **Properties** 요소에 있는 **[uap6:AllowExecution](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap6-allowexecution)** 을 사용할 수 있습니다. 또한 다음과 같이 되려면 **uap6**를 최상위 **Package** 요소에 추가해야 합니다. 

```XML
<Package IgnorableNamespaces="uap uap6" 
xmlns:uap6="http://schemas.microsoft.com/appx/manifest/uap/windows10/6" 
xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10" 
xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10">
```

 지정하지 않은 경우 **AllowExecution**의 기본값은 **true**입니다. 더 빠르게 게시하려면 실행 가능한 파일이 없는 자산 패키지에 대해 이를 **false**로 설정합니다.  



