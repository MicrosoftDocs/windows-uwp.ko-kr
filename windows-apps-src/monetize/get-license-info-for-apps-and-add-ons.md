---
author: Xansky
ms.assetid: 9630AF6D-6887-4BE3-A3CB-D058F275B58F
description: Windows.Services.Store 네임스페이스를 사용하여 현재 앱과 추가 기능에 대한 라이선스 정보를 가져오는 방법을 알아봅니다.
title: 앱과 추가 기능에 대한 라이선스 정보 가져오기
ms.author: mhopkins
ms.date: 12/04/2017
ms.topic: article
keywords: windows 10, uwp, 라이선스, 앱, 추가 기능, 앱에서 바로 구매, IAP, Windows.Services.Store
ms.localizationpriority: medium
ms.openlocfilehash: 032f2419f39e50c023e2c301b70778f421f447f8
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/30/2018
ms.locfileid: "5761850"
---
# <a name="get-license-info-for-apps-and-add-ons"></a>앱 및 추가 기능에 대한 라이선스 정보 가져오기

이 문서는 [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) 네임스페이스에 있는 [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) 클래스의 메서드를 사용하여 현재 앱과 추가 기능에 대한 라이선스 정보를 가져오는 방법을 보여줍니다. 예를 들어 이 정보를 사용하여 앱이나 추가 기능에 대한 라이선스가 활성 상태인지 또는 평가판 라이선스인지 확인할 수 있습니다.

> [!NOTE]
> **Windows.Services.Store** 네임스페이스는 Windows 10 버전, 1607에 도입되었으며 **Windows 10 Anniversary Edition(10.0, 빌드 14393)** 또는 Visual Studio의 최신 릴리스를 대상으로 하는 프로젝트에만 사용할 수 있습니다. 앱이 이전 버전의 Windows 10을 대상으로 하는 경우 **Windows.Services.Store** 네임스페이스 대신 [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx) 네임스페이스를 사용해야 합니다. 자세한 내용은 [이 문서](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md)를 참조하세요.

## <a name="prerequisites"></a>필수 구성 요소

이 예제의 필수 조건은 다음과 같습니다.
* **Windows 10 Anniversary Edition(10.0, 빌드 14393)** 이상 릴리스를 대상으로 하는 UWP(유니버설 Windows 플랫폼) 앱에 대한 Visual Studio 프로젝트.
* Windows 개발자 센터 대시보드에서 [앱 제출](https://msdn.microsoft.com/windows/uwp/publish/app-submissions)을 만들었으며, 이 앱은 Store에서 게시되었습니다. 테스트 하는 동안 스토어에서 검색이 되지 않도록 앱을 구성할 수도 있습니다. 자세한 내용은 [테스트 지침](in-app-purchases-and-trials.md#testing)을 참조하세요.
* 앱 추가 기능에 대한 라이선스 정보를 얻고 싶다면, [개발자 센터 대시보드에서 추가 기능을 생성](../publish/add-on-submissions.md)해야 합니다.

이 예제의 코드에서는 다음과 같이 가정합니다.
* 코드는 ```workingProgressRing```이라는 [ProgressRing](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.progressring.aspx)과 ```textBlock```이라는 [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx)을 포함하는 [페이지](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.page.aspx)의 컨텍스트에서 실행됩니다. 해당 개체를 사용하여 각각 비동기 작업이 발생함을 나타내고 출력 메시지를 표시합니다.
* 코드 파일에는 **Windows.Services.Store** 네임스페이스에 대한 **using** 문이 있습니다.
* 앱은 해당 앱을 실행한 사용자의 컨텍스트에서만 실행되는 단일 사용자 앱입니다. 자세한 내용은 [앱에서 바로 구매 및 평가판](in-app-purchases-and-trials.md#api_intro)을 참조하세요.

> [!NOTE]
> [데스크톱 브리지](https://developer.microsoft.com/windows/bridges/desktop)를 사용하는 데스크톱 응용 프로그램이 있는 경우 이 예에서 표시되지 않는 별도의 코드를 추가하여 [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) 개체를 구성해야 할 수도 있습니다. 자세한 내용은 [데스크톱 브리지를 사용하는 데스크톱 응용 프로그램에서 StoreContext 클래스 사용](in-app-purchases-and-trials.md#desktop)을 참조하세요.

## <a name="code-example"></a>코드 예제

현재 앱에 대한 라이선스 정보를 가져오려면 [GetAppLicenseAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getapplicenseasync) 메서드를 사용합니다. 이는 앱에 대한 라이선스 정보를 제공하는 [StoreAppLicense](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeapplicense.aspx) 개체를 반환하는 비동기 메서드로, 사용자에게 현재 앱을 사용할 수 있는 올바른 라이선스가 있는지([IsActive](https://docs.microsoft.com/uwp/api/windows.services.store.storeapplicense.isactive)), 라이선스가 평가판 버전용인지([IsTrial](https://docs.microsoft.com/uwp/api/windows.services.store.storeapplicense.istrial)) 여부를 나타내는 속성이 포함되어 있습니다.

사용할 자격이 있는 사용자가 현재 앱의 지속형 추가 기능에 대한 라이선스에 액세스하려면 [StoreAppLicense](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeapplicense.aspx) 개체의 [AddOnLicenses](https://docs.microsoft.com/uwp/api/windows.services.store.storeapplicense.addonlicenses) 속성을 사용합니다. 이 속성은 추가 기능 라이선스를 나타내는 [StoreLicense](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storelicense.aspx) 개체의 컬렉션을 반환합니다.

> [!div class="tabbedCodeSnippets"]
[!code-cs[GetLicenseInfo](./code/InAppPurchasesAndLicenses_RS1/cs/GetLicenseInfoPage.xaml.cs#GetLicenseInfo)]

전체 샘플 응용 프로그램은 [Microsoft Store 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store)을 참조하세요.

## <a name="related-topics"></a>관련 항목

* [앱에서 바로 구매 및 평가판](in-app-purchases-and-trials.md)
* [앱 및 추가 기능에 대한 제품 정보 가져오기](get-product-info-for-apps-and-add-ons.md)
* [앱에서 바로 앱 및 추가 기능 구매 사용](enable-in-app-purchases-of-apps-and-add-ons.md)
* [소모성 추가 기능 구매 사용](enable-consumable-add-on-purchases.md)
* [앱의 평가판 구현](implement-a-trial-version-of-your-app.md)
* [스토어 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store)
