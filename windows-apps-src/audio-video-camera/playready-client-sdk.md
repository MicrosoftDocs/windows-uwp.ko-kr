---
ms.assetid: DD8FFA8C-DFF0-41E3-8F7A-345C5A248FC2
description: 이 항목에서는 유니버설 Windows 플랫폼 (UWP) 앱에 PlayReady로 보호 된 미디어 콘텐츠를 추가 하는 방법에 대해 설명 합니다.
title: PlayReady DRM
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: be670dfab9e7bd27c4c380e9b00ec8a655704885
ms.sourcegitcommit: 75e1f49be211e8b4b3e825978d67625776f992f5
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/17/2020
ms.locfileid: "94691531"
---
# <a name="playready-drm"></a>PlayReady DRM



이 항목에서는 유니버설 Windows 플랫폼 (UWP) 앱에 PlayReady로 보호 된 미디어 콘텐츠를 추가 하는 방법에 대해 설명 합니다.

PlayReady DRM을 통해 개발자는 콘텐츠 공급자가 정의한 액세스 규칙을 적용 하면서 사용자에 게 PlayReady 콘텐츠를 제공할 수 있는 UWP 앱을 만들 수 있습니다. 이 섹션에서는 Windows 10 용 Microsoft PlayReady DRM에 대 한 변경 내용 및 이전 Windows 8.1 버전에서 Windows 10 버전으로의 변경 내용을 지원 하도록 PlayReady UWP 앱을 수정 하는 방법을 설명 합니다.
 
| 항목                                                                     | Description                                                                                                                                                                                                                                                                             |
|---------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [하드웨어 DRM](hardware-drm.md)                                           | 이 항목에서는 PlayReady 하드웨어 기반 DRM (디지털 권한 관리)을 UWP 앱에 추가 하는 방법에 대 한 개요를 제공 합니다.                                                                                                                                                                 |
| [PlayReady를 사용 하 여 적응 스트리밍](adaptive-streaming-with-playready.md) | 이 문서에서는 Microsoft PlayReady content protection을 사용 하는 멀티미디어 콘텐츠의 적응 스트리밍을 유니버설 Windows 플랫폼 (UWP) 앱에 추가 하는 방법을 설명 합니다. 이 기능은 현재 HLS (Http 라이브 스트리밍) 및 HTTP (대시) 콘텐츠를 통한 동적 스트리밍을 재생할 수 있도록 지원 합니다. |

## <a name="whats-new-in-playready-drm"></a>PlayReady DRM의 새로운 기능

다음 목록에서는 Windows 10에 대 한 PlayReady DRM에 대 한 새로운 기능 및 변경 사항을 설명 합니다.

-   하드웨어 HWDRM (디지털 권한 관리)가 추가 되었습니다.

    하드웨어 기반 콘텐츠 보호 지원을 사용 하면 여러 장치 플랫폼에서 HD (high definition) 및 UHD (ultra high definition) 콘텐츠를 안전 하 게 재생할 수 있습니다. 키 자료 (개인 키, 콘텐츠 키 및 언급 된 키를 파생 시키거나 잠금 해제 하는 데 사용 되는 기타 모든 키 자료 포함), 암호 해독 된 압축 및 압축 되지 않은 비디오 샘플은 하드웨어 보안을 활용 하 여 보호 됩니다. 하드웨어 DRM을 사용 하는 경우 알 수 없는 HWDRM 파이프라인은 사용 되는 출력을 항상 알 수 있기 때문에 알 수 없습니다. 자세한 내용은 [하드웨어 DRM](hardware-drm.md)을 참조 하세요.

-   PlayReady는 더 이상 appX framework 구성 요소가 아니지만 기본 운영 체제 구성 요소입니다. 네임 스페이스는 PlayReadyClient에서 **Microsoft.Media.PlayReadyClient** [**로 변경 되었습니다.**](/uwp/api/Windows.Media.Protection.PlayReady)
-   PlayReady 오류 코드를 정의 하는 다음 헤더는 이제 Windows SDK (소프트웨어 개발 키트)에 포함 되어 있습니다: PlayReadyErrors 및 PlayReadyResults.
-   영구적이 지 않은 라이선스의 사전 획득 획득을 제공 합니다.

    이전 버전의 PlayReady DRM은 비영구 라이선스의 자동 관리 획득을 지원 하지 않았습니다. 이 기능은이 버전에 추가 되었습니다. 이렇게 하면 첫 번째 프레임까지 시간이 줄어들 수 있습니다. 자세한 내용은 [재생 전에 비영구 라이선스 사전 확보](#proactively-acquire-a-non-persistent-license-before-playback)를 참조 하세요.

-   단일 메시지에서 여러 라이선스의 획득을 제공 합니다.

    클라이언트 앱이 하나의 라이선스 취득 메시지에서 비영구 라이선스를 여러 개 가져올 수 있습니다. 이렇게 하면 사용자가 콘텐츠 라이브러리를 검색 하는 동안 여러 콘텐츠에 대 한 라이선스를 확보 하 여 첫 번째 프레임까지 걸리는 시간을 줄일 수 있습니다. 이렇게 하면 사용자가 재생할 콘텐츠를 선택할 때 라이선스 획득에 대 한 지연이 발생 하지 않습니다. 또한 여러 키 식별자 (어린이)를 포함 하는 콘텐츠 헤더를 사용 하도록 설정 하 여 오디오 및 비디오 스트림을 개별 키로 암호화할 수 있습니다. 이렇게 하면 단일 라이선스 획득으로 동일한 결과를 얻기 위해 사용자 지정 논리 및 여러 라이선스 획득 요청을 사용 하지 않고 콘텐츠 파일 내의 모든 스트림에 대해 모든 라이선스를 획득할 수 있습니다.

-   실시간 만료 지원 또는 제한 된 기간 라이선스 (LDL)가 추가 되었습니다.

    라이선스에 대 한 실시간 만료를 설정 하 고 만료 되는 라이선스에서 재생 도중에 다른 (유효한) 라이선스로 원활 하 게 전환 하는 기능을 제공 합니다. 단일 메시지에서 여러 라이선스의 획득과 함께 사용 하는 경우 사용자가 콘텐츠 라이브러리를 검색 하는 동안 앱이 여러 LDLs를 비동기적으로 가져올 수 있으며, 사용자가 재생할 콘텐츠를 선택한 후에도 더 긴 기간 라이선스를 획득할 수 있습니다. 그러면 재생이 더 빠르게 시작 되 고 (라이선스를 이미 사용할 수 있기 때문에) 앱이 LDL 만료 된 시간에 더 긴 기간 라이선스를 획득 했으므로 중단 없이 콘텐츠 끝까지 원활 하 게 재생 됩니다.

-   영구적이 지 않은 라이선스 체인이 추가 되었습니다.
-   비 영구적인 라이선스에 대 한 시간 기반 제한 (만료, 첫 번째 재생 후 만료 및 실시간 만료 포함)에 대 한 지원이 추가 되었습니다.
-   HDCP 유형 1 (Windows 10의 버전 2.2) 정책 지원을 추가 했습니다.

    자세한 내용은 [고려 사항을](#things-to-consider) 참조 하세요.

-   이제 Miracast는 출력으로 암시적입니다.
-   보안 중지를 추가 했습니다.

    Secure stop은 PlayReady 장치가 지정 된 콘텐츠에 대해 미디어 재생이 중지 되었음을 미디어 스트리밍 서비스에 자신 있게 어설션할 수 있는 수단을 제공 합니다. 이 기능을 통해 미디어 스트리밍 서비스는 지정 된 계정에 대 한 다양 한 장치에 대 한 사용 제한 사항을 정확 하 게 적용 하 고 보고할 있습니다.

-   오디오 및 비디오 라이선스 분리를 추가 했습니다.

    별도의 트랙은 비디오가 오디오로 디코딩되는 것을 방지 합니다. 더 강력한 콘텐츠 보호를 사용 하도록 설정 합니다. 새로운 표준에는 오디오 및 시각적 추적을 위한 별도의 키가 필요 합니다.

-   MaxResDecode를 추가 했습니다.

    이 기능은 더 강력한 지원 키 (라이선스 제외)가 있는 경우에도 콘텐츠 재생을 최대 해상도로 제한 하기 위해 추가 되었습니다. 여러 스트림 크기가 단일 키로 인코딩된 경우를 지원 합니다.

다음과 같은 새 인터페이스, 클래스 및 열거가 PlayReady DRM에 추가 되었습니다.

-   [**IPlayReadyLicenseAcquisitionServiceRequest**](/uwp/api/Windows.Media.Protection.PlayReady.IPlayReadyLicenseAcquisitionServiceRequest) 인터페이스
-   [**IPlayReadyLicenseSession**](/uwp/api/Windows.Media.Protection.PlayReady.IPlayReadyLicenseSession) 인터페이스
-   [**IPlayReadySecureStopServiceRequest**](/uwp/api/Windows.Media.Protection.PlayReady.IPlayReadySecureStopServiceRequest) 인터페이스
-   [**PlayReadyLicenseSession**](/uwp/api/Windows.Media.Protection.PlayReady.PlayReadyLicenseSession) 클래스
-   [**PlayReadySecureStopIterable**](/uwp/api/Windows.Media.Protection.PlayReady.PlayReadySecureStopIterable) 클래스
-   [**PlayReadySecureStopIterator**](/uwp/api/Windows.Media.Protection.PlayReady.PlayReadySecureStopIterator) 클래스
-   [**PlayReadyHardwareDRMFeatures**](/uwp/api/Windows.Media.Protection.PlayReady.PlayReadyHardwareDRMFeatures) 열거자

PlayReady DRM의 새 기능을 사용 하는 방법을 보여 주기 위해 새 샘플을 만들었습니다. 샘플은 [코드 샘플 브라우저](/samples/microsoft/windows-universal-samples/playready/)에서 다운로드할 수 있습니다.

## <a name="things-to-consider"></a>고려할 사항

-   PlayReady DRM은 이제 HDCP 유형 1 (HDCP 버전 2.1 이상에서 지원 됨)을 지원 합니다. PlayReady는 장치에서 적용할 수 있는 HDCP 유형 제한 정책을 라이선스에 전달 합니다. Windows 10에서이 정책은 HDCP 2.2 이상에 적용 되는 것을 적용 합니다. 이 기능은 PlayReady Server v 3.0 SDK 라이선스에서 사용 하도록 설정할 수 있습니다. 서버는 HDCP 유형 제한 GUID를 사용 하 여 라이선스에서이 정책을 제어 합니다. 자세한 내용은 [PlayReady 준수 및 견고성 규칙](https://www.microsoft.com/playready/licensing/compliance/)을 참조 하세요.
-   하드웨어 DRM에서는 Windows Media 비디오 (VC-1이 라고도 함)가 지원 되지 않습니다 ( [하드웨어 Drm 재정의](hardware-drm.md#override-hardware-drm)참조).
-   PlayReady DRM은 이제 높은 효율성 비디오 코딩 (HEVC/H.265) 비디오 압축 표준을 지원 합니다. HEVC을 지원 하려면 앱에서 콘텐츠 조각 헤더를 clear로 유지 하는 CENC (Common Encryption 체계) 버전 2 콘텐츠를 사용 해야 합니다. ISO/IEC 23001-7 정보 기술--MPEG systems 기술--7 부: ISO 기본 미디어 파일 형식 파일의 일반 암호화 (사양 버전 ISO/IEC 23001-7:2015 이상이 필요 함)를 참조 하세요. Microsoft는 또한 모든 HWDRM 콘텐츠에 대해 CENC 버전 2를 사용 하는 것이 좋습니다. 또한 일부 하드웨어 DRM은 HEVC를 지원 하 고 일부는 그렇지 않습니다 ( [하드웨어 Drm 재정의](hardware-drm.md#override-hardware-drm)참조).
-   특정 한 새로운 PlayReady 3.0 기능 (하드웨어 기반 클라이언트에 대 한 SL3000, 한 라이선스 취득 메시지에서 비영구 라이선스를 여러 개 가져오기, 비영구 라이선스에 대 한 시간 기반 제한 포함)을 활용 하기 위해 PlayReady 서버는 Microsoft PlayReady Server Software Development Kit v 3.0.2769 릴리스 버전 이상 이어야 합니다.
-   콘텐츠 라이선스에 지정 된 출력 보호 정책에 따라 연결 된 출력이 해당 요구 사항을 지원 하지 않는 경우 최종 사용자에 게 미디어 재생이 실패할 수 있습니다. 다음 표에서는 결과로 발생 하는 일반적인 오류 집합을 나열 합니다. 자세한 내용은 [PlayReady 준수 및 견고성 규칙](https://www.microsoft.com/playready/licensing/compliance/)을 참조 하세요.

| 오류                                                   | 값      | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|---------------------------------------------------------|------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 오류 \_ GRAPHICS \_ OPM \_ OUTPUT \_ 은 \_ \_ HDCP를 지원 하지 않습니다 \_ .  | 0xC0262513 | 라이선스의 출력 보호 정책에서는 모니터가 HDCP를 사용 해야 하지만 HDCP를 사용할 수 없습니다.                                                                                                                                                                                                                                                                                                                                                                                              |
| MF \_ E \_ 정책 \_ 지원 안 됨                              | 0xC00D7159 | 라이선스의 출력 보호 정책에서는 모니터가 HDCP 유형 1에 참여 해야 하지만 HDCP 유형 1은 사용할 수 없습니다.                                                                                                                                                                                                                                                                                                                                                                                |
| DRM \_ E \_ 티 드 \_ 출력 \_ 보호 \_ 요구 사항 \_ \_ 충족 안 됨 | 0x8004CD22 | 이 오류 코드는 하드웨어 DRM에서 실행 하는 경우에만 발생 합니다. 라이선스의 출력 보호 정책에서 HDCP를 사용 하거나 콘텐츠의 유효 해상도를 줄이기 위해 모니터를 요구 하지만, 하드웨어 DRM이 콘텐츠의 해상도를 줄이는 것을 지원 하지 않기 때문에 HDCP를 사용할 수 없고 콘텐츠의 유효 해상도를 줄일 수 없습니다. 소프트웨어 DRM에서 콘텐츠가 재생 됩니다. [하드웨어 DRM 사용에 대 한 고려 사항을](hardware-drm.md#considerations-for-using-hardware-drm)참조 하세요. |
| 오류 \_ 그래픽 \_ OPM \_ 지원 되지 않음 \_                    | 0xc0262500 | 그래픽 드라이버는 출력 보호를 지원 하지 않습니다. 예를 들어 모니터가 VGA를 통해 연결 되었거나 디지털 출력에 적합 한 그래픽 드라이버가 설치 되어 있지 않습니다. 후자의 경우에는 설치 된 일반적인 드라이버가 Microsoft 기본 디스플레이 어댑터 이며 적절 한 그래픽 드라이버를 설치 하면 문제가 해결 됩니다.                                                                                                                                                  |

## <a name="output-protection"></a>출력 보호

다음 섹션에서는 PlayReady 라이선스에서 출력 보호 정책을 통해 Windows 10 용 PlayReady DRM을 사용 하는 경우의 동작에 대해 설명 합니다.

PlayReady DRM은 **Microsoft Playready 확장 가능 미디어 권한 사양** 에 포함 된 출력 보호 수준을 지원 합니다. 이 문서는 PlayReady 사용이 허가 된 제품과 함께 제공 되는 설명서 팩에서 찾을 수 있습니다.

> [!NOTE]
> 라이선스 서버에서 설정할 수 있는 출력 보호 수준에 대해 허용 되는 값은 [PlayReady 준수 규칙](https://www.microsoft.com/playready/licensing/compliance/)의 적용을 받습니다.

PlayReady DRM은 PlayReady 준수 규칙에 지정 된 대로 출력 커넥터 에서만 출력 보호 정책을 사용 하 여 콘텐츠를 재생할 수 있도록 합니다. PlayReady 준수 규칙에 지정 된 출력 커넥터 용어에 대 한 자세한 내용은 [Playready 준수 및 견고성 규칙에 대해 정의 된 용어](https://www.microsoft.com/playready/licensing/compliance/)를 참조 하세요.

이 섹션에서는 windows 10 용 PlayReady DRM 및 windows 10 용 PlayReady 하드웨어 DRM을 사용 하는 출력 보호 시나리오에 대해 집중적으로 설명 합니다 .이는 일부 Windows 클라이언트 에서도 사용할 수 있습니다. PlayReady HWDRM를 사용 하 여 모든 출력 보호는 Windows 티 구현 내에서 적용 됩니다 ( [하드웨어 DRM](hardware-drm.md)참조). 따라서 PlayReady SWDRM (소프트웨어 DRM)을 사용할 때와는 다른 동작이 있습니다.

* 압축 되지 않은 디지털 비디오 270에 대 한 OPL (출력 보호 수준) 지원: Windows 10 용 PlayReady HWDRM는 다운 (resolution)을 지원 하지 않으며 HDCP (고대역폭 디지털 Content Protection)가 적용 됩니다. HWDRM에 대 한 높은 정의 콘텐츠의 OPL이 270 보다 큰 것이 좋습니다 (필수는 아니지만). 또한 라이선스 (HDCP 버전 2.2 이상)에서 HDCP 유형 제한을 설정 해야 합니다.
* SWDRM과 달리 HWDRM에서 출력 보호는 최소 지원 모니터를 기준으로 모든 모니터에 적용 됩니다. 예를 들어 사용자에 게 HDCP를 지 원하는 두 개의 모니터와 연결 되어 있는 경우 콘텐츠가 HDCP를 지 원하는 모니터 에서만 렌더링 되는 경우에도 해당 라이선스에 HDCP가 필요한 경우 재생에 실패 합니다. SWDRM에서 콘텐츠는 HDCP를 지 원하는 모니터에서 렌더링 되는 동안에만 재생 됩니다.
* 콘텐츠 키 및 라이선스에서 다음 조건을 충족 하지 않는 경우 HWDRM는 클라이언트에서 사용 하도록 보장 되지 않습니다.
    * 비디오 콘텐츠 키에 사용 되는 라이선스의 보안 수준은 3000 이상 이어야 합니다.
    * 오디오는 비디오와 다른 콘텐츠 키로 암호화 되어야 하 고 오디오에 사용 되는 라이선스의 보안 수준은 2000 이상 이어야 합니다. 또는 투명 하 게 오디오를 남겨둘 수 있습니다.
* 모든 SWDRM 시나리오를 사용 하려면 오디오 및/또는 비디오 콘텐츠 키에 사용 되는 PlayReady 라이선스의 최소 보안 수준이 2000 보다 작거나 같아야 합니다.

### <a name="output-protection-levels"></a>출력 보호 수준

다음 표에서는 PlayReady 라이선스에서 다양 한 OPLs 간의 매핑과 Windows 10 용 PlayReady DRM이 적용 되는 방식을 간략하게 설명 합니다.

#### <a name="video"></a>동영상

<table>
    <tr>
        <th rowspan="2">OPL</th>
        <th>압축 된 디지털 비디오</th>
        <th colspan="2">압축 되지 않은 디지털 비디오</th>
        <th>아날로그 TV</th>
    </tr>
    <tr>
        <th>모두</th>
        <th colspan="2">HDMI, DVI, DisplayPort, MHL</th>
        <th>구성 요소, 복합</th>
    </tr>
    <tr>
        <th>100</th>
        <td rowspan="6">해당 없음\*</td>
        <td colspan="2">콘텐츠 전달</td>
        <td>콘텐츠 전달</td>
    </tr>
    <tr>
        <th>150</th>
        <td colspan="2" rowspan="2">해당 없음\*</td>
        <td>CGMS 때 콘텐츠를 전달 합니다. CGMS가 개입 되지 않거나</td>
    </tr>
    <tr>
        <th>200</th>
        <td>CGMS 때 콘텐츠를 전달 합니다.</td>
    </tr>
    <tr>
        <th>250</th>
        <td colspan="2">HDCP를 참여 하려고 시도 하지만 결과에 관계 없이 콘텐츠를 전달 합니다.</td>
        <td rowspan="5">해당 없음\*</td>
    </tr>
    <tr>
        <th>270</th>
        <td><b>Swdrm</b>: HDCP를 참여 하려고 시도 합니다. HDCP가 참여 하지 못하는 경우 PC는 유효한 해상도를 프레임당 52만 픽셀로 제한 하 고 콘텐츠를 전달 합니다.</td>
        <td><b>HWDRM</b>: HDCP로 콘텐츠를 전달 합니다. HDCP가 실패 하는 경우 HDMI/DVI 포트로 재생이 차단 됩니다.</td>
    </tr>
    <tr>
        <th>300</th>
        <td colspan="2">
            <p>
                **HDCP 유형 제한이 정의 되지 않은 경우:** HDCP로 콘텐츠를 전달 합니다. HDCP가 실패 하는 경우 HDMI/DVI 포트로 재생이 차단 됩니다.
            </p>
            <p>
                **Hdcp 유형 제한이 정의 된 경우**: hdcp 2.2 및 콘텐츠 스트림 유형을 1로 설정 하 여 콘텐츠를 전달 합니다. HDCP가 실패 하거나 콘텐츠 스트림 형식을 1로 설정할 수 없는 경우 HDMI/DVI 포트로 재생이 차단 됩니다.
            </p>
        </td>
    </tr>
    <tr>
        <th>400</th>
        <td rowspan="2">Windows 10은 후속 OPL 값에 관계 없이 압축 된 디지털 비디오 콘텐츠를 출력에 전달 하지 않습니다. 압축 된 디지털 비디오 콘텐츠에 대 한 자세한 내용은 <a href="https://www.microsoft.com/playready/licensing/compliance/">PlayReady 제품에 대 한 준수 규칙</a>을 참조 하세요.</td>
        <td colspan="2" rowspan="2">해당 없음\*</td>
    </tr>
    <tr>
        <th>500</th>
    </tr>
</table>
<br/>

\* 라이선스 서버에서 출력 보호 수준에 대 한 모든 값을 설정할 수 있는 것은 아닙니다. 자세한 내용은 [PlayReady 준수 규칙](https://www.microsoft.com/playready/licensing/compliance/)을 참조하세요.

#### <a name="audio"></a>오디오

<table>
    <tr>
        <th rowspan="2">OPL</th>
        <th>압축 된 디지털 오디오</th>
        <th>압축 되지 않은 디지털 오디오</th>
        <th>아날로그 또는 USB 오디오</th>
    </tr>
    <tr>
        <th>HDMI, DisplayPort, MHL</th>
        <th>HDMI, DisplayPort, MHL</th>
        <th>모두</th>
    </tr>
    <tr>
        <th>100</th>
        <td rowspan="3">콘텐츠 전달</td>
        <td>콘텐츠 전달</td>
        <td rowspan="5">콘텐츠 전달</td>
    </tr>
    <tr>
        <th>150</th>
        <td rowspan="4">콘텐츠를 전달 하지 않음</td>
    </tr>
    <tr>
        <th>200</th>
    </tr>
    <tr>
        <th>250</th>
        <td>HDCP가 HDMI, DisplayPort 또는 MHL에서 관여 하거나 SCMS가 참여 하 고 CopyNever로 설정 된 경우 콘텐츠를 전달 합니다.</td>
    </tr>
    <tr>
        <th>300</th>
        <td>HDCP가 HDMI, DisplayPort 또는 MHL에서 관여 하는 경우 콘텐츠를 전달 합니다.</td>
    </tr>
</table>
<br/>

### <a name="miracast"></a>Miracast

PlayReady DRM을 사용 하면 HDCP 2.0 이상이 참여 하는 즉시 Miracast 출력을 통해 콘텐츠를 재생할 수 있습니다. 그러나 Windows 10에서는 Miracast가 *디지털* 출력으로 간주 됩니다. Miracast 시나리오에 대 한 자세한 내용은 [PlayReady 준수 규칙](https://www.microsoft.com/playready/licensing/compliance/)을 참조 하세요. 다음 표에서는 PlayReady 라이선스에서 다양 한 OPLs 간의 매핑과 PlayReady DRM이이를 Miracast 출력에 적용 하는 방법을 간략하게 설명 합니다.

<table>
    <tr>
        <th>OPL</th>
        <th>압축 된 디지털 오디오</th>
        <th>압축 되지 않은 디지털 오디오</th>
        <th>압축 된 디지털 비디오</th>
        <th>압축 되지 않은 디지털 비디오</th>
    </tr>
    <tr>
        <th>100</th>
        <td rowspan="4">HDCP 2.0 이상이 참여 하는 경우 콘텐츠를 전달 합니다. 참여 하지 못한 경우 콘텐츠를 전달 하지 않습니다.</td>
        <td>HDCP 2.0 이상이 참여 하는 경우 콘텐츠를 전달 합니다. 참여 하지 못한 경우 콘텐츠를 전달 하지 않습니다.</td>
        <td rowspan="6">해당 없음\*</td>
        <td>HDCP 2.0 이상이 참여 하는 경우 콘텐츠를 전달 합니다. 참여 하지 못한 경우 콘텐츠를 전달 하지 않습니다.</td>
    </tr>
    <tr>
        <th>150</th>
        <td rowspan="3">콘텐츠를 전달 하지 않음</td>
        <td rowspan="2">해당 없음\*</td>
    </tr>
    <tr>
        <th>200</th>
    </tr>
    <tr>
        <th>250</th>
        <td rowspan="2">HDCP 2.0 이상이 참여 하는 경우 콘텐츠를 전달 합니다. 참여 하지 못한 경우 콘텐츠를 전달 하지 않습니다.</td>
    </tr>
    <tr>
        <th>270</th>
        <td colspan="2">해당 없음\*</td>
    </tr>
    <tr>
        <th>300</th>
        <td>HDCP 2.0 이상이 참여 하는 경우 콘텐츠를 전달 합니다. 참여 하지 못한 경우 콘텐츠를 전달 하지 않습니다.</td>
        <td>콘텐츠를 전달 하지 않음</td>
        <td>
            <p>
                **HDCP 유형 제한이 정의 되지 않은 경우:** HDCP 2.0 이상이 참여 하는 경우 콘텐츠를 전달 합니다. 참여 하지 못한 경우에는 콘텐츠를 전달 하지 않습니다.
            </p>
            <p>
                **HDCP 유형 제한이 정의 된 경우:** 는 HDCP 2.2 및 콘텐츠 스트림 형식을 1로 설정 하 여 콘텐츠를 전달 합니다. HDCP가 실패 하거나 콘텐츠 스트림 형식이 1로 설정 되지 않은 경우 콘텐츠를 전달 하지 않습니다.
            </p>        
        </td>
    </tr>
    <tr>
        <th>400</th>
        <td rowspan="2" colspan="2">해당 없음\*</td>
        <td rowspan="2">Windows 10은 후속 OPL 값에 관계 없이 압축 된 디지털 비디오 콘텐츠를 출력에 전달 하지 않습니다. 압축 된 디지털 비디오 콘텐츠에 대 한 자세한 내용은 <a href="https://www.microsoft.com/playready/licensing/compliance/">PlayReady 제품에 대 한 준수 규칙</a>을 참조 하세요.</td>
        <td rowspan="2">해당 없음\*</td>
    </tr>
    <tr>
        <th>500</th>
    </tr>
</table>
<br/>

\* 라이선스 서버에서 출력 보호 수준에 대 한 모든 값을 설정할 수 있는 것은 아닙니다. 자세한 내용은 [PlayReady 준수 규칙](https://www.microsoft.com/playready/licensing/compliance/)을 참조하세요.

### <a name="additional-explicit-output-restrictions"></a>추가 명시적 출력 제한 사항

다음 표에서는 명시적인 디지털 비디오 출력 보호 제한 사항에 대 한 Windows 10의 PlayReady DRM 구현을 설명 합니다.

<table>
    <tr>
        <th>시나리오</th>
        <th>GUID</th>
        <th>조건</th>
        <th>결과</th>
    </tr>
    <tr>
        <th>최대 유효 해상도 디코드 크기</th>
        <td>9645E831-E01D-4FFF-8342-0A720E3E028F</td>
        <td>연결 된 출력: 디지털 비디오 출력, Miracast, HDMI, DVI 등</td>
        <td>
            <p>
                다음으로 제한 될 때 콘텐츠를 전달 합니다.  
            </p>
            <ul>
                <li>(a) 프레임의 너비는 최대 프레임 너비 (픽셀 단위)와 프레임 높이 (픽셀 단위) 보다 작거나 같아야 합니다.</li>
                <li>(b) 프레임의 높이는 최대 프레임 너비 (픽셀) 보다 작거나 같고 프레임의 너비는 최대 프레임 높이 (픽셀) 보다 작거나 같아야 합니다.</li>
            </ul>                   
        </td>
    </tr>
    <tr>
        <th>HDCP 유형 제한</th>
        <td>ABB2C6F1-E663-4625-A945-972D17B231E7</td>
        <td>연결 된 출력: 디지털 비디오 출력, Miracast, HDMI, DVI 등</td>
        <td>는 HDCP 2.2 및 콘텐츠 스트림 형식을 1로 설정 하 여 콘텐츠를 전달 합니다. HDCP 2.2에 참여 하지 못하거나 콘텐츠 스트림 형식을 1로 설정할 수 없는 경우 콘텐츠를 전달 하지 않습니다. 271 보다 크거나 같은 값의 압축 되지 않은 디지털 비디오 출력 보호 수준도 지정 해야 합니다.</td>
    </tr>
</table>
<br/>

다음 표에서는 명시적 아날로그 비디오 출력 보호 제한의 Windows 10 구현에 대 한 PlayReady DRM에 대해 설명 합니다.

<table>
    <tr>
        <th>시나리오</th>
        <th>GUID</th>
        <th>조건</th>
        <th colspan="2">결과</th>
    </tr>
    <tr>
        <th>아날로그 컴퓨터 모니터</th>
        <td>D783A191-E083-4BAF-B2DA-E69F910B3772</td>
        <td>연결 된 출력: VGA, DVI &ndash; 아날로그 등</td>
        <td><b>Swdrm:</b> PC는 유효 해상도를 프레임당 52만 window.epx.codesnippet로 제한 하 고 콘텐츠를 전달 합니다.</td>
        <td><b>HWDRM:</b> 콘텐츠를 전달 하지 않음</td>
    </tr>
    <tr>
        <th>아날로그 구성 요소</th>
        <td>811C5110-46C8-4C6E-8163-C0482A15D47E</td>
        <td>연결 된 출력: 구성 요소</td>
        <td><b>Swdrm:</b> PC는 유효 해상도를 프레임당 52만 window.epx.codesnippet로 제한 하 고 콘텐츠를 전달 합니다.</td>
        <td><b>HWDRM:</b> 콘텐츠를 전달 하지 않음</td>
    </tr>
    <tr>
        <th rowspan="2">아날로그 TV 출력</th>
        <td>2098DE8D-7DDD-4BAB-96C6-32EBB6FABEA3</td>
        <td>아날로그 TV OPL이 151 미만</td>
        <td colspan="2">CGMS-A가 관여 해야 합니다.</td>
    </tr>
    <tr>
        <td>225CD36F-F132-49EF-BA8C-C91EA28E4369</td>
        <td>아날로그 TV OPL이 101 보다 작고 라이선스에 2098DE8D-7DDD-4BAB-96C6-32EBB6FABEA3가 포함 되어 있지 않습니다.</td>
        <td colspan="2">CGMS-참여를 시도 해야 하지만 결과에 관계 없이 콘텐츠가 재생 될 수 있습니다.</td>
    </tr>
    <tr>
        <th>자동 게인 컨트롤 및 색 줄무늬</th>
        <td>C3FD11C6-F8B7-4D20-B008-1DB17D61F2DA</td>
        <td>해상도가 52만 px 보다 작거나 같은 콘텐츠를 아날로그 TV 출력으로 전달</td>
        <td colspan="2">확인이 52만 px 미만이 면 component video 및 PAL 모드에 대해서만 AGC를 설정 하 고, 3.5.7.3 테이블에 따라 해상도가 52만 px 보다 작은 경우에는 NTSC에 대해 AGC 및 색 줄무늬 정보를 설정 합니다. 준수 규칙</td>
    </tr>
    <tr>
        <th>디지털 전용 출력</th>
        <td>760AE755-682A-41E0-B1B3-DCDF836A7306</td>
        <td>연결 된 출력이 아날로그입니다.</td>
        <td colspan="2">콘텐츠를 전달 하지 않음</td>
    </tr>
</table>
<br/>

> [!NOTE]
> 재생을 위해 "Mini DisplayPort to VGA"와 같은 어댑터 동글을 사용 하는 경우 Windows 10은 출력을 디지털 비디오 출력으로 표시 하 고 아날로그 비디오 정책을 적용할 수 없습니다.

다음 표에서는 다른 상황에서의 재생을 가능 하 게 하는 Windows 10 용 PlayReady DRM 구현을 설명 합니다.

<table>
    <tr>
        <th>시나리오</th>
        <th>GUID</th>
        <th>조건</th>
        <th colspan="2">결과</th>
    </tr>
    <tr>
        <th>알 수 없는 출력</th>
        <td>786627D8-C2A6-44BE-8F88-08AE255B01A7</td>
        <td>출력을 합리적으로 결정할 수 없는 경우 또는 그래픽 드라이버를 사용 하 여 OPM를 설정할 수 없는 경우</td>
        <td><b>Swdrm:</b> 콘텐츠 전달</td>
        <td><b>HWDRM:</b> 콘텐츠를 전달 하지 않음</td>
    </tr>
    <tr>
        <th>Constriction를 사용 하 여 알 수 없는 출력</th>
        <td>B621D91F-EDCC-4035-8D4B-DC71760D43E9</td>
        <td>출력을 합리적으로 결정할 수 없는 경우 또는 그래픽 드라이버를 사용 하 여 OPM를 설정할 수 없는 경우</td>
        <td><b>Swdrm:</b> PC는 유효 해상도를 프레임당 52만 window.epx.codesnippet로 제한 하 고 콘텐츠를 전달 합니다.</td>
        <td><b>HWDRM:</b> 콘텐츠를 전달 하지 않음</td>
    </tr>
</table>
<br/>

## <a name="prerequisites"></a>사전 요구 사항

PlayReady로 보호 된 UWP 앱 만들기를 시작 하기 전에 시스템에 다음 소프트웨어를 설치 해야 합니다.

-   Windows 10.
-   UWP 앱에 대 한 PlayReady DRM에 대 한 샘플을 컴파일하는 경우 Microsoft Visual Studio 2015 이상 버전을 사용 하 여 예제를 컴파일해야 합니다. Microsoft Visual Studio 2013를 사용 하 여 Windows 8.1 스토어 앱에 대 한 PlayReady DRM의 샘플을 컴파일할 수 있습니다.

<!--This is no longer available-->
<!--If you are planning to play back MPEG-2/H.262 content on your app, you must also download and install [Windows 8.1 Media Center Pack](https://windows.microsoft.com/windows-8/feature-packs).-->

## <a name="playready-uwp-app-migration-guide"></a>PlayReady UWP 앱 마이그레이션 가이드

이 섹션에는 기존 PlayReady Windows 8.x 스토어 앱을 Windows 10으로 마이그레이션하는 방법에 대 한 정보가 포함 되어 있습니다.

Windows 10의 PlayReady UWP 앱에 대 한 네임 스페이스가 **PlayReadyClient** 에서 [**windows**](/uwp/api/Windows.Media.Protection.PlayReady)로 변경 되었습니다. 즉, 이전 네임 스페이스를 검색 하 고 코드의 새 네임 스페이스와 바꾸어야 합니다. 여전히 winmd 파일을 참조합니다. Windows 10 운영 체제에 포함 된 windows의 일부입니다. 이 파일은 TH의 Windows SDK 일부로 서 windows에 있습니다. UWP의 경우 univeralappcontract에서 참조 됩니다.

PlayReady로 보호 된 고화질 (HD) 콘텐츠 (UHD) 콘텐츠를 재생 하려면 PlayReady 하드웨어 DRM을 구현 해야 합니다. PlayReady 하드웨어 DRM을 구현 하는 방법에 대 한 자세한 내용은 [하드웨어 drm](hardware-drm.md)을 참조 하세요.

일부 콘텐츠는 하드웨어 DRM에서 지원 되지 않습니다. 하드웨어 DRM을 사용 하지 않도록 설정 하 고 소프트웨어 DRM을 사용 하도록 설정 하는 방법은 [하드웨어 Drm 재정의](hardware-drm.md#override-hardware-drm)를 참조 하세요.

Media protection manager와 관련 하 여 아직 다음 설정이 없는 경우 코드의 설정이 올바른지 확인 합니다.

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

## <a name="proactively-acquire-a-non-persistent-license-before-playback"></a>재생 전에 비영구 라이선스 사전 확보

이 섹션에서는 재생이 시작 되기 전에 비영구 라이선스를 사전에 확보 하는 방법을 설명 합니다.

이전 버전의 PlayReady DRM에서는 비영구 라이선스를 재생 하는 동안에만 대응적를 가져올 수 있었습니다. 이 버전에서는 재생이 시작 되기 전에 비영구 라이선스를 사전에 확보할 수 있습니다.

1.  비영구 라이선스를 저장할 수 있는 재생 세션을 사전에 만듭니다. 다음은 그 예입니다.

    ```cs
    var cpsystems = new Windows.Foundation.Collections.PropertySet();       
    cpsystems["{F4637010-03C3-42CD-B932-B48ADF3A6A54}"] = "Windows.Media.Protection.PlayReady.PlayReadyWinRTTrustedInput"; // PlayReady

    var pmpSystemInfo = new Windows.Foundation.Collections.PropertySet();
    pmpSystemInfo["Windows.Media.Protection.MediaProtectionSystemId"] = "{F4637010-03C3-42CD-B932-B48ADF3A6A54}";
    pmpSystemInfo["Windows.Media.Protection.MediaProtectionSystemIdMapping"] = cpsystems;
    var pmpServer = new Windows.Media.Protection.MediaProtectionPMPServer( pmpSystemInfo );
    ```

2.  해당 재생 세션을 라이선스 취득 클래스에 연결 합니다. 다음은 그 예입니다.

    ```cs
    var licenseSessionProperties = new Windows.Foundation.Collections.PropertySet();
    licenseSessionProperties["Windows.Media.Protection.MediaProtectionPMPServer"] = pmpServer;
    var licenseSession = new Windows.Media.Protection.PlayReady.PlayReadyLicenseSession( licenseSessionProperties );
    ```

3.  라이선스 서비스 요청을 만듭니다. 다음은 그 예입니다.

    ```cs
    var laSR = licenseSession.CreateLAServiceRequest();
    ```

4.  3 단계에서 만든 서비스 요청을 사용 하 여 라이선스 취득을 수행 합니다. 라이선스는 재생 세션에 저장 됩니다.
5.  재생 세션을 재생을 위해 미디어 원본에 연결 합니다. 다음은 그 예입니다.

    ```cs
    licenseSession.configureMediaProtectionManager( mediaProtectionManager );
    videoPlayer.msSetMediaProtectionManager( mediaProtectionManager );
    ```
    
## <a name="query-for-protection-capabilities"></a>보호 기능에 대 한 쿼리
Windows 10 버전 1703부터 코덱, 해상도 및 출력 보호 (HDCP)와 같은 HW DRM 기능을 쿼리할 수 있습니다. [**IsTypeSupported**](/uwp/api/windows.media.protection.protectioncapabilities.istypesupported) 메서드를 사용 하 여 쿼리를 수행 합니다 .이 메서드는 지원 쿼리 기능을 나타내는 문자열과 쿼리가 적용 되는 키 시스템을 지정 하는 문자열을 사용 합니다. 지원 되는 문자열 값 목록은 [**IsTypeSupported**](/uwp/api/windows.media.protection.protectioncapabilities.istypesupported)에 대 한 API 참조 페이지를 참조 하세요. 다음 코드 예제에서는이 메서드를 사용 하는 방법을 보여 줍니다.  

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

이 섹션에서는 UWP 앱에 보안 중지를 추가 하는 방법을 설명 합니다.

Secure stop은 PlayReady 장치가 지정 된 콘텐츠에 대해 미디어 재생이 중지 되었음을 미디어 스트리밍 서비스에 자신 있게 어설션할 수 있는 수단을 제공 합니다. 이 기능을 통해 미디어 스트리밍 서비스는 지정 된 계정에 대 한 다양 한 장치에 대 한 사용 제한 사항을 정확 하 게 적용 하 고 보고할 있습니다.

보안 중지 챌린지를 전송 하는 두 가지 기본 시나리오는 다음과 같습니다.

-   콘텐츠의 끝에 도달했거나 사용자가 미디어 프레젠테이션을 중간에 중지했으므로 미디어 프레젠테이션이 중지되는 경우.
-   이전 세션이 예기치 않게 종료 된 경우 (예: 시스템 또는 앱 작동 중단으로 인해) 응용 프로그램은 시작 또는 종료 시, 진행 중인 모든 보안 중지 세션 및 다른 미디어 재생과 별도로 챌린지를 전송 하 여 쿼리 해야 합니다.

Secure stop의 샘플 구현을 보려면 [코드 샘플 브라우저](/samples/microsoft/windows-universal-samples/playready/)에 있는 PlayReady 샘플의 **securestop.cs** 파일을 참조 하세요.

## <a name="use-playready-drm-on-xbox-one"></a>Xbox One에서 PlayReady DRM 사용

Xbox One의 UWP 앱에서 PlayReady DRM을 사용 하려면 먼저 PlayReady를 사용 하기 위한 권한 부여를 위해 앱을 게시 하는 데 사용 중인 [파트너 센터](https://partner.microsoft.com/dashboard) 계정을 등록 해야 합니다. 이 작업은 다음 두 가지 방법 중 한 가지로 수행할 수 있습니다.

* Microsoft 요청 권한으로 연락 드립니다.
* 파트너 센터 계정 및 회사 이름을로 전송 하 여 권한 부여에 적용 [pronxbox@microsoft.com](mailto:pronxbox@microsoft.com) 합니다.

권한 부여를 받은 후에는 `<DeviceCapability>` 앱 매니페스트에 추가 해야 합니다. 지금은 응용 프로그램 매니페스트 디자이너에서 사용할 수 있는 설정이 없으므로이를 수동으로 추가 해야 합니다. 구성 하려면 다음 단계를 따르세요.

1. Visual Studio에서 프로젝트를 연 상태에서 **솔루션 탐색기** 를 열고 **appxmanifest.xml** 를 마우스 오른쪽 단추로 클릭 합니다.
2. **연결 프로그램 ...** 을 선택 하 고 **XML (텍스트) 편집기** 를 선택한 다음 **확인** 을 클릭 합니다.
3. 태그 사이에 `<Capabilities>` 다음을 추가 합니다 `<DeviceCapability>` .

    ```xml
    <DeviceCapability Name="6a7e5907-885c-4bcb-b40a-073c067bd3d5" />
    ```

4. 파일을 저장합니다.

마지막으로 Xbox One에서 PlayReady를 사용 하는 경우 마지막으로 고려할 사항이 하나 있습니다. 개발 키트에서는 SL150 제한이 있습니다. 즉, SL2000 또는 SL3000 콘텐츠를 재생할 수 없습니다. 소매점 장치는 더 높은 보안 수준으로 콘텐츠를 재생할 수 있지만 dev kit에서 앱을 테스트 하려면 SL150 콘텐츠를 사용 해야 합니다. 다음 방법 중 하나를 수행 하 여이 콘텐츠를 테스트할 수 있습니다.

* SL150 라이선스를 요구 하는 큐 레이트 테스트 콘텐츠를 사용 합니다.
* 인증 된 특정 테스트 계정만 특정 콘텐츠에 대 한 SL150 라이선스를 얻을 수 있도록 논리를 구현 합니다.

회사 및 제품에 가장 적합 한 방법을 사용 합니다.


## <a name="see-also"></a>참조
- [미디어 재생](media-playback.md)