---
author: mcleanbyron
Description: "앱이 무료인지 여부와 상관없이, 앱 내에서 바로 콘텐츠, 기타 앱 또는 새 앱 기능(예&#58; 게임의 다음 단계 잠금 해제)을 판매할 수 있습니다. 여기서는 앱에서 이러한 제품을 사용하도록 설정하는 방법을 보여 줍니다."
title: "앱에서 바로 구매 제품 사용"
ms.assetid: D158E9EB-1907-4173-9889-66507957BD6B
keywords: in-app offer code sample
ms.sourcegitcommit: bb28828463b14130deede9f7cf796c6e32fcb48b
ms.openlocfilehash: 2e9a011a248e4c7e1d3f06064a7f82e308f07131

---

# 앱에서 바로 구매 제품 사용

\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]

앱이 무료인지 여부와 상관없이, 앱 내에서 바로 콘텐츠, 기타 앱 또는 새 앱 기능(예: 게임의 다음 단계 잠금 해제)을 판매할 수 있습니다. 여기서는 앱에서 이러한 제품을 사용하도록 설정하는 방법을 보여 줍니다.

> **참고** 앱의 평가판에서는 앱에서 바로 구매 제품을 제공할 수 없습니다. 앱 평가판을 사용하는 고객은 처음 사용자용 앱 버전을 구매할 경우 앱에서 바로 구매 제품만 구입할 수 있습니다.

## 필수 조건

-   고객이 구매하는 기능을 추가할 Windows 앱
-   새 앱에서 바로 구매 제품을 처음 코딩하고 테스트할 때는 [**CurrentApp**](https://msdn.microsoft.com/library/windows/apps/hh779765) 개체 대신 [**CurrentAppSimulator**](https://msdn.microsoft.com/library/windows/apps/hh779766) 개체를 사용해야 합니다. 이렇게 하면 라이브 서버를 호출하는 대신 라이선스 서버에 대한 호출을 시뮬레이션하여 라이선스 논리를 확인할 수 있습니다. 이렇게 하려면 %userprofile%\\AppData\\local\\packages\\&lt;package name&gt;\\LocalState\\Microsoft\\Windows Store\\ApiData에 있는 "WindowsStoreProxy.xml" 파일을 사용자 지정해야 합니다. 처음으로 앱이 실행될 때 Microsoft Visual Studio 시뮬레이터에서 이 파일을 만들거나 런타임에 사용자 지정 파일을 로드할 수도 있습니다. 자세한 내용은 [**CurrentAppSimulator**](https://msdn.microsoft.com/library/windows/apps/hh779766)를 참조하세요.
-   이 항목에서는 [스토어 샘플](http://go.microsoft.com/fwlink/p/?LinkID=627610)에 제공된 코드 예제도 참조합니다. 이 샘플은 UWP(유니버설 Windows 플랫폼) 앱에 제공된 다양한 수익 창출 옵션을 실습할 수 있는 좋은 방법입니다.

## 1단계: 앱의 라이선스 정보 초기화

앱이 초기화되는 중이면 [**CurrentApp**](https://msdn.microsoft.com/library/windows/apps/hh779765) 또는 [**CurrentAppSimulator**](https://msdn.microsoft.com/library/windows/apps/hh779766) 초기화를 통해 앱에 대한 [**LicenseInformation**](https://msdn.microsoft.com/library/windows/apps/br225157) 개체를 가져와 앱에서 바로 구매 제품 구매를 사용하도록 설정합니다.

```CSharp
void AppInit()
{
    // some app initialization functions 

    // Get the license info
    // The next line is commented out for testing.
    // licenseInformation = CurrentApp.LicenseInformation;

    // The next line is commented out for production/release.       
    licenseInformation = CurrentAppSimulator.LicenseInformation;

    // other app initialization functions
}
```

## 2단계: 앱에서 바로 판매를 앱에 추가

앱에서 바로 구매 제품을 통해 제공하려는 각 기능에 대해 판매를 만들고 앱에 추가하세요.

> **중요** 스토어에 앱을 제출하기 전에 고객에게 제공하려는 앱에서 바로 구매 제품을 앱에 모두 추가해야 합니다. 나중에 새 앱에서 바로 구매 제품을 추가하려면 앱을 업데이트하고 새 버전을 다시 제출해야 합니다.

1.  **앱에서 바로 판매 토큰 만들기**

    앱에서는 각 앱에서 바로 구매 제품을 토큰으로 식별합니다. 이 토큰은 사용자가 정의하고 앱과 스토어에서 특정 앱에서 바로 구매 제품을 식별하는 데 사용하는 문자열입니다. 코딩하는 동안 토큰이 나타내는 올바른 기능을 빨리 식별할 수 있도록 앱에서 고유하고 의미 있는 이름을 지정하세요. 다음은 이름의 몇 가지 예입니다.

    -   "SpaceMissionLevel4"
    
    -   "ContosoCloudSave"
    
    -   "RainbowThemePack"

2.  **조건부 블록에 기능 코딩**

    고객에게 이 기능을 사용할 수 있는 라이선스가 있는지 테스트하는 조건부 블록에 앱에서 바로 구매 제품과 연결된 각 기능의 코드 줄을 넣어야 합니다.

    다음 예제에서는 라이선스별 조건부 블록에 **featureName**이라는 제품 기능을 코딩하는 방법을 보여 줍니다. **featureName** 문자열은 앱에서 이 제품을 고유하게 식별하고 스토어에서 앱을 식별하는 데도 사용되는 토큰입니다.

    ```    CSharp
    if (licenseInformation.ProductLicenses["featureName"].IsActive) 
    {
        // the customer can access this feature
    } 
    else
    {
        // the customer can' t access this feature
    }
    ```

3.  **이 기능의 구매 UI 추가**

    앱에서 바로 구매 제품을 통해 제공되는 제품 또는 기능을 고객이 구매할 방법도 앱에서 제공해야 합니다. 스토어에서 전체 앱을 구매한 것과 동일한 방법으로 구매할 수는 없습니다.

    고객이 이미 앱에서 바로 구매 제품을 소유하고 있는지 테스트하고, 없는 경우 구매할 수 있도록 구매 대화 상자를 표시하는 방법은 다음과 같습니다. "show the purchase dialog" 주석을 구매 대화 상자(예: 친숙한 "앱 구매!" 단추가 있는 페이지)의 사용자 지정 코드로 바꾸세요.

    ```    CSharp
    void BuyFeature1() 
    {
        if (!licenseInformation.ProductLicenses["featureName"].IsActive)
        {
            try
            {
                // The customer doesn't own this feature, so 
                // show the purchase dialog.
                await CurrentAppSimulator.RequestProductPurchaseAsync("featureName", false);
        
                //Check the license state to determine if the in-app purchase was successful.
            }
            catch (Exception)
            {
                // The in-app purchase was not completed because 
                // an error occurred.
            }
        } 
        else
        {
            // The customer already owns this feature.
        }
    }
    ```

## 3단계: 테스트 코드를 최종 호출로 변경

이 단계에서는 앱 코드의 모든 [**CurrentAppSimulator**](https://msdn.microsoft.com/library/windows/apps/hh779766) 참조를 [**CurrentApp**](https://msdn.microsoft.com/library/windows/apps/hh779765)로 변경합니다. WindowsStoreProxy.xml 파일은 더 이상 제공할 필요가 없으므로 앱 경로에서 제거하세요. 다음 단계에서 앱에서 바로 판매를 구성할 때 참조하기 위해 저장해 둘 수도 있습니다.

## 4단계: 스토어에서 앱에서 바로 제품 판매 구성

개발자 센터 대시보드에서 제품 ID, 유형, 가격 및 앱에서 바로 구매 제품에 대한 기타 속성을 정의합니다. 테스트할 때 WindowsStoreProxy.xml에서 설정한 구성과 동일하게 구성해야 합니다. 자세한 내용은 [IAP 제출](https://msdn.microsoft.com/library/windows/apps/mt148551)을 참조하세요.

## 설명

고객에게 소모성 앱에서 바로 구매 제품 옵션(구매하고 다 사용한 후 원하는 경우 다시 구매할 수 있는 항목)을 제공하려는 경우 [앱에서 바로 소모성 제품 구매 사용](enable-consumable-in-app-product-purchases.md) 항목을 진행하세요.

확인 메일을 사용하여 사용자가 앱에서 바로 구매했는지 확인해야 할 경우 [확인 메일을 사용하여 제품 구매 검증](use-receipts-to-verify-product-purchases.md)을 검토하세요.

## 관련 항목


* [앱에서 바로 소모성 제품 구매 사용](enable-consumable-in-app-product-purchases.md)
* [앱에서 바로 구매 제품의 큰 카탈로그 관리](manage-a-large-catalog-of-in-app-products.md)
* [확인 메일을 사용하여 제품 구매 검증](use-receipts-to-verify-product-purchases.md)
* [스토어 샘플(평가판 및 앱에서 바로 구매 설명)](http://go.microsoft.com/fwlink/p/?LinkID=627610)







<!--HONumber=Jun16_HO4-->


