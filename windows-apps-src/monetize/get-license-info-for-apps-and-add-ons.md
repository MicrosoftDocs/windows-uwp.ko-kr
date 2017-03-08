---
author: mcleanbyron
ms.assetid: 9630AF6D-6887-4BE3-A3CB-D058F275B58F
description: "Windows.Services.Store 네임스페이스를 사용하여 현재 앱과 추가 기능에 대한 라이선스 정보를 가져오는 방법을 알아봅니다."
title: "앱과 추가 기능에 대한 라이선스 정보 가져오기"
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, 라이선스, 앱, 추가 기능, 앱에서 바로 구매, IAP, Windows.Services.Store"
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 56fd2d02dc17e5bf7c3903315c685da077c05abd
ms.lasthandoff: 02/07/2017

---

# <a name="get-license-info-for-apps-and-add-ons"></a>앱 및 추가 기능에 대한 라이선스 정보 가져오기

Windows 10 버전 1607 이상을 대상으로 하는 앱은 [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) 네임스페이스에 있는 [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) 클래스의 메서드를 사용하여 현재 앱과 추가 기능(앱에서 바로 구매 제품 또는 IAP라고도 함)에 대한 라이선스 정보를 가져올 수 있습니다. 예를 들어 이 정보를 사용하여 앱이나 추가 기능에 대한 라이선스가 활성 상태인지 또는 평가판 라이선스인지 확인할 수 있습니다.

>**참고**&nbsp;&nbsp;이 문서는 Windows 10 버전 1607 이상을 대상으로 하는 앱에 적용할 수 있습니다. 앱이 이전 버전의 Windows 10을 대상으로 하는 경우 **Windows.Services.Store** 네임스페이스 대신 [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx) 네임스페이스를 사용해야 합니다. 자세한 내용은 [Windows.ApplicationModel.Store 네임스페이스를 사용하는 앱에서 바로 구매 및 평가판](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md)을 참조하세요.

## <a name="prerequisites"></a>필수 조건

이 예제의 필수 조건은 다음과 같습니다.
* Windows 10 버전 1607 이상을 대상으로 하는 UWP(유니버설 Windows 플랫폼) 앱에 대한 Visual Studio 프로젝트.
* Windows 개발자 센터 대시보드에서 앱을 만들었으며, 이 앱은 스토어에서 게시되고 사용할 수 있습니다. 고객에게 릴리스하려는 앱일 수도 있고, 테스트용으로만 사용 중인 최소 [Windows 앱 인증 키트](https://developer.microsoft.com/windows/develop/app-certification-kit) 요구 사항을 충족하는 기본 앱일 수도 있습니다. 자세한 내용은 [테스트 지침](in-app-purchases-and-trials.md#testing)을 참조하세요.

이 예제의 코드에서는 다음과 같이 가정합니다.
* 코드는 ```workingProgressRing```이라는 [ProgressRing](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.progressring.aspx)과 ```textBlock```이라는 [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx)을 포함하는 [페이지](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.page.aspx)의 컨텍스트에서 실행됩니다. 해당 개체를 사용하여 각각 비동기 작업이 발생함을 나타내고 출력 메시지를 표시합니다.
* 코드 파일에는 **Windows.Services.Store** 네임스페이스에 대한 **using** 문이 있습니다.
* 앱은 해당 앱을 실행한 사용자의 컨텍스트에서만 실행되는 단일 사용자 앱입니다. 자세한 내용은 [앱에서 바로 구매 및 평가판](in-app-purchases-and-trials.md#api_intro)을 참조하세요.

>**참고**&nbsp;&nbsp;[데스크톱 브리지](https://developer.microsoft.com/windows/bridges/desktop)를 사용하는 데스크톱 응용 프로그램이 있는 경우 이 예에서 표시되지 않는 별도의 코드를 추가하여 [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) 개체를 구성해야 할 수도 있습니다. 자세한 내용은 [데스크톱 브리지를 사용하는 데스크톱 응용 프로그램에서 StoreContext 클래스 사용](in-app-purchases-and-trials.md#desktop)을 참조하세요.

## <a name="code-example"></a>코드 예제

현재 앱에 대한 라이선스 정보를 가져오려면 [GetAppLicenseAsync](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.getapplicenseasync.aspx) 메서드를 사용합니다. 이는 앱에 대한 라이선스 정보를 제공하는 [StoreAppLicense](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeapplicense.aspx) 개체를 반환하는 비동기 메서드로, 사용자에게 앱을 사용할 수 있는 라이선스가 있는지([IsActive](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeapplicense.isactive.aspx)), 라이선스가 체험 버전용인지([IsTrial](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeapplicense.istrial.aspx)) 여부를 나타내는 속성이 포함되어 있습니다.

앱에 대한 추가 기능 라이선스를 검색 하려면 [StoreAppLicense](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeapplicense.aspx) 개체의 [AddOnLicenses](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeapplicense.addonlicenses.aspx) 속성을 사용하세요. 이 속성은 앱에 대한 추가 기능 라이선스를 나타내는 컬렉션 [StoreLicense](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storelicense.aspx) 개체를 반환합니다. 사용자에게 추가 기능을 사용할 수 있는 라이선스가 있는지 여부를 확인하려면 [IsActive](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storelicense.isactive.aspx) 속성을 사용합니다.

> [!div class="tabbedCodeSnippets"]
[!code-cs[GetLicenseInfo](./code/InAppPurchasesAndLicenses_RS1/cs/GetLicenseInfoPage.xaml.cs#GetLicenseInfo)]

전체 샘플 응용 프로그램은 [스토어 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store)을 참조하세요.

## <a name="related-topics"></a>관련 항목

* [앱에서 바로 구매 및 평가판](in-app-purchases-and-trials.md)
* [앱 및 추가 기능에 대한 제품 정보 가져오기](get-product-info-for-apps-and-add-ons.md)
* [앱에서 바로 앱 및 추가 기능 구매 사용](enable-in-app-purchases-of-apps-and-add-ons.md)
* [소모성 추가 기능 구매 사용](enable-consumable-add-on-purchases.md)
* [앱의 평가판 구현](implement-a-trial-version-of-your-app.md)
* [스토어 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store)

