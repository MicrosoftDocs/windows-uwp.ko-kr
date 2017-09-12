---
author: PatrickFarley
ms.assetid: A5320094-DF53-42FC-A6BA-A958F8E9210B
title: "Visual Studio를 사용하여 Surface Hub 앱 테스트"
description: "Visual Studio 시뮬레이터는 Surface Hub용으로 빌드된 앱을 포함하여 UWP 앱을 디자인, 개발, 디버그 및 테스트하는 환경을 제공합니다."
ms.author: pafarley
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: 859c28d59e3c289ac3cb7151f0b9e396d52546c3
ms.sourcegitcommit: e8cc657d85566768a6efb7cd972ebf64c25e0628
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/26/2017
---
# <a name="test-surface-hub-apps-using-visual-studio"></a>Visual Studio를 사용하여 Surface Hub 앱 테스트
Visual Studio 시뮬레이터는 Microsoft Surface Hub용으로 빌드한 앱을 포함하여 UWP(유니버설 Windows 플랫폼) 앱을 디자인, 개발, 디버그 및 테스트할 수 있는 환경을 제공합니다. 시뮬레이터는 Surface Hub와 동일한 사용자 인터페이스를 사용하지 않지만 Surface Hub의 화면 크기와 해상도에서 앱의 모양과 동작을 테스트하는 데 유용합니다.

자세한 내용은 [시뮬레이터에서 Windows 스토어 앱 실행](https://msdn.microsoft.com/library/hh441475.aspx)을 참조하세요.

## <a name="add-surface-hub-resolutions-to-the-simulator"></a>시뮬레이터에 Surface Hub 해상도 추가
시뮬레이터에 Surface Hub 해상도를 추가하려면

1. **HardwareConfigurations SurfaceHub55.xml**이라는 파일에 다음 XML을 저장하여 55" Surface Hub에 대한 구성을 만듭니다.  

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <ArrayOfHardwareConfiguration xmlns:xsd="http://www.w3.org/2001/XMLSchema"
                                  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <HardwareConfiguration>
            <Name>SurfaceHub55</Name>
            <DisplayName>Surface Hub 55"</DisplayName>
            <Resolution>
                <Height>1080</Height>
                <Width>1920</Width>
            </Resolution>
            <DeviceSize>55</DeviceSize>
            <DeviceScaleFactor>100</DeviceScaleFactor>
        </HardwareConfiguration>
    </ArrayOfHardwareConfiguration>
    ```

2. **HardwareConfigurations SurfaceHub84.xml**이라는 파일에 다음 XML을 저장하여 84" Surface Hub에 대한 구성을 만듭니다.

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <ArrayOfHardwareConfiguration xmlns:xsd="http://www.w3.org/2001/XMLSchema"
                                  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <HardwareConfiguration>
            <Name>SurfaceHub84</Name>
            <DisplayName>Surface Hub 84"</DisplayName>
            <Resolution>
                <Height>2160</Height>
                <Width>3840</Width>
            </Resolution>
            <DeviceSize>84</DeviceSize>
            <DeviceScaleFactor>150</DeviceScaleFactor>
        </HardwareConfiguration>
    </ArrayOfHardwareConfiguration>
    ```

3. 두 개의 XML 파일을 **C:\Program Files (x86)\Common Files\Microsoft Shared\Windows Simulator\\&lt;버전 번호&gt;\HardwareConfigurations**에 복사합니다.

   > **참고**&nbsp;&nbsp;이 폴더에 파일을 저장하려면 관리자 권한이 필요합니다.

4. Visual Studio 시뮬레이터에서 앱을 실행합니다. 팔레트에서 **해상도 변경** 단추를 클릭하고 목록에서 Surface Hub 구성을 선택합니다.

    ![Visual Studio 시뮬레이터 해상도](images/vs-simulator-resolutions.png)

   > **팁**&nbsp;&nbsp;Surface Hub의 환경을 더 효과적으로 시뮬레이트하려면 [태블릿 모드를 켭니다](http://windows.microsoft.com/windows-10/getstarted-like-a-tablet).

## <a name="deploy-apps-to-a-surface-hub-from-visual-studio"></a>Visual Studio에서 Surface Hub에 앱 배포
수동으로 앱 배포는 간단한 프로세스입니다.

### <a name="enable-developer-mode"></a>개발자 모드 사용
기본적으로 Surface Hub는 Windows 스토어 앱만 설치합니다. 다른 출처에서 서명한 앱을 설치하려면 개발자 모드를 사용하도록 설정해야 합니다.

> **참고**&nbsp;&nbsp;개발자 모드를 사용하도록 설정한 후 다시 사용하지 않도록 설정하려면 Surface Hub를 초기화해야 합니다. 디바이스를 초기화하면 모든 로컬 사용자 파일과 구성이 제거되고 Windows가 다시 설치됩니다.

1. Surface Hub의 **시작** 메뉴에서 설정 앱을 엽니다.

   >  **참고**&nbsp;&nbsp;설정 앱에 액세스하려면 관리자 권한이 필요합니다.

2. **업데이트 및 보안 &gt; 개발자용**으로 이동합니다.

3. **개발자 모드**를 선택하고 경고 프롬프트를 수락합니다.

### <a name="deploy-your-app-from-visual-studio"></a>Visual Studio에서 앱 배포
자세한 내용은 [UWP(유니버설 Windows 플랫폼) 앱 배포 및 디버그](https://msdn.microsoft.com/windows/uwp/debug-test-perf/deploying-and-debugging-uwp-apps)를 참조하세요.

   > **참고**&nbsp;&nbsp;이 기능을 사용하려면 **Visual Studio 2015 업데이트 1** 이상이 필요합니다.

1. **디버깅 시작** 단추 옆에 있는 디버그 대상 드롭다운으로 이동하여 **원격 컴퓨터**를 선택합니다.

    <!--lcap: in your screenshot, you have local machine selected-->

   ![Visual Studio 디버그 대상 드롭다운](images/vs-debug-target.png)

2. Surface Hub의 IP 주소를 입력합니다. **유니버설** 인증 모드가 선택되었는지 확인합니다.

   > **팁**&nbsp;&nbsp;개발자 모드를 사용하도록 설정한 후 시작 화면에서 Surface Hub의 IP 주소를 확인할 수 있습니다.

3. **디버깅 시작(F5)**을 선택하여 Surface Hub에 앱을 배포하고 디버그하거나 Ctrl+F5를 눌러 앱 배포만 수행합니다.

   > **팁**&nbsp;&nbsp;Surface Hub가 시작 화면에 있으면 아무 단추나 선택하여 해제합니다.
