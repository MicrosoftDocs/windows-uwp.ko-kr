---
description: 웹 앱과 기타 \* nix 기반 플랫폼 (Unix, Linux 및 변형) 간의 최적 호환성을 위해 utf-8 문자 인코딩을 사용 하 고 지역화 버그를 최소화 하 고 테스트 오버 헤드를 줄입니다.
title: Windows UTF-8 코드 페이지 사용
template: detail.hbs
ms.date: 06/12/2019
ms.topic: article
keywords: windows 10, uwp, 세계화, 지역화 가능성, 지역화
ms.localizationpriority: medium
ms.openlocfilehash: 8aefe7bc45bc7c41347fe8fc4b8192347c3e4354
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93034306"
---
# <a name="use-the-utf-8-code-page"></a>UTF-8 코드 페이지 사용

웹 [UTF-8](http://www.utf-8.com/) 앱과 기타 \* nix 기반 플랫폼 (Unix, Linux 및 변형) 간의 최적 호환성을 위해 utf-8 문자 인코딩을 사용 하 고 지역화 버그를 최소화 하 고 테스트 오버 헤드를 줄입니다.

U t f-8은 국제화를 위한 유니버설 코드 페이지 이며 전체 유니코드 문자 집합을 인코딩할 수 있습니다. 웹에서 pervasively 사용 되며 * nix 기반 플랫폼의 기본값입니다.

> [!NOTE]
> 인코딩된 문자는 1 바이트에서 4 바이트 사이를 사용 합니다. UTF-8 인코딩은 최대 6 바이트의 긴 바이트 시퀀스를 지원 하지만 유니코드 6.0의 가장 큰 코드 포인트 (U + 10FFFF)는 4 바이트만 사용 합니다.

## <a name="-a-vs--w-apis"></a>-A와-W Api 비교
  
Win32 Api는 종종-A 및-W 변형을 모두 지원 합니다.

-변형은 시스템 및 지원에 구성 된 ANSI 코드 페이지를 인식 `char*` 하는 반면-W 변형은 u t f-16 및 지원으로 작동 `WCHAR` 합니다.

최근 까지는 Windows에서 "유니코드"-W 변형 오버 Api를 강조 했습니다. 그러나 최근 릴리스는 응용 프로그램에 UTF-8 지원을 도입 하기 위한 수단으로 ANSI 코드 페이지 및-A Api를 사용 했습니다. ANSI 코드 페이지가 u t f-8로 구성 된 경우-Api는 u t f-8로 작동 합니다. 이 모델은 코드를 변경 하지 않고 Api로 빌드된 기존 코드를 지원할 수 있는 이점을 제공 합니다.

## <a name="set-a-process-code-page-to-utf-8"></a>프로세스 코드 페이지를 u t f-8로 설정

Windows 버전 1903 (2019 업데이트 될 수 있음)에서 패키지 된 앱에 대 한 appxmanifest.xml의 ActiveCodePage 속성 또는 패키지 되지 않은 앱에 대 한 fusion 매니페스트를 사용 하 여 프로세스가 프로세스 코드 페이지로 u t f-8을 강제로 사용 하도록 할 수 있습니다.

이 속성을 선언 하 고 이전 Windows 빌드에서 대상/실행 하지만 기존 코드 페이지 검색 및 변환을 일반적인 방법으로 처리 해야 합니다. Windows 버전 1903의 최소 대상 버전을 사용 하는 경우 프로세스 코드 페이지는 항상 u t f-8 이므로 레거시 코드 페이지를 검색 하 고 변환을 피할 수 있습니다.

## <a name="examples"></a>예

**패키지 된 앱에 대 한 Appx 매니페스트:**

```xaml
<?xml version="1.0" encoding="utf-8"?>
<Package xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
         ...
         xmlns:uap7="http://schemas.microsoft.com/appx/manifest/uap/windows10/7"
         xmlns:uap8="http://schemas.microsoft.com/appx/manifest/uap/windows10/8"
         ...
         IgnorableNamespaces="... uap7 uap8 ...">

  <Applications>
    <Application ...>
      <uap7:Properties>
        <uap8:ActiveCodePage>UTF-8</uap8:ActiveCodePage>
      </uap7:Properties>
    </Application>
  </Applications>
</Package>
```

**패키지 되지 않은 Win32 앱에 대 한 Fusion 매니페스트:**

``` xaml
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<assembly manifestVersion="1.0" xmlns="urn:schemas-microsoft-com:asm.v1">
  <assemblyIdentity type="win32" name="..." version="6.0.0.0"/>
  <application>
    <windowsSettings>
      <activeCodePage xmlns="http://schemas.microsoft.com/SMI/2019/WindowsSettings">UTF-8</activeCodePage>
    </windowsSettings>
  </application>
</assembly>
```

> [!NOTE]
> 를 사용 하 여 명령줄에서 기존 실행 파일에 매니페스트를 추가 합니다. `mt.exe -manifest <MANIFEST> -outputresource:<EXE>;#1`

## <a name="code-page-conversion"></a>코드 페이지 변환

Windows가 u t f-16 ()에서 기본적으로 작동할 때 `WCHAR` utf-8 데이터를 u t f-16으로 변환 하거나 그 반대로 변환 하 여 Windows api와 상호 운용 해야 할 수 있습니다.

[MultiByteToWideChar](/windows/desktop/api/stringapiset/nf-stringapiset-multibytetowidechar) 및 [WideCharToMultiByte](/windows/desktop/api/stringapiset/nf-stringapiset-widechartomultibyte) 를 사용 하면 u t f-8과 utf-16 ( `WCHAR` ) (및 다른 코드 페이지) 사이를 변환할 수 있습니다. 이 기능은 레거시 Win32 API만 이해할 수 있는 경우에 특히 유용 `WCHAR` 합니다. 이러한 함수를 사용 하면 u t f-8 입력을로 변환 하 여 `WCHAR` 를 a W API에 전달 하 고 필요한 경우 결과를 다시 변환할 수 있습니다.
를로 설정 하 여 이러한 함수를 사용 하는 경우 `CodePage` `CP_UTF8` 또는 중 `dwFlags` 하나를 사용 `0` `MB_ERR_INVALID_CHARS` 합니다. 그렇지 않으면이 `ERROR_INVALID_FLAGS` 발생 합니다.

> [!NOTE]
> `CP_ACP``CP_UTF8`Windows 버전 1903 (2019 업데이트) 이상에서 실행 중이 고 위에서 설명한 ActiveCodePage 속성이 u t f-8로 설정 된 경우에만 해당 합니다. 그렇지 않으면 레거시 시스템 코드 페이지를 따릅니다. 명시적으로를 사용 하는 것이 좋습니다 `CP_UTF8` .

## <a name="related-topics"></a>관련 항목

- [코드 페이지](/windows/desktop/Intl/code-pages)
- [코드 페이지 식별자](/windows/desktop/Intl/code-page-identifiers)
