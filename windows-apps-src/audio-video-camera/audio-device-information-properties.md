---
author: drewbatgit
ms.assetid: 3b75d881-bdcf-402b-a330-23cd29d68e53
description: 이 문서에는 오디오 디바이스와 관련된 DeviceInformation 속성이 나열되어 있습니다.
title: 오디오 장치 정보 속성
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: d612a259785c8ba29ab975ba910c4f3e7df61d6f
ms.sourcegitcommit: 4d88adfaf544a3dab05f4660e2f59bbe60311c00
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/13/2018
ms.locfileid: "6454313"
---
# <a name="audio-device-information-properties"></a>오디오 장치 정보 속성

이 문서에는 오디오 디바이스와 관련된 디바이스 정보 속성이 나열되어 있습니다. Windows에서 각 하드웨어 디바이스에는 디바이스에 대한 특정 정보가 필요하거나 디바이스 선택기를 빌드할 때 사용할 수 있는 디바이스에 대한 자세한 정보를 제공하는 [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/BR225393) 속성이 연결되어 있습니다. Windows에서 디바이스를 열거하는 방법에 대한 일반적인 내용은 [디바이스 열거](../devices-sensors/enumerate-devices.md) 및 [디바이스 정보 속성](../devices-sensors/device-information-properties.md)을 참조하세요.


|이름|유형|설명|
|------------------------------------------------------------|------------|------------------------------------------------------|
|**System.Devices.AudioDevice.Microphone.SensitivityInDbfs**|이중|마이크 민감도를 전체 범위(dBFS) 단위를 기준으로 데시벨로 지정합니다.|
|**System.Devices.AudioDevice.Microphone.SignalToNoiseRatioInDb**|이중|데시벨(dB) 단위로 측정되는 마이크 SNR(신호-잡음 비율)을 지정합니다.|
|**System.Devices.AudioDevice.SpeechProcessingSupported**|부울|오디오 디바이스가 음성 처리를 지원하는지 여부를 나타냅니다.|
|**System.Devices.AudioDevice.RawProcessingSupported**|부울|오디오 디바이스가 원시 처리를 지원하는지 여부를 나타냅니다.|
|**System.Devices.MicrophoneArray.Geometry**|부호 없는 문자[]|마이크 배열에 대한 기하 도형 데이터입니다.|

## <a name="related-topics"></a>관련 항목

* [디바이스 열거](../devices-sensors/enumerate-devices.md)
* [디바이스 정보 속성](../devices-sensors/device-information-properties.md)
* [디바이스 선택기 빌드](../devices-sensors/build-a-device-selector.md)
* [미디어 재생](media-playback.md)




