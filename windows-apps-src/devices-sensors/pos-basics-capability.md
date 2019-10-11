---
title: PointOfService 장치 접근 권한 값
description: PointOfService 접근 권한 값은 Windows.Devices.PointOfService 네임스페이스의 사용에 필요합니다.
ms.date: 05/02/2018
ms.topic: article
keywords: windows 10, uwp, 서비스 지점, pos
ms.localizationpriority: medium
ms.openlocfilehash: f120e093ab65224ca4c32b64640100b6abd1a36a
ms.sourcegitcommit: 0301f794f994e604ffc131de7e40ffcede3530c9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/08/2019
ms.locfileid: "72036267"
---
# <a name="pointofservice-device-capability"></a>PointOfService 장치 접근 권한 값
응용 프로그램 패키지 매니페스트에서 접근 권한 값을 선언하여 PointOfService API에 대한 액세스를 요청합니다. 대부분의 접근 권한 값은 매니페스트 디자이너를 사용하여, 또는 Microsoft Visual Studio에서 선언하거나 수동으로 추가할 수 있습니다.  

> [!Important]
> 응용 프로그램 매니페스트에서 **pointofservice** 기능을 선언 하지 않은 경우에는 **system.unauthorizedaccessexception** 서비스 네임 스페이스에서 API를 사용 하려고 할 때 오류를 받게 됩니다. 

## <a name="declare-capability-using-manifest-designer"></a>매니페스트 디자이너를 사용하여 접근 권한 값 선언

1. **솔루션 탐색기**에서 UWP 응용 프로그램의 프로젝트 노드를 확장합니다.
2. **Package.appxmanifest** 파일을 두 번 클릭합니다.  
*매니페스트 파일이 XML 코드 뷰에서 이미 열려 있는 경우 Visual Studio에서 파일을 닫으라는 메시지를 표시 합니다.*
3. **접근 권한 값** 탭을 클릭합니다.
4. 접근 권한 값 목록에서 **서비스 지점** 옆에 있는 확인란을 클릭하여 서비스 지점 장치 접근 권한 값을 활성화합니다.


## <a name="declare-capability-manually"></a>수동으로 접근 권한 값 선언

1. **솔루션 탐색기**에서 UWP 응용 프로그램의 프로젝트 노드를 확장합니다.
2. **Package.appxmanifest** 파일을 마우스 오른쪽 단추로 클릭하고 **코드 보기**를 선택합니다.
3. 응용 프로그램 매니페스트의 접근 권한 값 섹션에 PointOfService DeviceCapability 요소를 추가합니다.  

```xml
  <Capabilities>
    <DeviceCapability Name="pointOfService" />
  </Capabilities>
   ```
