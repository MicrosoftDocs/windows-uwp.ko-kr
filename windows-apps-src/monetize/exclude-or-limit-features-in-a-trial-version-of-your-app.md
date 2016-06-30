---
author: mcleanbyron
Description: "평가 기간 동안 고객이 앱을 무료로 사용할 수 있게 하는 경우 평가 기간 동안 일부 기능을 제외하거나 제한하여 고객이 앱 정식 버전으로 업그레이드하도록 유도할 수 있습니다."
title: "평가판의 기능 제외 또는 제한"
ms.assetid: 1B62318F-9EF5-432A-8593-F3E095CA7056
keywords: free trial code sample
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 9c38784325f4dc51052f70a819012508f2a0bdbb

---

# 평가판의 기능 제외 또는 제한


\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]

평가 기간 동안 고객이 앱을 무료로 사용할 수 있게 하는 경우 평가 기간 동안 일부 기능을 제외하거나 제한하여 고객이 앱 정식 버전으로 업그레이드하도록 유도할 수 있습니다. 코딩을 시작하기 전에 제한할 기능을 결정한 다음 정식 라이선스를 구입한 다음에만 해당 기능이 작동하도록 해야 합니다. 또한 고객이 앱을 구매하기 전 체험 기간 동안에만 표시되는 배너 또는 워터마크와 같은 기능을 사용하도록 설정할 수도 있습니다.

앱에 체험 버전을 추가하는 방법을 살펴보겠습니다.

## 필수 조건

고객이 구매하는 기능을 추가할 Windows 앱

## 1단계: 체험 기간 중에 사용하거나 사용하지 않도록 설정할 기능 선택

앱의 현재 라이선스 상태는 [**LicenseInformation**](https://msdn.microsoft.com/library/windows/apps/br225157) 클래스의 속성으로 저장됩니다. 일반적으로 다음 단계의 설명과 같이 라이선스 상태를 사용하는 기능을 조건부 블록에 넣습니다. 이러한 기능을 고려할 때 모든 라이선스 상태에서 작동하도록 구현할 수 있는지 확인하세요.

또한 앱이 실행되는 동안 앱의 라이선스 변경을 처리하는 방법을 결정하세요. 체험 앱에서 전체 기능을 제공할 수 있지만 구매한 버전에는 없는 앱에서 바로 구매 광고 배너가 있습니다. 또는 체험 앱에서 특정 기능을 사용하지 않도록 설정하거나 사용자에게 구매를 권유하는 메시지를 주기적으로 표시할 수 있습니다.

만들려는 앱의 유형과 적합한 체험 또는 만료 전략을 생각해 보세요. 게임 체험 버전의 경우 사용자가 플레이할 수 있는 게임 콘텐츠 양을 제한하는 것이 좋습니다. 유틸리티 체험 버전의 경우 만료 날짜를 설정하거나 잠재 구매자가 사용할 수 있는 기능을 제한할 수 있습니다.

게임이 아닌 대부분의 앱은 사용자가 전체 앱에 대해 잘 알 수 있으므로 만료 날짜 설정이 잘 작동합니다. 다음은 일반적인 만료 시나리오 및 이를 처리하는 옵션입니다.

-   **앱이 실행 중일 때 체험 라이선스 만료**

    앱이 실행 중일 때 체험이 만료되는 경우 앱은 다음을 할 수 있습니다.

    -   아무것도 하지 않습니다.
    -   고객에게 메시지를 표시합니다.
    -   닫힙니다.
    -   고객에게 앱을 구입할지 묻는 메시지를 표시합니다.

    앱을 구매할지 묻는 메시지를 표시하는 것이 좋습니다. 고객이 앱을 구매하면 계속해서 모든 기능을 사용할 수 있습니다. 사용자가 앱을 구매하지 않는 경우 앱을 닫거나 정기적으로 앱을 구매하도록 알립니다.

-   **앱을 실행하기 전에 체험 라이선스 만료**

    사용자가 앱을 실행하기 전에 체험이 만료되는 경우 앱은 실행되지 않습니다. 대신 스토어에서 앱을 구입할 수 있는 옵션을 제공하는 대화 상자가 표시됩니다.

-   **앱이 실행 중일 때 고객이 앱 구입**

    앱이 실행 중일 때 고객이 앱을 구입하는 경우 앱이 수행할 수 있는 몇 가지 작업은 다음과 같습니다.

    -   아무것도 하지 않고 고객이 앱을 다시 시작할 때까지 계속 체험 모드로 실행됩니다.
    -   구매에 대한 감사 인사를 표시하거나 메시지를 표시합니다.
    -   정식 라이선스에서 사용 가능한 기능을 자동으로 사용하도록 설정하거나 체험 버전 전용 알림을 사용하지 않도록 설정합니다.

앱에서 라이선스 변경을 감지하고 조치를 취하도록 하려면 다음 단계에서 설명하는 대로 이를 위한 이벤트 처리기를 추가해야 합니다.
## 2단계: 라이선스 정보 초기화

앱을 초기화하는 경우 이 예에 표시된 대로 앱의 [**LicenseInformation**](https://msdn.microsoft.com/library/windows/apps/br225157) 개체를 가져옵니다. **licenseInformation**은 **LicenseInformation** 유형의 전역 변수 또는 필드로 가정됩니다.

[
            **CurrentApp**](https://msdn.microsoft.com/library/windows/apps/hh779765) 또는 [**CurrentAppSimulator**](https://msdn.microsoft.com/library/windows/apps/hh779766)를 초기화하여 앱의 라이선스 정보에 액세스합니다.

```CSharp
void initializeLicense()
{
    // Initialize the license info for use in the app that is uploaded to the Store.
    // uncomment for release
    //   licenseInformation = CurrentApp.LicenseInformation;

    // Initialize the license info for testing.
    // comment the next line for release
    licenseInformation = CurrentAppSimulator.LicenseInformation;
      
}
```

앱이 실행되는 동안 라이선스가 변경될 경우 알림을 받는 이벤트 처리기를 추가합니다. 예를 들어 체험 기간이 만료되거나 고객이 스토어를 통해 앱을 구매하면 앱의 라이선스가 변경될 수 있습니다.

```CSharp
void InitializeLicense()
{
    // Initialize the license info for use in the app that is uploaded to the Store.
    // uncomment for release
    //   licenseInformation = CurrentApp.LicenseInformation;

    // Initialize the license info for testing.
    // comment the next line for release
    licenseInformation = CurrentAppSimulator.LicenseInformation;

    // Register for the license state change event.
     licenseInformation.LicenseChanged += new LicenseChangedEventHandler(licenseChangedEventHandler);

}

// ...

void licenseChangedEventHandler()
{
    ReloadLicense(); // code is in next steps
}
```

## 3단계: 조건부 블록에 기능 코딩

라이선스 변경 이벤트가 발생할 경우 앱이 라이선스 API를 호출하여 체험 상태가 변경되었는지 확인해야 합니다. 이 단계의 코드는 이 이벤트의 처리기를 구성하는 방법을 보여 줍니다. 이때 사용자가 앱을 구매한 경우 라이선스 상태가 변경되었다는 피드백을 사용자에게 제공하는 것이 좋습니다. 코딩 방식에 따라 앱을 다시 시작하라는 메시지를 사용자에게 표시해야 할 수도 있습니다. 그러나 이러한 전환이 가능한 한 매끄럽고 불편 없이 진행되도록 하세요.

이 예에서는 앱의 라이선스 상태를 평가하고, 그에 따라 앱의 기능을 사용하거나 사용하지 않도록 설정할 수 있는 방법을 보여 줍니다.

```CSharp
void ReloadLicense()
{
    if (licenseInformation.IsActive)
    {
         if (licenseInformation.IsTrial)
         {
             // Show the features that are available during trial only.
         }
         else
         {
             // Show the features that are available only with a full license.
         }
     }
     else
     {
         // A license is inactive only when there' s an error.
     }
}
```

## 4단계: 앱의 체험 만료 날짜 확인

앱의 체험 만료 날짜를 확인하는 코드를 포함합니다.

이 예의 코드는 앱 체험 라이선스의 만료 날짜를 가져오는 함수를 정의합니다. 라이선스가 유효하면 만료 날짜가 체험 만료 시까지 남은 일수와 함께 표시됩니다.

```CSharp
void DisplayTrialVersionExpirationTime()
{
    if (licenseInformation.IsActive)
    {
        if (licenseInformation.IsTrial)
        {
            var longDateFormat = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter("longdate");
                                                
            // Display the expiration date using the DateTimeFormatter. 
            // For example, longDateFormat.Format(licenseInformation.ExpirationDate)

            var daysRemaining = (licenseInformation.ExpirationDate - DateTime.Now).Days;

            // Let the user know the number of days remaining before the feature expires
        }
        else
        {
            // ...
        }
    }
    else
    {
       // ...
    }
}
```

## 5단계: 라이선스 API 호출을 시뮬레이션하여 기능 테스트

이제 라이선스 API 호출을 시뮬레이션하여 앱을 테스트하세요. JavaScript, C#, Visual Basic 또는 Visual C++에서 앱의 초기화 코드에 있는 [**CurrentApp**](https://msdn.microsoft.com/library/windows/apps/hh779765) 참조를 [**CurrentAppSimulator**](https://msdn.microsoft.com/library/windows/apps/hh779766)로 바꾸세요.

[
            **CurrentAppSimulator**](https://msdn.microsoft.com/library/windows/apps/hh779766)는 %userprofile%\\AppData\\local\\packages\\&lt;package name&gt;\\LocalState\\Microsoft\\Windows Store\\ApiData에 있는 "WindowsStoreProxy.xml"이라는 XML 파일에서 테스트 관련 라이선스 정보를 가져옵니다. 이 경로와 파일이 없는 경우 설치 중에 또는 런타임에 만들어야 합니다. 이 특정 위치에 WindowsStoreProxy.xml이 없는 상태에서 [**CurrentAppSimulator.LicenseInformation**](https://msdn.microsoft.com/library/windows/apps/hh779768) 속성에 액세스하면 오류가 발생합니다.

이 예에서는 앱에 코드를 추가하여 다른 라이선스 상태에서 테스트할 수 있는 방법을 보여 줍니다.

```CSharp
void appInit()
{
    // some app initialization functions

    // Initialize the license info for use in the app that is uploaded to the Store.
    // uncomment for release
    //   licenseInformation = CurrentApp.LicenseInformation;

    // Initialize the license info for testing.
    // comment the next line for release
    licenseInformation = CurrentAppSimulator.LicenseInformation;

    // other app initialization functions
}
```

WindowsStoreProxy.xml을 편집하여 앱과 해당 기능에 대해 시뮬레이션된 만료 날짜를 변경할 수 있습니다. 가능한 만료 및 라이선스 구성을 모두 테스트하여 모든 것이 예상대로 작동하는지 확인하세요.

## 6단계: 시뮬레이션된 라이선스 API 메서드를 실제 API로 바꾸기

시뮬레이션된 라이선스 서버로 앱을 테스트한 후, 인증을 위해 앱을 스토어로 제출하기 전에 다음 코드 샘플과 같이 [**CurrentAppSimulator**](https://msdn.microsoft.com/library/windows/apps/hh779766)를 [**CurrentApp**](https://msdn.microsoft.com/library/windows/apps/hh779765)으로 바꾸세요.

**중요** 앱을 스토어로 제출할 때 앱에서 [**CurrentApp**](https://msdn.microsoft.com/library/windows/apps/hh779765) 개체를 사용해야 합니다. 그러지 않으면 앱 인증에 실패합니다.

```CSharp
void appInit()
{
    // some app initialization functions

    // Initialize the license info for use in the app that is uploaded to the Store.
    // uncomment for release
    licenseInformation = CurrentApp.LicenseInformation;

    // Initialize the license info for testing.
    // comment the next line for release
    //   licenseInformation = CurrentAppSimulator.LicenseInformation;

    // other app initialization functions
}
```

## 7단계: 고객에게 무료 체험 버전이 작동하는 방식 설명

고객이 앱의 동작에 놀라지 않도록 무료 체험 기간 동안 및 이후에 앱이 어떻게 동작하는지 고객에게 설명해야 합니다.

앱 설명 지정에 대한 자세한 내용은 [앱 설명 작성](https://msdn.microsoft.com/library/windows/apps/mt148529)을 참조하세요.

## 관련 항목

* [스토어 샘플(평가판 및 앱에서 바로 구매 설명)](http://go.microsoft.com/fwlink/p/?LinkID=627610)
* [앱 가격 책정 및 가용성 설정](https://msdn.microsoft.com/library/windows/apps/mt148548)
* [**CurrentApp**](https://msdn.microsoft.com/library/windows/apps/hh779765)
* [**CurrentAppSimulator**](https://msdn.microsoft.com/library/windows/apps/hh779766)
 

 







<!--HONumber=Jun16_HO4-->


