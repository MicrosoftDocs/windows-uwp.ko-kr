---
title: 앱 패키지 아키텍처
description: UWP 앱 패키지를 빌드할 때 사용해야 할 프로세스 아키텍처에 대해 알아봅니다.
ms.date: 07/13/2017
ms.topic: article
keywords: windows 10, uwp, 패키징, 아키텍처, 패키지 구성
ms.localizationpriority: medium
ms.openlocfilehash: 338dac1d43e08257fa00b51c0c311a090f3d95c0
ms.sourcegitcommit: 079801609165bc7eb69670d771a05bffe236d483
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2019
ms.locfileid: "9116112"
---
# <a name="app-package-architectures"></a>앱 패키지 아키텍처

앱 패키지를 특정 프로세스 아키텍처에서 실행되도록 구성합니다. 아키텍처를 선택하여 앱을 실행시키고 싶은 장치를 지정합니다. UWP(Universal Windows Platform) 앱은 다음 아키텍처에서 실행되도록 구성할 수 있습니다.
- x86
- x64
- ARM
- ARM64

모든 대상 아키텍처를 대상으로 앱을 빌드하는 것이 **아주** 좋습니다. 장치 아키텍처 선택을 해제, 앱이 실행될 수 있는 장치의 수를 제한합니다. 이는 앱을 사용할 수 있는 사람의 수를 제한하는 역할을 합니다.

## <a name="windows-10-devices-and-architectures"></a>Windows 10 장치와 아키텍처

> [!div class="mx-tableFixed"]
| UWP 아키텍처 | 데스크톱(x86)      | 데스크톱(x64)      | 데스크톱(ARM)      | 모바일             | Windows Mixed Reality 및 HoloLens           | Xbox               | IoT Core(장치에 따라 다름) | Surface Hub        |
|------------------|--------------------|--------------------|--------------------|--------------------|--------------------|--------------------|-----------------------------|--------------------|
| x86              | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :x:                | :heavy_check_mark: | :x:                | :heavy_check_mark:          | :heavy_check_mark: |
| x64              | :x:                | :heavy_check_mark: | :x:                | :x:                | :x:                | :heavy_check_mark: | :heavy_check_mark:          | :heavy_check_mark: |
| ARM 및 ARM64              | :x:                | :x:                | :heavy_check_mark: | :heavy_check_mark: | :x:                | :x:                | :heavy_check_mark:          | :x:                |


이러한 아키텍처에 대해 더 자세히 알아보겠습니다.

### <a name="x86"></a>x86
x86이 앱 패키지에 가장 안전한 구성입니다. 거의 모든 장치에서 실행될 것이기 때문입니다. 일부 장치의 경우, x86 구성의 앱 패키지가 실행되지 않을 수 있습니다. Xbox나 일부 IoT Core 장치가 여기에 해당됩니다. 그러나 PC는 x86 패키지가 가장 안전한 선택이며, 가장 많은 장치에 배포되어 있습니다. Windows 10 장치 중 상당수는 앞으로도 계속 Windows x86 버전을 실행할 것입니다.

### <a name="x64"></a>x64
x86 구성보다 적게 사용되는 구성입니다. Windows 10 64비트 버전을 사용하는 데스크톱, [Xbox의 UWP 앱](https://docs.microsoft.com/windows/uwp/xbox-apps/system-resource-allocation), Intel Joule의 Windows 10 IoT Core에 및 Windows 10 IoT Core에 예약되어 있습니다.

### <a name="arm-and-arm64"></a>ARM 및 ARM64
ARM 구성의 Windows 10은 데스크톱 PC, 모바일 장치, 일부 IoT Core 장치(Rasperry Pi 2, Raspberry Pi 3 및 DragonBoard)를 예로 들 수 있습니다. Windows 패밀리에 ARM 기반의 Windows 10 데스크톱 PC가 추가되었습니다. UWP 앱 개발자라면 이러한 PC에 최고의 환경을 제공하기 위해 스토어에 ARM 패키지를 제출해야 합니다.

>[!NOTE]
> 기본적으로 ARM64 플랫폼을 대상으로 UWP 응용 프로그램을 빌드하려면 Visual Studio 2017 버전 15.9 이상 있어야 합니다. 자세한 내용은 [이 블로그 게시물](https://blogs.windows.com/buildingapps/2018/11/15/official-support-for-windows-10-on-arm-development)을 참조 하세요.

자세한 내용은 [arm 기반 Windows 10](../porting/apps-on-arm.md)을 참조 하세요. [ARM 기반 Windows 10](https://channel9.msdn.com/Events/Build/2017/P4171) 데모를 보려면 여기 //Build 대화를 확인한 후, 작동 방식에 대해 더 자세히 알아봅니다.

IoT에 특정 적인 주제에 대 한 자세한 내용은 [Visual Studio를 사용 하 여 앱 배포](https://developer.microsoft.com/windows/iot/Docs/AppDeployment)를 참조 하세요.
