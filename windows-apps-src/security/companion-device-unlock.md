---
title: Windows Hello 및 동반 장치를 사용 하 여 잠금 해제
description: Windows Hello 동반 장치는 Windows 10 데스크톱과 함께 작동 하 여 사용자 인증 환경을 향상 시킬 수 있는 장치입니다. Windows Hello 부록 장치 프레임 워크를 사용 하는 경우에는 생체 인식 기능을 사용할 수 없는 경우에도 도우미 장치에서 Windows Hello에 대 한 풍부한 환경을 제공할 수 있습니다. 예를 들어 Windows 10 desktop에 얼굴 인증 또는 지문 판독기 장치를 위한 카메라가 없는 경우를 들 수 있습니다.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 보안
ms.assetid: 89f3d331-20cd-457b-83e8-1a22aaab2658
ms.localizationpriority: medium
ms.openlocfilehash: 96aea61073cf0c62f0c9636519018e1f19d0c8b3
ms.sourcegitcommit: 53c00939b20d4b0a294936df3d395adb0c13e231
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91933094"
---
# <a name="windows-unlock-with-windows-hello-companion-iot-devices"></a>Windows Hello 부록 (IoT) 장치를 사용 하 여 windows 잠금 해제

Windows Hello 동반 장치는 Windows 10 데스크톱과 함께 작동 하 여 사용자 인증 환경을 향상 시킬 수 있는 장치입니다. Windows Hello 부록 장치 프레임 워크를 사용 하는 경우에는 생체 인식 기능을 사용할 수 없는 경우에도 도우미 장치에서 Windows Hello에 대 한 풍부한 환경을 제공할 수 있습니다. 예를 들어 Windows 10 desktop에 얼굴 인증 또는 지문 판독기 장치를 위한 카메라가 없는 경우를 들 수 있습니다.

> [!NOTE]
> Windows Hello 부록 장치 프레임 워크에 대 한 API는 Windows 10 버전 2004에서 더 이상 사용 되지 않습니다.

> [!NOTE]
> Windows Hello 부록 장치 프레임 워크는 모든 앱 개발자가 사용할 수 없는 특수 한 기능입니다. 이 프레임 워크를 사용 하려면 앱을 특별히 Microsoft에서 프로 비전 하 고 해당 매니페스트에 제한 된 *Secondaryauthenticationfactor* 기능을 나열 해야 합니다. 승인을 받으려면에 문의 하세요 [cdfonboard@microsoft.com](mailto:cdfonboard@microsoft.com) .

## <a name="introduction"></a>소개

> 비디오 개요는 Channel 9의 Build 2016에서 [IoT 장치 세션을 사용 하 여 Windows 잠금 해제](https://channel9.msdn.com/Events/Build/2016/P491) 를 참조 하세요.

> 코드 샘플은 [Windows Hello 부록 장치 프레임 워크 Github 리포지토리](https://github.com/Microsoft/companion-device-framework)를 참조 하세요.

### <a name="use-cases"></a>사용 사례

다양 한 방법으로 Windows Hello 부록 장치 프레임 워크를 사용 하 여 함께 제공 되는 장치를 통해 뛰어난 Windows 잠금 해제 환경을 구축할 수 있습니다. 예를 들어 사용자는 다음을 할 수 있습니다.

- USB를 통해 장치를 PC에 연결 하 고, 동반 장치에서 단추를 터치 하 고, PC의 잠금을 자동으로 해제 합니다.
- Bluetooth를 통해 PC와 이미 페어링 된 휴대폰을 포켓에 전달 합니다. PC의 스페이스바에 도달 하면 휴대폰에 알림이 수신 됩니다. 승인 및 PC는 단순히 잠금을 해제 합니다.
- 장치를 신속 하 게 잠금 해제 하려면 NFC 판독기에 대 한 도우미 장치를 누릅니다.
- Wearer를 이미 인증 한 적합성 밴드를 착용 합니다. PC에 근접 하 고 특수 제스처 (예: 박수)를 수행 하면 PC의 잠금이 해제 됩니다.

### <a name="biometric-enabled-windows-hello-companion-devices"></a>생체 인식 사용 Windows Hello 동반 장치

동반 장치에서 생체 인식을 지 원하는 경우 경우에 따라 windows [생체 인식 프레임 워크](/windows-hardware/design/device-experiences/windows-hello) 는 windows Hello 부록 장치 프레임 워크 보다 더 나은 솔루션 일 수 있습니다. 연락 하시기 바랍니다 [cdfonboard@microsoft.com](mailto:cdfonboard@microsoft.com) . 적절 한 접근 방식을 선택 하는 데 도움이 될 것입니다.

### <a name="components-of-the-solution"></a>솔루션의 구성 요소

아래 다이어그램은 솔루션의 구성 요소 및 솔루션의 구성 요소를 보여 줍니다.

![프레임 워크 개요](images/companion-device-1.png)

Windows Hello 부록 장치 프레임 워크는 Windows에서 실행 되는 서비스로 구현 됩니다 (이 문서의 도우미 인증 서비스 라고 함). 이 서비스는 Windows Hello 부록 장치에 저장 된 HMAC 키로 보호 해야 하는 잠금 해제 토큰을 생성 해야 합니다. 이렇게 하면 잠금 해제 토큰에 대 한 액세스에 Windows Hello 동반 장치 유무가 필요 합니다. 각 (PC, Windows 사용자) 튜플에는 고유한 잠금 해제 토큰이 있습니다.

Windows Hello 부록 장치 프레임 워크와 통합 하려면 다음이 필요 합니다.

- Windows 앱 스토어에서 다운로드 되는 동반 장치용 [UWP (유니버설 Windows 플랫폼)](../get-started/universal-application-platform-guide.md) windows Hello 동반 장치 앱입니다. 
- Windows Hello 부록 장치에서 2 256 비트 HMAC 키를 만들고이를 사용 하 여 HMAC를 생성할 수 있습니다 (SHA-256 사용).
- Windows 10 desktop의 보안 설정이 올바르게 구성 되어 있습니다. 함께 제공 되는 인증 서비스를 사용 하려면 Windows Hello 도우미 장치를 연결 하기 전에이 PIN을 설정 해야 합니다. 사용자는 설정 > 계정 > 로그인 옵션을 통해 PIN을 설정 해야 합니다.

위의 요구 사항 외에도 Windows Hello 도우미 디바이스 앱은 다음을 담당합니다.

- 사용자 환경 및 Windows Hello 부록 장치의 초기 등록 및 이후 등록 취소
- 백그라운드에서 실행 하 여 windows hello 동반 장치 및 함께 제공 되는 인증 서비스와 통신 합니다.
- 오류 처리

일반적으로 도우미 장치는 처음으로 적합성 밴드를 설정 하는 등 초기 설치를 위해 앱과 함께 제공 됩니다. 이 문서에 설명 된 기능은 해당 앱의 일부가 될 수 있으며 별도의 앱은 필요 하지 않습니다.  

### <a name="user-signals"></a>사용자 신호

각 Windows Hello 부록 장치는 세 가지 사용자 신호를 지 원하는 앱과 결합 되어야 합니다. 이러한 신호는 동작이 나 제스처 형식이 될 수 있습니다.

- **의도 신호**: 사용자가 잠금 해제에 대 한 의도를 표시할 수 있습니다. 예를 들어 Windows Hello 부록 장치의 단추를 누를 수 있습니다. **Windows Hello 동반 장치** 쪽에서 의도 신호를 수집 해야 합니다.
- **사용자 상태 신호**: 사용자의 존재를 증명 합니다. 예를 들어 Windows Hello 부록 장치는 pc의 잠금 해제 (PC PIN과 혼동 하지 않음)에 사용할 수 있는 PIN을 요구 하거나 단추를 눌러야 할 수 있습니다.
- **명확성 신호**: windows Hello 부록 장치에서 여러 옵션을 사용할 수 있는 경우 사용자가 잠금 해제 하려는 windows 10 데스크톱을 구분 합니다.

이러한 사용자 신호는 개수에 관계 없이 하나로 결합할 수 있습니다. 사용자의 현재 상태 및 의도 신호는 각 사용에 필요 합니다.

### <a name="registration-and-future-communication-between-a-pc-and-windows-hello-companion-devices"></a>PC와 Windows Hello 부록 장치 간의 등록 및 향후 통신

Windows hello 부록 장치를 Windows Hello 부록 장치 프레임 워크에 연결 하려면 프레임 워크에 등록 해야 합니다. 등록 환경은 Windows Hello 부록 장치 앱에서 완전히 소유 합니다.

Windows Hello 동반 장치와 Windows 10 데스크톱 장치 간의 관계는 일 대 다 일 수 있습니다. 즉, 여러 Windows 10 데스크톱 장치에 하나의 도우미 장치를 사용할 수 있습니다. 그러나 각 windows Hello 동반 장치는 각 Windows 10 데스크톱 장치에서 한 사용자 에게만 사용할 수 있습니다.   

Windows Hello 부록 장치가 PC와 통신할 수 있으려면 먼저 사용할 전송에 동의 해야 합니다. 이러한 선택은 Windows Hello 부록 장치 앱에 남아 있습니다. windows Hello 부록 장치 프레임 워크는 windows Hello 동반 장치 및 windows 10 desktop 장치 측의 windows hello 부록 장치 앱 간에 사용 되는 전송 방식 (USB, NFC, WiFi, BT, 등) 또는 프로토콜에 대 한 제한 사항을 적용 하지 않습니다. 그러나이 문서의 "보안 요구 사항" 섹션에 설명 된 대로 전송 계층에 대 한 특정 보안 고려 사항을 제안 합니다. 이러한 요구 사항을 제공 하는 것은 장치 공급자의 책임입니다. 프레임 워크는 이러한 기능을 제공 하지 않습니다.


## <a name="user-interaction-model"></a>사용자 상호 작용 모델

### <a name="windows-hello-companion-device-app-discovery-installation-and-first-time-registration"></a>Windows Hello 도우미 디바이스 앱 검색, 설치 및 최초 등록

일반적인 사용자 워크플로는 다음과 같습니다.

- 사용자는 Windows Hello 동반 장치를 사용 하 여 잠금을 해제 하려는 각 대상 Windows 10 데스크톱 장치에 대 한 PIN을 설정 합니다.
- 사용자는 windows 10 desktop 장치에서 windows Hello 부록 장치 앱을 실행 하 여 windows 10 desktop에 windows Hello 부록 장치를 등록 합니다.

참고:

- Windows Hello 부록 장치 앱의 검색, 다운로드 및 시작이 간소화 되 고, 가능한 경우 자동화 된 경우 (예: Windows 10 desktop 장치 쪽에서 NFC 판독기의 Windows Hello 도우미 장치를 누르면 앱을 다운로드할 수 있음) 그러나이는 Windows Hello 부록 장치 및 Windows Hello 부록 장치 앱의 책임입니다.
- 엔터프라이즈 환경에서는 MDM을 통해 Windows Hello 부록 장치 앱을 배포할 수 있습니다.
- Windows Hello 부록 장치 앱은 등록의 일부로 발생 하는 오류 메시지를 사용자에 게 표시 하는 일을 담당 합니다.

### <a name="registration-and-de-registration-protocol"></a>등록 및 등록 취소 프로토콜

다음 다이어그램에서는 등록 하는 동안 Windows Hello 도우미 장치가 함께 제공 된 인증 서비스와 상호 작용 하는 방법을 보여 줍니다.  

![등록 흐름의 다이어그램입니다.](images/companion-device-2.png)

프로토콜에 사용 되는 키에는 두 가지가 있습니다.

- 장치 키 (**devicekey**): PC에서 Windows 잠금을 해제 하는 데 필요한 잠금 해제 토큰을 보호 하는 데 사용 됩니다.
- 인증 키 (**authkey**): Windows Hello 부록 장치와 함께 인증 서비스를 상호 인증 하는 데 사용 됩니다.

장치 키 및 인증 키는 등록 시 Windows Hello 동반 장치 앱과 Windows Hello 도우미 장치 간에 교환 됩니다. 따라서 Windows Hello 부록 장치 앱 및 Windows Hello 동반 장치는 키를 보호 하기 위해 보안 전송을 사용 해야 합니다.

위의 다이어그램에는 Windows Hello 부록 장치에 생성 되는 두 개의 HMAC 키가 표시 되는 반면, 앱에서이를 생성 하 고 저장을 위해 Windows Hello 부록 장치로 보낼 수도 있습니다.

### <a name="starting-authentication-flows"></a>인증 흐름 시작

사용자가 Windows Hello 동반 장치 프레임 워크를 사용 하 여 Windows 10 desktop에 대 한 흐름을 시작 하는 방법에는 다음 두 가지 방법이 있습니다. 즉, 의도 신호를 제공 합니다.

- 뚜껑 노트북을 열거나 스페이스바를 누르거나 PC에서 위로 살짝 밉니다.
- Windows Hello 동반 장치 쪽에서 제스처 또는 작업을 수행 합니다.

시작점을 선택 하는 데 사용할 수 있는 Windows Hello 동반 장치의 선택입니다. Windows Hello 동반 장치 프레임 워크는 옵션 하나가 발생 하면 도우미 장치 앱에 알립니다. 옵션 2의 경우 Windows Hello 부록 장치 앱은 해당 이벤트가 캡처 되었는지 확인 하기 위해 도우미 장치를 쿼리해야 합니다. 이렇게 하면 잠금 해제가 성공 하기 전에 Windows Hello 도우미 장치가 의도 신호를 수집 합니다.

### <a name="windows-hello-companion-device-credential-provider"></a>Windows Hello 부록 장치 자격 증명 공급자

Windows 10에는 모든 Windows Hello 부록 장치를 처리 하는 새로운 자격 증명 공급자가 있습니다.

Windows Hello 동반 장치 자격 증명 공급자는 트리거를 활성화 하 여 동반 장치 백그라운드 작업을 시작 합니다. 트리거는 PC가 절전 모드에서 해제되고 잠금 화면이 표시될 때 처음 설정됩니다. 두 번째는 PC에서 로그온 UI를 입력 하 고 Windows Hello 부록 장치 자격 증명 공급자가 선택 된 타일이 되는 시간입니다.

Windows Hello 부록 장치 앱에 대 한 도우미 라이브러리는 잠금 화면 상태 변경을 수신 대기 하 고 Windows Hello 부록 장치 백그라운드 작업에 해당 하는 이벤트를 보냅니다.

Windows Hello 도우미 장치 백그라운드 작업이 여러 개 있는 경우 인증 프로세스를 완료 한 첫 번째 백그라운드 작업은 PC 잠금을 해제 합니다. 동반 장치 인증 서비스는 나머지 인증 통화를 무시 합니다.

Windows Hello 부록 장치 측의 환경은 Windows Hello 부록 장치 앱에서 소유 하 고 관리 합니다. Windows Hello 부록 장치 프레임 워크는 사용자 환경의이 부분을 제어 하지 않습니다. 구체적으로, 함께 제공 되는 인증 공급자는 Windows Hello 부록 장치 앱 (백그라운드 앱을 통해)이 로그온 UI의 상태 변경 내용에 대 한 정보를 제공 합니다. 예를 들어 잠금 화면에는 사용자가 입력 하 고 스페이스바를 누르면 Windows Hello 부록 장치 앱을 사용 하 여이에 대 한 환경을 구축 합니다 (예: 사용자가 스페이스바 및 dispelled 잠금 해제 화면). 에서 USB를 통해 장치를 검색 하기 시작 합니다.)

Windows Hello 부록 장치 프레임 워크는 Windows Hello 부록 장치 앱에서 선택할 수 있는 (지역화 된) 텍스트 및 오류 메시지를 제공 합니다. 이는 잠금 화면 (또는 로그온 UI)의 맨 위에 표시 됩니다. 자세한 내용은 메시지 및 오류 처리 단원을 참조 하세요.

### <a name="authentication-protocol"></a>인증 프로토콜

Windows Hello 부록 장치 앱과 연결 된 백그라운드 작업이 시작 되 면 Windows Hello 도우미 장치에서 부록 인증 서비스에 의해 계산 된 HMAC 값의 유효성을 검사 하 고 두 개의 HMAC 값을 계산 하는 데 도움이 됩니다.
- Service HMAC = hmac (인증 키, nonce 서비스 | | 장치 nonce | | 세션 nonce)의 유효성을 검사 합니다.
- Nonce를 사용 하 여 장치 키의 HMAC를 계산 합니다.
- 도우미 인증 서비스에 의해 생성 된 nonce와 연결 된 첫 번째 HMAC 값을 사용 하 여 인증 키의 HMAC를 계산 합니다.

두 번째 계산 된 값은 서비스에서 장치를 인증 하 고 전송 채널의 재생 공격을 방지 하는 데 사용 됩니다.

![업데이트 된 등록 흐름의 다이어그램입니다.](images/companion-device-3.png)

## <a name="lifecycle-management"></a>수명 주기 관리

### <a name="register-once-use-everywhere"></a>한 번 등록, 모든 곳에서 사용

백 엔드 서버를 사용 하지 않을 경우 사용자는 windows Hello 부록 장치를 각 Windows 10 데스크톱 장치에 별도로 등록 해야 합니다.

동반 장치 공급 업체 또는 OEM은 웹 서비스를 구현 하 여 사용자 Windows 10 데스크톱 또는 모바일 장치에서 등록 상태를 로밍할 수 있습니다. 자세한 내용은 로밍, 해지 및 필터 서비스 섹션을 참조 하세요.

### <a name="pin-management"></a>고정 관리

도우미 장치를 사용 하려면 Windows 10 데스크톱 장치에서 PIN을 설정 해야 합니다. 이렇게 하면 해당 Windows Hello 도우미 디바이스가 작동하지 않을 경우 사용자가 백업을 사용할 수 있습니다. PIN은 Windows에서 관리 하는 것 이며 앱에는 표시 되지 않습니다. 이를 변경 하려면 사용자가 설정 > 계정 > 로그인 옵션으로 이동 합니다.

### <a name="management-and-policy"></a>관리 및 정책

사용자는 해당 데스크톱 장치에서 Windows Hello 부록 장치 앱을 실행 하 여 windows 10 데스크톱에서 Windows Hello 부록 장치를 제거할 수 있습니다.

기업에는 Windows Hello 부록 장치 프레임 워크를 제어 하는 두 가지 옵션이 있습니다.

- 기능 설정 또는 해제
- Windows 앱 보관을 사용 하 여 허용 되는 Windows Hello 동반 장치 허용 목록 정의

Windows Hello 부록 장치 프레임 워크는 사용 가능한 보조 장치의 인벤토리를 유지 하는 중앙 집중식 방법을 지원 하지 않습니다. 또는 Windows Hello 동반 장치 유형의 인스턴스를 추가로 필터링 하는 방법을 지원 하지 않습니다. 예를 들어 X와 Y 사이에 일련 번호가 있는 도우미 장치만 허용 됩니다. 그러나 앱 개발자는 이러한 기능을 제공 하는 서비스를 빌드할 수 있습니다. 자세한 내용은 로밍, 해지 및 필터 서비스 섹션을 참조 하세요.

### <a name="revocation"></a>해지

Windows Hello 부록 장치 프레임 워크는 특정 Windows 10 데스크톱 장치에서 원격으로 도우미 장치를 제거 하는 기능을 지원 하지 않습니다. 대신, 사용자는 windows 10 desktop에서 실행 되는 Windows Hello 부록 장치 앱을 통해 Windows Hello 도우미 장치를 제거할 수 있습니다.

그러나 도우미 장치 공급 업체는 원격 해지 기능을 제공 하는 서비스를 빌드할 수 있습니다. 자세한 내용은 로밍, 해지 및 필터 서비스 섹션을 참조 하세요.

### <a name="roaming-and-filter-services"></a>로밍 및 필터 서비스

동반 장치 공급 업체는 다음과 같은 시나리오에 사용할 수 있는 웹 서비스를 구현할 수 있습니다.

- Enterprise 용 필터 서비스: 기업은 해당 환경에서 사용할 수 있는 Windows Hello 부록 장치 집합을 특정 공급 업체에서 선택 하는 것으로 제한할 수 있습니다. 예를 들어 Contoso 회사는 공급 업체 X에서 1만 모델 Y를 주문 하 고 해당 장치만 Contoso 도메인에서 작동 하도록 할 수 있습니다 (공급 업체 X의 다른 장치 모델은 아님).
- 인벤토리: 엔터프라이즈는 엔터프라이즈 환경에서 사용 되는 기존 장치 목록을 확인할 수 있습니다.
- 실시간 해지: 직원이 해당 도우미 장치를 분실 하거나 도난당 한 것으로 보고 하는 경우 웹 서비스를 사용 하 여 해당 장치를 해지할 수 있습니다.
- 로밍: 사용자가 도우미 장치를 한 번 등록 하기만 하면 모든 Windows 10 데스크톱 및 모바일에서 사용할 수 있습니다.

이러한 기능을 구현 하려면 Windows Hello 부록 장치 앱이 등록 및 사용 시간에 웹 서비스를 확인 해야 합니다. Windows Hello 동반 장치 앱은 하루에 한 번만 웹 서비스를 확인 해야 하는 등의 캐시 된 로그온 시나리오를 최적화할 수 있습니다 (해지 시간을 최대 1 일로 확장 하는 비용).  

## <a name="windows-hello-companion-device-framework-api-model"></a>Windows Hello 부록 장치 프레임 워크 API 모델

### <a name="overview"></a>개요

Windows Hello 부록 장치 앱에는 장치 등록 및 등록 취소를 담당 하는 UI가 있는 foregroud 앱과 인증을 처리 하는 백그라운드 작업이 포함 되어 있습니다.

전반적인 API 흐름은 다음과 같습니다.

1. Windows Hello 부록 장치 등록
    * 장치가 가까이 있는지 확인 하 고 필요한 경우 해당 기능을 쿼리 합니다.
    * 두 개의 HMAC 키 (동반 장치 쪽 또는 앱 쪽)를 생성 합니다.
    * RequestStartRegisteringDeviceAsync 호출
    * FinishRegisteringDeviceAsync 호출
    * Windows Hello 부록 장치 앱에서 HMAC 키 (지원 되는 경우)를 저장 하 고 Windows Hello 부록 장치 앱에서 해당 복사본을 삭제 하는지 확인 합니다.
2. 백그라운드 작업 등록
3. 백그라운드 작업에서 올바른 이벤트를 기다립니다.
    * WaitingForUserConfirmation: 인증 흐름을 시작 하려면 Windows Hello 부록 장치 쪽의 사용자 작업/제스처가 필요한 경우이 이벤트를 기다립니다.
    * CollectingCredential: Windows Hello 도우미 장치가 PC 쪽의 사용자 작업/제스처를 사용 하 여 인증 흐름을 시작 하는 경우 (예: 스페이스바를 눌러)이 이벤트를 기다립니다.
    * 다른 트리거 (예: 스마트 카드): 올바른 Api를 호출 하는 현재 인증 상태를 쿼리해야 합니다.
4. ShowNotificationMessageAsync을 호출 하 여 사용자에 게 오류 메시지 또는 필요한 다음 단계에 대 한 정보를 유지 합니다. 의도 신호가 수집 된 후에만이 API를 호출 합니다.
5. 잠금 해제
    * 의도 및 사용자 상태 신호가 수집 되었는지 확인
    * StartAuthenticationAsync 호출
    * 필요한 HMAC 작업을 수행 하기 위해 동반 장치와 통신
    * 호출 인 요청-비동기
6. 사용자가 장치를 요청할 때 Windows Hello 도우미 장치 등록 취소 (예: 해당 장치를 분실 한 경우)
    * FindAllRegisteredDeviceInfoAsync를 통해 로그인 한 사용자에 대 한 Windows Hello 도우미 장치를 열거 합니다.
    * UnregisterDeviceAsync를 사용 하 여 등록 취소

### <a name="registration-and-de-registration"></a>등록 및 등록 취소

등록 하려면 RequestStartRegisteringDeviceAsync 및 FinishRegisteringDeviceAsync와 함께 제공 되는 인증 서비스에 대 한 두 개의 API 호출이 필요 합니다.

이러한 호출을 수행 하기 전에 Windows Hello 부록 장치 앱에서 Windows Hello 부록 장치를 사용할 수 있는지 확인 해야 합니다. Windows Hello 부록 장치가 HMAC 키 (인증 및 장치 키)를 생성 하는 일을 담당 하는 경우 Windows Hello 부록 장치 앱은 위의 두 호출을 수행 하기 전에 도우미 장치에도 해당 장치를 생성 하도록 요청 해야 합니다. Windows Hello 부록 장치 앱에서 HMAC 키 생성을 담당 하는 경우 위의 두 호출을 호출 하기 전에이 작업을 수행 해야 합니다.

또한 첫 번째 API 호출 (RequestStartRegisteringDeviceAsync)의 일부로 Windows Hello 부록 장치 앱은 장치 기능을 결정 하 고 API 호출의 일부로 전달 하도록 준비 해야 합니다. 예를 들어 Windows Hello 부록 장치가 HMAC 키에 대 한 보안 저장소를 지원 하는지 여부입니다. 동일한 Windows Hello 부록 장치 앱을 사용 하 여 동일한 도우미 장치의 여러 버전을 관리 하 고 이러한 기능이 변경 되 면 (그리고 장치 쿼리를 결정 해야 함) 먼저 API 호출을 수행 하기 전에이 쿼리가 발생 하는 것이 좋습니다.   

첫 번째 API (RequestStartRegisteringDeviceAsync)는 두 번째 API (FinishRegisteringDeviceAsync)에서 사용 하는 핸들을 반환 합니다. 등록에 대 한 첫 번째 호출에서 PIN 프롬프트를 시작 하 여 사용자가 존재 하는지 확인 합니다. PIN이 설정 되지 않은 경우에는이 호출이 실패 합니다. Windows Hello 동반 장치 앱은 KeyCredentialManager. IsSupportedAsync 호출을 통해 PIN이 설정 되었는지 여부를 쿼리할 수 있습니다. 정책에서 Windows Hello 동반 장치를 사용 하지 않도록 설정한 경우에도 RequestStartRegisteringDeviceAsync 호출이 실패할 수 있습니다.

SecondaryAuthenticationFactorRegistrationStatus 열거형을 통해 첫 번째 호출의 결과가 반환 됩니다.

```C#
{
    Failed = 0,         // Something went wrong in the underlying components
    Started,            // First call succeeded
    CanceledByUser,     // User cancelled PIN prompt
    PinSetupRequired,   // PIN is not set up
    DisabledByPolicy,   // Companion device framework or this app is disabled
}
```

두 번째 호출 (FinishRegisteringDeviceAsync)은 등록을 완료 합니다. 등록 프로세스의 일부로 Windows Hello 부록 장치 앱은 동반 인증 서비스와 함께 도우미 장치 구성 데이터를 저장할 수 있습니다. 이 데이터에는 4K 크기 제한이 있습니다. 이 데이터는 인증 시 Windows Hello 부록 장치 앱에서 사용할 수 있습니다. 예를 들어,이 데이터를 사용 하 여 MAC 주소와 같은 Windows Hello 도우미 장치에 연결 하거나, Windows Hello 동반 장치에서 저장소에 PC를 사용 하려는 경우에는 구성 데이터를 사용할 수 있습니다. 구성 데이터의 일부로 저장 된 중요 한 데이터는 Windows Hello 부록 장치 에서만 인식 되는 키로 암호화 되어야 합니다. 또한 Windows 서비스에서 구성 데이터를 저장 하는 경우 사용자 프로필에서 Windows Hello 부록 장치 앱에 사용할 수 있습니다.

Windows Hello 동반 장치 앱은 AbortRegisteringDeviceAsync를 호출 하 여 등록을 취소 하 고 오류 코드를 전달할 수 있습니다. 함께 제공 되는 인증 서비스는 원격 분석 데이터에 오류를 기록 합니다. 이 호출의 좋은 예는 Windows Hello 부록 장치에 문제가 발생 하 여 등록을 완료할 수 없는 경우입니다. 예를 들어, HMAC 키를 저장할 수 없거나 BT 연결이 손실 될 수 있습니다.

Windows Hello 부록 장치 앱은 사용자가 windows 10 desktop에서 Windows Hello 부록 장치를 등록 취소 하는 옵션을 제공 해야 합니다. 예를 들어 해당 장치를 분실 하거나 최신 버전을 구입한 경우를 들 수 있습니다. 사용자가 해당 옵션을 선택 하면 Windows Hello 부록 장치 앱에서 UnregisterDeviceAsync를 호출 해야 합니다. Windows Hello 부록 장치 앱에서이 호출은 PC 쪽에서 호출자 앱의 특정 장치 Id 및 AppId에 해당 하는 모든 데이터 (HMAC 키 포함)를 삭제 하기 위해 도우미 장치 인증 서비스를 트리거합니다. 이 API 호출은 Windows Hello 동반 장치 앱 또는 도우미 장치 쪽에서 HMAC 키를 삭제 하려고 시도 하지 않습니다. Windows Hello 부록 장치 앱을 구현 하기 위해 남아 있습니다.

Windows Hello 부록 장치 앱은 등록 및 등록 취소 단계에서 발생 하는 모든 오류 메시지를 표시 합니다.

```C#
using System;
using Windows.Security.Authentication.Identity.Provider;
using Windows.Storage.Streams;
using Windows.Security.Cryptography;
using Windows.UI.Popups;

namespace SecondaryAuthFactorSample
{
    public class DeviceRegistration
    {

        public void async OnRegisterButtonClick()
        {
            //
            // Pseudo function, the deviceId should be retrieved by the application from the device
            //
            string deviceId = await ReadSerialNumberFromDevice();

            IBuffer deviceKey = CryptographicBuffer.GenerateRandom(256/8);
            IBuffer mutualAuthenticationKey = CryptographicBuffer.GenerateRandom(256/8);

            SecondaryAuthenticationFactorRegistration registrationResult =
                await SecondaryAuthenticationFactorRegistration.RequestStartRegisteringDeviceAsync(
                    deviceId,  // deviceId: max 40 wide characters. For example, serial number of the device
                    SecondaryAuthenticationFactorDeviceCapabilities.SecureStorage |
                        SecondaryAuthenticationFactorDeviceCapabilities.HMacSha256 |
                        SecondaryAuthenticationFactorDeviceCapabilities.StoreKeys,
                    "My test device 1", // deviceFriendlyName: max 64 wide characters. For example: John's card
                    "SAMPLE-001", // deviceModelNumber: max 32 wide characters. The app should read the model number from device.
                    deviceKey,
                    mutualAuthenticationKey);

            switch(registerResult.Status)
            {
            case SecondaryAuthenticationFactorRegistrationStatus.Started:
                //
                // Pseudo function:
                // The app needs to retrieve the value from device and set into opaqueBlob
                //
                IBuffer deviceConfigData = ReadConfigurationDataFromDevice();

                if (deviceConfigData != null)
                {
                    await registrationResult.Registration.FinishRegisteringDeviceAsync(deviceConfigData); //config data limited to 4096 bytes
                    MessageDialog dialog = new MessageDialog("The device is registered correctly.");
                    await dialog.ShowAsync();
                }
                else
                {
                    await registrationResult.Registration.AbortRegisteringDeviceAsync("Failed to connect to the device");
                    MessageDialog dialog = new MessageDialog("Failed to connect to the device.");
                    await dialog.ShowAsync();
                }
                break;

            case SecondaryAuthenticationFactorRegistrationStatus.CanceledByUser:
                MessageDialog dialog = new MessageDialog("You didn't enter your PIN.");
                await dialog.ShowAsync();
                break;

            case SecondaryAuthenticationFactorRegistrationStatus.PinSetupRequired:
                MessageDialog dialog = new MessageDialog("Please setup PIN in settings.");
                await dialog.ShowAsync();
                break;

            case SecondaryAuthenticationFactorRegistrationStatus.DisabledByPolicy:
                MessageDialog dialog = new MessageDialog("Your enterprise prevents using this device to sign in.");
                await dialog.ShowAsync();
                break;
            }
        }

        public void async UpdateDeviceList()
        {
            IReadOnlyList<SecondaryAuthenticationFactorInfo> deviceInfoList =
                await SecondaryAuthenticationFactorRegistration.FindAllRegisteredDeviceInfoAsync(
                    SecondaryAuthenticationFactorDeviceFindScope.User);

            if (deviceInfoList.Count > 0)
            {
                foreach (SecondaryAuthenticationFactorInfo deviceInfo in deviceInfoList)
                {
                    //
                    // Add deviceInfo.FriendlyName and deviceInfo.DeviceId into a combo box
                    //
                }
            }
        }

        public void async OnUnregisterButtonClick()
        {
            string deviceId;
            //
            // Read the deviceId from the selected item in the combo box
            //
            await SecondaryAuthenticationFactorRegistration.UnregisterDeviceAsync(deviceId);
        }
    }
}
```

### <a name="authentication"></a>인증

인증을 사용 하려면 도우미 인증 서비스에 대 한 두 개의 API 호출 인 StartAuthenticationAsync 및 FinishAuthencationAsync가 필요 합니다.

첫 번째 시작 API는 두 번째 API에서 사용 하는 핸들을 반환 합니다.  첫 번째 호출은 다른 작업에 연결 된 nonce를 반환 합니다. 즉, Windows Hello 부록 장치에 저장 된 장치 키를 사용 하 여 HMAC'ed 해야 합니다. 두 번째 호출은 장치 키가 포함 된 HMAC의 결과를 반환 하며, 잠재적으로 인증을 종료할 수 있습니다 (즉, 사용자가 바탕 화면을 볼 수 있음).

초기 등록 후 정책에서 Windows Hello 도우미 장치를 사용 하지 않도록 설정 하는 경우 첫 번째 시작 API (StartAuthenticationAsync)가 실패할 수 있습니다. API 호출이 WaitingForUserConfirmation 또는 CollectingCredential 상태 외부에서 수행 된 경우에도 실패할 수 있습니다 (이 섹션의 뒷부분에 자세히 설명). 등록 되지 않은 도우미 장치 앱에서 호출 하는 경우에도 실패할 수 있습니다. SecondaryAuthenticationFactorAuthenticationStatus 열거형은 가능한 결과를 요약 합니다.

```C#
{
    Failed = 0,                     // Something went wrong in the underlying components
    Started,
    UnknownDevice,                  // Companion device app is not registered with framework
    DisabledByPolicy,               // Policy disabled this device after registration
    InvalidAuthenticationStage,     // Companion device framework is not currently accepting
                                    // incoming authentication requests
}
```

첫 번째 호출에서 제공 된 nonce가 만료 된 경우 두 번째 API 호출 (FinishAuthencationAsync)은 실패할 수 있습니다 (20 초). SecondaryAuthenticationFactorFinishAuthenticationStatus 열거형은 가능한 결과를 캡처합니다.

```C#
{
    Failed = 0,     // Something went wrong in the underlying components
    Completed,      // Success
    NonceExpired,   // Nonce is expired
}
```

두 API 호출 (StartAuthenticationAsync 및 Fiauthencasync)의 타이밍은 Windows Hello 동반 장치에서 의도, 사용자 유무 및 명확성 신호를 수집 하는 방법과 일치 해야 합니다 (자세한 내용은 사용자 신호 참조). 예를 들어, 두 번째 호출은 내재 된 신호를 사용할 수 있을 때까지 전송 되지 않아야 합니다. 즉, 사용자가 의도를 표시 하지 않은 경우 PC의 잠금을 해제 하면 안 됩니다. 이를 보다 명확 하 게 하려면 PC 잠금 해제에 Bluetooth 근접이 사용 된다고 가정 합니다. 그렇지 않으면 사용자가 자신의 PC에서 부엌을 사용 하는 즉시 PC의 잠금이 해제 됩니다. 또한 첫 번째 호출에서 반환 되는 nonce는 시간 범위 (20 초) 이며 특정 기간 후에 만료 됩니다. 따라서 첫 번째 호출은 Windows Hello 부록 장치 앱이 함께 제공 되는 장치 유무를 표시 하는 경우에만 수행 해야 합니다. 예를 들어 도우미 장치가 USB 포트에 삽입 되거나 NFC 판독기에서 탭 됩니다. Bluetooth를 사용 하 여 PC 쪽에서 배터리에 영향을 주지 않도록 하거나 Windows Hello 동반 장치 유무를 확인할 때 해당 지점에서 진행 되는 다른 Bluetooth 활동에 영향을 주지 않도록 주의 해야 합니다. 또한 PIN에를 입력 하 여와 같이 사용자 상태 신호를 제공 해야 하는 경우에는 신호를 수집한 후에만 첫 번째 인증 통화를 수행 하는 것이 좋습니다.

Windows Hello 부록 장치 프레임 워크를 사용 하면 Windows Hello 부록 장치 앱에서 사용자가 인증 흐름에 있는 위치에 대 한 전체 그림을 제공 하 여 위의 두 호출을 수행할 시기를 결정할 수 있습니다. Windows Hello 부록 장치 프레임 워크는 앱 백그라운드 작업에 잠금 상태 변경 알림을 제공 하 여이 기능을 제공 합니다.

![도우미 장치 흐름](images/companion-device-4.png)

이러한 각 상태에 대 한 세부 정보는 다음과 같습니다.

| 시스템 상태                         | 설명                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
|----------------------------   |-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------    |
| WaitingForUserConfirmation    | 이 상태 변경 알림 이벤트는 잠금 화면이 종료 될 때 발생 합니다 (예: 사용자가 Windows + L을 눌렀음을). 이 상태에서 장치를 찾는 데 어려움이 있는 경우와 관련 된 오류 메시지를 요청 하지 않는 것이 좋습니다. 일반적으로 내재 된 신호를 사용할 수 있는 경우에만 메시지를 표시 하는 것이 좋습니다. Windows Hello 부록 장치 앱은이 상태에서 인증에 대 한 첫 번째 API 호출을 수행 해야 합니다. 예를 들어, 동반 장치가 의도 신호를 수집 하 고 (예: NFC 판독기를 탭 하 고, 박수와 같은 특정 제스처를 사용 하 여) Windows Hello 부록 장치 앱 백그라운드 작업에서 의도 신호가 검색 된 도우미 장치에서 표시를 받습니다. 그렇지 않고 Windows Hello 동반 장치 앱이 PC를 사용 하 여 인증 흐름을 시작 하는 경우 (사용자가 잠금 해제 화면 또는 스페이스바를 눌러) Windows Hello 부록 장치 앱은 다음 상태 (CollectingCredential)를 기다려야 합니다.   |
| CollectingCredential          | 이 상태 변경 알림 이벤트는 사용자가 노트북을 열거나 키보드에서 아무 키나 적중 하거나 잠금 해제 화면으로 swipes 때 발생 합니다. Windows Hello 동반 장치에서 위의 작업을 사용 하 여 의도 신호 수집을 시작 하는 경우 Windows Hello 부록 장치 앱에서 수집을 시작 해야 합니다. 예를 들어 사용자가 PC의 잠금을 해제 하 고 있는지 확인 하는 기능을 제공 하는 장치에서 pop를 사용 합니다. Windows Hello 도우미 디바이스 앱에서 사용자가 Windows Hello 도우미 디바이스에 사용자 현재 상태를 제공하도록 요구하는 경우 이때 오류 사례를 제공하는 것이 좋습니다.                                                                                                                                                                                                                                                                                                                                             |
| SuspendingAuthentication      | Windows Hello 부록 장치 앱이이 상태를 수신 하면 도우미 인증 서비스가 인증 요청 수락을 중지 했음을 의미 합니다.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| CredentialCollected 됨           | 즉, 다른 Windows Hello 부록 장치 앱은 두 번째 API를 호출 했으며, 도우미 인증 서비스에서 제출 된 항목을 확인 하 고 있음을 의미 합니다. 현재 제출 된 인증 서비스에서 확인을 통과 하지 않는 한이 시점에서 도우미 인증 서비스는 다른 인증 요청을 수락 하지 않습니다. Windows Hello 부록 장치 앱은 다음 상태에 도달할 때까지 계속 조정 해야 합니다.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| CredentialAuthenticated       | 즉, 제출 된 자격 증명이 정상적으로 작동 합니다. CredentialAuthenticated에는 성공한 Windows Hello 도우미 장치의 장치 ID가 있습니다. Windows Hello 부록 장치 앱은 연결 된 장치가 승자 인지 확인 해야 합니다. 그렇지 않은 경우 Windows Hello 부록 장치 앱은 사후 인증 흐름을 표시 하지 않아야 합니다 (예: 관련 장치의 성공 메시지 또는 해당 장치에서 진동). 제출 된 자격 증명이 작동 하지 않으면 상태가 CollectingCredential 상태로 변경 됩니다.                                                                                                                                                                                                                                                                                                                                                                                       |
| Stop? 인증        | 인증에 성공 하 고 사용자가 데스크톱을 보았습니다. 백그라운드 작업을 중지 하는 시간입니다. 배경 작업을 끝내기 전에 stageevent 처리기를 명시적으로 등록 취소 합니다. 이렇게 하면 백그라운드 작업을 신속 하 게 종료 하는 데 도움이 됩니다.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |



Windows Hello 부록 장치 앱은 처음 두 상태에서 두 인증 Api만 호출 해야 합니다. Windows Hello 부록 장치 앱은이 이벤트가 발생 하는 시나리오를 확인 해야 합니다. 잠금 해제 또는 사후 잠금 해제의 두 가지 가능성이 있습니다. 현재 잠금 해제만 지원 됩니다. 이후 릴리스에서는 잠금 해제 시나리오가 지원 될 수 있습니다. SecondaryAuthenticationFactorAuthenticationScenario 열거형은 다음 두 가지 옵션을 캡처합니다.

```C#
{
    SignIn = 0,         // Running under lock screen mode
    CredentialPrompt,   // Running post unlock
}
```

전체 코드 샘플:

```C#
using System;
using Windows.Security.Authentication.Identity.Provider;
using Windows.Storage.Streams;
using Windows.Security.Cryptography;
using System.Threading;
using Windows.ApplicationModel.Background;

namespace SecondaryAuthFactorSample
{
    public sealed class AuthenticationTask : IBackgroundTask
    {
        private string _deviceId;
        private static AutoResetEvent _exitTaskEvent = new AutoResetEvent(false);
        private static IBackgroundTaskInstance _taskInstance;
        private BackgroundTaskDeferral _deferral;

        private void Authenticate()
        {
            int retryCount = 0;

            while (retryCount < 3)
            {
                //
                // Pseudo code, the svcAuthNonce should be passed to device or generated from device
                //
                IBuffer svcAuthNonce = CryptographicBuffer.GenerateRandom(256/8);

                SecondaryAuthenticationFactorAuthenticationResult authResult = await
                    SecondaryAuthenticationFactorAuthentication.StartAuthenticationAsync(
                        _deviceId,
                        svcAuthNonce);
                if (authResult.Status != SecondaryAuthenticationFactorAuthenticationStatus.Started)
                {
                    SecondaryAuthenticationFactorAuthenticationMessage message;
                    switch (authResult.Status)
                    {
                        case SecondaryAuthenticationFactorAuthenticationStatus.DisabledByPolicy:
                            message = SecondaryAuthenticationFactorAuthenticationMessage.DisabledByPolicy;
                            break;
                        case SecondaryAuthenticationFactorAuthenticationStatus.InvalidAuthenticationStage:
                            // The task might need to wait for a SecondaryAuthenticationFactorAuthenticationStageChangedEvent
                            break;
                        default:
                            return;
                    }

                    // Show error message. Limited to 512 characters wide
                    await SecondaryAuthenticationFactorAuthentication.ShowNotificationMessageAsync(null, message);
                    return;
                }

                //
                // Pseudo function:
                // The device calculates and returns sessionHmac and deviceHmac
                //
                await GetHmacsFromDevice(
                    authResult.Authentication.ServiceAuthenticationHmac,
                    authResult.Authentication.DeviceNonce,
                    authResult.Authentication.SessionNonce,
                    out deviceHmac,
                    out sessionHmac);
                if (sessionHmac == null ||
                    deviceHmac == null)
                {
                    await authResult.Authentication.AbortAuthenticationAsync(
                        "Failed to read data from device");
                    return;
                }

                SecondaryAuthenticationFactorFinishAuthenticationStatus status =
                    await authResult.Authentication.FinishAuthencationAsync(deviceHmac, sessionHmac);
                if (status == SecondaryAuthenticationFactorFinishAuthenticationStatus.NonceExpired)
                {
                    retryCount++;
                    continue;
                }
                else if (status == SecondaryAuthenticationFactorFinishAuthenticationStatus.Completed)
                {
                    // The credential data is collected and ready for unlock
                    return;
                }
            }
        }

        public void OnAuthenticationStageChanged(
            object sender,
            SecondaryAuthenticationFactorAuthenticationStageChangedEventArgs args)
        {
            // The application should check the args.StageInfo.Stage to determine what to do in next. Note that args.StageInfo.Scenario will have the scenario information (SignIn vs CredentialPrompt).

            switch(args.StageInfo.Stage)
            {
            case SecondaryAuthenticationFactorAuthenticationStage.WaitingForUserConfirmation:
                // Show welcome message
                await SecondaryAuthenticationFactorAuthentication.ShowNotificationMessageAsync(
                    null,
                    SecondaryAuthenticationFactorAuthenticationMessage.WelcomeMessageSwipeUp);
                break;

            case SecondaryAuthenticationFactorAuthenticationStage.CollectingCredential:
                // Authenticate device
                Authenticate();
                break;

            case SecondaryAuthenticationFactorAuthenticationStage.CredentialAuthenticated:
                if (args.StageInfo.DeviceId = _deviceId)
                {
                    // Show notification on device about PC unlock
                }
                break;

            case SecondaryAuthenticationFactorAuthenticationStage.StoppingAuthentication:
                // Quit from background task
                _exitTaskEvent.Set();
                break;
            }

            Debug.WriteLine("Authentication Stage = " + args.StageInfo.AuthenticationStage.ToString());
        }

        //
        // The Run method is the entry point of a background task.
        //
        public void Run(IBackgroundTaskInstance taskInstance)
        {
            _taskInstance = taskInstance;
            _deferral = taskInstance.GetDeferral();

            // Register canceled event for this task
            taskInstance.Canceled += TaskInstanceCanceled;

            // Find all device registred by this application
            IReadOnlyList<SecondaryAuthenticationFactorInfo> deviceInfoList =
                await SecondaryAuthenticationFactorRegistration.FindAllRegisteredDeviceInfoAsync(
                    SecondaryAuthenticationFactorDeviceFindScope.AllUsers);

            if (deviceInfoList.Count == 0)
            {
                // Quit the task silently
                return;
            }
            _deviceId = deviceInfoList[0].DeviceId;
            Debug.WriteLine("Use first device '" + _deviceId + "' in the list to signin");

            // Register AuthenticationStageChanged event
            SecondaryAuthenticationFactorRegistration.AuthenticationStageChanged += OnAuthenticationStageChanged;

            // Wait the task exit event
            _exitTaskEvent.WaitOne();

            _deferral.Complete();
        }

        void TaskInstanceCanceled(IBackgroundTaskInstance sender, BackgroundTaskCancellationReason reason)
        {
            _exitTaskEvent.Set();
        }
    }
}
```

### <a name="register-a-background-task"></a>백그라운드 작업 등록

Windows Hello 부록 장치 앱에서 첫 번째 동반 장치를 등록 하는 경우 장치 및 도우미 장치 인증 서비스 간에 인증 정보를 전달 하는 백그라운드 작업 구성 요소도 등록 해야 합니다.

```C#
using System;
using Windows.Security.Authentication.Identity.Provider;
using Windows.Storage.Streams;
using Windows.ApplicationModel.Background;

namespace SecondaryAuthFactorSample
{
    public class BackgroundTaskManager
    {
        // Register background task
        public static async Task<IBackgroundTaskRegistration> GetOrRegisterBackgroundTaskAsync(
            string bgTaskName,
            string taskEntryPoint)
        {
            // Check if there's an existing background task already registered
            var bgTask = (from t in BackgroundTaskRegistration.AllTasks
                          where t.Value.Name.Equals(bgTaskName)
                          select t.Value).SingleOrDefault();
            if (bgTask == null)
            {
                BackgroundAccessStatus status =
                    BackgroundExecutionManager.RequestAccessAsync().AsTask().GetAwaiter().GetResult();

                if (status == BackgroundAccessStatus.Denied)
                {
                    Debug.WriteLine("Background Execution is denied.");
                    return null;
                }

                var taskBuilder = new BackgroundTaskBuilder();
                taskBuilder.Name = bgTaskName;
                taskBuilder.TaskEntryPoint = taskEntryPoint;
                taskBuilder.SetTrigger(new SecondaryAuthenticationFactorAuthenticationTrigger());
                bgTask = taskBuilder.Register();
                // Background task is registered
            }

            bgTask.Completed += BgTask_Completed;
            bgTask.Progress += BgTask_Progress;

            return bgTask;
        }
    }
}
```

### <a name="errors-and-messages"></a>오류 및 메시지

Windows Hello 부록 장치 프레임 워크는 사용자에 게 로그인 성공 또는 실패에 대 한 피드백을 제공 합니다. Windows Hello 부록 장치 프레임 워크는 Windows Hello 부록 장치 앱에서 선택할 수 있는 (지역화 된) 텍스트 및 오류 메시지를 제공 합니다. 이러한 로그는 로그온 UI에 표시 됩니다.

![도우미 장치 오류](images/companion-device-5.png)

Windows Hello 부록 장치 앱은 ShowNotificationMessageAsync를 사용 하 여 로그온 UI의 일부로 사용자에 게 메시지를 표시할 수 있습니다. 의도 신호를 사용할 수 있는 경우이 API를 호출 합니다. 의도 신호는 항상 Windows Hello 동반 장치 쪽에서 수집 되어야 합니다.

메시지에는 지침과 오류 라는 두 가지 유형이 있습니다.

지침 메시지는 사용자에 게 잠금 해제 프로세스를 시작 하는 방법을 보여 주도록 설계 되었습니다. 이러한 메시지는 사용자에 게 잠금 화면에서 한 번만 표시 되며, 첫 번째 장치 등록 시에는 다시 표시 되지 않습니다. 이러한 메시지는 잠금 화면 아래에 계속 표시 됩니다.

오류 메시지는 항상 표시 되며 의도 신호가 제공 된 후에 표시 됩니다. 사용자에 게 메시지를 표시 하기 전에 의도 신호를 수집 해야 하 고 사용자가 Windows Hello 부록 장치 중 하나를 사용 하는 경우에만 사용자에 게 제공 하는 경우 여러 Windows Hello 부록 장치에서 오류 메시지를 표시 하기 위해 경쟁 하는 상황이 발생 하지 않아야 합니다. 따라서 Windows Hello 부록 장치 프레임 워크는 큐를 유지 관리 하지 않습니다. 호출자가 오류 메시지를 요청 하면 5 초 동안 표시 되 고 5 초 동안 오류 메시지를 표시 하는 다른 모든 요청은 삭제 됩니다. 5 초가 지나면 다른 호출자가 오류 메시지를 표시 하 게 됩니다. 모든 호출자가 오류 채널을 넣지 하지 못하도록 합니다.

지침 및 오류 메시지는 다음과 같습니다. 장치 이름은 ShowNotificationMessageAsync의 일부로 동반 장치 앱에서 전달 하는 매개 변수입니다.

**지침**

- " *장치 이름을*사용 하 여 로그인 하려면 스페이스바를 누르거나 스페이스바를 누릅니다."
- "도우미 장치를 설정 합니다. 기다리거나 다른 로그인 옵션을 사용 하십시오. "
- " *장치 이름을* 눌러 NFC 판독기에 로그인 합니다."
- " *장치 이름을* 찾고 있습니다 ..."
- " *장치 이름을* USB 포트에 연결 하 여 로그인 합니다."

**Errors**

- "로그인 지침은 *장치 이름* 을 참조 하세요."
- " *장치 이름을* 사용 하 여 로그인 하려면 Bluetooth를 켜 세요."
- " *장치 이름을* 사용 하 여 로그인 하려면 NFC를 켜 세요."
- " *장치 이름을* 사용 하 여 로그인 하려면 Wi-Fi 네트워크에 연결"을 사용 합니다.
- " *장치 이름을* 다시 누릅니다."
- "기업은 *장치 이름*으로 로그인 할 수 없습니다. 다른 로그인 옵션을 사용 하십시오. "
- " *장치 이름* 을 탭 하 여 로그인 합니다."
- "로그인 하려면 *장치 이름* 에 손가락을 두십시오."
- "로그인 하려면 *장치 이름* 에서 손가락을 살짝 밉니다."
- " *장치 이름을*사용 하 여 로그인 할 수 없습니다. 다른 로그인 옵션을 사용 하십시오. "
- "오류가 발생했습니다. 다른 로그인 옵션을 사용 하 고 *장치 이름을* 다시 설정 하십시오. "
- "다시 시도 하세요."
- "음성 암호를 *장치 이름*에 표시" 합니다.
- " *장치 이름을*사용 하 여 로그인 할 준비가 되었습니다."
- "다른 로그인 옵션을 먼저 사용 합니다. *장치 이름을* 사용 하 여 로그인 할 수 있습니다."

### <a name="enumerating-registered-devices"></a>등록 된 장치 열거

Windows Hello 동반 장치 앱은 FindAllRegisteredDeviceInfoAsync 호출을 통해 등록 된 도우미 장치 목록을 열거할 수 있습니다. 이 API는 enum SecondaryAuthenticationFactorDeviceFindScope를 통해 정의 된 두 개의 쿼리 형식을 지원 합니다.

```C#
{
    User = 0,
    AllUsers,
}
```

첫 번째 범위는 로그온 한 사용자에 대 한 장치 목록을 반환 합니다. 두 번째는 해당 PC의 모든 사용자에 대 한 목록을 반환 합니다. 다른 사용자의 Windows Hello 도우미 장치 등록을 취소 하지 않으려면 등록 취소 시 첫 번째 범위를 사용 해야 합니다. 두 번째는 인증 또는 등록 시간에 사용 해야 합니다. 등록 시이 열거를 사용 하면 앱에서 동일한 Windows Hello 도우미 장치를 두 번 등록 하지 않도록 방지할 수 있습니다.

앱이이 검사를 수행 하지 않는 경우에도 PC는 동일한 Windows Hello 도우미 장치를 두 번 이상 등록 하는 것을 거부 합니다. 인증 시에는 AllUsers 범위를 사용 하 여 Windows Hello 동반 장치 앱에서 사용자 B가 로그인 할 때 사용자 A 로그온 스위치 사용자 흐름을 지원 합니다 .이를 위해 두 사용자가 모두 Windows Hello 부록 장치 앱을 설치 하 고 사용자 A가 해당 장치를 PC에 등록 하 고 PC가 잠금 화면 (또는 로그온 화면)을 진행 합니다.

## <a name="security-requirements"></a>보안 요구 사항

동반 인증 서비스는 다음과 같은 보안 보호 기능을 제공 합니다.

- 보통 사용자 또는 앱 컨테이너로 실행 되는 Windows 10 데스크톱 장치의 맬웨어는 Windows Hello 동반 장치를 사용 하 여 PC의 사용자 자격 증명 키 (Windows Hello의 일부로 저장 됨)에 자동으로 액세스할 수 없습니다.
- Windows 10 desktop 장치의 악의적인 사용자는 windows 10 데스크톱 장치에서 다른 사용자에 게 속한 Windows Hello 도우미 장치를 사용 하 여 해당 사용자 자격 증명 키 (동일한 Windows 10 데스크톱 장치)에 대 한 자동 액세스 권한을 얻을 수 없습니다.
- Windows Hello 부록 장치의 맬웨어는 windows Hello 부록 장치 프레임 워크를 위해 특별히 개발 된 기능이 나 코드를 활용 하 여 Windows 10 desktop 장치의 사용자 자격 증명 키에 자동으로 액세스할 수 없습니다.
- 악의적인 사용자는 windows Hello 부록 장치와 Windows 10 데스크톱 장치 간에 트래픽을 캡처하여 나중에 재생 하 여 Windows 10 데스크톱 장치를 잠금 해제할 수 없습니다. 프로토콜에서 nonce, authkey 및 HMAC를 사용 하면 재생 공격 으로부터 보호 됩니다.
- 불량 PC의 맬웨어 또는 악의적인 사용자는 Windows Hello 동반 장치를 사용 하 여 사용자 PC에 액세스할 수 없습니다. 이는 authkey를 사용 하 고 프로토콜에서 HMAC를 사용 하 여 부록 인증 서비스와 Windows Hello 도우미 장치 간의 상호 인증을 통해 이루어집니다.

위에 열거 된 보안 보호를 얻기 위한 핵심은 무단 액세스 로부터 HMAC 키를 보호 하 고 사용자의 현재 상태를 확인 하는 것입니다. 구체적으로 말하면 다음과 같은 요구 사항을 충족 해야 합니다.

- Windows Hello 부록 장치 복제에 대 한 보호 제공
- 등록 시 PC로 HMAC 키를 보낼 때 도청 으로부터 보호 제공
- 사용자의 현재 상태 신호를 사용할 수 있는지 확인 합니다.