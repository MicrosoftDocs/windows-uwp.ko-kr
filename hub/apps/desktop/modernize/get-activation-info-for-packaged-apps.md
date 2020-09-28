---
description: 패키지된 .NET 및 기본 C++/Win32 앱에 대한 특정 종류의 활성화 정보를 가져오는 방법을 알아봅니다.
title: 패키지된 앱에 대한 활성화 정보 가져오기
ms.date: 09/17/2020
ms.topic: article
keywords: windows 10, uwp
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.openlocfilehash: 748646ce335e9e68ee7a22131ebd305db94e9801
ms.sourcegitcommit: 5d7168ebc9f43aa13051446aff45a46600e6aafe
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/18/2020
ms.locfileid: "90799856"
---
# <a name="get-activation-info-for-packaged-apps"></a>패키지된 앱에 대한 활성화 정보 가져오기

Windows 10, 버전 1809부터 패키지된 데스크톱 앱은 [AppInstance.GetActivatedEventArgs](/uwp/api/windows.applicationmodel.appinstance.getactivatedeventargs) 메서드를 호출하여 시작 시 특정 종류의 앱 활성화 정보를 검색할 수 있습니다. 예를 들어, 이 메서드를 호출하여 파일 열기, 대화형 알림 클릭 또는 프로토콜 사용에서 앱 활성화와 관련된 정보를 얻을 수 있습니다. Windows 10, 버전 2004부터 이 기능은 [스파스 패키지](/windows/apps/desktop/modernize/grant-identity-to-nonpackaged-apps)를 사용하는 앱에서도 지원 됩니다.

> [!NOTE]
> 이 문서에 설명된 대로 [AppInstance.GetActivatedEventArgs](/uwp/api/windows.applicationmodel.appinstance.getactivatedeventargs) 메서드를 사용하여 특정 유형의 활성화 정보를 검색하는 것 외에도 COM 클래스를 정의하여 백그라운드 작업에 대한 활성화 정보를 검색할 수도 있습니다. 자세한 내용은 [winmain COM 백그라운드 작업 만들기 및 등록](/windows/uwp/launch-resume/create-and-register-a-winmain-background-task)을 참조하세요.

## <a name="code-example"></a>코드 예제

다음 코드 예제에서는 Windows Forms 앱의 **Main** 함수에서 [AppInstance.GetActivatedEventArgs](/uwp/api/windows.applicationmodel.appinstance.getactivatedeventargs) 메서드를 호출하는 방법을 보여 줍니다. 앱에서 지원하는 각 활성화 형식에 대해 `args` 반환 값을 해당하는 이벤트 args 형식에 캐스팅합니다. 이 코드 예제에서 `Handlexxx` 메서드는 다른 곳에서 정의한 전용 활성화 처리기 코드로 간주됩니다.

```csharp
static void Main()
{
    Application.EnableVisualStyles();
    Application.SetCompatibleTextRenderingDefault(false);

    var args = AppInstance.GetActivatedEventArgs();
    switch (args.Kind)
    {
        case ActivationKind.Launch:
            HandleLaunch(args as LaunchActivatedEventArgs);
            break;
        case ActivationKind.ToastNotification:
            HandleToastNotification(args as ToastNotificationActivatedEventArgs);
            break;
        case ActivationKind.VoiceCommand:
            HandleVoiceCommand(args as VoiceCommandActivatedEventArgs);
            break;
        case ActivationKind.File:
            HandleFile(args as FileActivatedEventArgs);
            break;
        case ActivationKind.Protocol:
            HandleProtocol(args as ProtocolActivatedEventArgs);
            break;
        case ActivationKind.StartupTask:
            HandleStartupTask(args as StartupTaskActivatedEventArgs);
            break;
        default:
            HandleLaunch(null);
            break;
    }
```

## <a name="supported-activation-types"></a>지원되는 활성화 유형

[AppInstance.GetActivatedEventArgs](/uwp/api/windows.applicationmodel.appinstance.getactivatedeventargs) 메서드를 사용하여 다음 표에 나열된 지원되는 이벤트 args 개체 집합에서 활성화 정보를 검색할 수 있습니다. 이러한 활성화 형식 중 일부는 패키지 매니페스트에서 패키지 확장을 사용해야 합니다.

[ShareTargetActivatedEventArgs](/uwp/api/windows.applicationmodel.activation.sharetargetactivatedeventargs) 활성화 정보는 Windows 10, 버전 2004 이상에서만 지원됩니다. 다른 모든 활성화 정보 형식은 Windows 10, 버전 1809 이상에서 지원됩니다.

| 이벤트 args 유형 | 패키지 확장 | 관련 문서 | 
|-------------------|-----------------|-----------------------|
| [ShareTargetActivatedEventArgs](/uwp/api/windows.applicationmodel.activation.sharetargetactivatedeventargs) | [uap:ShareTarget](/uwp/schemas/appxpackage/uapmanifestschema/element-uap-sharetarget) | [데스크톱 애플리케이션을 공유 대상으로 만들기](/windows/apps/desktop/modernize/desktop-to-uwp-extend#making-your-desktop-application-a-share-target) |
| [ProtocolActivatedEventArgs](/uwp/api/windows.applicationmodel.activation.protocolactivatedeventargs) | [uap:Protocol](/uwp/schemas/appxpackage/uapmanifestschema/element-uap-protocol) | [프로토콜을 사용하여 애플리케이션 시작](/windows/apps/desktop/modernize/desktop-to-uwp-extensions#start-your-application-by-using-a-protocol) |
| [ToastNotificationActivatedEventArgs](/uwp/api/windows.applicationmodel.activation.toastnotificationactivatedeventarg) | desktop:ToastNotificationActivation | [데스크톱 앱에서 알림 메시지](/windows/uwp/design/shell/tiles-and-notifications/toast-desktop-apps). |
| [StartupTaskActivatedEventArgs](/uwp/api/windows.applicationmodel.activation.startuptaskactivatedeventargs)  | desktop:StartupTask | [사용자가 Windows에 로그인할 때 실행 파일 시작](/windows/apps/desktop/modernize/desktop-to-uwp-extensions#start-an-executable-file-when-users-log-into-windows) |
| [FileActivatedEventArgs](/uwp/api/windows.applicationmodel.activation.fileactivatedeventargs) | [uap:FileTypeAssociation](/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation) | [패키지된 애플리케이션을 파일 형식 세트와 연결](/windows/apps/desktop/modernize/desktop-to-uwp-extensions#associate-your-packaged-application-with-a-set-of-file-types) |
| [VoiceCommandActivatedEventArgs](/uwp/api/windows.applicationmodel.activation.voicecommandactivatedeventargs) | 없음 | [활성화 및 음성 명령 실행 처리](/cortana/voice-commands/launch-a-foreground-app-with-voice-commands-in-cortana) |
| [LaunchActivatedEventArgs](/uwp/api/windows.applicationmodel.activation.launchactivatedeventargs) | 없음 |  |
