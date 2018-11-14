---
author: joannaleecy
title: 소매 데모 (RDX) 기능을 앱 추가
description: 소매 판매 바닥에 앱을 표시 하는 데 도움이 소매 데모 모드에 대 한 앱을 준비 합니다.
ms.assetid: f83f950f-7fdd-4f18-8127-b92a8f400061
ms.author: joanlee
ms.date: 10/02/2018
ms.topic: article
keywords: windows 10, uwp, 소매 데모 앱
ms.localizationpriority: medium
ms.openlocfilehash: ee0344d521b4c31449a2291517b20a09280818fc
ms.sourcegitcommit: 4d88adfaf544a3dab05f4660e2f59bbe60311c00
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/13/2018
ms.locfileid: "6455666"
---
# <a name="add-retail-demo-rdx-features-to-your-app"></a>소매 데모 (RDX) 기능을 앱 추가

고객에 게 Pc 및 장치를 판매 바닥 사용해에서 바로 이동할 수 있도록 Windows 앱에 소매 데모 모드를 포함 합니다.

고객은 소매점에서 Pc 및 디바이스의 데모를 사용해 수를 기대 합니다. 종종 했느냐가의 [소매 데모 환경 (RDX)](https://docs.microsoft.com/windows-hardware/customize/desktop/retail-demo-experience)통해 앱을 사용 하 여 수행해 시간입니다.

_일반_ 또는 _정품_ 모드에 있는 동안 다양 한 환경을 제공 하기 위해 앱을 설정할 수 있습니다. 예를 들어 설치 프로세스를 사용 하 여 앱을 시작 하는 경우 정품 모드에 건너뛸 것 지난을에서 바로 이동할 수 있도록 샘플 데이터 및 기본 설정 사용 하 여 앱을 미리 채울 수 해야 합니다.

고객의 관점에서 한 앱에만 있습니다. 두 가지 모드를 구별 하는 고객을 위해 권장 앱 정품 모드에 있는 동안가 표시 되는지 단어 "소매" 눈에 띄게 제목 표시줄 또는 적절 한 위치입니다.

앱에 대 한 Microsoft Store 요구 사항 외에도 RDX 인식 앱도와 호환 되어야 RDX 설치, 삭제 및 업데이트 프로세스는 소매점에서 고객이 일관 되 게 긍정적인 경험 있는지 확인 합니다.

## <a name="design-principles"></a>디자인 원칙

* **최고를 표시**합니다. 소매 데모 환경을 사용 하 여 뛰어난 앱. 이 처음으로 고객에 게 것일 하므로 최상의 조각을 표시 앱!

* **빠른 표시**합니다. 고객은 참을성이 부족할 수 있습니다. 고객이 앱의 진정한 가치를 더 빨리 경험할수록 더 좋습니다.

* **스토리를 간단 하 게 유지**합니다. 소매 데모 환경은 앱의 가치를 요약해 합니다.

* **경험에 집중**합니다. 사용자에게 콘텐츠를 소화할 시간을 줍니다. 최고의 부분을 빠르게 파악하도록 하는 것도 중요하지만, 적절하게 멈출 수 있는 부분을 추가하면 사용자가 충분히 즐기면서 경험할 수 있습니다.

## <a name="technical-requirements"></a>기술 요구 사항

인식 RDX 앱은 소매 고객에 게 앱의 선보여 것 이므로, 기술 요구 사항을 충족 하 고 모든 소매 데모 환경 앱에 대 한 Microsoft Store는 개인 정보 보호 규정을 준수 해야 있습니다.

이 유효성 검사 프로세스를 준비 하는 데 도움이 하 고 테스트 프로세스에서 명확성을 제공 하는 검사 목록을으로 사용할 수 있습니다. 앱이 소매 데모 장치에서 실행되는 한, 유효성 검사 프로세스에서는 물론 소매 데모 환경 앱의 전체 수명 동안 이러한 요구 사항을 유지 관리해야 합니다.

### <a name="critical-requirements"></a>중요 한 요구 사항

가능한 한 빨리 중요 한 이러한 요구 사항을 충족 하지 않는 RDX 인식 앱 모든 소매 데모 장치에서 제거 됩니다.

* **(Pii 개인) 개인 식별이 가능한 정보에 대 한 요청 하지 않도록**합니다. 이때 로그인 정보, Microsoft 계정 정보 또는 연락처 세부 정보입니다.

* **오류가 발생**합니다. 앱이 오류 없이 실행되어야 합니다. 또한 소매 데모 장치를 사용하는 고객에게 오류나 알림이 표시되어서는 안 됩니다. 오류 부정적인 영향을 미친다는 앱 자체, 브랜드, 디바이스의 브랜드, 장치 제조업체 브랜드 및 Microsoft 브랜드 합니다.

* **유료 앱 평가판 모드가 있어야**합니다. 앱은 하거나 무료 이거나 [평가판 모드](https://msdn.microsoft.com/windows/uwp/monetize/exclude-or-limit-features-in-a-trial-version-of-your-app)를 포함 해야 합니다. 고객은 소매점에서 사용하는 앱에 대해 비용을 부담하려고 하지 않습니다.

### <a name="high-priority-requirements"></a>우선 순위가 높은 요구 사항

이러한 우선 순위가 높은 요구 사항을 충족 하지 않는 RDX 인식 앱 해결 하기 위해 즉시 확인 해야 합니다. 즉시 수정할 수 없는 경우 모든 소매 데모 장치에서 이 앱을 제거할 수 있습니다.

* **오프 라인 경험 Memorable**합니다. 앱은 장치의 50% 소매점 오프 라인으로 뛰어난 오프 라인 경험을 보여 주기 위해 해야 합니다. 이는 오프라인으로 앱을 조작하는 고객도 의미 있고 긍정적인 경험을 하도록 하려는 것입니다.

* **업데이트 된 콘텐츠 환경**입니다. 앱은 온라인 때 업데이트 확인 될 수 없습니다. 업데이트가 필요한 경우 자동으로 수행 되어야 합니다.

* **익명 통신 없음**입니다. 소매 데모 장치를 사용 하는 고객은 익명 사용자 이기 때문에 장치에서 메시지 콘텐츠를 공유할 수 있게 해야 합니다.

* **정리 프로세스를 사용 하 여 일관 된 환경을 제공**합니다. 소매 데모 장치를 이용하는 모든 고객은 동일한 경험을 해야 합니다. 앱은 사용할 때마다 후 동일한 기본 상태로 되돌리려면 [정리 프로세스](#clean-up-process) 사용 해야 합니다. 참조는 마지막 고객이 남긴 내용을 다음 고객이 싶지는 않을 것입니다. 여기에는 스코어보드, 도전 과제, 잠금 해제 등이 포함됩니다.

* **연령에 맞는 콘텐츠**입니다. 모든 앱 콘텐츠 되어야 할 청소년 이하의 평점 범주를 할당 합니다. 자세한 내용을 보려면 [IARC에서 비례 하는 앱 보기](https://www.globalratings.com/for-developers.aspx) 및 [ESRB 평점](https://www.esrb.org/ratings/ratings_guide.aspx)을 참조 하세요.

### <a name="medium-priority-requirements"></a>보통 우선 순위 요구 사항

이 문제의 해결 방법에 대해 논의하기 위해 Windows Retail Store 팀에서 개발자에게 직접 연락할 수 있습니다.

* **다양 한 장치 성공적으로 실행 하는 기능**입니다. 앱은 저급 사양의 장치를 비롯 한 모든 장치에서 잘 실행 되어야 합니다. 앱이 최소 사양에 맞지 않는 장치에 설치 된 경우 앱이에 대 한 사용자에 게 알리는 명확 하 게 해야 합니다. 앱이 항상 고성능으로 실행될 수 있도록 최소 장치 사양을 알려야 합니다.

* **소매 스토어 앱 크기 요구 사항을 충족**합니다. 앱은 800MB 미만이어야 합니다. 인식 RDX 앱 크기 요구 사항에 맞지 않는 경우 직접 대 한 자세한 내용은 Windows Retail Store 팀에 문의 합니다.

## <a name="retailinfo-api-preparing-your-code-for-demo-mode"></a>데모 모드에 대 한 코드를 준비 하는 RetailInfo API:

### <a name="isdemomodeenabled"></a>IsDemoModeEnabled
앱을 실행할 코드 경로 지정 하는 Windows 10 SDK에서 [Windows.System.Profile](https://docs.microsoft.com/uwp/api/windows.system.profile) 네임 스페이스의 일부인 [**RetailInfo**](https://docs.microsoft.com/uwp/api/Windows.System.Profile.RetailInfo) 유틸리티 클래스에서 [**IsDemoModeEnabled**](https://docs.microsoft.com/uwp/api/windows.system.profile.retailinfo.isdemomodeenabled) 속성 부울 표시기로 사용 _법선을 _모드 또는 _정품_ 모드.

``` csharp
using Windows.Storage;

StorageFolder folder = ApplicationData.Current.LocalFolder;

if (Windows.System.Profile.RetailInfo.IsDemoModeEnabled) 
{
    // Use the demo specific directory
    folder = await folder.GetFolderAsync(“demo”);
}

StorageFile file = await folder.GetFileAsync(“hello.txt”);
// Now read from file
```

``` cpp
using namespace Windows::Storage;

StorageFolder^ localFolder = ApplicationData::Current->LocalFolder;

if (Windows::System::Profile::RetailInfo::IsDemoModeEnabled) 
{
    // Use the demo specific directory
    create_task(localFolder->GetFolderAsync(“demo”).then([this](StorageFolder^ demoFolder)
    {
        return demoFolder->GetFileAsync(“hello.txt”);
    }).then([this](task<StorageFile^> fileTask)
    {
        StorageFile^ file = fileTask.get();
    });
    // Do something with file
}
else
{
    create_task(localFolder->GetFileAsync(“hello.txt”).then([this](StorageFile^ file)
    {
        // Do something with file
    });
}
```

``` javascript
if (Windows.System.Profile.retailInfo.isDemoModeEnabled) {
    console.log(“Retail mode is enabled.”);
} else {
    Console.log(“Retail mode is not enabled.”);
}
```

### <a name="retailinfoproperties"></a>RetailInfo.Properties

[**IsDemoModeEnabled**](https://docs.microsoft.com/uwp/api/windows.system.profile.retailinfo.isdemomodeenabled)가 true를 반환하면, 보다 사용자 지정 가능한 소매 데모 환경을 구축하기 위해 [**RetailInfo.Properties**](https://docs.microsoft.com/uwp/api/windows.system.profile.retailinfo.properties)를 사용하여 장치에 대한 속성 집합을 쿼리할 수 있습니다. 이러한 속성에는 [**ManufacturerName**](https://docs.microsoft.com/uwp/api/windows.system.profile.knownretailinfoproperties.manufacturername), [**Screensize**](https://docs.microsoft.com/uwp/api/windows.system.profile.knownretailinfoproperties.screensize), [**Memory**](https://docs.microsoft.com/uwp/api/windows.system.profile.knownretailinfoproperties.memory) 등이 포함됩니다.

```csharp
using Windows.UI.Xaml.Controls;
using Windows.System.Profile

TextBlock priceText = new TextBlock();
priceText.Text = RetailInfo.Properties[KnownRetailInfo.Price];
// Assume infoPanel is a StackPanel declared in XAML
this.infoPanel.Children.Add(priceText);
```

```cpp
using namespace Windows::UI::Xaml::Controls;
using namespace Windows::System::Profile;

TextBlock ^manufacturerText = ref new TextBlock();
manufacturerText.set_Text(RetailInfo::Properties[KnownRetailInfoProperties::Price]);
// Assume infoPanel is a StackPanel declared in XAML
this->infoPanel->Children->Add(manufacturerText);
```

```javascript
var pro = Windows.System.Profile;
console.log(pro.retailInfo.properties[pro.KnownRetailInfoProperties.price);
```

#### <a name="idl"></a>IDL

```
//  Copyright (c) Microsoft Corporation. All rights reserved.
//
//  WindowsRuntimeAPISet

import "oaidl.idl";
import "inspectable.idl";
import "Windows.Foundation.idl";
#include <sdkddkver.h>

namespace Windows.System.Profile
{
    runtimeclass RetailInfo;
    runtimeclass KnownRetailInfoProperties;

    [version(NTDDI_WINTHRESHOLD), uuid(0712C6B8-8B92-4F2A-8499-031F1798D6EF), exclusiveto(RetailInfo)]
    [version(NTDDI_WINTHRESHOLD, Platform.WindowsPhone)]
    interface IRetailInfoStatics : IInspectable
    {
        [propget] HRESULT IsDemoModeEnabled([out, retval] boolean *value);
        [propget] HRESULT Properties([out, retval, hasvariant] Windows.Foundation.Collections.IMapView<HSTRING, IInspectable *> **value);
    }

    [version(NTDDI_WINTHRESHOLD), uuid(50BA207B-33C4-4A5C-AD8A-CD39F0A9C2E9), exclusiveto(KnownRetailInfoProperties)]
    [version(NTDDI_WINTHRESHOLD, Platform.WindowsPhone)]
    interface IKnownRetailInfoPropertiesStatics : IInspectable
    {
        [propget] HRESULT RetailAccessCode([out, retval] HSTRING *value);
        [propget] HRESULT ManufacturerName([out, retval] HSTRING *value);
        [propget] HRESULT ModelName([out, retval] HSTRING *value);
        [propget] HRESULT DisplayModelName([out, retval] HSTRING *value);
        [propget] HRESULT Price([out, retval] HSTRING *value);
        [propget] HRESULT IsFeatured([out, retval] HSTRING *value);
        [propget] HRESULT FormFactor([out, retval] HSTRING *value);
        [propget] HRESULT ScreenSize([out, retval] HSTRING *value);
        [propget] HRESULT Weight([out, retval] HSTRING *value);
        [propget] HRESULT DisplayDescription([out, retval] HSTRING *value);
        [propget] HRESULT BatteryLifeDescription([out, retval] HSTRING *value);
        [propget] HRESULT ProcessorDescription([out, retval] HSTRING *value);
        [propget] HRESULT Memory([out, retval] HSTRING *value);
        [propget] HRESULT StorageDescription([out, retval] HSTRING *value);
        [propget] HRESULT GraphicsDescription([out, retval] HSTRING *value);
        [propget] HRESULT FrontCameraDescription([out, retval] HSTRING *value);
        [propget] HRESULT RearCameraDescription([out, retval] HSTRING *value);
        [propget] HRESULT HasNfc([out, retval] HSTRING *value);
        [propget] HRESULT HasSdSlot([out, retval] HSTRING *value);
        [propget] HRESULT HasOpticalDrive([out, retval] HSTRING *value);
        [propget] HRESULT IsOfficeInstalled([out, retval] HSTRING *value);
        [propget] HRESULT WindowsVersion([out, retval] HSTRING *value);
    }

    [version(NTDDI_WINTHRESHOLD), static(IRetailInfoStatics, NTDDI_WINTHRESHOLD)]
    [version(NTDDI_WINTHRESHOLD, Platform.WindowsPhone), static(IRetailInfoStatics, NTDDI_WINTHRESHOLD, Platform.WindowsPhone)]
    [threading(both)]
    [marshaling_behavior(agile)]
    runtimeclass RetailInfo
    {
    }

    [version(NTDDI_WINTHRESHOLD), static(IKnownRetailInfoPropertiesStatics, NTDDI_WINTHRESHOLD)]
    [version(NTDDI_WINTHRESHOLD, Platform.WindowsPhone), static(IKnownRetailInfoPropertiesStatics, NTDDI_WINTHRESHOLD, Platform.WindowsPhone)]
    [threading(both)]
    [marshaling_behavior(agile)]
    runtimeclass KnownRetailInfoProperties
    {
    }
}
```

## <a name="cleanup-process"></a>정리 프로세스

정리는 구매자 디바이스와 상호 작용 중지 되 면 2 분 시작 됩니다. 소매 데모를 재생 하 고 Windows 연락처, 사진 및 다른 앱에서 샘플 데이터 초기화를 시작 합니다. 장치에 따라 간에 되돌려 모든 다시 설정 하는 데 1 ~ 5 분이 걸릴 수 있습니다. 이렇게 하면 모든 고객은 소매점에서 장치를 이용 하 고 한 디바이스와 상호 작용할 때 동일한 환경을 수 있습니다.

1 단계: 정리
* 모든 Win32 및 스토어 앱이 닫힙니다.
* __사진__, __동영상__, __음악__, __문서__, __SavedPictures__, __CameraRoll__, __바탕 화면__ 및 __다운로드__와 같은 알려진 폴더의 모든 파일이 삭제됩니다.
* 구조화되지 않은 로밍 상태 및 구조화된 로밍 상태가 삭제됩니다.
* 구조화된 로컬 상태가 삭제됩니다.

2 단계: 설치
* 오프라인 장치: 폴더는 계속 비어 있습니다.
* 온라인 장치: Microsoft Store에서 장치로 소매 데모 자산이 푸시될 수 있습니다.

### <a name="store-data-across-user-sessions"></a>사용자 세션 간에 데이터를 저장 합니다.

사용자 세션 간에 데이터를 저장, 기본 정리 프로세스에서이 폴더의 데이터를 자동으로 삭제 되지 __ApplicationData.Current.TemporaryFolder__ 에 정보를 저장할 수 있습니다. 참고 *LocalState* 를 사용 하 여 저장 된 정보는 정리 프로세스 중 삭제 됩니다.

### <a name="customize-the-cleanup-process"></a>정리 프로세스를 사용자 지정

정리 프로세스를 사용자 지정 하려면 다음을 구현 합니다 `Microsoft-RetailDemo-Cleanup` 앱 서비스를 앱.

사용자 지정 정리 논리가 필요한 시나리오 다운로드 및 데이터를 캐시 *LocalState* 데이터 삭제 될 경우 또는 광범위 한 설치를 실행 하는 포함 되어 있습니다.

1 단계: 앱 매니페스트에 _Microsoft RetailDemo 정리_ 서비스를 선언 합니다.
``` CSharp
  <Applications>
      <Extensions>
        <uap:Extension Category="windows.appService" EntryPoint="MyCompany.MyApp.RDXCustomCleanupTask">
          <uap:AppService Name="Microsoft-RetailDemo-Cleanup" />
        </uap:Extension>
      </Extensions>
   </Application>
  </Applications>

```

2 단계: 아래의 샘플 템플릿을 사용 하 여 _AppdataCleanup_ 사례 함수 아래에 사용자 지정 정리 논리를 구현 합니다.
``` CSharp
using System;
using System.IO;
using System.Runtime.Serialization.Json;
using System.Threading;
using System.Threading.Tasks;
using Windows.ApplicationModel.AppService;
using Windows.ApplicationModel.Background;
using Windows.Foundation.Collections;
using Windows.Storage;

namespace MyCompany.MyApp
{
    public sealed class RDXCustomCleanupTask : IBackgroundTask
    {
        BackgroundTaskCancellationReason _cancelReason = BackgroundTaskCancellationReason.Abort;
        BackgroundTaskDeferral _deferral = null;
        IBackgroundTaskInstance _taskInstance = null;
        AppServiceConnection _appServiceConnection = null;

        const string MessageCommand = "Command";

        public void Run(IBackgroundTaskInstance taskInstance)
        {
            // Get the deferral object from the task instance, and take a reference to the taskInstance;
            _deferral = taskInstance.GetDeferral();
            _taskInstance = taskInstance;
            _taskInstance.Canceled += new BackgroundTaskCanceledEventHandler(OnCanceled);

            AppServiceTriggerDetails appService = _taskInstance.TriggerDetails as AppServiceTriggerDetails;
            if ((appService != null) && (appService.Name == "Microsoft-RetailDemo-Cleanup"))
            {
                _appServiceConnection = appService.AppServiceConnection;
                _appServiceConnection.RequestReceived += _appServiceConnection_RequestReceived;
                _appServiceConnection.ServiceClosed += _appServiceConnection_ServiceClosed;
            }
            else
            {
                _deferral.Complete();
            }
        }

        void _appServiceConnection_ServiceClosed(AppServiceConnection sender, AppServiceClosedEventArgs args)
        {
        }

        async void _appServiceConnection_RequestReceived(AppServiceConnection sender, AppServiceRequestReceivedEventArgs args)
        {
            //Get a deferral because we will be calling async code
            AppServiceDeferral requestDeferral = args.GetDeferral();
            string command = null;
            var returnData = new ValueSet();

            try
            {
                ValueSet message = args.Request.Message;
                if (message.ContainsKey(MessageCommand))
                {
                    command = message[MessageCommand] as string;
                }

                if (command != null)
                {
                    switch (command)
                    {
                        case "AppdataCleanup":
                            {
                                // Do custom clean up logic here
                                break;
                            }
                    }
                }
            }
            catch (Exception e)
            {
            }
            finally
            {
                requestDeferral.Complete();
                // Also release the task deferral since we only process one request per instance.
                _deferral.Complete();
            }
        }

        private void OnCanceled(IBackgroundTaskInstance sender, BackgroundTaskCancellationReason reason)
        {
            _cancelReason = reason;
        }
    }
}
```

## <a name="related-links"></a>관련 링크

* [앱 데이터 저장 및 검색](https://msdn.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data)
* [앱 서비스를 만들고 사용하는 방법](https://msdn.microsoft.com/windows/uwp/launch-resume/how-to-create-and-consume-an-app-service)
* [앱 콘텐츠 지역화](https://msdn.microsoft.com/windows/uwp/globalizing/globalizing-portal)
* [소매 데모 환경을 RDX)](https://docs.microsoft.com/windows-hardware/customize/desktop/retail-demo-experience)
