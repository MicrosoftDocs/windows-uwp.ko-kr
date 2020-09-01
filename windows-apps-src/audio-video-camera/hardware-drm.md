---
ms.assetid: A7E0DA1E-535A-459E-9A35-68A4150EE9F5
description: 이 항목에서는 유니버설 Windows 플랫폼 (UWP) 앱에 PlayReady 하드웨어 기반 DRM (디지털 권한 관리)을 추가 하는 방법에 대 한 개요를 제공 합니다.
title: 하드웨어 DRM
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: c0e92b422272488e49613531e4304587dc00e08f
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89157537"
---
# <a name="hardware-drm"></a>하드웨어 DRM


이 항목에서는 유니버설 Windows 플랫폼 (UWP) 앱에 PlayReady 하드웨어 기반 DRM (디지털 권한 관리)을 추가 하는 방법에 대 한 개요를 제공 합니다.

> [!NOTE] 
> 하드웨어 기반 PlayReady DRM은 TV 세트, 휴대폰, 태블릿 등의 windows 장치 및 windows 이외 장치를 포함 하 여 다 수의 장치에서 지원 됩니다. Windows 장치에서 PlayReady 하드웨어 DRM을 지원 하려면 Windows 10을 실행 하 고 있고 하드웨어 구성이 지원 되어야 합니다.

콘텐츠 공급자는 앱에서 전체 높은 값 콘텐츠를 재생할 수 있는 권한을 부여 하기 위해 하드웨어 기반 보호로 전환 하 고 있습니다. 이러한 요구를 충족 하기 위해 암호화 코어의 하드웨어 구현에 대 한 강력한 지원이 PlayReady에 추가 되었습니다. 이 지원을 통해 여러 장치 플랫폼에서 고화질 (1080p) 및 초소형 정의 (UHD) 콘텐츠를 안전 하 게 재생할 수 있습니다. 키 자료 (개인 키, 콘텐츠 키 및 언급 된 키를 파생 시키거나 잠금 해제 하는 데 사용 되는 기타 모든 키 자료 포함), 암호 해독 된 압축 및 압축 되지 않은 비디오 샘플은 하드웨어 보안을 활용 하 여 보호 됩니다.

## <a name="windows-tee-implementation"></a>Windows 티 구현

이 항목에서는 Windows 10에서 티 (신뢰할 수 있는 실행 환경)를 구현 하는 방법에 대 한 간략 한 개요를 제공 합니다.

Windows t 구현에 대 한 세부 정보는이 문서의 범위를 벗어났습니다. 그러나 표준 포팅 키트 티 포트와 Windows 포트 간의 차이점에 대 한 간략 한 설명은 도움이 됩니다. Windows에서는 OEM 프록시 계층을 구현 하 고 serialize 된 PRITEE 함수 호출을 Windows 미디어 파운데이션 하위 시스템의 사용자 모드 드라이버로 전송 합니다. 결과적으로 Windows 트리 (신뢰할 수 있는 실행 환경) 드라이버 또는 OEM의 그래픽 드라이버로 라우팅됩니다. 이러한 방법 중 하나에 대 한 세부 정보는이 문서의 범위를 벗어나는 것입니다. 다음 다이어그램에서는 Windows 포트의 일반적인 구성 요소 상호 작용을 보여 줍니다. Windows PlayReady 티 구현을 개발 하려는 경우에 문의할 수 있습니다 <WMLA@Microsoft.com> .

![windows 티 구성 요소 다이어그램](images/windowsteecomponentdiagram720.jpg)

## <a name="considerations-for-using-hardware-drm"></a>하드웨어 DRM 사용에 대 한 고려 사항

이 항목에서는 하드웨어 DRM을 사용 하도록 설계 된 앱을 개발할 때 고려해 야 하는 항목에 대 한 간단한 목록을 제공 합니다. Playready [DRM](playready-client-sdk.md#output-protection)에서 설명한 대로 playready HWDRM for windows 10을 사용 하 여 모든 출력 보호는 출력 보호 동작에 몇 가지 결과가 발생 하는 windows t t 구현 내에서 적용 됩니다.

-   **압축 되지 않은 디지털 비디오 270에 대 한 OPL (출력 보호 수준) 지원:** Windows 10 용 PlayReady HWDRM는 다운 (resolution)을 지원 하지 않으며 HDCP가 지원 됩니다. HWDRM에 대 한 높은 정의 콘텐츠의 OPL이 270 보다 큰 것이 좋습니다 (필수는 아니지만). 또한 라이선스 (Windows 10의 HDCP 버전 2.2)에서 HDCP 유형 제한을 설정 하는 것이 좋습니다.
-   **SWDRM (소프트웨어 DRM)과 달리 출력 보호는 최소 지원 모니터를 기준으로 모든 모니터에 적용 됩니다.** 예를 들어 사용자에 게 모니터 중 하나에서 HDCP를 지 원하는 두 개의 모니터가 연결 되어 있으면 HDCP를 지 원하는 모니터 에서만 콘텐츠가 렌더링 되는 경우에도 해당 라이선스에 HDCP가 필요한 경우 재생이 실패 합니다. 소프트웨어 DRM에서 콘텐츠는 HDCP를 지 원하는 모니터에만 렌더링 되는 동안 재생 됩니다.
-   콘텐츠 키 및 라이선스에서 **다음 조건을 충족 하지 않는 경우 HWDRM는 클라이언트에서 사용 하도록 보장 되지 않습니다** .
    -   비디오 콘텐츠 키에 사용 되는 라이선스의 보안 수준 속성은 3000 이상 이어야 합니다.
    -   오디오는 비디오와 다른 콘텐츠 키로 암호화 되어야 하 고 오디오에 사용 되는 라이선스의 보안 수준 속성은 2000 이상 이어야 합니다. 또는 투명 하 게 오디오를 남겨둘 수 있습니다.
    
또한 HWDRM를 사용 하는 경우 다음 항목을 고려해 야 합니다.

-   보호 된 미디어 프로세스 (PMP)는 지원 되지 않습니다.
-   Windows Media 비디오 (VC-1이 라고도 함)은 지원 되지 않습니다 ( [하드웨어 DRM 재정의](#override-hardware-drm)참조).
-   여러 Gpu (그래픽 처리 장치)는 영구 라이선스에 대해 지원 되지 않습니다.

여러 Gpu를 사용 하는 컴퓨터에서 영구 라이선스를 처리 하려면 다음 시나리오를 고려 하세요.

1.  고객은 통합 그래픽 카드로 새 컴퓨터를 구입 합니다.
2.  고객은 하드웨어 DRM을 사용 하는 동안 영구 라이선스를 획득 하는 앱을 사용 합니다.
3.  이제 영구 라이선스가 해당 그래픽 카드의 하드웨어 키에 바인딩되어 있습니다.
4.  그런 다음 고객이 새 그래픽 카드를 설치 합니다.
5.  HDS (해시 된 데이터 저장소)의 모든 라이선스는 통합 된 비디오 카드에 바인딩되므로 이제 고객이 새로 설치한 그래픽 카드를 사용 하 여 보호 된 콘텐츠를 재생 하려고 합니다.

하드웨어에서 라이선스 암호를 해독할 수 없으므로 재생이 실패 하지 않도록 하기 위해 PlayReady는 발견 되는 각 그래픽 카드에 대해 별도의 HDS를 사용 합니다. 그러면 playready가 일반적으로 라이선스를 보유 하 고 있는 콘텐츠 (즉, 소프트웨어 DRM 사례 또는 하드웨어 변경 없이 모든 경우)에 대 한 라이선스 취득을 시도 합니다. PlayReady에서 라이선스를 다시 가져올 필요가 없습니다. 따라서 앱이 하드웨어 DRM을 사용 하는 동안 영구 라이선스를 획득 하는 경우 앱은 최종 사용자가 그래픽 카드를 설치 (또는 제거) 하는 경우 해당 라이선스를 효과적으로 "분실" 하는 경우를 처리할 수 있어야 합니다. 일반적인 시나리오는 아니지만 클라이언트/서버 코드에서 하드웨어 변경을 처리 하는 방법을 확인 하는 것이 아니라 하드웨어 변경 후 콘텐츠가 더 이상 재생 되지 않는 경우 지원 요청을 처리 하도록 결정할 수 있습니다.

## <a name="override-hardware-drm"></a>하드웨어 DRM 재정의

이 섹션에서는 재생할 콘텐츠가 하드웨어 DRM을 지원 하지 않는 경우 하드웨어 DRM (HWDRM)을 재정의 하는 방법을 설명 합니다.

기본적으로 시스템에서 지 원하는 경우 하드웨어 DRM이 사용 됩니다. 그러나 일부 콘텐츠는 하드웨어 DRM에서 지원 되지 않습니다. 이에 대 한 한 가지 예는 칵테일 콘텐츠입니다. 또 다른 예는 h.264 및 HEVC 이외의 비디오 코덱을 사용 하는 콘텐츠입니다. 또 다른 예는 일부 하드웨어 DRM이 HEVC을 지원 하기 때문에 HEVC 콘텐츠입니다. 따라서 콘텐츠를 재생 하려고 하는데 하드웨어 DRM이 해당 시스템에서이를 지원 하지 않는 경우 하드웨어 DRM을 옵트아웃 (opt out) 할 수 있습니다.

다음 예제에서는 하드웨어 DRM 옵트아웃 (opt out) 하는 방법을 보여 줍니다. 전환 하기 전에이 작업을 수행 해야 합니다. 또한 메모리에 PlayReady 개체가 없는지 확인 합니다. 그렇지 않으면 동작이 정의 되지 않습니다.

```js
var applicationData = Windows.Storage.ApplicationData.current;
var localSettings = applicationData.localSettings.createContainer("PlayReady", Windows.Storage.ApplicationDataCreateDisposition.always);
localSettings.values["SoftwareOverride"] = 1;
```

하드웨어 DRM으로 다시 전환 하려면 **적용 재정의** 값을 **0**으로 설정 합니다.

모든 미디어 재생에 대해 **MediaProtectionManager** 를 다음과 같이 설정 해야 합니다.

```js
mediaProtectionManager.properties["Windows.Media.Protection.UseSoftwareProtectionLayer"] = true;
```

하드웨어 DRM 또는 소프트웨어 DRM에 있는지를 확인 하는 가장 좋은 방법은 C: \\ Users \\ &lt; username &gt; \\ AppData \\ 로컬 \\ 패키지 \\ &lt; 응용 프로그램 이름 &gt; \\ localcache \\ PlayReady를 확인 하는 것입니다.\\\*

-   Mspr 파일이 있는 경우 소프트웨어 DRM에 있습니다.
-   다른 \* . hds 파일이 있는 경우 하드웨어 DRM에 있습니다.
-   전체 PlayReady 폴더를 삭제 하 고 테스트를 다시 시도할 수 있습니다.

## <a name="detect-the-type-of-hardware-drm"></a>하드웨어 DRM 유형 검색

이 섹션에서는 시스템에서 지원 되는 하드웨어 DRM 유형을 검색 하는 방법을 설명 합니다.

[**PlayReadyStatics**](/uwp/api/windows.media.protection.playready.playreadystatics.checksupportedhardware) 를 사용 하 여 시스템에서 특정 하드웨어 DRM 기능을 지원 하는지 여부를 확인할 수 있습니다. 예:

```csharp
bool isFeatureSupported = PlayReadyStatics.CheckSupportedHardware(PlayReadyHardwareDRMFeatures.HEVC);
```

[**PlayReadyHardwareDRMFeatures**](/uwp/api/Windows.Media.Protection.PlayReady.PlayReadyHardwareDRMFeatures) 열거형에는 쿼리할 수 있는 하드웨어 DRM 기능 값의 유효한 목록이 포함 되어 있습니다. 하드웨어 DRM이 지원 되는지 확인 하려면 쿼리에서 **HardwareDRM** 멤버를 사용 합니다. 하드웨어가 High HEVC (High Video 코딩)를 지원 하는지 확인 하려면 쿼리에서 **HEVC** 멤버를 사용 합니다.

[**PlayReadyStatics PlayReadyCertificateSecurityLevel**](/uwp/api/windows.media.protection.playready.playreadystatics.playreadycertificatesecuritylevel) 속성을 사용 하 여 클라이언트 인증서의 보안 수준을 가져와서 하드웨어 DRM이 지원 되는지 여부를 확인할 수도 있습니다. 반환 된 인증서 보안 수준이 3000 보다 크거나 같은 경우, 클라이언트가 개별화 되거나 프로 비전 되지 않은 경우 (이 속성은 0을 반환 함) 또는 하드웨어 DRM을 사용 하지 않습니다 .이 경우이 속성은 3000 보다 작은 값을 반환 합니다.

### <a name="detecting-support-for-aes128cbc-hardware-drm"></a>AES128CBC 하드웨어 DRM에 대 한 지원 검색
Windows 10, 버전 1709부터 **[PlayReadyStatics](/uwp/api/windows.media.protection.playready.playreadystatics.checksupportedhardware)** 를 호출 하 고 열거형 값 [**PlayReadyHardwareDRMFeatures**](/uwp/api/Windows.Media.Protection.PlayReady.PlayReadyHardwareDRMFeatures)를 지정 하 여 장치에서 하드웨어 암호화 AES128CBC 대 한 지원을 검색할 수 있습니다. 이전 버전의 Windows 10에서는이 값을 지정 하면 예외가 throw 됩니다. 따라서 **Checksupportedhardware**를 호출 하기 전에 **[IsApiContractPresent](/uwp/api/windows.foundation.metadata.apiinformation.isapicontractpresent)** 를 호출 하 고 주 계약 버전 5를 지정 하 여 열거형 값이 있는지 확인 해야 합니다.

```csharp
bool supportsAes128Cbc = ApiInformation.IsApiContractPresent("Windows.Foundation.UniversalApiContract", 5);

if (supportsAes128Cbc)
{
    supportsAes128Cbc = PlayReadyStatics.CheckSupportedHardware(PlayReadyHardwareDRMFeatures.Aes128Cbc);
}
```

## <a name="see-also"></a>참고 항목
- [PlayReady DRM](playready-client-sdk.md)