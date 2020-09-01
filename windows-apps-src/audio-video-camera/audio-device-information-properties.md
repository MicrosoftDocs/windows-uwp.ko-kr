---
ms.assetid: 3b75d881-bdcf-402b-a330-23cd29d68e53
description: 이 문서에는 오디오 디바이스와 관련된 DeviceInformation 속성이 나열되어 있습니다.
title: 오디오 디바이스 정보 속성
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 6181625690b4abddd4fdd4cbd9032bee64b6658d
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89161207"
---
# <a name="audio-device-information-properties"></a>오디오 디바이스 정보 속성

이 문서에는 오디오 장치와 관련 된 장치 정보 속성이 나열 되어 있습니다. Windows에서 각 하드웨어 장치에는 장치에 대 한 특정 정보가 필요 하거나 장치 선택기를 빌드할 때 사용할 수 있는 장치에 대 한 자세한 정보를 제공 하는 장치에 대 한 자세한 정보를 제공 하는 연결 된 [**deviceinformation**](/uwp/api/Windows.Devices.Enumeration.DeviceInformation) 속성이 있습니다. Windows에서 장치를 열거 하는 방법에 대 한 일반적인 정보는 [열거 장치](../devices-sensors/enumerate-devices.md) 및 [장치 정보 속성](../devices-sensors/device-information-properties.md)을 참조 하세요.


|이름|유형|Description|
|------------------------------------------------------------|------------|------------------------------------------------------|
|**SensitivityInDbfs (시스템 장치.**|Double|DBFS (전체 크기 조정) 단위를 기준으로 하 여 마이크 민감도를 데시벨 단위로 지정 합니다.|
|**SignalToNoiseRatioInDb (시스템 장치.**|Double|데시벨 (dB) 단위로 측정 되는 마이크 신호를 SNR (노이즈 비율)로 지정 합니다.|
|**System.object. SpeechProcessingSupported**|부울|오디오 장치가 음성 처리를 지원 하는지 여부를 나타냅니다.|
|**System.object. RawProcessingSupported**|부울|오디오 장치가 원시 처리를 지원 하는지 여부를 나타냅니다.|
|**MicrophoneArray.**|unsigned char []|마이크 배열의 기 하 도형 데이터입니다.|

## <a name="related-topics"></a>관련 항목

* [디바이스 열거](../devices-sensors/enumerate-devices.md)
* [디바이스 정보 속성](../devices-sensors/device-information-properties.md)
* [디바이스 선택기 빌드](../devices-sensors/build-a-device-selector.md)
* [미디어 재생](media-playback.md)