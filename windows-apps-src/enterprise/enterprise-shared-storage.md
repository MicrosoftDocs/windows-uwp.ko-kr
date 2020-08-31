---
ms.assetid: B48E21AB-0EA5-444B-8333-393DD8D1B76D
title: 엔터프라이즈 공유 저장소
description: 엔터프라이즈 공유 저장소는 데이터를 공유 하는 lob (기간 업무) 앱에 대 한 로컬 데이터 위치를 정의 합니다.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 595d2d23cca3f128e8be5447f125ab30aedccf9d
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89168407"
---
# <a name="enterprise-shared-storage"></a>엔터프라이즈 공유 저장소

공유 저장소는 제한 된 기능  **Enterprisedevicelockdown** 구성 된 앱과 모든 읽기 및 쓰기 액세스 권한이 있는 두 개의 위치로 구성 됩니다. **Enterprisedevicelockdown** 기능을 사용 하면 앱에서 장치 잠금 API를 사용 하 고 엔터프라이즈 공유 저장소 폴더에 액세스할 수 있습니다. API에 대 한 자세한 내용은 [**Windows Embedded. DeviceLockdown**](/uwp/api/Windows.Embedded.DeviceLockdown) 네임 스페이스를 참조 하세요.  

이러한 위치는 로컬 드라이브에 설정 됩니다.
- \Data\SharedData\Enterprise\Persistent
- \Data\SharedData\Enterprise\Non-Persistent

## <a name="scenarios"></a>시나리오

엔터프라이즈 공유 저장소는 다음과 같은 시나리오에 대 한 지원을 제공 합니다.

- 앱의 인스턴스 내에서 동일한 앱의 인스턴스 간에 데이터를 공유 하거나, 둘 다 적절 한 기능 및 인증서를 포함 하 고 있는 경우에도 앱 간에 데이터를 공유할 수 있습니다.
- \Data\SharedData\Enterprise\Persistent 폴더에 있는 로컬 하드 드라이브에 데이터를 저장할 수 있으며 장치가 다시 설정 된 후에도 지속 됩니다.
- MDM(모바일 디바이스 관리) 서비스를 통해 디바이스에서 파일의 읽기, 쓰기 및 삭제 등과 같이 파일을 조작합니다. MDM 서비스를 통해 엔터프라이즈 공유 저장소를 사용 하는 방법에 대 한 자세한 내용은 [Enterpriseextfilesystem CSP](/windows/client-management/mdm/enterpriseextfilessystem-csp)를 참조 하세요.

## <a name="access-enterprise-shared-storage"></a>엔터프라이즈 공유 저장소 액세스

다음 예제에서는 패키지 매니페스트에서 엔터프라이즈 공유 저장소에 액세스 하는 기능을 선언 하는 방법과 Windows. StorageFolder 클래스를 사용 하 여 공유 저장소 폴더에 액세스 하는 방법을 보여 줍니다.

앱 패키지 매니페스트에는 다음 기능이 포함 됩니다.

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

공유 데이터 위치에 액세스 하기 위해 앱은 다음 코드를 사용 합니다.

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