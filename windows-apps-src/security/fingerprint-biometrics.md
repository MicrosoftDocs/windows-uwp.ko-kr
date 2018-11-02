---
title: 지문 생체 인식
description: 이 문서에서는 UWP(Universal Windows Platform) 앱에 지문 생체 인식을 추가하는 방법을 설명합니다.
ms.assetid: 55483729-5F8A-401A-8072-3CD611DDFED2
author: PatrickFarley
ms.author: pafarley
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 보안
ms.localizationpriority: medium
ms.openlocfilehash: 6ccd2efee17c7f1205bcac6f05b016af7e6c6dae
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/01/2018
ms.locfileid: "5929679"
---
# <a name="fingerprint-biometrics"></a>지문 생체 인식




이 문서에서는 UWP(Universal Windows Platform) 앱에 지문 생체 인식을 추가하는 방법을 설명합니다. 사용자가 특정 작업에 동의해야 하는 경우 지문 인증 요청을 포함하면 앱의 보안이 향상됩니다. 예를 들어 제한된 리소스 액세스 또는 앱에서 바로 구매 권한을 부여하기 전에 지문 인증을 요구할 수 있습니다. 지문 인증은 [**Windows.Security.Credentials.UI**](https://msdn.microsoft.com/library/windows/apps/hh701356) 네임스페이스의 [**UserConsentVerifier**](https://msdn.microsoft.com/library/windows/apps/dn279134) 클래스를 사용하여 관리됩니다.

## <a name="check-the-device-for-a-fingerprint-reader"></a>장치의 지문 판독기 확인


장치에 지문 판독기가 있는지 확인하려면 [**UserConsentVerifier.CheckAvailabilityAsync**](https://msdn.microsoft.com/library/windows/apps/dn279138)를 호출합니다. 장치에서 지문 인증을 지원하는 경우에도 앱에서는 지문 인증을 사용하거나 사용하지 않도록 설정하도록 사용자에게 설정 옵션을 제공해야 합니다.

```cs
public async System.Threading.Tasks.Task<string> CheckFingerprintAvailability()
{
    string returnMessage = "";

    try
    {
        // Check the availability of fingerprint authentication.
        var ucvAvailability = await Windows.Security.Credentials.UI.UserConsentVerifier.CheckAvailabilityAsync();

        switch (ucvAvailability)
        {
            case Windows.Security.Credentials.UI.UserConsentVerifierAvailability.Available:
                returnMessage = "Fingerprint verification is available.";
                break;
            case Windows.Security.Credentials.UI.UserConsentVerifierAvailability.DeviceBusy:
                returnMessage = "Biometric device is busy.";
                break;
            case Windows.Security.Credentials.UI.UserConsentVerifierAvailability.DeviceNotPresent:
                returnMessage = "No biometric device found.";
                break;
            case Windows.Security.Credentials.UI.UserConsentVerifierAvailability.DisabledByPolicy:
                returnMessage = "Biometric verification is disabled by policy.";
                break;
            case Windows.Security.Credentials.UI.UserConsentVerifierAvailability.NotConfiguredForUser:
                returnMessage = "The user has no fingerprints registered. Please add a fingerprint to the " +
                                "fingerprint database and try again.";
                break;
            default:
                returnMessage = "Fingerprints verification is currently unavailable.";
                break;
        }
    }
    catch (Exception ex)
    {
        returnMessage = "Fingerprint authentication availability check failed: " + ex.ToString();
    }

    return returnMessage;
}
```

## <a name="request-consent-and-return-results"></a>동의 요청 및 결과 반환


지문 스캔에서 사용자 동의를 요청하려면 [**UserConsentVerifier.RequestVerificationAsync**](https://msdn.microsoft.com/library/windows/apps/dn279139) 메서드를 호출합니다. 지문 인증이 작동하려면 사용자가 이전에 지문 "서명"을 지문 데이터베이스에 추가한 상태여야 합니다.

[**UserConsentVerifier.RequestVerificationAsync**](https://msdn.microsoft.com/library/windows/apps/dn279139)를 호출하면 지문 스캔을 요청하는 모달 대화 상자가 사용자에게 표시됩니다. 다음 이미지와 같이 모달 대화 상자의 일부로 사용자에게 표시되는 메시지를 **UserConsentVerifier.RequestVerificationAsync** 메서드에 제공할 수 있습니다.

```cs
private async System.Threading.Tasks.Task<string> RequestConsent(string userMessage)
{
    string returnMessage;

    if (String.IsNullOrEmpty(userMessage))
    {
        userMessage = "Please provide fingerprint verification.";
    }

    try
    {
        // Request the logged on user's consent via fingerprint swipe.
        var consentResult = await Windows.Security.Credentials.UI.UserConsentVerifier.RequestVerificationAsync(userMessage);

        switch (consentResult)
        {
            case Windows.Security.Credentials.UI.UserConsentVerificationResult.Verified:
                returnMessage = "Fingerprint verified.";
                break;
            case Windows.Security.Credentials.UI.UserConsentVerificationResult.DeviceBusy:
                returnMessage = "Biometric device is busy.";
                break;
            case Windows.Security.Credentials.UI.UserConsentVerificationResult.DeviceNotPresent:
                returnMessage = "No biometric device found.";
                break;
            case Windows.Security.Credentials.UI.UserConsentVerificationResult.DisabledByPolicy:
                returnMessage = "Biometric verification is disabled by policy.";
                break;
            case Windows.Security.Credentials.UI.UserConsentVerificationResult.NotConfiguredForUser:
                returnMessage = "The user has no fingerprints registered. Please add a fingerprint to the " +
                                "fingerprint database and try again.";
                break;
            case Windows.Security.Credentials.UI.UserConsentVerificationResult.RetriesExhausted:
                returnMessage = "There have been too many failed attempts. Fingerprint authentication canceled.";
                break;
            case Windows.Security.Credentials.UI.UserConsentVerificationResult.Canceled:
                returnMessage = "Fingerprint authentication canceled.";
                break;
            default:
                returnMessage = "Fingerprint authentication is currently unavailable.";
                break;
        }
    }
    catch (Exception ex)
    {
        returnMessage = "Fingerprint authentication failed: " + ex.ToString();
    }

    return returnMessage;
}
```