---
title: 지문 생체 인식
description: 이 문서에서는 유니버설 Windows 플랫폼 (UWP) 앱에 지문 생체 인식을 추가 하는 방법을 설명 합니다.
ms.assetid: 55483729-5F8A-401A-8072-3CD611DDFED2
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 보안
ms.localizationpriority: medium
ms.openlocfilehash: bcb82280d80467157faa8aa5195ad7b9d9a7336b
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89167217"
---
# <a name="fingerprint-biometrics"></a>지문 생체 인식




이 문서에서는 유니버설 Windows 플랫폼 (UWP) 앱에 지문 생체 인식을 추가 하는 방법을 설명 합니다. 사용자가 특정 작업에 동의 해야 하는 경우 지문 인증에 대 한 요청을 포함 하 여 앱의 보안을 강화 합니다. 예를 들어 앱 내 구매에 권한을 부여 하기 전에 또는 제한 된 리소스에 액세스 하기 전에 지문 인증을 요구할 수 있습니다. 지문 인증은 [**UserConsentVerifier**](/uwp/api/Windows.Security.Credentials.UI.UserConsentVerifier) 네임 스페이스의 클래스를 사용 하 여 관리 [**됩니다.**](/uwp/api/Windows.Security.Credentials.UI)

## <a name="check-the-device-for-a-fingerprint-reader"></a>장치에서 지문 판독기를 확인 합니다.


장치에 지문 판독기가 있는지 확인 하려면 [**UserConsentVerifier. CheckAvailabilityAsync**](/uwp/api/windows.security.credentials.ui.userconsentverifier.checkavailabilityasync)를 호출 합니다. 장치에서 지문 인증을 지 원하는 경우에도 앱은 사용자에 게 설정에서 옵션을 제공 하 여 사용 하거나 사용 하지 않도록 설정 해야 합니다.

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

## <a name="request-consent-and-return-results"></a>요청 동의 및 반환 결과


지문 검색에서 사용자 동의를 요청 하려면 [**UserConsentVerifier RequestVerificationAsync**](/uwp/api/windows.security.credentials.ui.userconsentverifier.requestverificationasync) 메서드를 호출 합니다. 지문 인증이 작동 하려면 사용자가 이전에 지문 데이터베이스에 지문 "서명"을 추가 해야 합니다.

[**UserConsentVerifier**](/uwp/api/windows.security.credentials.ui.userconsentverifier.requestverificationasync)를 호출 하면 사용자에 게 지문 검사를 요청 하는 모달 대화 상자가 표시 됩니다. 다음 그림에 표시 된 것 처럼 모달 대화 상자의 일부로 사용자에 게 표시 되는 **RequestVerificationAsync** 메서드에 메시지를 제공할 수 있습니다.

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