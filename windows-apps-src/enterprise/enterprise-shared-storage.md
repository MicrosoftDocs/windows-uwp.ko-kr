---
ms.assetid: B48E21AB-0EA5-444B-8333-393DD8D1B76D
title: 엔터프라이즈 공유 저장소
description: 엔터프라이즈 공유 저장소는 LOB(기간 업무) 앱의 로컬 데이터 위치를 정의하여 데이터를 공유합니다.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 006507d4665f5578310b8d3e31fb8f7fba4117a2
ms.sourcegitcommit: b5c9c18e70625ab770946b8243f3465ee1013184
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/29/2018
ms.locfileid: "7984828"
---
# <a name="enterprise-shared-storage"></a>엔터프라이즈 공유 저장소

공유 저장소는 두 개의 위치로 구성되며 여기서 제한된 접근 권한 값 **enterpriseDeviceLockdown** 및 엔터프라이즈 인증서가 있는 앱이 모든 읽기 및 쓰기 권한을 가집니다. **enterpriseDeviceLockdown** 접근 권한 값을 통해 앱은 디바이스 잠금 API를 사용하고 엔터프라이즈 공유 저장소 폴더에 액세스할 수 있습니다. API에 대한 자세한 내용은 [**Windows.Embedded.DeviceLockdown**](http://go.microsoft.com/fwlink/?LinkId=699331) 네임스페이스를 참조하세요.  

이러한 위치는 로컬 드라이브에 설정됩니다.
- \Data\SharedData\Enterprise\Persistent
- \Data\SharedData\Enterprise\Non-Persistent

## <a name="scenarios"></a>시나리오

엔터프라이즈 공유 저장소는 다음과 같은 시나리오를 지원합니다.

- 동일한 앱의 인스턴스 간에 앱의 인스턴스 내에서 데이터를 공유할 수 있습니다. 적절한 접근 권한 값 및 인증서가 있다고 가정하는 두 앱 간에도 마찬가지입니다.
- 로컬 하드 드라이브에서 \Data\SharedData\Enterprise\Persistent 폴더에 데이터를 저장할 수 있으며 디바이스가 초기화된 후에도 지속됩니다.
- MDM(모바일 디바이스 관리) 서비스를 통해 디바이스에서 파일의 읽기, 쓰기 및 삭제 등과 같이 파일을 조작합니다. MDM 서비스를 통해 엔터프라이즈 공유 저장소를 사용하는 방법에 대한 자세한 내용은 [EnterpriseExtFileSystem CSP](http://go.microsoft.com/fwlink/?LinkId=699333)를 참조하세요.

## <a name="access-enterprise-shared-storage"></a>엔터프라이즈 공유 저장소 액세스

다음 예제에서는 패키지 매니페스트의 엔터프라이즈 공유 저장소에 액세스할 수 있는 접근 권한 값을 선언하는 방법과 Windows.Storage.StorageFolder 클래스를 사용하여 공유 저장소 폴더에 액세스하는 방법을 보여 줍니다.

앱 패키지 매니페스트에서 다음 접근 권한 값을 포함합니다.

```xml
<Package
  xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
  xmlns:mp="http://schemas.microsoft.com/appx/2014/phone/manifest"
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities"
  IgnorableNamespaces="uap mp rescap">

…

<Capabilities>
    <rescap:Capability Name="enterpriseDeviceLockdown"/>
</Capabilities>
```

공유 데이터 위치에 액세스하려면 앱에서 다음 코드를 사용합니다.

```csharp
using System;
using System.Collections.Generic;
using System.Diagnostics;
using Windows.Storage;

…

// Get the Enterprise Shared Storage folder.
var enterprisePersistentFolderRoot = @"C:\Data\SharedData\Enterprise\Persistent";

StorageFolder folder =
    await StorageFolder.GetFolderFromPathAsync(enterprisePersistentFolderRoot);

// Get the files in the folder.
IReadOnlyList<StorageFile> sortedItems =
    await folder.GetFilesAsync();

// Iterate over the results and print the list of files
// to the Visual Studio Output window.
foreach (StorageFile file in sortedItems)
    Debug.WriteLine(file.Name + ", " + file.DateCreated);
```

