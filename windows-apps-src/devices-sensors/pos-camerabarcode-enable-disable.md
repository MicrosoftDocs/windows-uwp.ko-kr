---
title: 카메라 바코드 스캐너 구성
description: Windows 10에서 시스템 레지스트리 키를 설정 하 여 카메라 바코드 스캐너에 대 한 소프트웨어 디코더를 사용 하거나 사용 하지 않도록 설정 하는 방법에 대해 알아봅니다.
ms.date: 04/08/2019
ms.topic: article
keywords: windows 10, uwp, 서비스 지점, pos
ms.localizationpriority: medium
ms.openlocfilehash: fefe15dd36cbc08fcae3b5bc0199eafad400cf1f
ms.sourcegitcommit: 8e0e4cac79554e86dc7f035c4b32cb1f229142b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/26/2020
ms.locfileid: "88943063"
---
# <a name="enable-or-disable-the-software-decoder-that-ships-with-windows"></a>Windows와 함께 제공 되는 소프트웨어 디코더 사용 또는 사용 안 함

Windows 10 버전 1803에서는 소프트웨어 디코더가 기본적으로 설치 되 고 사용 하도록 설정 됩니다.  카메라 바코드 스캐너를 사용 하지 않으려는 경우 Windows와 함께 제공 되는 소프트웨어 디코더를 사용 하지 않도록 설정 하 고, Windows. PointOfService.. l i n c. l i n e.

## <a name="enable-or-disable-using-the-system-registry"></a>시스템 레지스트리를 사용 하거나 사용 하지 않도록 설정

Windows와 함께 제공 되는 소프트웨어 디코더는 *HKLM\Software\Microsoft\PointOfService\BarcodeScanner* 아래에 레지스트리 키 *InboxDecoder* 를 추가 하 고 아래에 설명 된 대로 *사용* 값을 설정 하 여 시스템 레지스트리를 통해 사용 하거나 사용 하지 않도록 설정할 수 있습니다.

| 값 이름  | 값 형식 | 값 | 상태 |
| ----------- | --------- | -------|--------|
| 사용      | DWORD     | 1(기본값)<br/>0 |  Windows와 함께 제공 되는 소프트웨어 디코더를 사용 하도록 설정 합니다. <br/> Windows와 함께 제공 되는 소프트웨어 디코더를 사용 하지 않도록 설정 |

Windows와 함께 제공 되는 소프트웨어 디코더를 **사용 하지 않도록 설정** 하는 데 사용할 수 있는 예제 레지스트리 파일은 다음과 같습니다.

```text
Windows Registry Editor Version 5.00

[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\PointOfService\BarcodeScanner\InboxDecoder]
"Enable"=dword:0000000
```  

Windows와 함께 제공 되는 소프트웨어 디코더를 **사용 하도록 설정** 하려면 다음 예제 레지스트리 파일을 사용 합니다.

```text
Windows Registry Editor Version 5.00

[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\PointOfService\BarcodeScanner\InboxDecoder]
"Enable"=dword:0000001
```  

> [!Warning]
> 레지스트리를 잘못 수정하면 심각한 문제가 발생할 수 있습니다.  추가적인 보호를 위해, 수정하기 전에 레지스트리를 백업하세요.  그러면 문제가 발생했을 때 레지스트리를 복원할 수 있습니다.  레지스트리를 백업 및 복원 하는 방법에 대 한 자세한 내용은 다음 문서 번호를 클릭 하 여 Microsoft 기술 자료 문서를 참조 하십시오. <br/><br/> [322756](https://support.microsoft.com/help/322756/how-to-back-up-and-restore-the-registry-in-windows) Windows에서 레지스트리를 백업 하 고 복원 하는 방법을 설명 합니다.

> [!NOTE]
> Windows 10에 기본 제공 되는 소프트웨어 디코더는  [**Digimarc Corporation**](https://www.digimarc.com/)을 통해 제공 됩니다.

## <a name="see-also"></a>참조

### <a name="samples"></a>샘플

- [바코드 스캐너 샘플](https://github.com/microsoft/Windows-universal-samples/tree/master/Samples/BarcodeScanner)
