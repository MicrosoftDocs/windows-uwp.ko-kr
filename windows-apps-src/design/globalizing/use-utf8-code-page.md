---
Description: 웹 앱 간에 최적의 호환성에 대 한 인코딩 및 기타 utf-8 문자를 사용 하 여 * nix 기반 플랫폼 (Unix, Linux 및 변형) 지역화 버그를 최소화 하 고 테스트 오버 헤드를 줄입니다.
title: Windows utf-8 코드 페이지 사용
template: detail.hbs
ms.date: 06/12/2019
ms.topic: article
keywords: windows 10, uwp, 세계화, 현지화, 지역화
ms.localizationpriority: medium
ms.openlocfilehash: 453d58b0d52aaa24461784b6f393b26b93e572a1
ms.sourcegitcommit: 51d884c3646ba3595c016e95bbfedb7ecd668a88
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67827379"
---
# <a name="use-the-utf-8-code-page"></a>Utf-8 코드 페이지 사용

사용 하 여 [u t F-8](http://www.utf-8.com/) 웹 앱 및 기타 간의 최적의 호환성에 대 한 인코딩 문자 * nix 기반 플랫폼 (Unix, Linux 및 변형) 지역화 버그를 최소화 하 고 테스트 오버 헤드를 줄입니다.

U t F-8은 국제화에 대 한 유니버설 코드 페이지 및 1-4 바이트 가변 너비 인코딩을 사용 하 여 모든 유니코드 코드 포인트를 지원 합니다. 웹에서 pervasively 사용 되 고 기본값은 * nix 기반 플랫폼입니다.

## <a name="-a-vs--w-apis"></a>-A-W Api 비교
  
Win32 Api는 종종-및-W 변형을 지원합니다.

--W 변형 u t F-16 및 지원에서 작동 하는 동안에 시스템 및 지원 char *를 구성 하는 ANSI 코드 페이지를 인식 하는 변형을 `WCHAR`합니다.

최근까지 Windows-A Api를 통해 "Unicode"-W 변형 강조 했습니다. 그러나 최신 릴리스는 ANSI 코드 페이지를 사용 하는 지원과-A u t F-8을 도입 하는 수단으로 Api 앱에 있습니다. ANSI 코드 페이지 u t F-8으로 구성 된 경우-Api 작동 u t F-8에서입니다. 이 모델에 사용 하 여 빌드한-A Api 코드 변경 없이 기존 코드를 지원 합니다.

## <a name="set-a-process-code-page-to-utf-8"></a>프로세스 코드 페이지 u t F-8로 설정

패키지 된 앱에 대 한 appxmanifest 또는 ActiveCodePage 속성을 사용 하 여 패키지 되지 않은 앱에 대 한 fusion 매니페스트를 통해 프로세스 코드 페이지 u t F-8을 사용 하는 프로세스를 적용할 수 있습니다.

이 속성을 선언할 수 있습니다 및 대상/실행 이전 Windows에서 빌드, 하지만 레거시 코드 페이지 검색 및 변환을 정상적으로 처리 해야 합니다 (19 시간 1 중 최소 대상 버전을 사용 하 여 프로세스 코드 페이지는 항상 u t F-8).

## <a name="examples"></a>예

**패키지 된 앱의 Appx 매니페스트:**

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

**패키지 되지 않은 Win32 앱에 대 한 fusion 매니페스트:**

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
> 매니페스트를 사용 하 여 명령줄에서 기존 실행 파일에 추가 `mt.exe -manifest <MANIFEST> -outputresource:<EXE>;#1`

## <a name="code-page-conversion"></a>코드 페이지 변환

Windows 작동 u t F-16 (WCHAR)에서 고유 하 게 변환할 u t F-16으로 (또는 그 반대로) 데이터를 u t F-8 Windows Api와 상호 운용 해야 할 수도 있습니다.

[MultiByteToWideChar](https://docs.microsoft.com/windows/desktop/api/stringapiset/nf-stringapiset-multibytetowidechar) 하 고 [WideCharToMultiByte](https://docs.microsoft.com/windows/desktop/api/stringapiset/nf-stringapiset-widechartomultibyte) utf-8 및 utf-16 (WCHAR) (및 다른 코드 페이지) 간에 변환할 수 있습니다. 레거시 Win32 API WCHAR 파악할 수 있습니다 하는 경우 특히 유용 합니다. 이러한 함수를 사용 하면를-W API에 전달 하 고 필요한 경우에 다시 결과 고용해 WCHAR u t F-8 입력을 변환할 수 있습니다.
코드 페이지 CP_UTF8, 0 또는 MB_ERR_INVALID_CHARS를 사용 하 여 dwFlags 사용 하 여 Windows에서 이러한 함수를 사용 하 여 그렇지 않은 경우는 ERROR_INVALID_FLAGS 발생 합니다.

참고: CP_ACP 매월 CP_UTF8 Windows 버전이 1903 (월 2019 업데이트)를 실행 하는 경우에 하 고 위에서 설명한 ActiveCodePage 속성이 u t F-8로 설정 됩니다. 그렇지 않은 경우 레거시 시스템 코드 페이지는 유지합니다. CP_UTF8를 명시적으로 사용 하는 것이 좋습니다.

## <a name="related-topics"></a>관련 항목

- [코드 페이지](https://docs.microsoft.com/windows/desktop/Intl/code-pages)
- [코드 페이지 식별자](https://docs.microsoft.com/windows/desktop/Intl/code-page-identifiers)
