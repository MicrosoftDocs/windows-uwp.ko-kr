---
author: drewbatgit
ms.assetid: DD8FFA8C-DFF0-41E3-8F7A-345C5A248FC2
description: 이 항목에서는 UWP(유니버설 Windows 플랫폼) 앱에 PlayReady 보호된 미디어 콘텐츠를 추가하는 방법을 설명합니다.
title: PlayReady DRM
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 773216dc392f7bb234e232f3dd3e7c2190a22de1
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/31/2018
ms.locfileid: "5871755"
---
# <a name="playready-drm"></a>PlayReady DRM



이 항목에서는 UWP(유니버설 Windows 플랫폼) 앱에 PlayReady 보호된 미디어 콘텐츠를 추가하는 방법을 설명합니다.

PlayReady DRM 미디어 요소를 사용하면 개발자가 UWP 앱을 만들어, 콘텐츠 공급자가 정의한 액세스 규칙을 적용하는 한편 사용자에게 PlayReady 콘텐츠를 제공할 수 있습니다. 이 섹션에서는 Microsoft PlayReady DRM Windows10 및 변경 내용을 Windows8.1 버전과에서 Windows10 버전을 지원 하도록 PlayReady UWP 앱을 수정 하는 방법에 대 한 변경 설명 합니다.
 
| 항목                                                                     | 설명                                                                                                                                                                                                                                                                             |
|---------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [하드웨어 DRM](hardware-drm.md)                                           | 이 항목에서는 UWP 앱에 PlayReady 하드웨어 기반 DRM(디지털 권한 관리)을 추가하는 방법의 개요를 제공합니다.                                                                                                                                                                 |
| [PlayReady를 사용한 적응 스트리밍](adaptive-streaming-with-playready.md) | 이 문서에서는 Microsoft PlayReady 콘텐츠 보호와 함께 멀티미디어 콘텐츠의 적응 스트리밍을 UWP(유니버설 Windows 플랫폼) 앱에 추가하는 방법을 설명합니다. 이 기능은 현재 HLS(Http 라이브 스트리밍) 및 DASH(Dynamic Streaming over HTTP) 콘텐츠를 지원합니다. |

## <a name="whats-new-in-playready-drm"></a>PlayReady DRM의 새로운 기능

다음은 새로운 기능과 변경 내용이 Windows10 용 PlayReady DRM을 설명 합니다.

-   HWDRM(하드웨어 디지털 권한 관리)이 추가되었습니다.

    하드웨어 기반 콘텐츠 보호 지원을 통해 여러 장치 플랫폼에서 고해상도(HD) 및 초고해상도(UHD) 콘텐츠를 안전하게 재생할 수 있습니다. 개인 키, 콘텐츠 키 및 이러한 키를 파생하거나 잠금 해제하는 데 사용하는 기타 키 자료를 포함하는 키 자료와 암호 해독되어 압축 및 압축 해제된 비디오 샘플은 하드웨어 보안을 활용하여 보호합니다. 하드웨어 DRM을 사용 중인 경우, HWDRM 파이프라인은 사용 중인 출력을 언제나 알고 있으므로 이 알 수 없음 인에이블러(알 수 없음으로 재생/downres와 함께 알 수 없음으로 실행)는 의미가 없습니다. 자세한 내용은 [하드웨어 DRM](hardware-drm.md)을 참조하세요.

-   PlayReady는 이제 Windows 제공 운영 체제 구성 요소이며 더 이상 appX 프레임워크 구성 요소가 아닙니다. 네임스페이스가 **Microsoft.Media.PlayReadyClient**에서 **[Windows.Media.Protection.PlayReady](https://msdn.microsoft.com/library/windows/apps/dn986454)** 로 변경되었습니다.
-   PlayReady 오류 코드를 정의하는 Windows.Media.Protection.PlayReadyErrors.h 및 Windows.Media.Protection.PlayReadyResults.h 헤더는 이제 Windows SDK(소프트웨어 개발 키트)의 일부입니다.
-   비영구적 라이선스를 미리 취득할 수 있습니다.

    PlayReady DRM의 이전 버전에서는 비영구적 라이선스를 미리 취득할 수 없습니다. 이 버전에는 이 기능이 추가되었습니다. 따라서 첫 번째 프레임의 시간이 단축될 수 있습니다. 자세한 내용은 [재생하기 전에 미리 비영구 라이선스 취득](#proactively-acquire-a-non-persistent-license-before-playback)을 참조하세요.

-   한 메시지로 여러 라이선스를 취득할 수 있습니다.

    클라이언트 앱에서 하나의 라이선스 취득 메시지를 통해 여러 비영구 라이선스를 취득할 수 있습니다. 사용자가 여전히 콘텐츠 라이브러리를 검색하는 동안 여러 콘텐츠의 라이선스를 취득하므로 첫 번째 프레임의 시간을 단축시킬 수 있습니다. 따라서 사용자가 재생할 콘텐츠를 선택할 때 라이선스 취득을 위해 지연되지 않습니다. 또한 여러 키 ID(KID)를 포함하는 콘텐츠 헤더를 사용하여 오디오 및 비디오 스트림을 개별 키로 암호화할 수 있습니다. 따라서 이와 동일한 결과를 달성하기 위해 사용자 지정 논리 및 여러 라이선스 취득 요청을 사용할 필요 없이 단일 라이선스 취득을 통해 콘텐츠 파일에서 모든 스트림의 모든 라이선스를 취득할 수 있습니다.

-   실시간 만료 지원 또는 LDL(제한된 기간 라이선스)이 추가되었습니다.

    라이선스에 실시간 만료를 설정하고 재생 중에 만료되는 라이선스에서 다른 (유효한) 라이선스로 원활하게 전환하는 기능을 제공합니다. 이 기능이 한 메시지의 여러 라이선스 취득 기능과 결합되면 사용자가 콘텐츠 라이브러리를 검색하는 동안 앱에서 비동기적으로 여러 LDL을 취득할 수 있으며 사용자가 재생할 콘텐츠를 선택한 후에만 더 긴 기간의 라이선스를 취득할 수 있습니다. 그러면 재생이 더 빨리 시작되며(라이선스가 이미 사용 가능하기 때문) LDL이 만료될 때 앱에서 더 긴 기간의 라이선스를 취득하게 되므로 콘텐츠의 끝까지 중단 없이 재생이 원활하게 계속됩니다.

-   비영구 라이선스 체인이 추가되었습니다.
-   비영구적 라이선스에서 시간 기반 제한 사항 지원이 추가되었습니다(만료, 첫 번째 재생 후 만료 및 실시간 만료 등).
-   HDCP 유형 1(Windows 10의 버전 2.2) 정책 지원이 추가되었습니다.

    자세한 내용은 [고려 사항](#things-to-consider)을 참조하세요.

-   이제 출력으로서의 Miracast는 암시적입니다.
-   보안 중지가 추가되었습니다.

    보안 중지를 통해 PlayReady 디바이스는 지정된 콘텐츠에 대해 미디어 재생이 중지된 미디어 스트리밍 서비스로 안정적으로 어설션됩니다. 이 기능은 미디어 스트리밍 서비스가 지정된 계정에 대해 다양한 장치의 사용 제한을 정확하게 적용하고 보고하도록 합니다.

-   오디오 및 동영상 라이선스 분리가 추가되었습니다.

    별도 트랙을 통해 동영상이 오디오로 디코드되지 않아 콘텐츠가 보다 강력하게 보호됩니다. 새 표준에서는 오디오 및 동영상 트랙에 별도의 키가 필요합니다.

-   MaxResDecode가 추가되었습니다.

    이 기능은 강력한 기능 키(라이선스는 아님)를 소유한 경우에도 콘텐츠를 최대 해상도로 재생하는 것을 제한하기 위해 추가되었습니다. 이 기능은 여러 스트림 크기가 단일 키로 인코드된 경우를 지원합니다.

다음과 같은 새 인터페이스, 클래스 및 열거가 PlayReady DRM에 추가되었습니다.

-   [**IPlayReadyLicenseAcquisitionServiceRequest**](https://msdn.microsoft.com/library/windows/apps/dn986077) 인터페이스
-   [**IPlayReadyLicenseSession**](https://msdn.microsoft.com/library/windows/apps/dn986080) 인터페이스
-   [**IPlayReadySecureStopServiceRequest**](https://msdn.microsoft.com/library/windows/apps/dn986090) 인터페이스
-   [**PlayReadyLicenseSession**](https://msdn.microsoft.com/library/windows/apps/dn986309) 클래스
-   [**PlayReadySecureStopIterable**](https://msdn.microsoft.com/library/windows/apps/dn986371) 클래스
-   [**PlayReadySecureStopIterator**](https://msdn.microsoft.com/library/windows/apps/dn986375) 클래스
-   [**PlayReadyHardwareDRMFeatures**](https://msdn.microsoft.com/library/windows/apps/dn986265) 열거자

PlayReady DRM의 새 기능을 사용하는 방법을 설명하기 위해 새 샘플이 생성되었습니다. 샘플은 [http://go.microsoft.com/fwlink/p/?linkid=331670&clcid=0x409](http://go.microsoft.com/fwlink/p/?linkid=331670)에서 다운로드할 수 있습니다.

## <a name="things-to-consider"></a>고려 사항

-   이제 PlayReady DRM에서 HDCP 유형 1(HDCP 버전 2.1 이상에서 지원됨)을 지원합니다. PlayReady는 장치에서 적용할 HDCP 유형 제한 정책을 라이선스에서 제공합니다. Windows 10에서 이 정책은 HDCP 2.2 이상이 사용되도록 강제합니다. 이 기능은 PlayReady Server v3.0 SDK 라이선스에서 사용하도록 설정할 수 있습니다(서버에서 HDCP 유형 제한 GUID를 사용하여 라이선스에서 이 정책을 제어함). 자세한 내용은 [PlayReady 규정 준수 및 견고성 규칙](http://www.microsoft.com/playready/licensing/compliance/)을 참조하세요.
-   Windows Media 비디오(VC-1이라고도 함)는 하드웨어 DRM에서는 지원되지 않습니다([하드웨어 DRM 재정의](hardware-drm.md#override-hardware-drm) 참조).
-   PlayReady DRM에서는 이제 고효율성 비디오 코딩(HEVC /H.265) 비디오 압축 표준을 지원합니다. HEVC를 지원하려면 앱에서 콘텐츠의 슬라이스 헤더가 지워진 상태로 포함되어 있는 CENC(Common Encryption Scheme) 버전 2 콘텐츠를 사용해야 합니다. 자세한 내용은 ISO/IEC 23001-7 정보 기술 - MPEG 시스템 기술 - 파트 7: ISO 기본 미디어 파일 형식 파일의 일반적인 암호화 (사양 버전 ISO/IEC 23001-7:2015 이상 필요)를 참조하세요. 모든 HWDRM 콘텐츠에 CENC 버전 2를 사용하는 것이 좋습니다. 또한 일부 하드웨어 DRM에서는 HEVC를 지원하고 일부에서는 지원하지 않습니다([하드웨어 DRM 재정의](hardware-drm.md#override-hardware-drm) 참조).
-   새로운 PlayReady 3.0 기능(하드웨어 기반 클라이언트의 SL3000, 하나의 라이선스 취득 메시지로 여러 비영구적 라이선스 취득 및 비영구적 라이선스에 대한 시간 기반 제한을 포함하지만 이에 한하지는 않음)을 활용하려면 PlayReady 서버가 Microsoft PlayReady Server 소프트웨어 개발 키트 v3.0.2769 릴리스 버전 이상에 있어야 합니다.
-   콘텐츠 라이선스에 지정된 출력 보호 정책에 따라 연결된 출력에서 해당 요구 사항을 지원하지 않는 경우 최종 사용자가 미디어를 재생하지 못할 수 있습니다. 다음 테이블에는 결과적으로 발생하는 일반적인 오류 집합이 나열되어 있습니다. 자세한 내용은 [PlayReady 규정 준수 및 견고성 규칙](http://www.microsoft.com/playready/licensing/compliance/)을 참조하세요.

| 오류                                                   | 값      | 설명                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|---------------------------------------------------------|------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ERROR\_GRAPHICS\_OPM\_OUTPUT\_DOES\_NOT\_SUPPORT\_HDCP  | 0xC0262513 | 라이선스의 출력 보호 정책에서는 모니터가 HDCP를 사용해야 하지만 HDCP를 사용할 수 없습니다.                                                                                                                                                                                                                                                                                                                                                                                              |
| MF\_E\_POLICY\_UNSUPPORTED                              | 0xC00D7159 | 라이선스의 출력 보호 정책에서는 모니터가 HDCP 유형 1을 사용해야 하지만 HDCP 유형 1을 사용할 수 없습니다.                                                                                                                                                                                                                                                                                                                                                                                |
| DRM\_E\_TEE\_OUTPUT\_PROTECTION\_REQUIREMENTS\_NOT\_MET | 0x8004CD22 | 이 오류 코드는 하드웨어 DRM에서 실행할 때만 발생합니다. 라이선스의 출력 보호 정책에서는 모니터가 HDCP를 사용하거나 콘텐츠의 유효 해상도를 줄여야 하지만, HDCP를 사용할 수 없거나 하드웨어 DRM에서 콘텐츠의 해상도 줄이기가 지원되지 않으므로 콘텐츠의 유효 해상도를 줄일 수 없습니다. 소프트웨어 DRM에서 콘텐츠를 재생합니다. [하드웨어 DRM 사용에 대한 앱 고려 사항](hardware-drm.md#considerations-for-using-hardware-drm)을 참조하세요. |
| ERROR\_GRAPHICS\_OPM\_NOT\_SUPPORTED                    | 0xc0262500 | 그래픽 드라이버에서 출력 보호를 지원하지 않습니다. 예를 들어, 모니터가 VGA로 연결되어 있거나 디지털 출력에 적합한 그래픽 드라이버가 설치되지 않았습니다. 후자의 경우 일반적으로 설치되는 드라이버는 Microsoft 기본 디스플레이 어댑터이며 적절한 그래픽 드라이버를 설치하면 문제가 해결됩니다.                                                                                                                                                  |

## <a name="output-protection"></a>출력 보호

다음 섹션에서는 Windows 10용 PlayReady DRM을 PlayReady 라이선스의 출력 보호 정책과 함께 사용하는 경우의 동작에 대해 설명합니다.

PlayReady DRM은 **Microsoft PlayReady 확장 가능 미디어 권한 사양**에 포함된 출력 보호 수준을 지원합니다. 이 문서는 PlayReady 사용이 허가된 제품과 함께 제공되는 설명서 팩에서 찾을 수 있습니다.

> [!NOTE]
> 라이선스 서버에 의해 설정될 수 있는 출력 보호 수준의 허용되는 값에는 [PlayReady 규정 준수 규칙](https://www.microsoft.com/playready/licensing/compliance/)이 적용됩니다.

PlayReady DRM은 PlayReady 규정 준수 규칙에 지정된 대로 출력 커넥터에서 출력 보호 정책을 사용한 콘텐츠의 재생만을 허용합니다. PlayReady 준수 규칙에 지정된 출력 커넥터 조건에 대한 자세한 내용은 [PlayReady 규정 준수 및 견고성 규칙에 대해 정의된 조건](https://www.microsoft.com/playready/licensing/compliance/)을 참조하세요.

이 섹션에서는 일부 Windows 클라이언트에서도 사용할 수 있는 Windows 10용 PlayReady 하드웨어 DRM 및 Windows 10용 PlayReady DRM을 사용하는 출력 보호 시나리오를 중점적으로 살펴봅니다. PlayReady HWDRM을 사용하는 경우 모든 출력 보호가 Windows TEE 구현 내에서 적용됩니다([하드웨어 DRM](hardware-drm.md) 참조). 따라서 일부 동작이 PlayReady SWDRM(소프트웨어 DRM)을 사용하는 경우와 다릅니다.

* 압축되지 않은 디지털 비디오 270에 대한 OPL(출력 보호 수준) 지원: Windows 10용 PlayReady HWDRM은 다운 해상도를 지원하지 않으며 HDCP(높은 대역폭 디지털 콘텐츠 보호)가 사용되도록 강제합니다. HWDRM의 고해상도 콘텐츠에는 270보다 큰 OPL이 있는 것이 좋습니다(필수는 아님). 또한 라이선스에서 HDCP 유형 제한을 설정해야 합니다(HDCP 버전 2.2 이상).
* SWDRM과 달리 HWDRM에서는 출력 보호가 최소 기능 모니터를 기반으로 모든 모니터에 적용됩니다. 예를 들어 한 모니터에서는 HDCP를 지원하고 다른 모니터에서는 지원하지 않는 두 개의 모니터가 연결된 경우, 라이선스에 HDCP가 필요하면 HDCP를 지원하는 모니터에서만 콘텐츠가 렌더링되는 경우에도 재생에 실패합니다. SWDRM에서 콘텐츠는 HDCP를 지원하는 모니터에서 렌더링되는 경우에만 재생됩니다.
* 콘텐츠 키와 라이선스가 다음 조건을 만족하지 않으면 클라이언트에서 HWDRM을 사용하지 않거나 안전하지 않을 수 있습니다.
    * 비디오 콘텐츠 키에 사용되는 라이선스의 최소 보안 수준은 3000이어야 합니다.
    * 오디오는 비디오와 다른 콘텐츠 키로 암호화되어야 하고, 오디오에 사용되는 라이선스의 최소 보안 수준은 2000이어야 합니다. 또는 오디오를 암호화하지 않은 상태로 둘 수 있습니다.
* 모든 SWDRM 시나리오에서는 오디오 및/또는 비디오 콘텐츠 키에 사용되는 PlayReady 라이선스의 최소 보안 수준이 2000보다 작거나 같아야 합니다.

### <a name="output-protection-levels"></a>출력 보호 수준

다음 표에서는 PlayReady 라이선스의 다양한 OPL 간 매핑과 Windows 10용 PlayReady DRM이 OPL을 적용하는 방식에 대해 간략하게 설명합니다.

#### <a name="video"></a>비디오

<table>
    <tr>
        <th rowspan="2">OPL</th>
        <th>압축된 디지털 비디오</th>
        <th colspan="2">압축되지 않은 디지털 비디오</th>
        <th>아날로그 TV</th>
    </tr>
    <tr>
        <th>임의</th>
        <th colspan="2">HDMI, DVI, DisplayPort, MHL</th>
        <th>컴포넌트, 컴포지트</th>
    </tr>
    <tr>
        <th>100</th>
        <td rowspan="6">해당 없음\*</td>
        <td colspan="2">콘텐츠를 전달합니다.</td>
        <td>콘텐츠를 전달합니다.</td>
    </tr>
    <tr>
        <th>150</th>
        <td colspan="2" rowspan="2">해당 없음\*</td>
        <td>CGMS-A CopyNever가 사용되거나 CGMS-A가 사용될 수 없는 경우 콘텐츠를 전달합니다.</td>
    </tr>
    <tr>
        <th>200</th>
        <td>CGMS-A CopyNever가 사용되는 경우 콘텐츠를 전달합니다.</td>
    </tr>
    <tr>
        <th>250</th>
        <td colspan="2">HDCP를 사용하려고 하지만 결과에 관계없이 콘텐츠를 전달합니다.</td>
        <td rowspan="5">해당 없음\*</td>
    </tr>
    <tr>
        <th>270</th>
        <td><b>SWDRM</b>: HDCP를 사용하려고 합니다. HDCP를 사용하지 못하는 경우 PC는 유효 해상도를 프레임당 520,000픽셀로 제한하고 콘텐츠 전달</td>
        <td><b>HWDRM</b>: HDCP를 사용하여 콘텐츠를 전달합니다. HDCP를 사용하지 못하는 경우 HDMI/DVI 포트에 대한 재생이 차단됩니다.</td>
    </tr>
    <tr>
        <th>300</th>
        <td colspan="2">
            <p>
                **HDCP 유형 제한이 정의되지 않은 경우**: HDCP를 사용하여 콘텐츠를 전달합니다. HDCP를 사용하지 못하는 경우 HDMI/DVI 포트에 대한 재생이 차단됩니다.
            </p>
            <p>
                **HDCP 유형 제한이 정의된 경우**: HDCP 2.2를 사용하고 콘텐츠 스트림 형식을 1로 설정하여 콘텐츠를 전달합니다. HDCP를 사용하지 못하거나 콘텐츠 스트림 형식을 1로 설정할 수 없는 경우 HDMI/DVI 포트에 대한 재생이 차단됩니다.
            </p>
        </td>
    </tr>
    <tr>
        <th>400</th>
        <td rowspan="2">Windows 10에서는후속 OPL 값에 관계없이 압축된 디지털 비디오 콘텐츠를 출력으로 전달하지 않습니다. 압축된 디지털 비디오 콘텐츠에 대한 자세한 내용은 <a href="https://www.microsoft.com/playready/licensing/compliance/">PlayReady 제품에 대한 규정 준수 규칙</a>을 참조하세요.</td>
        <td colspan="2" rowspan="2">해당 없음\*</td>
    </tr>
    <tr>
        <th>500</th>
    </tr>
</table>
<br/>

\* 출력 보호 수준에 대한 모든 값을 라이선스 서버에서 설정할 수 있는 것은 아닙니다. 자세한 내용은 [PlayReady 규정 준수 규칙](https://www.microsoft.com/playready/licensing/compliance/)을 참조하세요.

#### <a name="audio"></a>오디오

<table>
    <tr>
        <th rowspan="2">OPL</th>
        <th>압축된 디지털 오디오</th>
        <th>압축되지 않은 디지털 오디오</th>
        <th>아날로그 또는 USB 오디오</th>
    </tr>
    <tr>
        <th>HDMI, DisplayPort, MHL</th>
        <th>HDMI, DisplayPort, MHL</th>
        <th>임의</th>
    </tr>
    <tr>
        <th>100</th>
        <td rowspan="3">콘텐츠를 전달합니다.</td>
        <td>콘텐츠를 전달합니다.</td>
        <td rowspan="5">콘텐츠를 전달합니다.</td>
    </tr>
    <tr>
        <th>150</th>
        <td rowspan="4">콘텐츠를 전달하지 않습니다.</td>
    </tr>
    <tr>
        <th>200</th>
    </tr>
    <tr>
        <th>250</th>
        <td>HDCP가 HDMI, DisplayPort 또는 MHL에서 사용되거나 SCMS가 사용되고 CopyNever로 설정된 경우 콘텐츠를 전달합니다.</td>
    </tr>
    <tr>
        <th>300</th>
        <td>HDCP가 HDMI, DisplayPort 또는 MHL에서 사용되는 경우 콘텐츠를 전달합니다.</td>
    </tr>
</table>
<br/>

### <a name="miracast"></a>Miracast

PlayReady DRM은 HDCP 2.0 이상이 사용되는 즉시 Miracast 출력을 통해 콘텐츠를 재생할 수 있도록 허용합니다. 그러나 Windows 10에서 Miracast는 *디지털* 출력으로 간주됩니다. Miracast 시나리오에 대한 자세한 내용은 [PlayReady 규정 준수 규칙](https://www.microsoft.com/playready/licensing/compliance/)을 참조하세요. 다음 표에서는 PlayReady 라이선스의 다양한 OPL 간 매핑과 PlayReady DRM이 Miracast 출력에서 OPL을 적용하는 방식에 대해 간략하게 설명합니다.

<table>
    <tr>
        <th>OPL</th>
        <th>압축된 디지털 오디오</th>
        <th>압축되지 않은 디지털 오디오</th>
        <th>압축된 디지털 비디오</th>
        <th>압축되지 않은 디지털 비디오</th>
    </tr>
    <tr>
        <th>100</th>
        <td rowspan="4">HDCP 2.0 이상이 사용되는 경우 콘텐츠를 전달합니다. 사용되지 못하는 경우에는 콘텐츠를 전달하지 않습니다.</td>
        <td>HDCP 2.0 이상이 사용되는 경우 콘텐츠를 전달합니다. 사용되지 못하는 경우에는 콘텐츠를 전달하지 않습니다.</td>
        <td rowspan="6">해당 없음\*</td>
        <td>HDCP 2.0 이상이 사용되는 경우 콘텐츠를 전달합니다. 사용되지 못하는 경우에는 콘텐츠를 전달하지 않습니다.</td>
    </tr>
    <tr>
        <th>150</th>
        <td rowspan="3">콘텐츠를 전달하지 않습니다.</td>
        <td rowspan="2">해당 없음\*</td>
    </tr>
    <tr>
        <th>200</th>
    </tr>
    <tr>
        <th>250</th>
        <td rowspan="2">HDCP 2.0 이상이 사용되는 경우 콘텐츠를 전달합니다. 사용되지 못하는 경우에는 콘텐츠를 전달하지 않습니다.</td>
    </tr>
    <tr>
        <th>270</th>
        <td colspan="2">해당 없음\*</td>
    </tr>
    <tr>
        <th>300</th>
        <td>HDCP 2.0 이상이 사용되는 경우 콘텐츠를 전달합니다. 사용되지 못하는 경우에는 콘텐츠를 전달하지 않습니다.</td>
        <td>콘텐츠를 전달하지 않습니다.</td>
        <td>
            <p>
                **HDCP 유형 제한이 정의되지 않은 경우**: HDCP 2.0 이상이 사용되는 경우 콘텐츠를 전달합니다. 사용되지 못하는 경우에는 콘텐츠를 전달하지 않습니다.
            </p>
            <p>
                **HDCP 유형 제한이 정의된 경우**: HDCP 2.2를 사용하고 콘텐츠 스트림 형식을 1로 설정하여 콘텐츠를 전달합니다. HDCP를 사용하지 못하거나 콘텐츠 스트림 형식을 1로 설정할 수 없는 경우에는 콘텐츠를 전달하지 않습니다.
            </p>        
        </td>
    </tr>
    <tr>
        <th>400</th>
        <td rowspan="2" colspan="2">해당 없음\*</td>
        <td rowspan="2">Windows 10에서는후속 OPL 값에 관계없이 압축된 디지털 비디오 콘텐츠를 출력으로 전달하지 않습니다. 압축된 디지털 비디오 콘텐츠에 대한 자세한 내용은 <a href="https://www.microsoft.com/playready/licensing/compliance/">PlayReady 제품에 대한 규정 준수 규칙</a>을 참조하세요.</td>
        <td rowspan="2">해당 없음\*</td>
    </tr>
    <tr>
        <th>500</th>
    </tr>
</table>
<br/>

\* 출력 보호 수준에 대한 모든 값을 라이선스 서버에서 설정할 수 있는 것은 아닙니다. 자세한 내용은 [PlayReady 규정 준수 규칙](https://www.microsoft.com/playready/licensing/compliance/)을 참조하세요.

### <a name="additional-explicit-output-restrictions"></a>추가 명시적 출력 제한

다음 표에서는 명시적 디지털 비디오 출력 보호 제한의 Windows 10용 PlayReady DRM 구현에 대해 설명합니다.

<table>
    <tr>
        <th>시나리오</th>
        <th>GUID</th>
        <th>해당하는 경우</th>
        <th>발생하는 결과</th>
    </tr>
    <tr>
        <th>최대 유효 해상도 디코드 크기</th>
        <td>9645E831-E01D-4FFF-8342-0A720E3E028F</td>
        <td>연결된 출력: 디지털 비디오 출력, Miracast, HDMI, DVI 등</td>
        <td>
            <p>
                아래의 경우 중 하나로 제한되면 콘텐츠를 전달합니다.  
            </p>
            <ul>
                <li>(a) 프레임의 너비가 최대 프레임 너비(픽셀)보다 작거나 같고 프레임의 높이가 최대 프레임 높이(픽셀)보다 작거나 같은 경우</li>
                <li>(b) 프레임의 높이가 최대 프레임 너비(픽셀)보다 작거나 같고 프레임의 너비가 최대 프레임 높이(픽셀)보다 작거나 같은 경우</li>
            </ul>                   
        </td>
    </tr>
    <tr>
        <th>HDCP 유형 제한</th>
        <td>ABB2C6F1-E663-4625-A945-972D17B231E7</td>
        <td>연결된 출력: 디지털 비디오 출력, Miracast, HDMI, DVI 등</td>
        <td>HDCP 2.2를 사용하고 콘텐츠 스트림 형식을 1로 설정하여 콘텐츠를 전달합니다. HDCP 2.2를 사용하지 못하거나 콘텐츠 스트림 형식을 1로 설정할 수 없는 경우에는 콘텐츠를 전달하지 않습니다. 또한 압축되지 않은 디지털 비디오 출력 보호 수준을 271보다 크거나 같은 값으로 지정해야 합니다.</td>
    </tr>
</table>
<br/>

다음 표에서는 명시적 아날로그 비디오 출력 보호 제한의 Windows 10용 PlayReady DRM 구현에 대해 설명합니다.

<table>
    <tr>
        <th>시나리오</th>
        <th>GUID</th>
        <th>해당하는 경우</th>
        <th colspan="2">발생하는 결과</th>
    </tr>
    <tr>
        <th>아날로그 컴퓨터 모니터</th>
        <td>D783A191-E083-4BAF-B2DA-E69F910B3772</td>
        <td>연결된 출력: VGA, DVI&ndash;아날로그 등</td>
        <td><b>SWDRM:</b> PC는 유효 해상도를 프레임당 520,000epx로 제한하고 콘텐츠를 전달합니다.</td>
        <td><b>HWDRM:</b> 콘텐츠를 전달하지 않습니다.</td>
    </tr>
    <tr>
        <th>아날로그 컴포넌트</th>
        <td>811C5110-46C8-4C6E-8163-C0482A15D47E</td>
        <td>연결된 출력: 컴포넌트</td>
        <td><b>SWDRM:</b> PC는 유효 해상도를 프레임당 520,000epx로 제한하고 콘텐츠를 전달합니다.</td>
        <td><b>HWDRM:</b> 콘텐츠를 전달하지 않습니다.</td>
    </tr>
    <tr>
        <th rowspan="2">아날로그 TV 출력</th>
        <td>2098DE8D-7DDD-4BAB-96C6-32EBB6FABEA3</td>
        <td>아날로그 TV OPL이 151 미만입니다.</td>
        <td colspan="2">CGMS-A가 사용되어야 합니다.</td>
    </tr>
    <tr>
        <td>225CD36F-F132-49EF-BA8C-C91EA28E4369</td>
        <td>아날로그 TV OPL이 101 미만이고 라이선스에 2098DE8D-7DDD-4BAB-96C6-32EBB6FABEA3이 포함되어 있지 않습니다.</td>
        <td colspan="2">CGMS-A 사용이 시도되어야 하지만 결과에 관계없이 콘텐츠가 재생될 수 있습니다.</td>
    </tr>
    <tr>
        <th>Automatic Gain Control(자동 이득 제어) 및 컬러 스트라이프</th>
        <td>C3FD11C6-F8B7-4D20-B008-1DB17D61F2DA</td>
        <td>520,000px보다 작거나 같은 해상도로 아날로그 TV 출력에 콘텐츠를 전달합니다.</td>
        <td colspan="2">규정 준수 규칙의 표 3.5.7.3.에 따라, 해상도가 520,000px 미만인 경우 컴포넌트 비디오 및 PAL 모드에만 AGC를 설정하고, 해상도가 520,000px 미만인 경우 NTSC에 대해 AGC 및 컬러 스트라이프 정보를 설정합니다.</td>
    </tr>
    <tr>
        <th>디지털 전용 출력</th>
        <td>760AE755-682A-41E0-B1B3-DCDF836A7306</td>
        <td>연결된 출력: 아날로그</td>
        <td colspan="2">콘텐츠를 전달하지 않습니다.</td>
    </tr>
</table>
<br/>

> [!NOTE]
> "Mini DisplayPort-VGA"와 같은 어댑터 동글을 재생에 사용하는 경우 Windows 10에서는 출력을 디지털 비디오 출력으로 간주하고 아날로그 비디오 정책을 적용할 수 없습니다.

다음 표에서는 다른 상황에서 재생할 수 있도록 하는 Windows 10용 PlayReady DRM 구현에 대해 설명합니다.

<table>
    <tr>
        <th>시나리오</th>
        <th>GUID</th>
        <th>해당하는 경우</th>
        <th colspan="2">발생하는 결과</th>
    </tr>
    <tr>
        <th>알 수 없는 출력</th>
        <td>786627D8-C2A6-44BE-8F88-08AE255B01A7</td>
        <td>출력을 적절하게 결정할 수 없거나 그래픽 드라이버와 OPM을 설정할 수 없는 경우</td>
        <td><b>SWDRM:</b> 콘텐츠를 전달합니다.</td>
        <td><b>HWDRM:</b> 콘텐츠를 전달하지 않습니다.</td>
    </tr>
    <tr>
        <th>압축된 알 수 없는 출력</th>
        <td>B621D91F-EDCC-4035-8D4B-DC71760D43E9</td>
        <td>출력을 적절하게 결정할 수 없거나 그래픽 드라이버와 OPM을 설정할 수 없는 경우</td>
        <td><b>SWDRM:</b> PC는 유효 해상도를 프레임당 520,000epx로 제한하고 콘텐츠를 전달합니다.</td>
        <td><b>HWDRM:</b> 콘텐츠를 전달하지 않습니다.</td>
    </tr>
</table>
<br/>

## <a name="prerequisites"></a>필수 조건

PlayReady 보호된 UWP 앱 만들기를 시작하기 전에 다음 소프트웨어를 시스템에 설치해야 합니다.

-   Windows10 합니다.
-   Microsoft Visual Studio2015를 사용 해야 컴파일하는 경우 샘플 PlayReady DRM에 대 한 UWP 앱에 대해, 또는 나중 샘플을 컴파일해야 합니다. 여전히 Microsoft Visual Studio2013를 Windows8.1 스토어 앱 용 PlayReady DRM의 샘플을 컴파일하는 데 사용할 수 있습니다.

<!--This is no longer available-->
<!--If you are planning to play back MPEG-2/H.262 content on your app, you must also download and install [Windows 8.1 Media Center Pack](http://go.microsoft.com/fwlink/p/?LinkId=626876).-->

## <a name="playready-uwp-app-migration-guide"></a>PlayReady UWP 앱 마이그레이션 가이드

이 섹션에 기존 PlayReady Windows 8.x 스토어 앱 Windows10 마이그레이션하는 방법에 대 한 정보를 포함 합니다.

Windows10에서 PlayReady UWP 앱에 대 한 네임 스페이스 [**Windows.Media.Protection.PlayReady**](https://msdn.microsoft.com/library/windows/apps/dn986454) **Microsoft.Media.PlayReadyClient** 에서 변경 되었습니다. 즉, 코드에서 이전 네임스페이스를 검색하여 새 네임스페이스로 바꿔야 합니다. 여전히 winmd 파일을 참조합니다. 파일은 Windows10 운영 체제에서 windows.media.winmd의 일부입니다. 이 파일은 TH의 Windows SDK의 일부로 windows.winmd에 있습니다. UWP의 경우 windows.foundation.univeralappcontract.winmd에서 참조됩니다.

PlayReady 보호된 HD(고해상도) 콘텐츠(1080) 및 UHD(초고해상도) 콘텐츠를 재생하려면 PlayReady 하드웨어 DRM을 구현해야 합니다. PlayReady 하드웨어 DRM을 구현하는 방법에 대한 정보는 [하드웨어 DRM](hardware-drm.md)을 참조하세요.

일부 콘텐츠는 하드웨어 DRM에서 지원되지 않습니다. 하드웨어 DRM을 사용하지 않고 소프트웨어 DRM을 사용하도록 설정하는 방법에 대한 정보는 [하드웨어 DRM 재정의](hardware-drm.md#override-hardware-drm)를 참조하세요.

미디어 보호 관리자와 관련하여 코드에 다음 설정이 있어야 합니다.

```cs
var mediaProtectionManager = new Windows.Media.Protection.MediaProtectionManager();

mediaProtectionManager.Properties["Windows.Media.Protection.MediaProtectionSystemId"] = 
             '{F4637010-03C3-42CD-B932-B48ADF3A6A54}'
var cpsystems = new Windows.Foundation.Collections.PropertySet();
cpsystems["{F4637010-03C3-42CD-B932-B48ADF3A6A54}"] = 
                "Windows.Media.Protection.PlayReady.PlayReadyWinRTTrustedInput";
mediaProtectionManager.Properties["Windows.Media.Protection.MediaProtectionSystemIdMapping"] = cpsystems;

mediaProtectionManager.Properties["Windows.Media.Protection.MediaProtectionContainerGuid"] = 
                "{9A04F079-9840-4286-AB92-E65BE0885F95}";
```

## <a name="proactively-acquire-a-non-persistent-license-before-playback"></a>재생하기 전에 미리 비영구 라이선스 취득

이 섹션에서는 재생을 시작하기 전에 미리 비영구적 라이선스를 취득하는 방법을 설명합니다.

PlayReady DRM의 이전 버전에서는 비영구적 라이선스를 사후 방식으로 재생 중에만 취득할 수 있습니다. 이 버전에서는 재생을 시작하기 전에 미리 비영구적 라이선스를 취득할 수 있습니다.

1.  비영구 라이선스를 저장할 수 있는 재생 세션을 사전에 만듭니다. 예:

    ```cs
    var cpsystems = new Windows.Foundation.Collections.PropertySet();       
    cpsystems["{F4637010-03C3-42CD-B932-B48ADF3A6A54}"] = "Windows.Media.Protection.PlayReady.PlayReadyWinRTTrustedInput"; // PlayReady

    var pmpSystemInfo = new Windows.Foundation.Collections.PropertySet();
    pmpSystemInfo["Windows.Media.Protection.MediaProtectionSystemId"] = "{F4637010-03C3-42CD-B932-B48ADF3A6A54}";
    pmpSystemInfo["Windows.Media.Protection.MediaProtectionSystemIdMapping"] = cpsystems;
    var pmpServer = new Windows.Media.Protection.MediaProtectionPMPServer( pmpSystemInfo );
    ```

2.  이 재생 세션을 라이선스 취득 클래스에 연결합니다. 예:

    ```cs
    var licenseSessionProperties = new Windows.Foundation.Collections.PropertySet();
    licenseSessionProperties["Windows.Media.Protection.MediaProtectionPMPServer"] = pmpServer;
    var licenseSession = new Windows.Media.Protection.PlayReady.PlayReadyLicenseSession( licenseSessionProperties );
    ```

3.  라이선스 서비스 요청을 만듭니다. 예를 들면 다음과 같습니다.

    ```cs
    var laSR = licenseSession.CreateLAServiceRequest();
    ```

4.  3단계에서 만든 서비스 요청을 사용하여 라이선스 취득을 수행합니다. 라이선스가 재생 세션에 저장됩니다.
5.  재생 세션을 재생할 미디어 소스에 연결합니다. 예:

    ```cs
    licenseSession.configureMediaProtectionManager( mediaProtectionManager );
    videoPlayer.msSetMediaProtectionManager( mediaProtectionManager );
    ```
    
## <a name="query-for-protection-capabilities"></a>보호 기능에 대한 쿼리
Windows 10 버전 1703부터, 디코드 코덱, 해상도, 출력 보호(HDCP)와 같은 HW DRM 기능을 쿼리할 수 있습니다. 쿼리는 [**IsTypeSupported**](https://docs.microsoft.com/uwp/api/windows.media.protection.protectioncapabilities.istypesupported) 메서드를 사용하여 수행되며, 지원되는지 쿼리하는 기능에 대한 문자열 표현과 쿼리가 적용되는 주요 시스템을 지정하는 문자열을 받습니다. 지원되는 문자열 값 목록은 [**IsTypeSupported**](https://docs.microsoft.com/uwp/api/windows.media.protection.protectioncapabilities.istypesupported)에 대한 API 참조 페이지를 참조하세요. 다음 코드 예제는 이 메서드의 사용 방법을 보여 줍니다.  

    ```cs
    using namespace Windows::Media::Protection;

    ProtectionCapabilities^ sr = ref new ProtectionCapabilities();

    ProtectionCapabilityResult result = sr->IsTypeSupported(
    L"video/mp4; codecs=\"avc1.640028\"; features=\"decode-bpp=10,decode-fps=29.97,decode-res-x=1920,decode-res-y=1080\"",
    L"com.microsoft.playready");

    switch (result)
    {
        case ProtectionCapabilityResult::Probably:
        // Queue up UHD HW DRM video
        break;

        case ProtectionCapabilityResult::Maybe:
        // Check again after UI or poll for more info.
        break;

        case ProtectionCapabilityResult::NotSupported:
        // Do not queue up UHD HW DRM video.
        break;
    }
    ```
## <a name="add-secure-stop"></a>보안 중지 추가

이 섹션에서는 UWP 앱에 보안 중지를 추가하는 방법을 설명합니다.

보안 중지를 통해 PlayReady 장치는 지정된 콘텐츠에 대해 미디어 재생이 중지된 미디어 스트리밍 서비스로 안정적으로 어설션됩니다. 이 기능은 미디어 스트리밍 서비스가 지정된 계정에 대해 다양한 디바이스의 사용 제한을 정확하게 적용하고 보고하도록 합니다.

보안 중지 챌린지를 보내는 주요 시나리오는 두 가지가 있습니다.

-   콘텐츠의 끝에 도달했거나 사용자가 미디어 프레젠테이션을 중간에 중지했으므로 미디어 프레젠테이션이 중지되는 경우.
-   이전 세션이 예기치 않게 종료되는 경우(예: 시스템 또는 앱 크래시가 원인). 앱에서 시작 또는 종료 시 해결되지 않은 보안 중지 세션을 쿼리하고 다른 미디어 재생과 별도로 챌린지를 보내야 합니다.

보안 중지의 샘플 구현에 대해서는 PlayReady 샘플([http://go.microsoft.com/fwlink/p/?linkid=331670&clcid=0x409](http://go.microsoft.com/fwlink/p/?linkid=331670))의 securestop.cs 파일을 참조하세요.

## <a name="use-playready-drm-on-xbox-one"></a>Xbox One에서 PlayReady DRM 사용

Xbox One의 UWP 앱에서 PlayReady DRM을 사용하려면 먼저 앱을 게시하는 데 사용할 개발자 센터 계정을 등록하여 PlayReady를 사용할 권한을 부여해야 합니다. 다음 두 가지 방법 중 하나로 이 작업을 수행할 수 있습니다.

* Microsoft의 담당자에게 권한을 요청하도록 합니다.
* 개발자 센터 계정과 회사 이름을 [pronxbox@microsoft.com](mailto:pronxbox@microsoft.com)으로 보내 권한 부여를 신청합니다.

권한을 부여받으면 추가 `<DeviceCapability>`를 앱 매니페스트에 추가해야 합니다. 현재 앱 매니페스트 디자이너에서 사용할 수 있는 설정이 없으므로 수동으로 추가해야 합니다. 구성하려면 다음 단계를 따르세요.

1. Visual Studio에서 프로젝트를 열고 **솔루션 탐색기**를 열고 **Package.appxmanifest**를 마우스 오른쪽 단추로 클릭합니다.
2. **연결 프로그램...** 을 선택하고 **XML(텍스트) 편집기**를 선택한 다음 **확인**을 클릭합니다.
3. `<Capabilities>` 태그 사이에 다음 `<DeviceCapability>`를 추가합니다.

    ```xml
    <DeviceCapability Name="6a7e5907-885c-4bcb-b40a-073c067bd3d5" />
    ```

4. 파일을 저장합니다.

마지막으로 Xbox One에서 PlayReady를 사용할 경우 마지막으로 고려할 사항이 있습니다. 개발 키트에는 SL150 제한이 있다는 것입니다(즉, SL2000 또는 SL3000 콘텐츠는 재생할 수 없음). 정품 장치는 더 높은 보안 수준으로 콘텐츠를 재생할 수 있지만 개발 키트에서 앱을 테스트하려면 SL150 콘텐츠를 사용해야 합니다. 다음 방법 중 하나로 이 콘텐츠를 테스트할 수 있습니다.

* SL150 라이선스가 필요한 큐레이트 테스트 콘텐츠를 사용합니다.
* 인증된 특정 테스트 계정만 특정 콘텐츠에 대해 SL150 라이선스를 획득할 수 있도록 논리를 구현합니다.

회사 및 제품에 가장 적합한 방법을 사용합니다.


## <a name="see-also"></a>참고 항목
- [미디어 재생](media-playback.md)




