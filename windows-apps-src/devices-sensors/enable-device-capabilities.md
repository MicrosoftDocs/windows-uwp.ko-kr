---
author: muhsinking
ms.assetid: 949D1CE0-DD7D-420E-904D-758FADEBE85A
title: 장치 기능 사용
description: 이 자습서에서는 Microsoft Visual Studio에서 디바이스 접근 권한 값을 선언하는 방법을 설명합니다. 이를 통해 앱에서 카메라, 마이크, 위치 센서 및 기타 디바이스를 사용할 수 있습니다.
ms.author: mukin
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: a7250c41795373b089f7a4c76b603c169b1e4dc3
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/21/2018
ms.locfileid: "7440600"
---
# <a name="enable-device-capabilities"></a>디바이스 기능 사용



이 자습서에서는 Microsoft Visual Studio에서 디바이스 접근 권한 값을 선언하는 방법을 설명합니다. 이를 통해 앱에서 카메라, 마이크, 위치 센서 및 기타 디바이스를 사용할 수 있습니다.

## <a name="specify-the-device-capabilities-your-app-will-use"></a>앱에서 사용할 장치 접근 권한 값 지정


Windows 앱에서는 특정 유형의 장치를 사용할 경우 앱 패키지 매니페스트에 지정해야 합니다. Visual Studio에서 [매니페스트 디자이너](https://msdn.microsoft.com/library/windows/apps/xaml/br230259.aspx)를 사용하여 대부분의 접근 권한 값을 선언할 수 있으며, [패키지 매니페스트에서 장치 접근 권한 값을 지정하는 방법(수동)](https://msdn.microsoft.com/library/windows/apps/Dn263092)에 설명된 대로 수동으로 해당 접근 권한 값을 추가할 수 있습니다. 이 자습서에서는 매니페스트 디자이너를 사용한다고 가정합니다.

**참고**  장치 프린터, 스캐너 및 센터와 같은 일부 유형의 앱 패키지 매니페스트에서 선언할 필요가 없습니다.

-   Visual Studio 솔루션 탐색기에서 패키지 매니페스트 파일 **Package.appxmanifest**을 두 번 클릭합니다.
-   **접근 권한 값** 탭을 엽니다.
-   앱에서 사용하는 장치 접근 권한 값을 선택합니다. 매니페스트 디자이너에서 찾고 있는 접근 권한 값이 보이지 않으면 수동으로 추가 합니다. 자세한 내용은 [패키지 매니페스트에 장치 접근 권한 값을 지정하는 방법](https://msdn.microsoft.com/library/windows/apps/Dn263092)을 참조하세요.

| 장치 접근 권한 값 | 매니페스트 디자이너 | 설명 |
|-------------------|-------------------|-------------|    
| AllJoyn | ![매니페스트 디자이너에서 사용 가능](images/ap-tools.png) | 네트워크의 AllJoyn 사용 앱 및 장치가 서로를 검색하고 상호 작용할 수 있도록 허용합니다. [**Windows.Devices.AllJoyn**](https://msdn.microsoft.com/library/windows/apps/Dn894971) 네임스페이스의 API에 액세스하는 모든 앱은 이 접근 권한 값을 사용해야 합니다. |
| 차단된 채팅 메시지 | ![매니페스트 디자이너에서 사용 가능](images/ap-tools.png) | 앱에서 스팸 필터 앱에 의해 차단된 SMS 및 MMS 메시지를 읽을 수 있도록 허용합니다. |
| 채팅 메시지 액세스 | ![매니페스트 디자이너에서 사용 가능](images/ap-tools.png) | 앱에서 문자 메시지를 읽고 삭제할 수 있도록 허용합니다. 또한 앱에서 시스템 데이터 저장소의 채팅 메시지를 저장할 수 있도록 허용합니다. |
| 코드 생성 | ![매니페스트 디자이너에서 사용 가능](images/ap-tools.png) | 앱에서 코드를 동적으로 생성할 수 있도록 허용합니다. |
| 엔터프라이즈 인증 | ![매니페스트 디자이너에서 사용 가능](images/ap-tools.png) | 이 접근 권한 값은 Microsoft Store 정책의 영향을 받습니다. 도메인 자격 증명이 필요한 엔터프라이즈 인트라넷 리소스에 연결하는 접근 권한 값을 제공합니다. 이 접근 권한 값은 일반적으로 대부분의 앱에 필요하지 않습니다. | 
| 인터넷(클라이언트) | ![매니페스트 디자이너에서 사용 가능](images/ap-tools.png) | 인터넷 및 공공 장소(예: 공항, 커피숍)의 네트워크에 대한 아웃바운드 액세스를 제공합니다. 사용자가 네트워크를 공용으로 지정한 인트라넷 네트워크를 예로 들 수 있습니다. 인터넷 액세스가 필요한 대부분의 앱은 이 접근 권한 값을 사용해야 합니다. |
| 인터넷(클라이언트 &amp; 서버) | ![매니페스트 디자이너에서 사용 가능](images/ap-tools.png) | 인터넷 및 공공 장소(예: 공항, 커피숍)의 네트워크에 대한 인바운드 및 아웃바운드 액세스를 제공합니다. 이 접근 권한 값은 **Internet (Client)** 의 상위 집합입니다. 이 접근 권한 값도 사용할 수 있는 경우 **Internet (Client)** 을 사용할 필요가 없습니다. 중요 포트에 대한 인바운드 액세스는 항상 차단됩니다. |
| 위치| ![매니페스트 디자이너에서 사용 가능](images/ap-tools.png) | 현재 위치에 대한 액세스를 제공합니다. PC의 GPS 센서 등의 전용 하드웨어에서 가져왔거나 사용 가능한 네트워크 정보에서 파생된 것입니다. | 
| 마이크 | ![매니페스트 디자이너에서 사용 가능](images/ap-tools.png) | 마이크의 오디오 피드에 대한 액세스를 제공합니다. 이를 통해 앱이 연결된 마이크에서 녹음할 수 있습니다. | 
| 음악 라이브러리 | ![매니페스트 디자이너에서 사용 가능](images/ap-tools.png) | 로컬 PC 및 **홈 그룹** PC의 **음악 라이브러리**에서 파일을 추가, 변경 또는 삭제할 수 있는 접근 권한 값을 제공합니다. | 
| 개체 3D | ![매니페스트 디자이너에서 사용 가능](images/ap-tools.png) | 사용자의 **3D 개체**에 프로그래밍 방식으로 액세스할 수 권한을 제공하므로 앱이 사용자 조작 없이 라이브러리의 모든 파일을 열거하고 액세스할 수 있습니다. 이 접근 권한 값은 일반적으로 전체 **3D 개체** 라이브러리에 액세스해야 하는 3D 앱과 게임에서 사용됩니다. | 
| 전화 통화 | ![매니페스트 디자이너에서 사용 가능](images/ap-tools.png) | 앱이 장치에 있는 모든 전화선에 액세스하여 여러 가지 기능(사용자에게 메시지를 표시하지 않고 전화선에서 전화를 걸고 시스템 전화 걸기 표시, 회선 관리 메타데이터 액세스, 회선 관리 트리거 액세스)을 수행할 수 있도록 허용합니다. 사용자가 선택한 스팸 필터 앱에서 차단 목록 및 통화 출처 정보를 설정하고 확인하도록 허용합니다. | 
| 사진 라이브러리 | ![매니페스트 디자이너에서 사용 가능](images/ap-tools.png) | 로컬 PC 및 **홈 그룹** PC의 **사진 라이브러리**에서 파일을 추가, 변경 또는 삭제할 수 있는 접근 권한 값을 제공합니다. | 
| 서비스 지점 | ![매니페스트 디자이너에서 사용 가능](images/ap-tools.png) | 서비스 지점 주변 장치에 대한 액세스를 제공합니다. 이 접근 권한 값은 [**Windows.Devices.PointOfService**](https://docs.microsoft.com/uwp/api/Windows.Devices.PointOfService) 네임스페이스의 API에 액세스하는 데 필요합니다. | 
| 개인 네트워크(클라이언트 &amp; 서버) | ![매니페스트 디자이너에서 사용 가능](images/ap-tools.png) | 인증된 도메인 컨트롤러가 있거나 사용자가 가정용 또는 작업용 네트워크로 지정한 인트라넷 네트워크에 대한 인바운드 및 아웃바운드 액세스를 제공합니다. 중요한 포트에 대한 인바운드 액세스는 항상 차단됩니다. | 
| 근접 | ![매니페스트 디자이너에서 사용 가능](images/ap-tools.png) | NFC(근거리 통신)를 통해 PC에 가까이 있는 장치에 연결할 수 있는 접근 권한 값을 제공합니다. 근거리 근접 연결을 사용하여 가까운 장치의 앱과 통신하거나 파일을 보낼 수 있습니다. | 
| 이동식 저장소 | ![매니페스트 디자이너에서 사용 가능](images/ap-tools.png) | 이동식 저장 장치에서 파일을 추가, 변경 또는 삭제할 수 있는 접근 권한 값을 제공합니다. 앱은 이동식 저장소에서 **파일 형식 연결** 선언을 사용하는 매니페스트에서 정의된 파일 형식에만 액세스할 수 있습니다. 앱은 **홈 그룹** PC의 이동식 저장소에는 액세스할 수 없습니다. | 
| 공유 사용자 인증서 | ![매니페스트 디자이너에서 사용 가능](images/ap-tools.png) | 이 접근 권한 값은 Microsoft Store 정책의 영향을 받습니다. 스마트 카드 인증서와 같이 사용자의 ID를 확인하기 위한 소프트웨어 및 하드웨어 인증서에 액세스할 수 있는 접근 권한 값을 제공합니다. 런타임에 연결된 API가 호출되면 카드를 삽입하거나 인증서를 선택하는 등의 사용자 작업이 필요합니다. 앱에 **Certificates** 선언을 통한 개인 인증서가 있는 경우에는 이 접근 권한 값이 필요하지 않습니다. | 
| 사용자 계정 정보 | ![매니페스트 디자이너에서 사용 가능](images/ap-tools.png) | 앱에서 사용자의 이름 및 사진에 액세스할 수 있도록 합니다. [**Windows.System.UserProfile**](https://msdn.microsoft.com/library/windows/apps/BR241881) 네임스페이스에서 일부 API에 액세스하려면 이 접근 권한 값이 필요합니다. | 
| 비디오 라이브러리 | ![매니페스트 디자이너에서 사용 가능](images/ap-tools.png) | 로컬 PC 및 **홈 그룹** PC의 **비디오 라이브러리**에서 파일을 추가, 변경 또는 삭제할 수 있는 접근 권한 값을 제공합니다. | 
| VOIP 호출 | ![매니페스트 디자이너에서 사용 가능](images/ap-tools.png) | 앱이 [**Windows.ApplicationModel.Calls**](https://msdn.microsoft.com/library/windows/apps/Dn297266) 네임스페이스에서 API를 호출하는 VOIP에 액세스할 수 있도록 허용합니다. | 
| Webcam | ![매니페스트 디자이너에서 사용 가능](images/ap-tools.png) | 기본 제공 카메라 또는 연결된 webcam 화상 대화에 액세스할 수 있게 해줍니다. 이를 통해 앱에서 스냅숏과 동영상을 캡처할 수 있습니다. | 
| USB | | 사용자 지정 USB 장치에 대한 액세스를 제공합니다. 이 접근 권한 값에는 자식 요소가 필요합니다. Windows Phone에서는 이 기능이 지원되지 않습니다. | 
| HID(휴먼 인터페이스 장치) | | HID(휴먼 인터페이스 장치)에 대한 액세스를 제공합니다. 이 접근 권한 값에는 자식 요소가 필요합니다. 자세한 내용은 [HID 관련 장치 접근 권한 값을 지정하는 방법](https://msdn.microsoft.com/library/windows/apps/Dn263091)을 참조하세요. | 
| Bluetooth GATT | | 주 서비스, 포함된 서비스, 특징 및 설명자의 컬렉션을 통해 Bluetooth LE 장치에 대한 액세스를 제공합니다. 이 접근 권한 값에는 자식 요소가 필요합니다. 자세한 내용은 [Bluetooth 관련 장치 접근 권한 값을 지정하는 방법](https://msdn.microsoft.com/library/windows/apps/Dn263090)을 참조하세요. | 
| Bluetooth RFCOMM |  | BR/EDR(기본 속도/확장된 데이터 속도) 전송을 지원하는 API에 대한 액세스를 제공하고, 또한 UWP 앱이 SPP(직렬 포트 프로필)를 구현하는 장치에 액세스할 수 있습니다. 이 접근 권한 값에는 자식 요소가 필요합니다. 자세한 내용은 [Bluetooth 관련 장치 접근 권한 값을 지정하는 방법](https://msdn.microsoft.com/library/windows/apps/Dn263090)을 참조하세요. |

## <a name="use-the-windows-runtime-api-for-communicating-with-your-device"></a>Windows 런타임 API를 사용하여 장치와 통신

다음 표에서는 일부 접근 권한 값을 Windows 런타임 API에 연결합니다.

| 장치 접근 권한 값        | API             | 
|--------------------------|-----------------|
| AllJoyn                  | [**Windows.Devices.AllJoyn**](https://msdn.microsoft.com/library/windows/apps/Dn894971) | 
| 차단된 채팅 메시지    | [**Windows.ApplicationModel.CommunicationBlocking**](https://msdn.microsoft.com/library/windows/apps/Dn974207) | 
| 위치                 | 자세한 내용은 [지도 및 위치 개요](https://msdn.microsoft.com/library/windows/apps/Mt219699)를 참조하세요. | 
| 전화 통화               | [**Windows.ApplicationModel.Calls**](https://msdn.microsoft.com/library/windows/apps/Dn297266) | 
| 사용자 계정 정보 | [**Windows.System.UserProfile**](https://msdn.microsoft.com/library/windows/apps/BR241881) | 
| VOIP 호출             | [**Windows.ApplicationModel.Calls**](https://msdn.microsoft.com/library/windows/apps/Dn297266) | 
| USB                      | [**Windows.Devices.Usb**](https://msdn.microsoft.com/library/windows/apps/Dn278466) | 
| HID                      | [**Windows.Devices.HumanInterfaceDevice**](https://msdn.microsoft.com/library/windows/apps/Dn264174) | 
| Bluetooth GATT           | [**Windows.Devices.Bluetooth.GenericAttributeProfile**](https://msdn.microsoft.com/library/windows/apps/Dn297685) | 
| Bluetooth RFCOMM         | [**Windows.Devices.Bluetooth.Rfcomm**](https://msdn.microsoft.com/library/windows/apps/Dn263529) | 
| 서비스 지점         | [**Windows.Devices.PointOfService**](https://msdn.microsoft.com/library/windows/apps/Dn298071) |

