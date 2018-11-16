---
author: TerryWarwick
title: 카메라 바코드 스캐너 구성
description: 카메라 바코드 스캐너 활성화 또는 비활성화
ms.author: jken
ms.date: 05/1/2018
ms.topic: article
keywords: windows 10, uwp, 서비스 지점, pos
ms.localizationpriority: medium
ms.openlocfilehash: 35666f64c88ad56b8f5bd3052ebbee069ccaecfc
ms.sourcegitcommit: e38b334edb82bf2b1474ba686990f4299b8f59c7
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/15/2018
ms.locfileid: "6839229"
---
# <a name="enable-or-disable-the-software-decoder-that-ships-with-windows"></a>Windows와 함께 배송된 소프트웨어 디코더를 활성화 또는 비활성화
Windows 10 버전 1803에서는 기본적으로 소프트웨어 디코더가 설치 및 활성화됩니다.  카메라 바코드 스캐너를 사용하고 싶지 않거나 Windows.Devices.PointOfService.BarcodeScanner API에서 작동하는 타사 디코더를 구입했고 둘 모두를 사용하고 싶지 않은 경우에는 Windows와 함께 배송된 소프트웨어 디코더를 비활성화할 수 있습니다.

## <a name="enable-or-disable-using-the-system-registry"></a>시스템 레지스트리 활성화 또는 비활성화
*HKLM\Software\Microsoft\PointOfService\BarcodeScanner* 아래의 레지스트리 키를 추가하고 *Enable* 아래의 *InboxDecoder* 값을 아래 설명과 같이 설정하여 시스템 레지스트를 통해 Windows와 함께 배송된 소프트웨어 디코더를 활성화 또는 비활성화할 수 있습니다.

| 값 이름  | 값 유형 | 값 | 상태 |
| ----------- | --------- | -------|--------|
| 활성화      | DWORD     | 1(기본값)<br/>0 |  Windows와 함께 배송된 소프트웨어 디코더 활성화 <br/> Windows와 함께 배송된 소프트웨어 디코더 비활성화 |


Windows와 함께 배송된 소프트웨어 디코더를 **비활성화**하는 데 사용할 수 있는 레지스트리 파일의 예는 다음과 같습니다.

```
Windows Registry Editor Version 5.00

[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\PointOfService\BarcodeScanner\InboxDecoder]
"Enable"=dword:0000000


```  
    
이 예제 레지스트리 파일을 사용하여 Windows와 함께 배송된 소프트웨어 디코더를 **활성화**합니다.

```
Windows Registry Editor Version 5.00

[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\PointOfService\BarcodeScanner\InboxDecoder]
"Enable"=dword:0000001


```  

> [!Warning] 
> 레지스트리를 잘못 수정할 경우 심각한 문제가 발생할 수 있습니다.  보호를 강화하기 위해 수정 전에 레지스트리를 백업합니다.  그러면 문제가 발생했을 때 레지스트리를 복원할 수 있습니다.  레지스트리를 백업 및 복원하는 방법에 대한 자세한 내용은 다음 문서 번호를 클릭하여 Microsoft 기술 자료의 문서를 확인하세요. <br/><br/> [322756](http://support.microsoft.com/kb/322756) Windows에서 레지스트리를 백업 및 복원하는 방법입니다.

> [!NOTE]
> Windows 10에서 기본 제공되는 소프트웨어 디코더는  [**Digimarc Corporation**](https://www.digimarc.com/)이 무료로 제공합니다.
