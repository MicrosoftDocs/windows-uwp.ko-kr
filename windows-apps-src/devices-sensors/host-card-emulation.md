---
author: msatranjr
ms.assetid: 26834A51-512B-485B-84C8-ABF713787588
title: "NFC 스마트 카드 앱 만들기"
description: "Windows Phone 8.1에서는 SIM 기반 보안 요소를 사용하여 NFC 카드 에뮬레이션 앱을 지원했지만, 해당 모델에서는 보안 결제 앱이 MNO(모바일 네트워크 운영자)와 밀접하게 결합되어야 합니다."
ms.sourcegitcommit: 62e97bdb8feb78981244c54c76a00910a8442532
ms.openlocfilehash: f47303826b9d2d2040a2bd2f2dbd5e2da3dd3cd0

---
# NFC 스마트 카드 앱 만들기

\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]

**중요** 이 항목은 Windows 10 Mobile에만 적용됩니다.

Windows Phone 8.1에서는 SIM 기반 보안 요소를 사용하여 NFC 카드 에뮬레이션 앱을 지원했지만, 해당 모델에서는 보안 결제 앱이 MNO(모바일 네트워크 운영자)와 밀접하게 결합되어야 합니다. 이는 MNO와 결합되어 있지 않은 다른 판매자 또는 개발자에 의해 가능한 결제 솔루션의 다양성을 제한합니다. Windows 10 Mobile에서는 HCE(호스트 카드 에뮬레이션)라는 새로운 카드 에뮬레이션 기술이 도입되었습니다. HCE 기술을 통해 앱이 NFC 카드 판독기와 직접 통신할 수 있습니다. 이 항목에서는 HCE(호스트 카드 에뮬레이션)가 Windows 10 Mobile 장치에서 작동하는 방식을 설명하며 MNO와 공동 작업을 하지 않고도 고객이 실제 카드 대신 휴대폰을 통해 서비스에 액세스할 수 있도록 HCE 앱을 개발하는 방법을 보여 줍니다.

## HCE 앱을 개발하는 데 필요한 사항


Windows 10 Mobile용 HCE 기반 카드 에뮬레이션 앱을 개발하려면 개발 환경을 설정해야 합니다. NFC 에뮬레이션 지원과 함께 Windows 개발자 도구 및 Windows 10 Mobile 에뮬레이터가 포함되어 있는 Microsoft Visual Studio 2015를 설치하여 환경을 설정할 수 있습니다. 설정하는 방법에 대한 자세한 내용은 [설정](https://msdn.microsoft.com/library/windows/apps/Dn726766)을 참조하세요.

선택적으로, 포함된 Windows 10 Mobile 에뮬레이터 대신 실제 Windows 10 Mobile 장치를 사용하여 테스트하려는 경우 다음 항목도 필요합니다.

-   NFC HCE 지원이 포함된 Windows 10 Mobile 장치. 현재 Lumia 730, 830, 640 및 640 XL에 NFC HCE 앱을 지원하는 하드웨어가 있습니다.
-   프로토콜 ISO/IEC 14443-4 및 ISO/IEC 7816-4를 지원하는 판독기 터미널.

Windows 10 Mobile에서는 다음 기능을 제공하는 HCE 서비스를 구현합니다.

-   앱이 에뮬레이트하려는 카드의 AID(애플릿 식별자)를 등록할 수 있습니다.
-   외부 리더 카드 선택 및 사용자 기본 설정에 따라 등록된 앱 중 하나에 대한 APDU(응용 프로그램 프로토콜 데이터 단위) 명령과 응답 쌍의 충돌 해결 및 라우팅.
-   이벤트 처리 및 사용자 작업의 결과로 앱에 알림 처리.

Windows 10은 ISO-DEP(ISO-IEC 14443-4)를 기반으로 하는 스마트 카드 에뮬레이션을 지원하며 ISO-IEC 7816-4 사양에 정의된 대로 APDU를 사용하여 통신합니다. Windows 10은 HCE 앱에 대해 ISO/IEC 14443-4 형식 A 기술을 지원합니다. 형식 B, 형식 F 및 비 ISO-DEP(예: MIFARE) 기술은 기본적으로 SIM으로 라우트됩니다.

Windows 10 Mobile 장치만 카드 에뮬레이션 기능을 통해 사용하도록 설정할 수 있습니다. SIM 기반 및 HCE 기반 카드 에뮬레이션은 다른 버전의 Windows 10에서 사용할 수 없습니다.

HCE 및 SIM 기반 카드 에뮬레이션 지원에 대한 아키텍처가 아래 다이어그램에 표시되어 있습니다. 

![](./images/nfc-architecture.png)

## 앱 선택 및 AID 라우팅

HCE 앱을 개발하려면 Windows 10 Mobile 장치에서 특정 앱으로 AID를 라우트하는 방법을 이해해야 합니다. 이는 사용자가 다양한 여러 HCE 앱을 설치할 수 있기 때문입니다. 각 앱은 여러 HCE 및 SIM 기반 카드를 등록할 수 있습니다. SIM 기반의 레거시 Windows Phone 8.1 앱은 사용자가 NFC 설정 메뉴에서 해당 기본 결제 카드로 "SIM 카드" 옵션을 선택하는 한 Windows 10 Mobile에서 계속 작동됩니다. 이는 장치를 처음 켤 때 기본적으로 설정됩니다.

사용자가 터미널에 Windows 10 Mobile 장치를 탭하는 경우 데이터가 장치에 설치된 적절한 앱으로 자동으로 라우트됩니다. 이러한 라우팅은 5-16바이트의 식별자인 AID(애플릿 ID)를 기반으로 합니다. 탭하는 동안 외부 터미널은 SELECT 명령 APDU를 전송하여 모든 후속 APDU 명령을 라우트하려는 대상 AID를 지정합니다. 후속 SELECT 명령이 라우팅을 다시 변경합니다. 앱 및 사용자 설정에서 등록한 AID에 따라, APDU 트래픽은 응답 APDU를 보낼 특정 앱으로 라우트됩니다. 터미널은 동일한 탭 동안에 다른 여러 앱과 통신하려고 할 수 있습니다. 따라서 다른 앱의 백그라운드 작업이 APDU에 응답할 여지를 제공하기 위해 비활성화될 때 되도록 빨리 앱의 백그라운드 작업이 종료되도록 해야 합니다. 백그라운드 작업은 이 항목의 뒷부분에서 설명합니다.

HCE 앱은 처리할 수 있는 특정 AID를 사용하여 직접 등록해야 하므로 AID의 APDU를 받습니다. 앱은 AID 그룹을 사용하여 AID를 선언합니다. AID 그룹은 개별 물리적 카드와 개념적으로 같습니다. 예를 들어 하나의 신용 카드는 하나의 AID 그룹을 사용하여 선언하며 다른 은행의 두 번째 신용 카드는 다른 두 번째 AID 그룹을 사용하여 선언합니다. 이들 두 카드가 동일한 AID를 갖더라도 그렇습니다.

## 결제 AID 그룹의 충돌 해결

앱이 실제 카드(AID 그룹)를 등록할 때 AID 그룹 범주를 "결제" 또는 "기타"로 선언할 수 있습니다. 지정된 시간에 여러 결제 AID 그룹이 등록되어 있을 수 있지만, 이러한 결제 AID 그룹은 한 번에 하나만 탭 및 지급에 사용할 수 있으며 이는 사용자가 선택합니다. 사용자가 터미널에 장치를 탭할 때 의도하지 않은 다른 카드로 결제되지 않도록, 사용할 단일 결제, 신용 또는 직불 카드를 의식적으로 선택할 수 있기를 기대하기 때문에 이러한 동작이 필요합니다.

그러나 "기타"로 등록된 여러 AID 그룹은 사용자 조작 없이 동시에 사용할 수 있습니다. 이는 포인트 적립, 쿠폰 또는 대중교통과 같은 기타 유형의 카드는 휴대폰을 탭할 때마다 사용자의 조작 없이 또는 사용자에게 메시지를 표시하지 않고도 작동해야 하기 때문입니다.

"결제"로 등록된 모든 AID 그룹은 사용자가 기본 결제 카드를 선택할 수 있는 NFC 설정 페이지의 카드 목록에 표시됩니다. 기본 결제 카드를 선택하면 이 결제 AID 그룹을 등록한 앱이 기본 결제 앱이 됩니다. 기본 결제 앱은 사용자 조작 없이 해당 AID 그룹을 사용하거나 사용하지 않도록 설정할 수 있습니다. 사용자가 기본 결제 앱 프롬프트를 거부하면 현재 기본 결제 앱(있는 경우)이 계속 기본값으로 유지됩니다. 다음 스크린샷은 NFC 설정 페이지를 보여 줍니다.

![](./images/nfc-settings.png)

위의 예제 스크린샷을 사용하여 사용자가 기본 결제 카드를 "HCE 응용 프로그램 1"로 등록되지 않은 다른 카드로 변경하는 경우 시스템에서 사용자의 동의를 확인하는 메시지를 표시합니다. 그러나 사용자가 기본 결제 카드를 "HCE 응용 프로그램 1"로 등록된 다른 카드로 변경하는 경우 "HCE 응용 프로그램 1"이 이미 기본 결제 앱이므로 시스템에서 사용자에게 확인 메시지를 표시하지 않습니다.

## 미결제 AID 그룹의 충돌 해결

"기타"로 분류된 미결제 카드는 NFC 설정 페이지에 표시되지 않습니다.

앱은 결제 AID 그룹과 동일한 방식으로 미결제 AID 그룹을 만들고 등록하고 사용하도록 설정할 수 있습니다. 미결제 AID 그룹의 경우 에뮬레이션 범주를 "결제"가 아니라 "기타"로 설정하는 것이 주요 차이점입니다. AID 그룹을 시스템에 등록한 후 AID 그룹을 사용하여 NFC 트래픽을 수신하도록 설정해야 합니다. 미결제 AID 그룹을 사용하여 트래픽을 수신하도록 설정하려고 할 때 기존에 다른 앱에서 시스템에 등록한 AID 중 하나와 충돌하지 않는 한 사용자에게 확인 메시지가 표시되지 않습니다. 충돌이 있는 경우 사용자가 새로 등록된 AID 그룹을 사용하도록 선택한 경우 사용하지 않도록 설정할 카드 및 해당하는 관련 앱에 관한 정보와 함께 메시지가 표시됩니다.

**SIM 기반 NFC 응용 프로그램과의 공존성**

Windows 10 Mobile에서는 시스템이 컨트롤러 계층에서 라우팅 결정을 내리는 데 사용되는 NFC 컨트롤러 라우팅 테이블을 설정합니다. 테이블에는 다음 항목에 대한 라우팅 정보가 있습니다.

-   개별 AID 경로
-   프로토콜 기반 경로(ISO-DEP)
-   기술 기반 라우팅(NFC-A/B/F)

외부 리더가 "SELECT AID" 명령을 보내는 경우 NFC 컨트롤러는 먼저 라우팅 테이블의 AID 경로가 일치하는지 확인합니다. 일치하는 항목이 없는 경우 ISO-DEP(14443-4-A) 트래픽에 대한 기본 경로로 프로토콜 기반 경로를 사용합니다. 다른 비 ISO-DEP 트래픽에 대해서는 기술 기반 라우팅을 사용합니다.

Windows 10 Mobile은 NFC 설정 페이지에서 "SIM 카드" 메뉴 옵션을 제공하여 레거시 Windows Phone 8.1 SIM 기반 앱(시스템에 해당 AID를 등록하지 않음)을 계속 사용합니다. 사용자가 기본 결제 카드로 "SIM 카드"를 선택하면 ISO-DEP 경로가 UICC로 설정됩니다. 드롭다운 메뉴의 다른 모든 선택 항목에 대해서는 ISO-DEP 경로가 호스트로 설정됩니다.

디바이스가 Windows 10 Mobile에서 처음으로 부팅될 때 ISO-DEP 경로는 SE 사용 SIM 카드가 있는 디바이스에 대해 "SIM 카드"로 설정됩니다. 사용자가 HCE 사용 앱을 설치하고 해당 앱에서 HCE AID 그룹 등록을 사용할 때 ISO-DEP 경로는 호스트로 지정됩니다. 특정 AID 경로가 컨트롤러 라우팅 테이블에 채워지도록 하려면 새 SIM 기반 응용 프로그램이 SIM에 AID를 등록해야 합니다.

## HCE 기반 앱 만들기

HCE 앱에는 두 부분이 있습니다.

-   사용자 조작을 위한 포그라운드 앱
-   지정된 AID의 APDU를 처리하기 위해 시스템에서 트리거하는 백그라운드 작업

NFC 탭에 대한 응답으로 백그라운드 작업을 로드하는 데 필요한 매우 엄격한 성능 요구 사항 때문에 전체 백그라운드 작업을 C# 또는 관리 코드보다는 C++/CX 네이티브 코드(사용하는 종속성, 참조 또는 라이브러리 포함)로 구현하는 것이 좋습니다. 일반적으로 C# 및 관리 코드가 제대로 수행되지만, .NET CLR 로드처럼 C++/CX로 작성하면 방지할 수 있는 오버헤드가 있습니다.
## 백그라운드 작업 만들기 및 등록

시스템에 의해 라우트된 APDU에 응답하고 처리하기 위해 HCE 앱에서 백그라운드 작업을 만들어야 합니다. 앱이 처음 시작되는 동안 포그라운드에서는 다음 코드에 표시된 대로 [**IBackgroundTaskRegistration**](https://msdn.microsoft.com/library/windows/apps/BR224803) 인터페이스를 구현하는 HCE 백그라운드 작업을 등록합니다.

```csharp
var taskBuilder = new BackgroundTaskBuilder();
taskBuilder.Name = bgTaskName;
taskBuilder.TaskEntryPoint = taskEntryPoint;
taskBuilder.SetTrigger(new SmartCardTrigger(SmartCardTriggerType.EmulatorHostApplicationActivated));
bgTask = taskBuilder.Register();
```

작업 트리거는 [**SmartCardTriggerType**](https://msdn.microsoft.com/library/windows/apps/Dn608017)으로 설정되어 있습니다. **EmulatorHostApplicationActivated**. 즉, SELECT AID 명령 APDU가 앱으로 확인되는 OS에 의해 수신될 때마다 백그라운드 작업이 시작됩니다.

## APDU 수신 및 응답

앱을 대상으로 하는 APDU가 있는 경우 시스템에서 백그라운드 작업이 시작됩니다. 백그라운드 작업은 [**SmartCardEmulatorApduReceivedEventArgs**](https://msdn.microsoft.com/library/windows/apps/Dn894640) 개체의 [**CommandApdu**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.smartcards.smartcardemulatorapdureceivedeventargs.commandapdu.aspx) 속성을 통해 전달된 APDU를 수신하고 동일한 개체의 [**TryRespondAsync**](https://msdn.microsoft.com/en-us/library/windows/apps/mt634299.aspx) 메서드를 사용해 APDU에 응답합니다. 성능상의 이유로 간단한 작업에는 백그라운드 작업을 유지하는 것이 좋습니다. 예를 들어 모든 처리가 완료되면 APDU에 즉시 응답하고 백그라운드 작업을 종료합니다. NFC 트랜잭션의 특성으로 인해 사용자는 매우 짧은 시간 동안만 리더에 장치를 대고 있는 경향이 있습니다. 백그라운드 작업은 연결이 비활성화될 때까지 계속 리더에서 트래픽을 수신합니다. 이 경우 [**SmartCardEmulatorConnectionDeactivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/Dn894644) 개체를 수신하게 됩니다. [**SmartCardEmulatorConnectionDeactivatedEventArgs.Reason**](https://msdn.microsoft.com/library/windows/apps/windows.devices.smartcards.smartcardemulatorconnectiondeactivatedeventargs.reason) 속성에 표시된 대로 다음과 같은 이유로 연결이 비활성화될 수 있습니다.

-   연결이 **ConnectionLost** 값으로 비활성화되는 경우 이는 사용자가 리더에서 장치를 떼었다는 의미입니다. 사용자가 터미널에 더 오래 탭해야 하는 앱인 경우 피드백을 사용하여 메시지를 표시하는 것이 좋습니다. 다시 탭하는 경우 이전 백그라운드 작업이 종료될 때까지 대기하는 동안 지연되지 않도록 신속하게(지연을 완료하여) 백그라운드 작업을 종료해야 합니다.
-   연결이 **ConnectionRedirected**로 비활성화되는 경우 이는 터미널이 다른 AID로 보내는 새로운 SELECT AID 명령 APDU를 전송했다는 의미입니다. 이 경우 앱은 다른 백그라운드 작업이 실행될 수 있도록 백그라운드 작업을 즉시(지연을 완료하여) 종료해야 합니다.

또한 백그라운드 작업은 [**IBackgroundTaskInstance interface**](https://msdn.microsoft.com/library/windows/apps/BR224797)에서 [**Canceled event**](https://msdn.microsoft.com/library/windows/apps/BR224798)에 대해 등록해야 하며, 이 이벤트가 백그라운드 작업에서 완료되면 시스템에 의해 해당 이벤트가 발생하므로 마찬가지로 신속하게(지연을 완료하여) 백그라운드 작업을 종료해야 합니다. 다음은 HCE 앱 백그라운드 작업을 보여 주는 코드입니다.

```csharp
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
## AID 그룹 만들기 및 등록

응용 프로그램을 처음 시작하는 동안 카드가 프로비전되면 AID 그룹을 만들고 시스템에 등록합니다. 시스템에서 외부 리더가 연결하여 등록 AID 및 사용자 설정에 따라 APDU를 라우트하려는 앱을 결정합니다.

대부분의 결제 카드가 추가 결제 네트워크 카드 관련 AID 함께 동일한 AID(즉, PPSE AID)에 등록합니다. 각 AID 그룹은 카드를 나타내며, 사용자가 카드를 사용하도록 설정하면 그룹의 모든 AID가 활성화됩니다. 마찬가지로, 사용자가 카드를 비활성화하면 그룹의 모든 AID 기능이 비활성화됩니다.

AID 그룹을 등록하려면 [**SmartCardAppletIdGroup**](https://msdn.microsoft.com/library/windows/apps/Dn910955) 개체를 만들고 HCE 기반 결제 카드인 것을 반영하도록 해당 속성을 설정해야 합니다. 표시 이름은 사용자 프롬프트뿐만 아니라 NFC 설정 메뉴에도 표시되므로 설명적이어야 합니다. HCE 결제 카드의 경우 [**SmartCardEmulationCategory**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.smartcards.smartcardappletidgroup.smartcardemulationcategory.aspx) 속성을 **Payment**로 설정하고 [**SmartCardEmulationType**](https://msdn.microsoft.com/library/windows/apps/windows.devices.smartcards.smartcardappletidgroup.smartcardemulationtype) 속성을 **Host**로 설정해야 합니다.

```csharp
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

미결제 HCE 카드는 [**SmartCardEmulationCategory**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.smartcards.smartcardappletidgroup.smartcardemulationcategory.aspx) 속성을 **Other**로 설정하고 [**SmartCardEmulationType**](https://msdn.microsoft.com/library/windows/apps/windows.devices.smartcards.smartcardappletidgroup.smartcardemulationtype) 속성을 **Host**로 설정해야 합니다.

```csharp
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

AID 그룹당 최대 9개의 AID(각기 5-16바이트 길이)를 포함할 수 있습니다.

[**RegisterAppletIdGroupAsync**](https://msdn.microsoft.com/library/windows/apps/Dn894656) 메서드를 사용하여 시스템에 AID 그룹을 등록합니다. 그러면 [**SmartCardAppletIdGroupRegistration**](https://msdn.microsoft.com/library/windows/apps/Dn910955registration) 개체가 반환됩니다. 기본적으로 등록 개체의 [**ActivationPolicy**](https://msdn.microsoft.com/library/windows/apps/Dn910955registration_activationpolicy) 속성은 **Disabled**로 설정됩니다. 즉, AID가 시스템에 등록되어 있는 경우에도 아직 활성화되지 않아서 트래픽을 수신하지 못합니다.

```csharp
reg = await SmartCardEmulator.RegisterAppletIdGroupAsync(appletIdGroup);
```

아래와 같이 [**SmartCardAppletIdGroupRegistration**](https://msdn.microsoft.com/library/windows/apps/Dn910955registration) 클래스의 [**RequestActivationPolicyChangeAsync**](https://msdn.microsoft.com/library/windows/apps/Dn910955registration_requestactivationpolicychangeasync) 메서드를 사용하여 등록된 카드(AID 그룹)를 사용하도록 설정할 수 있습니다. 시스템에서 한 번에 한 결제 카드만 사용하도록 설정할 수 있으므로 결제 AID 그룹의 [**ActivationPolicy**](https://msdn.microsoft.com/library/windows/apps/Dn910955registration_activationpolicy)를 **Enabled**로 설정하는 동작은 기본 결제 카드를 설정하는 동작과 같습니다. 이미 선택한 기본 결제 카드가 있는지와 상관없이 이 카드를 기본 결제 카드로 허용할 것인지를 묻는 메시지가 표시됩니다. 앱이 이미 기본 결제 응용 프로그램이며 단순히 자체 AID 그룹 사이에서 변경되고 있는 경우에는 이 내용이 해당하지 않습니다. 앱당 AID 그룹을 최대 10개 등록할 수 있습니다.

```csharp
reg.RequestActivationPolicyChangeAsync(AppletIdGroupActivationPolicy.Enabled);
```

앱의 등록된 AID 그룹을 OS에 쿼리하고 [**GetAppletIdGroupRegistrationsAsync**](https://msdn.microsoft.com/library/windows/apps/Dn894654) 메서드를 사용하여 해당하는 활성화 정책을 확인 수 있습니다.

사용 중인 앱이 아직 기본 결제 앱이 아닌 경우에만 결제 카드의 활성화 정책을 **Disabled**에서 **Enabled**로 변경하면 메시지가 나타납니다. AID 충돌이 있는 경우에만 미결제 카드의 활성화 정책을 **Disabled**에서 **Enabled**로 변경할 때 메시지가 표시됩니다.

```csharp
var registrations = await SmartCardEmulator.GetAppletIdGroupRegistrationsAsync();
    foreach (var registration in registrations)
    {
registration.RequestActivationPolicyChangeAsync (AppletIdGroupActivationPolicy.Enabled);
    }
```

**활성화 정책 변경 시 이벤트 알림**

백그라운드 작업에서, AID 그룹 등록 중 하나에 대한 활성화 정책이 앱 외부에서 변경되는 경우 이벤트를 받도록 등록할 수 있습니다. 예를 들어 NFC 설정 메뉴를 통해 사용 중인 카드 중 하나에서, 다른 앱에 호스트된 다른 카드로 기본 결제 앱을 변경할 수 있습니다. 앱에서 내부 설정에 대한 변경 사항(예: 라이브 타일 업데이트)을 인식해야 하는 경우 해당 변경 사항에 대한 이벤트 알림을 받을 수 있으며 그에 따라 앱에서 작업을 수행할 수 있습니다.

```csharp
var taskBuilder = new BackgroundTaskBuilder();
taskBuilder.Name = bgTaskName;
taskBuilder.TaskEntryPoint = taskEntryPoint;
taskBuilder.SetTrigger(new SmartCardTrigger(SmartCardTriggerType.EmulatorAppletIdGroupRegistrationChanged));
bgTask = taskBuilder.Register();
```

## 포그라운드 재정의 동작

앱이 포그라운드에 있는 동안 사용자에게 메시지를 표시하지 않고 임의의 AID 그룹 등록에 대한 [**ActivationPolicy**](https://msdn.microsoft.com/library/windows/apps/Dn910955registration_activationpolicy)를 **ForegroundOverride**로 변경할 수 있습니다. 앱이 포그라운드에 있는 동안 사용자가 터미널에 장치를 탭하면 사용자가 기본 결제 카드로 선택한 결제 카드가 없는 경우에도 트래픽이 앱으로 라우트됩니다. 카드의 활성화 정책을 **ForegroundOverride**로 변경하면 해당 변경 사항은 앱이 포그라운드에 있는 동안에만 일시적으로 유지되며 사용자가 설정한 현재 기본 결제 카드를 변경하지 않습니다. 다음과 같이 포그라운드 앱에서 결제 또는 미결제 카드의 **ActivationPolicy**를 변경할 수 있습니다. [**RequestActivationPolicyChangeAsync**](https://msdn.microsoft.com/library/windows/apps/Dn910955registration_requestactivationpolicychangeasync) 메서드는 포그라운드 앱에서만 호출할 수 있으며 백그라운드 작업에서는 호출할 수 없습니다.

```csharp
reg.RequestActivationPolicyChangeAsync(AppletIdGroupActivationPolicy.ForegroundOverride);
```

또한 길이가 0인 단일 AID로 구성된 AID 그룹을 등록할 수 있습니다. 그렇게 하면 시스템에서 AID와 상관없이 모든 APDU(SELECT AID 명령을 수신하기 전에 전송된 모든 APDU 포함)를 라우트합니다. 그러나 이러한 AID 그룹은 앱이 포그라운드에 있는 동안에만 작동합니다. **ForegroundOverride**로만 설정할 수 있고 영구적으로 사용하도록 설정할 수는 없기 때문입니다. 또한 이 메커니즘은 [**SmartCardEmulationType**](https://msdn.microsoft.com/library/windows/apps/Dn894639) 열거형 값이 **Host** 및 **UICC**인 경우 모두에 대해 작동하여 모든 트래픽을 HCE 백그라운드 작업을 또는 SIM 카드로 라우트합니다.

```csharp
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

## NFC 및 HCE 지원 확인

앱에서 장치가 NFC 하드웨어를 포함하는지, 카드 에뮬레이션 기능을 지원하는지, 그리고 해당 기능을 사용자에게 제공하기 전에 호스트 카드 에뮬레이션을 지원하는지 여부를 확인해야 합니다.

NFC 스마트 카드 에뮬레이션 기능은 Windows 10 Mobile에서만 사용하도록 설정되므로, Windows 10의 다른 버전에서 스마트 카드 에뮬레이터 API를 사용하려고 하면 오류가 발생합니다. 다음 코드 조각에서 스마트 카드 API 지원을 확인할 수 있습니다.

```csharp
Windows.Foundation.Metadata.ApiInformation.IsTypePresent("Windows.Devices.SmartCards.SmartCardEmulator");
```

또한 [**SmartCardEmulator.GetDefaultAsync**](https://msdn.microsoft.com/library/windows/apps/Dn608008) 메서드에서 Null을 반환하는지를 확인하여 디바이스에 특정 형식의 카드 에뮬레이션을 지원하는 NFC 하드웨어가 있는지를 알아볼 수 있습니다. Null을 반환하는 경우 장치에 지원되는 NFC 카드 에뮬레이션이 없는 것입니다.

```csharp
var smartcardemulator = await SmartCardEmulator.GetDefaultAsync();<
```

HCE 및 AID 기반 UICC 라우팅에 대한 지원은 Lumia 730, 830, 640 및 640 XL과 같이 최근에 출시된 장치에서 사용할 수 있습니다. Windows 10 Mobile 이상을 실행하는 새로운 NFC 지원 장치는 HCE를 지원합니다. 앱에서 다음과 같이 HCE 지원을 확인할 수 있습니다.

```csharp
Smartcardemulator.IsHostCardEmulationSupported();
```

## 화면 잠금 및 화면 끄기 동작

Windows 10 Mobile에는 장치 수준 카드 에뮬레이션 설정이 있습니다. 이 설정은 통신사 또는 장치 제조업체에서 지정할 수 있습니다. 기본적으로 "탭하여 결제" 토글은 사용되지 않으며, MO 또는 OEM이 이러한 값을 덮어쓰지 않는 한 "디바이스 수준에서 사용 정책"은 "항상"으로 설정됩니다.

응용 프로그램은 장치 수준에서 [**EnablementPolicy**](https://msdn.microsoft.com/library/windows/apps/Dn608006)의 값을 쿼리하고, 각 상태에서 앱의 원하는 동작에 따라 각 경우에 대한 작업을 수행할 수 있습니다.

```csharp
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

휴대폰이 잠기거나 화면이 꺼져 있는 경우에도 외부 리더에서 앱으로 확인되는 AID를 선택하기만 하면 앱의 백그라운드 작업이 시작됩니다. 백그라운드 작업에서 리더의 명령에 응답할 수 있지만 사용자의 입력이 필요하거나 사용자에게 메시지를 표시하려는 경우 몇 가지 인수를 사용하여 포그라운드 앱을 시작할 수 있습니다. 백그라운드 작업에서 다음 동작을 사용하여 포그라운드 앱을 시작할 수 있습니다.

-   장치 잠금 화면 아래에서(사용자가 장치를 잠금 해제한 후에만 포그라운드 앱이 표시됨)
-   장치 잠금 화면 위에서(사용자가 앱을 해제한 후 장치는 여전히 잠긴 상태에 있음)

```csharp
        if (Windows::Phone::System::SystemProtection::ScreenLocked)
        {
            // Launch above the lock with some arguments
            var result = await eventDetails.TryLaunchSelfAsync("app-specific arguments", SmartCardLaunchBehavior.AboveLock);
        } 
```

## AID 등록 및 SIM 기반 앱에 대한 기타 업데이트

SIM을 보안 요소로 사용하는 카드 에뮬레이션 앱에서 Windows 서비스에 등록하여 SIM에서 지원되는 AID를 선언할 수 있습니다. 이 등록은 HCE 기반 앱 등록과 매우 비슷합니다. 유일한 차이점은 [**SmartCardEmulationType**](https://msdn.microsoft.com/library/windows/apps/Dn894639)이며, 이 설정은 SIM 기반 앱에 대한 UICC로 지정해야 합니다. 결제 카드 등록의 결과로, 카드의 표시 이름은 NFC 설정 메뉴에도 표시됩니다.

```csharp
var appletIdGroup = new SmartCardAppletIdGroup(
                        "Example DisplayName", 
                                new List<IBuffer> {AID_PPSE.AsBuffer()},
                                SmartCardEmulationCategory.Payment,
                                SmartCardEmulationType.Uicc);
```

** 중요 **  
Windows Phone 8.1에서 레거시 이진 SMS 가로채기 지원이 제거되었으며 Windows 10 Mobile에서는 더 광범위한 새로운 SMS 지원으로 바뀌었지만 해당 지원을 사용하는 모든 레거시 Windows Phone 8.1 앱은 새로운 Windows 10 Mobile SMS API를 사용하도록 업데이트해야 합니다.






<!--HONumber=Jun16_HO5-->


