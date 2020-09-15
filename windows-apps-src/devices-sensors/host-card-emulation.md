---
ms.assetid: 26834A51-512B-485B-84C8-ABF713787588
title: NFC 스마트 카드 앱 만들기
description: Windows Phone 8.1에서는 SIM 기반 보안 요소를 사용하여 NFC 카드 에뮬레이션 앱을 지원했지만, 해당 모델에서는 보안 결제 앱이 MNO(모바일 네트워크 운영자)와 밀접하게 결합되어야 합니다.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: e7075f0de1ce01e9157c520f28b0b0dd70260498
ms.sourcegitcommit: bc8add1675070506371c1881b41c3727f1b55720
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/15/2020
ms.locfileid: "90093125"
---
# <a name="create-an-nfc-smart-card-app"></a>NFC 스마트 카드 앱 만들기

> [!Important]
> 이 항목은 Windows 10 Mobile에만 적용 됩니다.

Windows Phone 8.1에서는 SIM 기반 보안 요소를 사용하여 NFC 카드 에뮬레이션 앱을 지원했지만, 해당 모델에서는 보안 결제 앱이 MNO(모바일 네트워크 운영자)와 밀접하게 결합되어야 합니다. 이는 MNO와 결합되어 있지 않은 다른 판매자 또는 개발자에 의해 가능한 결제 솔루션의 다양성을 제한합니다. Windows 10 Mobile에는 HCE(호스트 카드 에뮬레이션)라는 새로운 카드 에뮬레이션 기술이 도입되었습니다. HCE 기술을 통해 앱이 NFC 카드 판독기와 직접 통신할 수 있습니다. 이 항목에서는 HCE(호스트 카드 에뮬레이션)가 Windows 10 Mobile 디바이스에서 작동하는 방법과 MNO와 공동 작업하지 않고도 고객이 실제 카드 대신 휴대폰을 통해 서비스에 액세스할 수 있도록 HCE 앱을 개발하는 방법을 보여 줍니다.

## <a name="what-you-need-to-develop-an-hce-app"></a>HCE 앱을 개발 하는 데 필요한 사항

Windows 10 Mobile 용 HCE 기반 카드 에뮬레이션 앱을 개발 하려면 개발 환경 설치를 시작 해야 합니다. Windows 개발자 도구 및 NFC 에뮬레이션을 지 원하는 Windows 10 모바일 에뮬레이터를 포함 하는 Microsoft Visual Studio 2015을 설치 하 여 설정을 가져올 수 있습니다. 설정 가져오기에 대 한 자세한 내용은 [설정 가져오기](../get-started/get-set-up.md) 를 참조 하세요.

필요에 따라 포함 된 Windows 10 모바일 에뮬레이터 대신 실제 Windows 10 모바일 장치로 테스트 하려는 경우에는 다음 항목도 필요 합니다.

- NFC HCE를 지 원하는 Windows 10 Mobile 장치. 현재 Lumia 730, 830, 640 및 640 XL은 NFC HCE 앱을 지 원하는 하드웨어를 포함 합니다.
- ISO/IEC 14443-4 및 ISO/IEC 7816-4 프로토콜을 지 원하는 판독기 터미널

Windows 10 Mobile은 다음과 같은 기능을 제공 하는 HCE 서비스를 구현 합니다.

- 앱은 에뮬레이션할 카드에 대해 애플릿 식별자 (지원)를 등록할 수 있습니다.
- 외부 판독기 카드 선택 및 사용자 기본 설정에 따라 등록 된 앱 중 하나에 대 한 APDU (응용 프로그램 프로토콜 데이터 단위) 명령 및 응답 쌍의 충돌 해결 및 라우팅
- 사용자 작업의 결과로 앱에 대 한 이벤트 및 알림 처리

Windows 10은 iso DEP (ISO-IEC 14443-4)를 기반으로 하는 스마트 카드의 에뮬레이션을 지원 하 고 ISO-IEC 7816-4 사양에 정의 된 대로 APDUs를 사용 하 여 통신 합니다. Windows 10은 HCE 앱에 대 한 기술을 ISO/IEC 14443-4 유형으로 지원 합니다. 유형 B, 유형 F 및 비-ISO-DEP (예: MIFARE) 기술이 기본적으로 SIM으로 라우팅됩니다.

Windows 10 Mobile 장치만 카드 에뮬레이션 기능을 사용할 수 있습니다. SIM 기반 및 HCE 기반 카드 에뮬레이션은 다른 버전의 Windows 10에서 사용할 수 없습니다.

HCE 및 SIM 기반 카드 에뮬레이션 지원에 대 한 아키텍처는 아래 다이어그램에 나와 있습니다.

![HCE 및 SIM 카드 에뮬레이션을 위한 아키텍처](./images/nfc-architecture.png)

## <a name="app-selection-and-aid-routing"></a>앱 선택 및 라우팅 지원

HCE 앱을 개발 하려면 사용자가 여러 다른 HCE 앱을 설치할 수 있으므로 Windows 10 Mobile 장치에서 특정 앱에 대 한 지원을 라우팅하는 방법을 이해 해야 합니다. 각 앱은 여러 HCE 및 SIM 기반 카드를 등록할 수 있습니다. 사용자가 NFC 설정 메뉴에서 기본 지불 카드로 "SIM Card" 옵션을 선택 하기만 하면 기존 Windows Phone 8.1 앱이 Windows 10 Mobile에서 계속 작동 합니다. 이는 장치를 처음 켤 때 기본적으로 설정 됩니다.

사용자가 Windows 10 Mobile 장치를 터미널에 탭 하면 해당 데이터가 장치에 설치 된 적절 한 앱으로 자동 라우팅됩니다. 이 라우팅은 5-16 바이트 식별자 인 애플릿 Id (보조 기능)를 기반으로 합니다. 탭 하는 동안 외부 터미널은 SELECT 명령 APDU를 전송 하 여 모든 후속 APDU 명령이 라우팅되는 데 도움이 되는 도움을 지정 합니다. 후속 SELECT 명령은 라우팅을 다시 변경 합니다. 응용 프로그램 및 사용자 설정에서 등록 한 보조 프로그램에 따라 APDU 트래픽이 특정 앱으로 라우팅되고,이는 응답 APDU를 보냅니다. 터미널은 동일한 탭에서 여러 다른 앱과 통신 하는 것이 좋습니다. 따라서 다른 앱의 백그라운드 작업이 APDU에 응답할 수 있는 공간을 확보 하기 위해 비활성화 된 경우 앱의 백그라운드 작업이 최대한 빨리 종료 되도록 해야 합니다. 백그라운드 작업은이 항목의 뒷부분에서 설명 합니다.

HCE 앱은 지원에 대 한 APDUs을 받을 수 있도록 처리할 수 있는 특정 지원에 등록 해야 합니다. 앱은 지원 그룹을 사용 하 여 도움을 다시 지원 합니다. 보조 그룹은 개념적으로 개별 실제 카드와 동일 합니다. 예를 들어 한 신용 카드는 보조 그룹으로 선언 되 고 다른 은행에서 두 번째 신용 카드는 다른 보조 보조 그룹을 사용 하 여 선언 됩니다. 두 가지 경우 모두 동일한 도움을 가질 수 있습니다.

## <a name="conflict-resolution-for-payment-aid-groups"></a>지불 지원 그룹에 대 한 충돌 해결

앱에서 물리적 카드 (지원 그룹)를 등록 하는 경우 지원 그룹 범주를 "결제" 또는 "기타"로 선언할 수 있습니다. 지정된 시간에 여러 결제 AID 그룹이 등록되어 있을 수 있지만, 이러한 결제 AID 그룹은 한 번에 하나만 탭 및 지급에 사용할 수 있으며 이는 사용자가 선택합니다. 이 동작은 사용자가 장치를 터미널에 탭 할 때 의도 하지 않은 다른 카드로 지불 하지 않도록 단일 지불, 신용 또는 직불 카드를 선택 하는 consciously을 제어 하는 것으로 예상 하기 때문에 존재 합니다.

그러나 "기타"로 등록 된 여러 가지 보조 그룹은 사용자 개입 없이 동시에 사용 하도록 설정할 수 있습니다. 이 동작은 사용자가 휴대폰을 누를 때마다 작업을 수행 하거나 메시지를 표시 하지 않고 예측, 쿠폰 또는 전송 같은 다른 유형의 카드를 사용 하기 때문에 존재 합니다.

"결제"로 등록 된 모든 보조 그룹은 사용자가 기본 지불 카드를 선택할 수 있는 NFC 설정 페이지의 카드 목록에 표시 됩니다. 기본 지불 카드를 선택 하면이 지불 지원 그룹을 등록 한 앱이 기본 지불 앱이 됩니다. 기본 지불 앱은 사용자 개입 없이 지원 그룹을 사용 하거나 사용 하지 않도록 설정할 수 있습니다. 사용자가 기본 지불 앱 프롬프트를 거부 하는 경우 현재 기본 결제 앱 (있는 경우)은 계속 기본값으로 계속 유지 됩니다. 다음 스크린샷은 NFC 설정 페이지를 보여 줍니다.

![NFC 설정 페이지의 스크린샷](./images/nfc-settings.png)

위의 예제 스크린샷을 사용 하 여 사용자가 기본 지불 카드를 "HCE 응용 프로그램 1"에서 등록 되지 않은 다른 카드로 변경 하는 경우 시스템은 사용자의 동의에 대 한 확인 메시지를 생성 합니다. 그러나 사용자가 기본 지불 카드를 "HCE 응용 프로그램 1"에서 등록 된 다른 카드로 변경 하는 경우 "HCE 응용 프로그램 1"가 이미 기본 지불 앱 이기 때문에 시스템은 사용자에 대 한 확인 메시지를 생성 하지 않습니다.

## <a name="conflict-resolution-for-non-payment-aid-groups"></a>비 지불 지원 그룹에 대 한 충돌 해결

"기타"로 분류 된 비 지불 카드는 NFC 설정 페이지에 표시 되지 않습니다.

앱은 지불 지원 그룹과 같은 방식으로 비 지불 지원 그룹을 만들고 등록 하 고 사용 하도록 설정할 수 있습니다. 주요 차이점은 비 지불 지원 그룹의 경우 에뮬레이션 범주가 "지불"이 아닌 "기타"로 설정 된다는 것입니다. 지원 그룹을 시스템에 등록 한 후에 지원 그룹을 사용 하도록 설정 하 여 NFC 트래픽을 받도록 해야 합니다. 지불 하지 않는 지원 그룹을 사용 하도록 설정 하면 다른 앱에서 시스템에 이미 등록 된 지원 중 하 나와 충돌 하지 않는 한 사용자에 게 확인 메시지를 표시 하지 않습니다. 충돌이 발생 하는 경우 사용자가 새로 등록 된 지원 그룹을 사용 하도록 선택 하는 경우 사용 하지 않도록 설정 되는 카드와 연결 된 앱에 대 한 정보를 묻는 메시지가 표시 됩니다.

### <a name="coexistence-with-sim-based-nfc-applications"></a>SIM 기반 NFC 응용 프로그램과 함께 사용

Windows 10 Mobile에서 시스템은 컨트롤러 계층에서 라우팅 결정을 내리는 데 사용 되는 NFC 컨트롤러 라우팅 테이블을 설정 합니다. 테이블에는 다음 항목에 대 한 라우팅 정보가 포함 되어 있습니다.

- 개별 지원 경로.
- 프로토콜 기반 경로 (ISO DEP).
- 기술 기반 라우팅 (NFC-A/B/F)

외부 판독기가 "선택 지원" 명령을 보내면 NFC 컨트롤러는 먼저 라우팅 테이블에서 일치 항목에 대 한 지원 경로를 확인 합니다. 일치 하는 항목이 없는 경우에는 프로토콜 기반 경로를 ISO DEP (14443-4-A) 트래픽에 대 한 기본 경로로 사용 합니다. 다른 비 ISO DEP 트래픽의 경우 기술 기반 라우팅을 사용 합니다.

Windows 10 Mobile은 시스템에 보조 기능을 등록 하지 않는 레거시 Windows Phone 8.1 SIM 기반 앱을 계속 사용할 수 있도록 NFC 설정 페이지에서 "SIM Card" 메뉴 옵션을 제공 합니다. 사용자가 기본 지불 카드로 "SIM card"를 선택 하면 ISO DEP 경로가 UICC로 설정 되 고 드롭다운 메뉴의 다른 모든 선택 항목에 대해 ISO-DEP 경로가 호스트에 전달 됩니다.

Windows 10 Mobile을 사용 하 여 장치를 처음 부팅 하는 경우 SE 사용 SIM 카드가 있는 장치에 대해 ISO DEP 경로는 "SIM Card"로 설정 됩니다. 사용자가 HCE 지원 앱을 설치 하 고 해당 앱에서 HCE 지원 그룹 등록을 사용 하도록 설정한 경우 ISO DEP 경로를 호스트에 가리킵니다. 새 SIM 기반 응용 프로그램은 컨트롤러 라우팅 테이블에 특정 지원 경로를 채울 수 있도록 SIM에 지원 기능을 등록 해야 합니다.

## <a name="creating-an-hce-based-app"></a>HCE 기반 앱 만들기

HCE 앱은 두 부분으로 구성 됩니다.

- 사용자 상호 작용을 위한 기본 전경 앱입니다.
- 지정 된 지원에 대 한 APDUs를 처리 하기 위해 시스템에 의해 트리거되는 백그라운드 작업입니다.

NFC 탭에 대 한 응답으로 백그라운드 작업을 로드 하기 위한 매우 엄격한 성능 요구 사항 때문에 c # 또는 관리 코드 대신 c + +/CX 네이티브 코드에서 전체 백그라운드 작업을 구현 하는 것이 좋습니다 (종속 된 모든 종속성, 참조 또는 라이브러리 포함). C # 및 관리 코드는 정상적으로 정상적으로 수행 되지만, .NET CLR을 로드 하는 것과 같은 오버 헤드가 발생 합니다 .이는 c + +/CX에 작성 하 여 피할 수 있습니다.

## <a name="create-and-register-your-background-task"></a>백그라운드 작업 만들기 및 등록

시스템에서 라우트된 APDUs에 대 한 처리 및 응답을 위해 HCE 앱에서 백그라운드 작업을 만들어야 합니다. 앱이 처음 시작 될 때 포그라운드는 다음 코드와 같이 [**IBackgroundTaskRegistration**](/uwp/api/Windows.ApplicationModel.Background.IBackgroundTaskRegistration) 인터페이스를 구현 하는 hce 백그라운드 작업을 등록 합니다.

```cppcx
var taskBuilder = new BackgroundTaskBuilder();
taskBuilder.Name = bgTaskName;
taskBuilder.TaskEntryPoint = taskEntryPoint;
taskBuilder.SetTrigger(new SmartCardTrigger(SmartCardTriggerType.EmulatorHostApplicationActivated));
bgTask = taskBuilder.Register();
```

작업 트리거는 [**Smart의 Triggertype**](/uwp/api/Windows.Devices.SmartCards.SmartCardTriggerType)으로 설정 되어 있습니다. **EmulatorHostApplicationActivated**. 즉, 앱을 확인 하는 OS에서 SELECT AID 명령 APDU를 수신할 때마다 백그라운드 작업이 시작 됩니다.

## <a name="receive-and-respond-to-apdus"></a>APDUs 수신 및 응답

앱을 대상으로 하는 APDU가 있는 경우 시스템은 백그라운드 작업을 시작 합니다. 백그라운드 작업은 [**SmartCardEmulatorApduReceivedEventArgs**](/uwp/api/Windows.Devices.SmartCards.SmartCardEmulatorApduReceivedEventArgs) 개체의 [**commandapdu**](/uwp/api/windows.devices.smartcards.smartcardemulatorapdureceivedeventargs.commandapdu) 속성을 통해 전달 된 apdu를 수신 하 고 동일한 개체의 [**TryRespondAsync**](/uwp/api/windows.devices.smartcards.smartcardemulatorapdureceivedeventargs.tryrespondwithcryptogramsasync) 메서드를 사용 하 여 apdu에 응답 합니다. 성능상의 이유로 간단한 작업의 백그라운드 작업을 유지 하는 것이 좋습니다. 예를 들어 APDUs에 즉시 응답 하 고 모든 처리가 완료 되 면 백그라운드 작업을 종료 합니다. NFC 트랜잭션의 특성으로 인해 사용자는 매우 짧은 시간 동안만 판독기에 대해 장치를 보유 하는 경향이 있습니다. 사용자의 백그라운드 작업은 연결이 비활성화 될 때까지 판독기에서 트래픽을 계속 받습니다 .이 경우 [**SmartCardEmulatorConnectionDeactivatedEventArgs**](/uwp/api/Windows.Devices.SmartCards.SmartCardEmulatorConnectionDeactivatedEventArgs) 개체를 받게 됩니다. [**SmartCardEmulatorConnectionDeactivatedEventArgs**](/uwp/api/windows.devices.smartcards.smartcardemulatorconnectiondeactivatedeventargs.reason) 속성에 표시 된 다음과 같은 이유 때문에 연결을 비활성화할 수 있습니다.

- 연결이 **끊어진 connectionlost** value를 사용 하 여 비활성화 되 면 사용자가 판독기에서 장치를 꺼냈습니다. 앱에서 사용자에 게 더 이상 터미널을 탭 해야 하는 경우 사용자에 게 피드백을 요청 하는 것이 좋습니다. 백그라운드 작업을 신속 하 게 종료 (지연 완료) 하 여 다시 탭 하면 이전 백그라운드 작업이 종료 될 때까지 지연 되지 않도록 해야 합니다.
- 연결이 **리디렉션되**는 상태에서 연결이 비활성화 된 경우에는 터미널이 다른 도움을 받는 새로운 SELECT AID 명령 APDU를 전송 했음을 의미 합니다. 이 경우 앱은 백그라운드 작업을 즉시 종료 (지연 완료) 하 여 다른 백그라운드 작업을 실행할 수 있도록 해야 합니다.

백그라운드 작업은 [**IBackgroundTaskInstance 인터페이스**](/uwp/api/Windows.ApplicationModel.Background.IBackgroundTaskInstance)에 취소 된 [**이벤트**](/uwp/api/windows.applicationmodel.background.ibackgroundtaskinstance.canceled) 에 등록 해야 합니다. 또한 백그라운드 작업을 완료 하면 시스템에서 발생 하는 백그라운드 작업 (지연 완료)을 빠르게 종료 합니다. 다음은 HCE 앱 백그라운드 작업을 보여 주는 코드입니다.

```cppcx
void BgTask::Run(
    IBackgroundTaskInstance^ taskInstance)
{
    m_triggerDetails = static_cast<SmartCardTriggerDetails^>(taskInstance->TriggerDetails);
    if (m_triggerDetails == nullptr)
    {
        // May be not a smart card event that triggered us
        return;
    }

    m_emulator = m_triggerDetails->Emulator;
    m_taskInstance = taskInstance;

    switch (m_triggerDetails->TriggerType)
    {
    case SmartCardTriggerType::EmulatorHostApplicationActivated:
        HandleHceActivation();
        break;

    case SmartCardTriggerType::EmulatorAppletIdGroupRegistrationChanged:
        HandleRegistrationChange();
        break;

    default:
        break;
    }
}

void BgTask::HandleHceActivation()
{
 try
 {
        auto lock = m_srwLock.LockShared();
        // Take a deferral to keep this background task alive even after this "Run" method returns
        // You must complete this deferal immediately after you have done processing the current transaction
        m_deferral = m_taskInstance->GetDeferral();

        DebugLog(L"*** HCE Activation Background Task Started ***");

        // Set up a handler for if the background task is cancelled, we must immediately complete our deferral
        m_taskInstance->Canceled += ref new Windows::ApplicationModel::Background::BackgroundTaskCanceledEventHandler(
            [this](
            IBackgroundTaskInstance^ sender,
            BackgroundTaskCancellationReason reason)
        {
            DebugLog(L"Cancelled");
            DebugLog(reason.ToString()->Data());
            EndTask();
        });

        if (Windows::Phone::System::SystemProtection::ScreenLocked)
        {
            auto denyIfLocked = Windows::Storage::ApplicationData::Current->RoamingSettings->Values->Lookup("DenyIfPhoneLocked");
            if (denyIfLocked != nullptr && (bool)denyIfLocked == true)
            {
                // The phone is locked, and our current user setting is to deny transactions while locked so let the user know
                // Denied
                DoLaunch(Denied, L"Phone was locked at the time of tap");

                // We still need to respond to APDUs in a timely manner, even though we will just return failure
                m_fDenyTransactions = true;
            }
        }
        else
        {
            m_fDenyTransactions = false;
        }

        m_emulator->ApduReceived += ref new TypedEventHandler<SmartCardEmulator^, SmartCardEmulatorApduReceivedEventArgs^>(
            this, &BgTask::ApduReceived);

        m_emulator->ConnectionDeactivated += ref new TypedEventHandler<SmartCardEmulator^, SmartCardEmulatorConnectionDeactivatedEventArgs^>(
                [this](
                SmartCardEmulator^ emulator,
                SmartCardEmulatorConnectionDeactivatedEventArgs^ eventArgs)
            {
                DebugLog(L"Connection deactivated");
                EndTask();
            });

  m_emulator->Start();
        DebugLog(L"Emulator started");
 }
 catch (Exception^ e)
 {
        DebugLog(("Exception in Run: " + e->ToString())->Data());
        EndTask();
 }
}
```

## <a name="create-and-register-aid-groups"></a>지원 그룹 만들기 및 등록

카드를 프로 비전 할 때 응용 프로그램을 처음 시작 하는 동안 시스템에 지원 그룹을 만들고 등록 합니다. 시스템은 등록 된 지원 및 사용자 설정에 따라 외부 판독기가 APDUs 하 고 그에 따라 라우팅할 앱을 결정 합니다.

대부분의 지불 카드는 추가 결제 네트워크 카드 특정 지원과 함께 동일한 지원 (PPSE 지원)을 등록 합니다. 각 보조 그룹은 카드를 나타내며 사용자가 카드를 사용 하도록 설정 하는 경우 그룹의 모든 보조 도구를 사용할 수 있습니다. 마찬가지로 사용자가 카드를 비활성화 하는 경우 그룹의 모든 보조 도구를 사용할 수 없습니다.

지원 그룹을 등록 하려면 [**SmartCardAppletIdGroup**](/uwp/api/Windows.Devices.SmartCards.SmartCardAppletIdGroup) 개체를 만들고 해당 속성을 설정 하 여 hce 기반 지불 카드를 반영 해야 합니다. 표시 이름은 사용자 프롬프트 뿐만 아니라 NFC 설정 메뉴에 표시 되기 때문에 사용자에 게 설명 해야 합니다. HCE 지불 카드의 경우 [**SmartCardEmulationCategory**](/uwp/api/windows.devices.smartcards.smartcardappletidgroup.smartcardemulationcategory) 속성은 **지불** 로 설정 되 고 [**SmartCardEmulationType**](/uwp/api/windows.devices.smartcards.smartcardappletidgroup.smartcardemulationtype) 속성은 **Host**로 설정 되어야 합니다.

```cppcx
public static byte[] AID_PPSE =
        {
            // File name "2PAY.SYS.DDF01" (14 bytes)
            (byte)'2', (byte)'P', (byte)'A', (byte)'Y',
            (byte)'.', (byte)'S', (byte)'Y', (byte)'S',
            (byte)'.', (byte)'D', (byte)'D', (byte)'F', (byte)'0', (byte)'1'
        };

var appletIdGroup = new SmartCardAppletIdGroup(
                        "Example DisplayName",
                                new List<IBuffer> {AID_PPSE.AsBuffer()},
                                SmartCardEmulationCategory.Payment,
                                SmartCardEmulationType.Host);
```

지불 되지 않은 HCE 카드의 경우 [**SmartCardEmulationCategory**](/uwp/api/windows.devices.smartcards.smartcardappletidgroup.smartcardemulationcategory) 속성을 **Other** 로 설정 하 고 [**SmartCardEmulationType**](/uwp/api/windows.devices.smartcards.smartcardappletidgroup.smartcardemulationtype) 속성을 **Host**로 설정 해야 합니다.

```cppcx
public static byte[] AID_OTHER =
        {
            (byte)'1', (byte)'2', (byte)'3', (byte)'4',
            (byte)'5', (byte)'6', (byte)'7', (byte)'8',
            (byte)'O', (byte)'T', (byte)'H', (byte)'E', (byte)'R'
        };

var appletIdGroup = new SmartCardAppletIdGroup(
                        "Example DisplayName",
                                new List<IBuffer> {AID_OTHER.AsBuffer()},
                                SmartCardEmulationCategory.Other,
                                SmartCardEmulationType.Host);
```

지원 그룹당 최대 9 개의 지원 (길이 5-16 바이트)을 포함할 수 있습니다.

[**RegisterAppletIdGroupAsync**](/uwp/api/windows.devices.smartcards.smartcardemulator.registerappletidgroupasync) 메서드를 사용 하 여 [**SmartCardAppletIdGroupRegistration**](/uwp/api/windows.devices.smartcards.smartcardappletidgroupregistration) 개체를 반환 하는 시스템에 AID 그룹을 등록 합니다. 기본적으로 등록 개체의 [**ActivationPolicy**](/uwp/api/windows.devices.smartcards.smartcardappletidgroupregistration) 속성은 **Disabled**로 설정 됩니다. 즉, 시스템이 시스템에 등록 되어 있더라도 아직 사용 하도록 설정 되어 있지 않으며 트래픽을 수신 하지 않습니다.

```cppcx
reg = await SmartCardEmulator.RegisterAppletIdGroupAsync(appletIdGroup);
```

아래와 같이[**SmartCardAppletIdGroupRegistration**](/uwp/api/windows.devices.smartcards.smartcardappletidgroupregistration) 클래스의 [**RequestActivationPolicyChangeAsync**](/uwp/api/windows.devices.smartcards.smartcardappletidgroupregistration) 메서드를 사용 하 여 등록 된 카드 (지원 그룹)를 사용 하도록 설정할 수 있습니다. 시스템에서 한 번에 하나의 지불 카드만 사용할 수 있으므로 지불 지원 그룹의 [**ActivationPolicy**](/uwp/api/windows.devices.smartcards.smartcardappletidgroupregistration) 을 **사용** 으로 설정 하는 것은 기본 지불 카드를 설정 하는 것과 같습니다. 기본 지불 카드가 이미 선택 되어 있는지 여부에 관계 없이 사용자에 게이 카드를 기본 지불 카드로 허용할지 묻는 메시지가 표시 됩니다. 앱이 이미 기본 지불 응용 프로그램이 고 자체의 지원 그룹 간을 변경 하는 경우에는이 문이 적용 되지 않습니다. 앱 당 최대 10 개의 지원 그룹을 등록할 수 있습니다.

```cppcx
reg.RequestActivationPolicyChangeAsync(AppletIdGroupActivationPolicy.Enabled);
```

OS를 사용 하 여 앱의 등록 된 지원 그룹을 쿼리하고 [**GetAppletIdGroupRegistrationsAsync**](/uwp/api/windows.devices.smartcards.smartcardemulator.getappletidgroupregistrationsasync) 메서드를 사용 하 여 해당 활성화 정책을 확인할 수 있습니다.

앱이 아직 기본 지불 앱이 아닌 경우에만 사용자에 게 지불 카드의 활성화 정책을 **사용 안 함** 에서 **사용**으로 변경 하면 메시지가 표시 됩니다. 지원 충돌이 있는 경우 사용자는 지불 되지 않은 카드의 활성화 정책을 **사용 안 함** 에서 **사용** 으로 변경 하는 경우에만 메시지를 표시 합니다.

```cppcx
var registrations = await SmartCardEmulator.GetAppletIdGroupRegistrationsAsync();
    foreach (var registration in registrations)
    {
registration.RequestActivationPolicyChangeAsync (AppletIdGroupActivationPolicy.Enabled);
    }
```

### <a name="event-notification-when-activation-policy-change"></a>활성화 정책 변경 시 이벤트 알림

사용자의 백그라운드 작업에서 지원 그룹 등록 중 하나의 활성화 정책이 앱 외부에서 변경 되는 경우에 대 한 이벤트를 수신 하도록 등록할 수 있습니다. 예를 들어 사용자는 카드 중 하나에서 다른 앱이 호스팅하는 다른 카드로 NFC 설정 메뉴를 통해 기본 결제 앱을 변경할 수 있습니다. 앱에서 라이브 타일 업데이트와 같은 내부 설정에 대해이 변경 사항을 알고 있어야 하는 경우이 변경에 대 한 이벤트 알림을 수신 하 고 그에 따라 앱에서 작업을 수행할 수 있습니다.

```cppcx
var taskBuilder = new BackgroundTaskBuilder();
taskBuilder.Name = bgTaskName;
taskBuilder.TaskEntryPoint = taskEntryPoint;
taskBuilder.SetTrigger(new SmartCardTrigger(SmartCardTriggerType.EmulatorAppletIdGroupRegistrationChanged));
bgTask = taskBuilder.Register();
```

## <a name="foreground-override-behavior"></a>전경 재정의 동작

사용자에 게 묻지 않고 응용 프로그램이 전경에 있는 동안 지원 그룹 등록의 [**ActivationPolicy**](/uwp/api/windows.devices.smartcards.smartcardappletidgroupregistration) 을 **ForegroundOverride** 로 변경할 수 있습니다. 앱이 전경에 있는 동안 사용자가 터미널에 장치를 탭 하면 사용자가 기본 지불 카드로 선택한 결제 카드가 없는 경우에도 트래픽이 앱으로 라우팅됩니다. 카드의 정품 인증 정책을 **ForegroundOverride**로 변경 하는 경우이 변경은 앱이 포그라운드로 나갈 때까지 일시적 이며 사용자가 설정한 현재 기본 지불 카드를 변경 하지 않습니다. 다음과 같이 포그라운드 앱에서 지불 또는 비 지불 카드의 **ActivationPolicy** 을 변경할 수 있습니다. [**RequestActivationPolicyChangeAsync**](/uwp/api/windows.devices.smartcards.smartcardappletidgroupregistration) 메서드는 포그라운드 앱 에서만 호출할 수 있으며 백그라운드 작업에서 호출할 수 없습니다.

```cppcx
reg.RequestActivationPolicyChangeAsync(AppletIdGroupActivationPolicy.ForegroundOverride);
```

또한 단일 0 길이 지원으로 구성 된 보조 기능 그룹을 등록할 수 있습니다. 그러면 지원 및 선택 지원 명령이 수신 되기 전에 전송 된 명령 APDUs 포함 하 여 시스템에서 모든 APDUs를 라우팅하도록 합니다. 그러나 해당 보조 그룹은 **ForegroundOverride** 로만 설정할 수 있고 영구적으로 사용 하도록 설정할 수 없기 때문에 응용 프로그램이 전경에 있는 동안에만 작동 합니다. 또한이 메커니즘은 [**SmartCardEmulationType**](/uwp/api/Windows.Devices.SmartCards.SmartCardEmulationType) 열거의 **호스트** 및 **UICC** 값 모두에서 hce 백그라운드 작업에 대 한 모든 트래픽 또는 SIM 카드를 라우팅합니다.

```cppcx
public static byte[] AID_Foreground =
        {};

var appletIdGroup = new SmartCardAppletIdGroup(
                        "Example DisplayName",
                                new List<IBuffer> {AID_Foreground.AsBuffer()},
                                SmartCardEmulationCategory.Other,
                                SmartCardEmulationType.Host);
reg = await SmartCardEmulator.RegisterAppletIdGroupAsync(appletIdGroup);
reg.RequestActivationPolicyChangeAsync(AppletIdGroupActivationPolicy.ForegroundOverride);
```

## <a name="check-for-nfc-and-hce-support"></a>NFC 및 HCE 지원 확인

앱은 장치에 NFC 하드웨어가 있는지 여부를 확인 하 고, 카드 에뮬레이션 기능을 지원 하며, 사용자에 게 이러한 기능을 제공 하기 전에 호스트 카드 에뮬레이션을 지원 해야 합니다.

NFC 스마트 카드 에뮬레이션 기능은 Windows 10 Mobile 에서만 사용 하도록 설정 되어 있으므로 다른 버전의 Windows 10에서 스마트 카드 에뮬레이터 Api를 사용 하려고 하면 오류가 발생 합니다. 다음 코드 조각에서 스마트 카드 API 지원을 확인할 수 있습니다.

```cppcx
Windows.Foundation.Metadata.ApiInformation.IsTypePresent("Windows.Devices.SmartCards.SmartCardEmulator");
```

또한 [**SmartCardEmulator GetDefaultAsync**](/uwp/api/windows.devices.smartcards.smartcardemulator.getdefaultasync) 메서드가 null을 반환 하는지 확인 하 여 장치에 특정 형태의 카드 에뮬레이션을 지 원하는 NFC 하드웨어가 있는지 확인할 수 있습니다. 이 경우 장치에서 NFC 카드 에뮬레이션은 지원 되지 않습니다.

```cppcx
var smartcardemulator = await SmartCardEmulator.GetDefaultAsync();<
```

HCE 및 AID 기반 UICC 라우팅은 Lumia 730, 830, 640 및 640 XL과 같은 최근에 시작 된 장치 에서만 사용할 수 있습니다. Windows 10 Mobile을 실행 하는 모든 새 NFC 지원 장치에서 HCE를 지원 해야 합니다. 앱은 다음과 같이 HCE 지원을 확인할 수 있습니다.

```cppcx
Smartcardemulator.IsHostCardEmulationSupported();
```

## <a name="lock-screen-and-screen-off-behavior"></a>잠금 화면 및 화면 해제 동작

Windows 10 Mobile에는 모바일 운영자 또는 장치의 제조업체에서 설정할 수 있는 장치 수준 카드 에뮬레이션 설정이 있습니다. MO 또는 OEM이 이러한 값을 덮어쓰지 않는 한, 기본적으로 "탭 하 여 지불" 토글은 사용 하지 않도록 설정 되 고 "장치 수준에서 사용 정책"은 "항상"으로 설정 됩니다.

응용 프로그램은 장치 수준에서 [**Enablementpolicy**](/uwp/api/Windows.Devices.SmartCards.SmartCardEmulatorEnablementPolicy) 값을 쿼리하고 각 상태에서 원하는 앱 동작에 따라 각 사례에 대 한 작업을 수행할 수 있습니다.

```cppcx
SmartCardEmulator emulator = await SmartCardEmulator.GetDefaultAsync();

switch (emulator.EnablementPolicy)
{
case Never:
// you can take the user to the NFC settings to turn "tap and pay" on
await Windows.System.Launcher.LaunchUriAsync(new Uri("ms-settings-nfctransactions:"));
break;

 case Always:
return "Card emulation always on";

 case ScreenOn:
 return "Card emulation on only when screen is on";

 case ScreenUnlocked:
 return "Card emulation on only when screen unlocked";
}
```

휴대폰은 잠겨 있는 경우에도 앱의 백그라운드 작업을 시작 하 고 외부 판독기가 앱을 확인 하는 도움을 선택 하는 경우에만 화면이 꺼져 있습니다. 백그라운드 작업에서 판독기의 명령에 응답할 수 있지만 사용자의 입력이 필요 하거나 사용자에 게 메시지를 표시 하려는 경우 일부 인수를 사용 하 여 전경 앱을 시작할 수 있습니다. 백그라운드 작업은 다음 동작을 사용 하 여 전경 앱을 시작할 수 있습니다.

- 장치 잠금 화면에서 (사용자가 장치를 잠금 해제 한 후에만 전경 앱을 볼 수 있습니다.)
- 장치 잠금 화면 위에 (사용자가 앱을 해제 한 후 장치가 여전히 잠긴 상태)

```cppcx
        if (Windows::Phone::System::SystemProtection::ScreenLocked)
        {
            // Launch above the lock with some arguments
            var result = await eventDetails.TryLaunchSelfAsync("app-specific arguments", SmartCardLaunchBehavior.AboveLock);
        }
```

## <a name="aid-registration-and-other-updates-for-sim-based-apps"></a>SIM 기반 앱에 대 한 등록 및 기타 업데이트 지원

SIM을 secure 요소로 사용 하는 카드 에뮬레이션 앱은 Windows 서비스에 등록 하 여 SIM에서 지원 되는 보조 서비스를 선언할 수 있습니다. 이 등록은 HCE 기반 앱 등록과 매우 유사 합니다. 유일한 차이점은 SIM 기반 앱의 Uicc로 설정 해야 하는 [**SmartCardEmulationType**](/uwp/api/Windows.Devices.SmartCards.SmartCardEmulationType)입니다. 지불 카드 등록의 결과로 카드의 표시 이름이 NFC 설정 메뉴에도 채워집니다.

```cppcx
var appletIdGroup = new SmartCardAppletIdGroup(
                        "Example DisplayName",
                                new List<IBuffer> {AID_PPSE.AsBuffer()},
                                SmartCardEmulationCategory.Payment,
                                SmartCardEmulationType.Uicc);
```

> [!IMPORTANT]
> Windows Phone 8.1의 레거시 바이너리 SMS 가로채기 지원이 제거 되어 Windows 10 Mobile에서 더 광범위 한 새로운 SMS 지원으로 대체 되었지만 새로운 Windows 10 Mobile SMS Api를 사용 하기 위해에 의존 하는 모든 레거시 Windows Phone 8.1 앱이 업데이트 되어야 합니다.
