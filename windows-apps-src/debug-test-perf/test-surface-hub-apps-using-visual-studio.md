---
ms.assetid: A5320094-DF53-42FC-A6BA-A958F8E9210B
title: Visual Studio를 사용하여 Surface Hub 앱 테스트
description: Visual Studio 시뮬레이터는 Surface Hub용으로 빌드된 앱을 포함하여 UWP 앱을 디자인, 개발, 디버그 및 테스트하는 환경을 제공합니다.
ms.date: 10/26/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: b40fd56a85be6dce441324a427790cda28f9d7ac
ms.sourcegitcommit: 681c70f964210ab49ac5d06357ae96505bb78741
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/26/2018
ms.locfileid: "7719237"
---
# <a name="test-surface-hub-apps-using-visual-studio"></a>Visual Studio를 사용하여 Surface Hub 앱 테스트
Visual Studio 시뮬레이터는 Microsoft Surface Hub용으로 빌드한 앱을 포함하여 UWP(유니버설 Windows 플랫폼) 앱을 디자인, 개발, 디버그 및 테스트할 수 있는 환경을 제공합니다. 시뮬레이터 Surface Hub와 동일한 사용자 인터페이스 사용 하지 않지만 앱의 모양 및 Surface Hub의 화면 크기와 해상도 사용 하 여이 동작을 테스트 하는 데 유용 합니다.

시뮬레이터 도구에 대 한 자세한 내용은 일반적으로 [시뮬레이터에서 실행 하는 UWP 앱](https://docs.microsoft.com/visualstudio/debugger/run-windows-store-apps-in-the-simulator)을 참조 하세요.

## <a name="add-surface-hub-resolutions-to-the-simulator"></a>시뮬레이터에 Surface Hub 해상도 추가
시뮬레이터에 Surface Hub 해상도를 추가하려면

1. 55"에 대 한 구성을 만듭니다 *hardwareconfigurations SurfaceHub55.xml*이라는 파일에 다음 XML 코드를 저장 하 여 Surface Hub 합니다.  

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

2. 84"에 대 한 구성을 만듭니다 *hardwareconfigurations SurfaceHub84.xml*이라는 파일에 다음 XML 코드를 저장 하 여 Surface Hub 합니다.

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

3. 두 개의 XML 파일을 *C:\Program Files (x86)\Common Files\Microsoft Shared\Windows Simulator\\&lt;버전 번호&gt;\HardwareConfigurations*에 복사합니다.

   > [!NOTE]
   > 이 폴더에 파일을 저장 하려면 관리자 권한이 필요 합니다.

4. Visual Studio 시뮬레이터에서 앱을 실행합니다. 팔레트에서 **해상도 변경** 단추를 클릭하고 목록에서 Surface Hub 구성을 선택합니다.

    ![Visual Studio 시뮬레이터 해상도](images/vs-simulator-resolutions.png)

   > [!TIP]
   > Surface Hub의 환경을 시뮬레이션할 보다 [태블릿 모드를 켭니다](http://windows.microsoft.com/windows-10/getstarted-like-a-tablet) .

## <a name="deploy-apps-to-a-surface-hub-device-from-visual-studio"></a>Visual Studio에서 Surface Hub 디바이스에 앱 배포
수동으로 Surface Hub에 앱을 배포는 간단한 프로세스입니다.

### <a name="enable-developer-mode"></a>개발자 모드 사용
기본적으로 Surface Hub에서 Microsoft Store 앱만 설치합니다. 다른 출처에서 서명한 앱을 설치하려면 개발자 모드를 사용하도록 설정해야 합니다.

> [!NOTE]
> 개발자 모드를 설정한 후 다시 비활성화 하려는 경우 Surface Hub를 초기화 해야 합니다. 디바이스를 초기화하면 모든 로컬 사용자 파일과 구성이 제거되고 Windows가 다시 설치됩니다.

1. Surface Hub의 **시작** 메뉴에서 설정 앱을 엽니다.

   > [!NOTE]
   > Surface Hub에서 설정 앱에 액세스 하려면 관리자 권한이 필요 합니다.

2. 이동 **업데이트 및 보안 \ > 개발자를 위한**합니다.

3. **개발자 모드**를 선택하고 경고 프롬프트를 수락합니다.

### <a name="deploy-your-app-from-visual-studio"></a>Visual Studio에서 앱 배포
배포 프로세스에 대 한 자세한 내용은 일반적으로 [UWP 앱 배포 및 디버깅](https://msdn.microsoft.com/windows/uwp/debug-test-perf/deploying-and-debugging-uwp-apps)을 참조 합니다.

   > [!NOTE]
   > 하지만이 기능은 최신 가장 최신 버전의 Visual Studio를 사용 하는 것이 좋습니다 Visual Studio 2015 업데이트 1 이상이 필요 합니다. 최신 Visual Studio 인스턴스는 모든 최신 개발 하 고 보안 업데이트 gibe 됩니다.

1. **디버깅 시작** 단추 옆에 있는 디버그 대상 드롭다운으로 이동하여 **원격 컴퓨터**를 선택합니다.

    <!--lcap: in your screenshot, you have local machine selected-->

   ![Visual Studio 디버그 대상 드롭다운](images/vs-debug-target.png)

2. Surface Hub의 IP 주소를 입력합니다. **유니버설** 인증 모드가 선택되었는지 확인합니다.

   > [!TIP] 
   > 개발자 모드를 사용 하도록 설정한 후 시작 화면에 Surface Hub의 IP 주소를 찾을 수 있습니다.

3. 선택 **디버깅 시작 (F5)을** 배포 하 고 Surface Hub에서 앱을 디버그 하거나 Ctrl + f 5를 눌러 앱 배포만 합니다.

   > [!TIP]
   > Surface Hub 시작 화면을 표시 하는 경우 아무 단추나 선택 하 여 해제 합니다.
