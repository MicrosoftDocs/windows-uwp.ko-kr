---
ms.assetid: 949D1CE0-DD7D-420E-904D-758FADEBE85A
title: 디바이스 기능 사용
description: 이 자습서에서는 Microsoft Visual Studio에서 장치 기능을 선언 하는 방법을 설명 합니다. 그러면 앱이 카메라, 마이크, 위치 센서 및 기타 장치를 사용할 수 있습니다.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 51481e30f3dc2f60bf4483e4a22d48b5df533994
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89159717"
---
# <a name="enable-device-capabilities"></a>디바이스 기능 사용



이 자습서에서는 Microsoft Visual Studio에서 장치 기능을 선언 하는 방법을 설명 합니다. 그러면 앱이 카메라, 마이크, 위치 센서 및 기타 장치를 사용할 수 있습니다.

## <a name="specify-the-device-capabilities-your-app-will-use"></a>앱에서 사용 하는 장치 기능 지정


특정 유형의 장치를 사용 하는 경우 Windows 앱에서 앱 패키지 매니페스트에를 지정 해야 합니다. Visual Studio에서 [매니페스트 디자이너](/previous-versions/br230259(v=vs.140))를 사용하여 대부분의 접근 권한 값을 선언할 수 있으며, [패키지 매니페스트에서 장치 접근 권한 값을 지정하는 방법(수동)](/uwp/schemas/appxpackage/how-to-specify-device-capabilities-in-a-package-manifest)에 설명된 대로 수동으로 해당 접근 권한 값을 추가할 수 있습니다. 이 자습서에서는 매니페스트 디자이너를 사용 하 고 있다고 가정 합니다.

**참고**    프린터, 스캐너 및 센서와 같은 일부 유형의 장치를 앱 패키지 매니페스트에 선언할 필요가 없습니다.

-   Visual Studio 솔루션 탐색기에서 패키지 매니페스트 파일 **appxmanifest.xml**를 두 번 클릭 합니다.
-   **기능** 탭을 엽니다.
-   앱에서 사용 하는 장치 기능을 선택 합니다. 매니페스트 디자이너에서 원하는 기능이 표시 되지 않으면 수동으로 추가 합니다. 자세한 내용은 [패키지 매니페스트에서 장치 기능을 지정 하는 방법](/uwp/schemas/appxpackage/how-to-specify-device-capabilities-in-a-package-manifest)을 참조 하세요.

| 장치 기능 | 매니페스트 디자이너 | Description |
|-------------------|-------------------|-------------|    
| AllJoyn | ![매니페스트 디자이너에서 사용 가능](images/ap-tools.png) | 네트워크의 AllJoyn 지원 앱 및 장치가 서로를 검색 하 고 상호 작용할 수 있도록 허용 합니다. [**Windows. AllJoyn**](/uwp/api/Windows.Devices.AllJoyn) 네임 스페이스의 api에 액세스 하는 앱 앱은이 기능을 사용 해야 합니다. |
| 차단 된 채팅 메시지 | ![매니페스트 디자이너에서 사용 가능](images/ap-tools.png) | 앱이 스팸 필터 앱으로 차단 된 SMS 및 MMS 메시지를 읽을 수 있도록 허용 합니다. |
| 채팅 메시지 액세스 | ![매니페스트 디자이너에서 사용 가능](images/ap-tools.png) | 앱에서 문자 메시지를 읽고 삭제할 수 있습니다. 또한 앱에서 채팅 메시지를 시스템 데이터 저장소에 저장할 수 있습니다. |
| 코드 생성 | ![매니페스트 디자이너에서 사용 가능](images/ap-tools.png) | 앱이 코드를 동적으로 생성할 수 있도록 합니다. |
| 엔터프라이즈 인증 | ![매니페스트 디자이너에서 사용 가능](images/ap-tools.png) | 이 기능은 Microsoft Store 정책이 적용 됩니다. 도메인 자격 증명이 필요한 엔터프라이즈 인트라넷 리소스에 연결 하는 기능을 제공 합니다. 일반적으로 대부분의 앱에는이 기능이 필요 하지 않습니다. | 
| 인터넷(클라이언트) | ![매니페스트 디자이너에서 사용 가능](images/ap-tools.png) | 공항 및 커피숍과 같은 공공 장소의 인터넷 및 네트워크에 대 한 아웃 바운드 액세스를 제공 합니다. 예를 들어 사용자가 네트워크를 공용으로 지정한 인트라넷 네트워크를 사용 합니다. 인터넷 액세스가 필요한 대부분의 앱은이 기능을 사용 해야 합니다. |
| 인터넷 (클라이언트 &amp; 서버) | ![매니페스트 디자이너에서 사용 가능](images/ap-tools.png) | 공항 및 커피숍과 같은 공공 장소의 인터넷 및 네트워크에 대 한 인바운드 및 아웃 바운드 액세스를 제공 합니다. 이 기능은 **인터넷 (클라이언트)** 의 상위 집합입니다. 이 기능을 사용 하도록 설정한 경우에는 **인터넷 (클라이언트)** 을 사용 하도록 설정할 필요가 없습니다. 중요 포트에 대한 인바운드 액세스는 항상 차단됩니다. |
| 위치| ![매니페스트 디자이너에서 사용 가능](images/ap-tools.png) | 현재 위치에 대 한 액세스를 제공 합니다. 이는 PC의 GPS 센서와 같은 전용 하드웨어에서 얻거나 사용 가능한 네트워크 정보에서 파생 됩니다. | 
| 마이크 | ![매니페스트 디자이너에서 사용 가능](images/ap-tools.png) | 마이크의 오디오 피드에 대한 액세스 권한을 제공합니다. 그러면 앱이 연결 된 마이크에서 녹음할 수 있습니다. | 
| 음악 라이브러리 | ![매니페스트 디자이너에서 사용 가능](images/ap-tools.png) | 로컬 PC 및 **홈 그룹** Pc의 **음악 라이브러리** 에서 파일을 추가, 변경 또는 삭제할 수 있는 기능을 제공 합니다. | 
| 개체 3D | ![매니페스트 디자이너에서 사용 가능](images/ap-tools.png) | 사용자의 **3D 개체**에 대 한 프로그래밍 방식의 액세스를 제공 하 여 응용 프로그램이 사용자 개입 없이 라이브러리의 모든 파일을 열거 하 고 액세스할 수 있도록 합니다. 이 기능은 전체 **3D 개체** 라이브러리에 액세스 해야 하는 3d 앱 및 게임에서 일반적으로 사용 됩니다. | 
| 전화 통화 | ![매니페스트 디자이너에서 사용 가능](images/ap-tools.png) | 앱이 장치의 모든 전화선에 액세스 하도록 허용 하 고 다음 기능을 수행 합니다. 전화에 전화를 걸어 사용자에 게 메시지를 표시 하지 않고 시스템 전화 걸기를 표시 합니다. 줄 관련 메타 데이터에 액세스 합니다. 줄 관련 트리거에 액세스 합니다. 사용자가 선택한 스팸 필터 앱을 설정 하 고 차단 목록과 호출 원본 정보를 확인할 수 있습니다. | 
| 사진 라이브러리 | ![매니페스트 디자이너에서 사용 가능](images/ap-tools.png) | 로컬 PC 및 **홈 그룹** PC의 **사진 라이브러리**에서 파일을 추가, 변경 또는 삭제할 수 있는 접근 권한 값을 제공합니다. | 
| 서비스 지점 | ![매니페스트 디자이너에서 사용 가능](images/ap-tools.png) | 서비스 주변 장치 지점에 대 한 액세스를 제공 합니다. 이 기능은 [**Windows. PointOfService**](/uwp/api/Windows.Devices.PointOfService) 네임 스페이스의 api에 액세스 하는 데 필요 합니다. | 
| 개인 네트워크 (클라이언트 &amp; 서버) | ![매니페스트 디자이너에서 사용 가능](images/ap-tools.png) | 인증 된 도메인 컨트롤러가 있거나 사용자가 홈 또는 회사 네트워크로 지정한 인트라넷 네트워크에 대 한 인바운드 및 아웃 바운드 액세스를 제공 합니다. 중요 포트에 대한 인바운드 액세스는 항상 차단됩니다. | 
| 근접성 | ![매니페스트 디자이너에서 사용 가능](images/ap-tools.png) | NFC (근거리 통신)를 통해 PC에 가까이 있는 장치에 연결 하는 기능을 제공 합니다. 근거리-근접 한 장치에서 파일을 보내거나 앱과 통신 하는 데 사용할 수 있습니다. | 
| 이동식 스토리지 | ![매니페스트 디자이너에서 사용 가능](images/ap-tools.png) | 이동식 저장 장치에서 파일을 추가, 변경 또는 삭제할 수 있는 기능을 제공 합니다. 앱은 **파일 형식 연결** 선언을 사용 하 여 매니페스트에 정의 된 이동식 저장소의 파일 형식에만 액세스할 수 있습니다. 앱은 **홈 그룹** pc의 이동식 저장소에 액세스할 수 없습니다. | 
| 공유 사용자 인증서 | ![매니페스트 디자이너에서 사용 가능](images/ap-tools.png) | 이 기능은 Microsoft Store 정책이 적용 됩니다. 사용자 id의 유효성을 검사 하는 데 사용 하는 소프트웨어 및 하드웨어 인증서 (예: 스마트 카드 인증서)에 액세스할 수 있는 기능을 제공 합니다. 런타임에 관련 Api를 호출 하는 경우 사용자는 작업을 수행 해야 합니다 (카드 삽입, 인증서 선택 등). 앱이 **인증서** 선언을 통해 개인 인증서를 포함 하는 경우에는이 기능이 필요 하지 않습니다. | 
| 사용자 계정 정보 | ![매니페스트 디자이너에서 사용 가능](images/ap-tools.png) | 앱에서 사용자 이름 및 그림에 액세스할 수 있는 기능을 제공 합니다. [  **Windows.System.UserProfile**](/uwp/api/Windows.System.UserProfile) 네임스페이스에서 일부 API에 액세스하려면 이 접근 권한 값이 필요합니다. | 
| 비디오 라이브러리 | ![매니페스트 디자이너에서 사용 가능](images/ap-tools.png) | 로컬 PC 및 **홈 그룹** Pc의 **비디오 라이브러리** 에서 파일을 추가, 변경 또는 삭제할 수 있는 기능을 제공 합니다. | 
| VOIP 호출 | ![매니페스트 디자이너에서 사용 가능](images/ap-tools.png) | 앱이 [**Windows. ApplicationModel.**](/uwp/api/Windows.ApplicationModel.Calls) 네임 스페이스에서 api를 호출 하는 VOIP에 액세스할 수 있도록 허용 합니다. | 
| 웹캠 | ![매니페스트 디자이너에서 사용 가능](images/ap-tools.png) | 기본 제공 카메라 또는 연결 된 웹캠 비디오 피드에 대 한 액세스를 제공 합니다. 이렇게 하면 앱에서 스냅숏과 영화를 캡처할 수 있습니다. | 
| USB | | 사용자 지정 USB 장치에 대 한 액세스를 제공 합니다. 이 기능에는 자식 요소가 필요 합니다. 이 기능은 Windows Phone에서 지원 되지 않습니다. | 
| HID (휴먼 인터페이스 장치) | | HID (휴먼 인터페이스 장치)에 대 한 액세스를 제공 합니다. 이 기능에는 자식 요소가 필요 합니다. 자세한 내용은 [HID 장치 기능을 지정 하는 방법](/uwp/schemas/appxpackage/how-to-specify-device-capabilities-for-hid)을 참조 하세요. | 
| Bluetooth GATT | | 기본 서비스, 포함 된 서비스, 특성 및 설명자의 컬렉션을 통해 Bluetooth LE 장치에 대 한 액세스를 제공 합니다. 이 기능에는 자식 요소가 필요 합니다. 자세한 내용은 [Bluetooth의 장치 기능을 지정 하는 방법](/uwp/schemas/appxpackage/how-to-specify-device-capabilities-for-bluetooth)을 참조 하세요. | 
| Bluetooth RFCOMM |  | Basic Rate/Extended Data Rate (BR/EDR) 전송을 지 원하는 Api에 대 한 액세스를 제공 하 고 UWP 앱이 직렬 포트 프로필 (SPP)을 구현 하는 장치에 액세스할 수 있도록 합니다. 이 기능에는 자식 요소가 필요 합니다. 자세한 내용은 [Bluetooth의 장치 기능을 지정 하는 방법](/uwp/schemas/appxpackage/how-to-specify-device-capabilities-for-bluetooth)을 참조 하세요. |

## <a name="use-the-windows-runtime-api-for-communicating-with-your-device"></a>장치와 통신 하는 데 Windows 런타임 API 사용

다음 표에서는 일부 기능을 Windows 런타임 Api에 연결 합니다.

| 장치 기능        | API             | 
|--------------------------|-----------------|
| AllJoyn                  | [**Windows. Devices**](/uwp/api/Windows.Devices.AllJoyn) | 
| 차단 된 채팅 메시지    | [**CommunicationBlocking**](/uwp/api/Windows.ApplicationModel.CommunicationBlocking) | 
| 위치                 | 자세한 내용은 [맵 및 위치 개요](../maps-and-location/index.md) 를 참조 하세요. | 
| 전화 통화               | [**Windows ApplicationModel. 호출**](/uwp/api/Windows.ApplicationModel.Calls) | 
| 사용자 계정 정보 | [** Tem를Windows.Sys합니다. UserProfile**](/uwp/api/Windows.System.UserProfile) | 
| VOIP 호출             | [**Windows ApplicationModel. 호출**](/uwp/api/Windows.ApplicationModel.Calls) | 
| USB                      | [**Windows. Usb**](/uwp/api/Windows.Devices.Usb) | 
| 숨겼습니다                      | [**HumanInterfaceDevice**](/uwp/api/Windows.Devices.HumanInterfaceDevice) | 
| Bluetooth GATT           | [**Windows..**](/uwp/api/Windows.Devices.Bluetooth.GenericAttributeProfile) | 
| Bluetooth RFCOMM         | [**Rfcomm.**](/uwp/api/Windows.Devices.Bluetooth.Rfcomm) | 
| 서비스 지점         | [**Windows.Devices.PointOfService**](/uwp/api/Windows.Devices.PointOfService) |