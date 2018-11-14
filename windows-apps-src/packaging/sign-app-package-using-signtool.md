---
author: laurenhughes
title: SignTool을 사용하여 앱 패키지에 서명
description: SignTool을 사용하여 인증서로 앱 패키지에 수동으로 서명합니다.
ms.author: lahugh
ms.date: 09/30/2018
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 171f332d-2a54-4c68-8aa0-52975d975fb1
ms.localizationpriority: medium
ms.openlocfilehash: adb94257f6a978ba0b5ea56facf3894e2cfae32b
ms.sourcegitcommit: 38f06f1714334273d865935d9afb80efffe97a17
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/09/2018
ms.locfileid: "6206026"
---
# <a name="sign-an-app-package-using-signtool"></a>SignTool을 사용하여 앱 패키지에 서명


**SignTool**은 인증서를 사용하여 디지털 방식으로 앱 패키지 또는 번들에 서명하는 데 사용되는 명령줄 도구입니다. 인증서는 사용자가 만들거나(테스트용) 회사가 발급할 수 있습니다(배포용). 앱 패키지 서명은 사용자에게 서명 이후 앱 데이터가 수정되지 않았다는 확인을 제공하는 동시에 앱 패키지에 서명한 사용자 또는 회사의 신원도 확인합니다. **SignTool**은 암호화되었거나 암호화되지 않은 앱 패키지 및 번들에 서명할 수 있습니다.

> [!IMPORTANT] 
> Visual Studio를 사용하여 앱을 개발하는 경우 Visual Studio 마법사를 사용하여 앱 패키지를 만들고 서명하는 것이 좋습니다. 자세한 내용은 [Visual Studio를 사용하여 UWP 앱 패키징](https://msdn.microsoft.com/windows/uwp/packaging/packaging-uwp-apps)을 참조하세요.

코드 서명 및 인증서 전반에 대한 자세한 내용은 [코드 서명 소개](https://msdn.microsoft.com/library/windows/desktop/aa380259.aspx#introduction_to_code_signing)를 참조하세요.

## <a name="prerequisites"></a>필수 구성 요소
- **앱 패키지**  
    수동으로 앱 패키지를 만드는 방법에 대한 자세한 내용은 [MakeAppx.exe 도구로 앱 패키지 만들기](https://msdn.microsoft.com/windows/uwp/packaging/create-app-package-with-makeappx-tool)를 참조하세요. 

- **유효한 서명 인증서**  
    유효한 서명 인증서를 만들거나 가져오는 방법에 대한 자세한 내용은 [패키지 서명용 인증서 만들기 또는 가져오기](https://msdn.microsoft.com/windows/uwp/packaging/create-certificate-package-signing)를 참조하세요.

- **SignTool.exe**  
    SDK 설치 경로에 따라 Windows 10 PC에서 **SignTool**이 있는 위치는 다음과 같습니다.
    - x86: C:\Program Files (x86)\Windows Kits\10\bin\x86\SignTool.exe
    - x64: C:\Program Files (x86)\Windows Kits\10\bin\x64\SignTool.exe

## <a name="using-signtool"></a>SignTool 사용하기

**SignTool**을 사용하여 파일에 서명하거나, 서명 또는 타임스탬프를 확인하거나, 서명을 제거하는 등의 작업을 할 수 있습니다. 앱 패키지 서명을 위해 여기서는 **sign** 명령을 중심으로 설명하겠습니다. **SignTool**에 대한 전체 정보는 [SignTool](https://msdn.microsoft.com/library/windows/desktop/aa387764.aspx) 참조 페이지를 참조하세요. 

### <a name="determine-the-hash-algorithm"></a>해시 알고리즘 결정
**SignTool**을 사용하여 앱 패키지 또는 번들에 서명하는 경우 **SignTool**에서 사용하는 해시 알고리즘이 앱을 패키징하는 데 사용한 알고리즘과 동일해야 합니다. 예를 들어 **MakeAppx.exe**를 사용하여 본 설정으로 앱 패키지를 만든 경우 **SignTool**을 사용할 때 SHA256을 지정해야 합니다. 이것이 **MakeAppx.exe**가 사용하는 기본 알고리즘이기 때문입니다.

앱을 패키징할 때 사용된 해시 알고리즘을 확인하려면 앱 패키지의 내용을 추출하여 AppxBlockMap.xml 파일을 검사합니다. 앱 패키지를 압축 해제/추출하는 방법을 알아보려면 [패키지나 번들에서 파일 추출](https://msdn.microsoft.com/windows/uwp/packaging/create-app-package-with-makeappx-tool#extract-files-from-a-package-or-bundle)을 참조하세요. 해시 매서드는 BlockMap 요소에 있으며 형식은 다음과 같습니다.
```
<BlockMap xmlns="http://schemas.microsoft.com/appx/2010/blockmap" 
HashMethod="http://www.w3.org/2001/04/xmlenc#sha256">
```

이 표는 각 HashMethod 값 및 해당 해시 알고리즘을 보여줍니다.


| HashMethod 값                              | 해시 알고리즘 |
|-----------------------------------------------|----------------|
| http://www.w3.org/2001/04/xmlenc#sha256       | SHA256         |
| http://www.w3.org/2001/04/xmldsig-more#sha384 | SHA384         |
| http://www.w3.org/2001/04/xmlenc#sha512       | SHA512         |

> [!NOTE]
> **SignTool**의 기본 알고리즘은 SHA1(**MakeAppx.exe**에서 사용할 수 없음)이므로 **SignTool**을 사용하는 경우 항상 해시 알고리즘을 지정해야 합니다.

### <a name="sign-the-app-package"></a>앱 패키지에 서명

필수 구성 요소를 모두 준비하고 앱 패키징에 사용된 해시 알고리즘을 확인했으며 이제 서명할 준비가 되었습니다. 

**SignTool** 패키지 서명을 위한 일반 명령줄 구문은 다음과 같습니다.
```
SignTool sign [options] <filename(s)>
```

앱에 설명하는 데 사용하는 인증서는 .pfx 파일이거나 인증서 저장소에서 설치되어 있어야 합니다.

.pfx 파일의 인증서를 사용하여 앱 패키지에 서명하려면 다음 구문을 사용합니다.
```
SignTool sign /fd <Hash Algorithm> /a /f <Path to Certificate>.pfx /p <Your Password> <File path>.appx
```
```
SignTool sign /fd <Hash Algorithm> /a /f <Path to Certificate>.pfx /p <Your Password> <File path>.msix
```
`/a` 옵션을 지정하면 **SignTool**이 자동으로 최선의 인증서를 선택할 수 있습니다.

인증서가 .pfx 파일이 아닌 경우 다음 구문을 사용합니다.
```
SignTool sign /fd <Hash Algorithm> /n <Name of Certificate> <File Path>.appx
```
```
SignTool sign /fd <Hash Algorithm> /n <Name of Certificate> <File Path>.msix
```

또는, 이 구문을 사용하여 &lt;인증서 이름&gt; 대신 원하는 인증서의 SHA1 해시를 지정할 수도 있습니다.
```
SignTool sign /fd <Hash Algorithm> /sha1 <SHA1 hash> <File Path>.appx
```
```
SignTool sign /fd <Hash Algorithm> /sha1 <SHA1 hash> <File Path>.msix
```

일부 인증서는 비밀 번호를 사용하지 않습니다. 인증서에 비밀 번호가 없는 경우 샘플 명령에서 "/p &lt;Your Password&gt;"를 생략합니다.

유효한 인증서를 사용하여 앱 패키지에 서명하면 패키지를 Microsoft Store에 업로드할 준비가 된 것입니다. Microsoft Store에 앱을 업로드 및 제출하는 방법에 대한 자세한 지침은 [앱 제출](https://msdn.microsoft.com/windows/uwp/publish/app-submissions)을 참조하세요.

## <a name="common-errors-and-troubleshooting"></a>일반 오류 및 문제 해결
**SignTool** 사용 시 가장 일반적인 유형의 오류는 내부 오류이며 일반적으로 다음과 같습니다.

```
SignTool Error: An unexpected internal error has occurred.
Error information: "Error: SignerSign() failed." (-2147024885 / 0x8007000B) 
```

오류 코드가 0x8008로 시작하는 경우(예: 0x80080206(APPX_E_CORRUPT_CONTENT)) 서명되는 패키지가 잘못된 것입니다. 이러한 오류가 발생하면 패키지를 다시 빌드하고 **SignTool**을 다시 실행해야 합니다.

**SignTool**에는 인증서 오류 및 필터링을 보여주는 디버그 옵션이 있습니다. 디버깅 기능을 사용하려면 `sign` 바로 뒤에 `/debug`옵션을 배치하고 그 다음에 전체 **SignTool** 명령을 배치합니다.
```
SignTool sign /debug [options]
``` 

보다 일반적인 오류는 0x8007000B입니다. 이러한 종류의 오류에 대한 자세한 내용은 이벤트 로그에서 확인할 수 있습니다.
 
이벤트 로그에서 자세한 내용을 확인하려면
- Eventvwr.msc를 실행합니다.
- 이벤트 로그를 엽니다. Event Viewer (Local) -> Applications and Services Logs -> Microsoft -> Windows -> AppxPackagingOM -> Microsoft-Windows-AppxPackaging/Operational
- 가장 최근의 오류 이벤트를 찾습니다.

내부 오류 0x8007000B는 일반적으로 다음 값 중 하나에 해당합니다.

| **이벤트 ID** | **이벤트 문자열 예** | **제안** |
|--------------|--------------------------|----------------|
| 150          | error 0x8007000B: The app manifest publisher name (CN=Contoso) must match the subject name of the signing certificate (CN=Contoso, C=US). | 앱 매니페스트 게시자 이름이 서명의 주체 이름과 정확히 일치해야 합니다.               |
| 151          | error 0x8007000B: The signature hash method specified (SHA512) must match the hash method used in the app package block map (SHA256).     | /fd 매개 변수에 지정된 해시 알고리즘이 잘못되었습니다. 앱 패키지 블록 맵(앱 패키지를 만드는 데 사용)과 일치하는 해시 알고리즘을 사용하여 **SignTool**을 다시 실행합니다.  |
| 152          | error 0x8007000B: The app package contents must validate against its block map.                                                           | 앱 패키지가 손상되었으며 앱 패키지를 다시 빌드하여 새 블록 앱을 생성해야 합니다. 앱 패키지를 만드는 방법에 대한 자세한 내용은 [MakeAppx.exe 도구로 앱 패키지 만들기](https://msdn.microsoft.com/windows/uwp/packaging/create-app-package-with-makeappx-tool)를 참조하세요. |