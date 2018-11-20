---
author: drewbatgit
ms.assetid: A7E0DA1E-535A-459E-9A35-68A4150EE9F5
description: 이 항목에서는 UWP(유니버설 Windows 플랫폼)앱에 PlayReady 하드웨어 기반 DRM(디지털 권한 관리)을 추가하는 방법의 개요를 제공합니다.
title: 하드웨어 DRM
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 6e898e342cb26ab86a74c30b5ced3dd3a9b68165
ms.sourcegitcommit: ed0304b8a214c03b8aab74b8ef12c9f82b8e3c5f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/19/2018
ms.locfileid: "7279411"
---
# <a name="hardware-drm"></a>하드웨어 DRM


이 항목에서는 UWP(유니버설 Windows 플랫폼)앱에 PlayReady 하드웨어 기반 DRM(디지털 권한 관리)을 추가하는 방법의 개요를 제공합니다.

> [!NOTE] 
> 하드웨어 기반 PlayReady DRM은 Windows 디바이스와 TV, 휴대폰, 태블릿 등의 비 Windows 디바이스를 비롯한 다양한 디바이스에서 지원됩니다. Windows 디바이스가 PlayReady 하드웨어 DRM을 지원하려면 Windows 10을 실행하고 있어야 하고 지원되는 하드웨어 구성을 포함해야 합니다.

점점 더 많은 콘텐츠 공급자가 앱에서 높은 가치의 콘텐츠를 모두 재생할 권한을 부여하기 위해 하드웨어 기반 보호로 전환하고 있습니다. 이러한 요구 사항을 충족시키기 위해 암호화 코어를 하드웨어에서 구현하는 강력한 기능이 PlayReady에 추가되었습니다. 이 지원을 통해 여러 디바이스 플랫폼에서 고해상도(1080p) 및 초고해상도(UHD) 콘텐츠를 안전하게 재생할 수 있습니다. 개인 키, 콘텐츠 키 및 이러한 키를 파생하거나 잠금 해제하는 데 사용하는 기타 키 자료를 포함하는 키 자료와 암호 해독되어 압축 및 압축 해제된 비디오 샘플은 하드웨어 보안을 활용하여 보호합니다.

## <a name="windows-tee-implementation"></a>Windows TEE 구현

이 항목에서는 Windows10 신뢰할 수 있는 실행 환경 (TEE)를 구현 하는 방법에 대 한 간략 한 개요를 제공 합니다.

Windows TEE 구현의 세부 정보는 이 문서의 범위를 벗어납니다. 그러나 표준 포팅 키트 TEE 포트와 Windows 포트 사이의 차이점을 간략하게 설명하면 도움이 될 수 있습니다. Windows에서는 OEM 프록시 계층을 구현하고 Windows Media Foundation 하위 시스템의 사용자 모드 드라이버로 직렬화된 PRITEE 함수 호출을 전송합니다. 결국 이 호출은 Windows TEE(신뢰할 수 있는 실행 환경) 드라이버 또는 OEM의 그래픽 드라이버로 라우트됩니다. 이러한 방법에 대한 세부 정보는 이 문서의 범위를 벗어납니다. 다음 다이어그램에서는 Windows 포트의 구성 요소 조작을 보여줍니다. Windows PlayReady TEE 구현을 개발하려는 경우 <WMLA@Microsoft.com>에 연락할 수 있습니다.

![Windows TEE 구성 요소 다이어그램](images/windowsteecomponentdiagram720.jpg)

## <a name="considerations-for-using-hardware-drm"></a>하드웨어 DRM 사용에 대한 고려 사항

이 항목에서는 하드웨어 DRM을 사용하도록 설계된 앱을 개발할 때 고려해야 하는 항목의 간략한 목록을 제공합니다. [PlayReady DRM](playready-client-sdk.md#output-protection)에서 설명했듯이 Windows 10용 PlayReady HWDRM을 사용하는 경우 모든 출력 보호가 Windows TEE 구현 내에서 적용되며 이에 따라 출력 보호 동작에 몇 가지 영향을 주게 됩니다.

-   **압축되지 않은 디지털 비디오 270에 대한 OPL(출력 보호 수준) 지원:** Windows 10용 PlayReady HWDRM은 다운 해상도를 지원하지 않으며 HDCP가 사용되도록 강제합니다. HWDRM의 고해상도 콘텐츠에는 270보다 큰 OPL이 있는 것이 좋습니다(필수는 아님). 또한 라이선스에서 HDCP 유형 제한을 설정하는 것이 좋습니다(Windows 10의 HDCP 버전 2.2).
-   **SWDRM(소프트웨어 DRM)과 달리 출력 보호는 최소 기능 모니터를 기반으로 모든 모니터에 적용됩니다.** 예를 들어, 한 모니터에서는 HDCP를 지원하고 다른 모니터에서는 지원하지 않는 두 개의 모니터가 연결된 경우, 라이선스에 HDCP가 필요하면 HDCP를 지원하는 모니터에서만 콘텐츠가 렌더링되는 경우에도 재생에 실패합니다. 소프트웨어 DRM에서 콘텐츠는 HDCP를 지원하는 모니터에서 렌더링되는 경우에만 재생됩니다.
-   콘텐츠 키와 라이선스가 **다음 조건을 만족하지 않으면 클라이언트에서 HWDRM을 사용하지 않거나 안전하지 않을 수 있습니다.**
    -   비디오 콘텐츠 키에 사용되는 라이선스의 최소 보안 수준 속성은 3000이어야 합니다.
    -   오디오는 비디오와 다른 콘텐츠 키로 암호화되어야 하고, 오디오에 사용되는 라이선스의 최소 보안 수준 속성은 2000이어야 합니다. 또는 오디오를 암호화하지 않은 상태로 둘 수 있습니다.
    
또한 HWDRM을 사용할 때 다음 항목을 고려해야 합니다.

-   PMP(Protected Media Process)는 지원되지 않습니다.
-   Windows Media 비디오(VC-1이라고도 함)는 지원되지 않습니다([하드웨어 DRM 재정의](#override-hardware-drm) 참조).
-   여러 GPU(그래픽 처리 단위)는 영구 라이선스에 지원되지 않습니다.

여러 GPU가 있는 컴퓨터에서 영구 라이선스를 처리하려면 다음 시나리오를 가정해 보세요.

1.  고객이 통합 그래픽 카드가 있는 새 컴퓨터를 구입합니다.
2.  고객이 하드웨어 DRM을 사용하는 동안 영구 라이선스를 취득하는 앱을 사용합니다.
3.  이제 영구 라이선스는 그래픽 카드의 하드웨어 키에 바인딩됩니다.
4.  고객이 새 그래픽 카드를 설치합니다.
5.  HDS(해시된 데이터 저장소)의 모든 라이선스가 통합 비디오 카드에 바인딩되었지만, 이제 고객이 새로 설치된 그래픽 카드를 사용하여 보호된 콘텐츠를 재생하려고 합니다.

하드웨어에서 라이선스의 암호를 해독할 수 없어 재생에 실패하는 경우를 방지하기 위해 PlayReady에서는 탐지된 각 그래픽 카드에 개별 HDS를 사용합니다. 그러므로 PlayReady에서는 일반적으로 이미 라이선스를 보유하고 있는 콘텐츠에 대한 라이선스를 취득하려고 시도합니다(즉, 소프트웨어 DRM의 경우나 하드웨어가 변경되지 않은 경우, PlayReady에서는 라이선스를 다시 취득할 필요가 없음). 따라서 하드웨어 DRM을 사용하는 동안 영구 라이선스를 취득하는 앱에서는 최종 사용자가 그래픽 카드를 설치(또는 제거)하면 라이선스가 실제로 “유실”되는 경우를 처리해야 합니다. 이 시나리오는 일반적이지 않으므로 하드웨어가 변경된 후 콘텐츠가 더 이상 재생되지 경우, 클라이언트/서버 코드에서 하드웨어 변경 처리 방법을 파악하는 것이 아니라 지원 요청을 처리하도록 결정할 수 있습니다.

## <a name="override-hardware-drm"></a>하드웨어 DRM 재정의

이 섹션에서는 재생할 콘텐츠에서 HWDRM(하드웨어 DRM)을 지원하지 않는 경우 하드웨어 DRM을 재정의하는 방법을 설명합니다.

기본적으로 시스템에서 지원하는 경우 하드웨어 DRM을 사용합니다. 그러나 일부 콘텐츠는 하드웨어 DRM에서 지원되지 않습니다. 그중 한 예로 Cocktail 콘텐츠가 있습니다. 또 다른 예로는 H.264와 HEVC가 아닌 비디오 코덱을 사용하는 콘텐츠가 있습니다. 일부 하드웨어 DRM은 HEVC를 지원하고 일부는 지원하지 않으므로 HEVC 콘텐츠도 한 예가 될 수 있습니다. 따라서 재생하려는 콘텐츠를 하드웨어 DRM이 해당 시스템에서 지원하지 않는 경우 하드웨어 DRM을 옵트아웃(opt out)할 수 있습니다.

다음 예제에서는 하드웨어 DRM의 선택을 취소하는 방법을 보여줍니다. 전환하기 전에 이 작업을 수행해야 합니다. 또한 메모리에 PlayReady 개체가 없어야 합니다. 그러지 않으면 동작이 정의되지 않습니다.

```js
var applicationData = Windows.Storage.ApplicationData.current;
var localSettings = applicationData.localSettings.createContainer("PlayReady", Windows.Storage.ApplicationDataCreateDisposition.always);
localSettings.values["SoftwareOverride"] = 1;
```

하드웨어 DRM으로 전환하려면 **SoftwareOverride** 값을 **0**으로 설정합니다.

모든 미디어 재생을 위해 **MediaProtectionManager**를 다음으로 설정해야 합니다.

```js
mediaProtectionManager.properties["Windows.Media.Protection.UseSoftwareProtectionLayer"] = true;
```

하드웨어 DRM 사용 중인 소프트웨어 DRM이 C:\\Users\\ 보면 확인할 가장 좋은 방법은&lt;사용자 이름&gt;\\AppData\\Local\\Packages\\&lt;응용 프로그램 이름&gt;\\LocalCache\\PlayReady\\\*

-   mspr.hds 파일이 있으면 소프트웨어 DRM이 적용되는 것입니다.
-   또 다른 \*.hds 파일이 있으면 하드웨어 DRM이 적용되는 것입니다.
-   전체 PlayReady 폴더를 삭제하고 테스트도 다시 시도할 수 있습니다.

## <a name="detect-the-type-of-hardware-drm"></a>하드웨어 DRM의 유형 감지

이 섹션에서는 시스템에서 지원되는 하드웨어 DRM의 유형을 감지하는 방법을 설명합니다.

[**PlayReadyStatics.CheckSupportedHardware**](https://msdn.microsoft.com/library/windows/apps/dn986441) 메서드를 사용하여 시스템에서 특정 하드웨어 DRM 기능을 지원하는지 확인할 수 있습니다. 예:

```csharp
bool isFeatureSupported = PlayReadyStatics.CheckSupportedHardware(PlayReadyHardwareDRMFeatures.HEVC);
```

[**PlayReadyHardwareDRMFeatures**](https://msdn.microsoft.com/library/windows/apps/dn986265) 열거에는 쿼리할 수 있는 하드웨어 DRM 기능 값의 올바른 목록이 포함되어 있습니다. 하드웨어 DRM이 지원되는지 확인하려면 쿼리에서 **HardwareDRM** 멤버를 사용합니다. 하드웨어에서 HEVC(고효율성 비디오 코딩)/H.265 코덱을 지원하는지 확인하려면 쿼리에서 **HEVC** 멤버를 사용합니다.

하드웨어 DRM이 지원되는지 확인하기 위해 클라이언트 인증서의 보안 수준을 얻으려면 [**PlayReadyStatics.PlayReadyCertificateSecurityLevel**](https://msdn.microsoft.com/library/windows/apps/windows.media.protection.playready.playreadystatics.playreadycertificatesecuritylevel.aspx) 속성도 사용할 수 있습니다. 반환된 인증서 보안 수준이 3000 이상인 경우 클라이언트가 개별화 또는 프로비전되지 않았거나(두 경우 모두 이 속성은 0을 반환) 하드웨어 DRM이 사용 중이 아닙니다(이 경우 이 속성은 3000 미만인 값을 반환).

### <a name="detecting-support-for-aes128cbc-hardware-drm"></a>AES128CBC 하드웨어 DRM에 대한 지원 검색
Windows 10, 버전 1709부터 디바이스에서 AES128CBC 하드웨어 암호화에 대한 지원을 검색하려면 **[PlayReadyStatics.CheckSupportedHardware](https://msdn.microsoft.com/library/windows/apps/dn986441)** 를 호출하고 열거 값 [**PlayReadyHardwareDRMFeatures.Aes128Cbc**](https://msdn.microsoft.com/library/windows/apps/dn986265)를 지정합니다. 이전 버전의 Windows 10에서 이 값을 지정하면 예외가 발생합니다. 이러한 이유로 열거 값이 있는지 확인하려면 **[ApiInformation.IsApiContractPresent](https://docs.microsoft.com/uwp/api/windows.foundation.metadata.apiinformation.isapicontractpresent)** 를 호출하고 주요 계약 버전 5를 지정한 후 **CheckSupportedHardware**를 호출합니다.

```csharp
bool supportsAes128Cbc = ApiInformation.IsApiContractPresent("Windows.Foundation.UniversalApiContract", 5);

if (supportsAes128Cbc)
{
    supportsAes128Cbc = PlayReadyStatics.CheckSupportedHardware(PlayReadyHardwareDRMFeatures.Aes128Cbc);
}
```

## <a name="see-also"></a>참고 항목
- [PlayReady DRM](playready-client-sdk.md)
