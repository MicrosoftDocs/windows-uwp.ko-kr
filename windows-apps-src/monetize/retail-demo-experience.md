---
title: 앱에 소매 데모(RDX) 기능 추가
description: 소매 데모 모드용으로 앱을 준비 하 여 소매 판매 바닥에서 앱을 소개 합니다.
ms.assetid: f83f950f-7fdd-4f18-8127-b92a8f400061
ms.date: 10/02/2018
ms.topic: article
keywords: windows 10, uwp, 소매 데모 앱
ms.localizationpriority: medium
ms.openlocfilehash: 39f1cb7439c02f215824c6c632fb2e2fc6afdb39
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89164537"
---
# <a name="add-retail-demo-rdx-features-to-your-app"></a>앱에 소매 데모(RDX) 기능 추가

매장에서 PC 및 디바이스를 사용해 보려는 고객이 바로 시작할 수 있도록 Windows 앱에 소매 데모 모드를 포함하세요.

고객이 소매점에 있으면 Pc와 장치의 데모를 사용해 볼 수 있는 것으로 간주 됩니다. 자주 사용 하는 [RDX (소매 데모 환경)](/windows-hardware/customize/desktop/retail-demo-experience)를 통해 앱을 재생 하는 데 상당한 시간이 걸릴 수 있습니다.

앱을 설정 하 여 _표준_ 또는 _소매점_ 모드에서 다양 한 환경을 제공할 수 있습니다. 예를 들어, 앱이 설치 프로세스로 시작 하는 경우 일반 정품 모드에서 건너뛸 수 있으며, 샘플 데이터 및 기본 설정을 사용 하 여 앱을 미리 채울 수 있습니다.

고객 관점에서 앱은 하나 뿐입니다. 두 모드를 구분 하는 고객을 돕기 위해 앱이 정품 모드에 있는 동안 제목 표시줄 또는 적합 한 위치에 "일반 정품" 단어를 표시 하는 것이 좋습니다.

RDX 인식 앱은 앱에 대 한 Microsoft Store 요구 사항 외에도 RDX 설치, 정리 및 업데이트 프로세스와 호환 되어야 고객에 게 소매 매장에서 일관 된 긍정적인 환경을 제공할 수 있습니다.

## <a name="design-principles"></a>디자인 원칙

* **최고를 표시**합니다. 소매 데모 환경을 사용 하 여 앱이 암석 지는 이유를 소개 합니다. 고객은 처음으로 앱을 보게 될 것 이므로 최상의 피스를 표시 합니다.

* **빠르게 표시**합니다. 고객은 성급 수 있습니다. 사용자가 앱의 실제 가치를 더 빨리 경험할 수 있습니다.

* **스토리를 단순하게 유지**합니다. 소매 데모 환경은 앱 값의 엘리베이터 피치입니다.

* **환경에 중점을 둡니다**. 사용자가 콘텐츠를 다이제스트 하도록 시간을 지정 합니다. 최적의 부분으로 신속 하 게 진행 하는 것이 중요 하지만 적절 한 일시 중지를 디자인 하면 환경을 완벽 하 게 활용할 수 있습니다.

## <a name="technical-requirements"></a>기술적인 요구 사항

RDX 인식 앱은 소매 고객에 게 가장 적합 한 앱을 소개 하는 것 이므로 기술 요구 사항을 충족 하 고 모든 소매 데모 환경 앱에 대 한 Microsoft Store 개인 정보 보호 규정을 준수 해야 합니다.

유효성 검사 프로세스를 준비 하 고 테스트 프로세스를 명확 하 게 제공 하는 데 도움이 되는 검사 목록으로 사용할 수 있습니다. 이러한 요구 사항은 유효성 검사 프로세스 뿐만 아니라 소매 데모 환경 앱의 전체 수명 동안 유지 해야 합니다. 앱이 소매 데모 장치에서 계속 실행 되는 경우

### <a name="critical-requirements"></a>중요 요구 사항

이러한 중요 한 요구 사항을 충족 하지 않는 RDX 인식 앱은 모든 소매 데모 장치에서 가능한 한 빨리 제거 됩니다.

* **PII (개인 식별이 가능한 정보)를 요청 하지 마세요**. 여기에는 로그인 정보, Microsoft 계정 정보 또는 연락처 정보가 포함 됩니다.

* **오류-무료 환경**. 앱이 오류 없이 실행 되어야 합니다. 또한 소매 데모 장치를 사용 하는 고객에 게는 오류 팝 ups 또는 알림이 표시 되지 않습니다. 오류는 앱 자체, 브랜드, 장치의 브랜드, 장치의 manufacturer's 브랜드 및 Microsoft 브랜드에 부정적인 영향을 주지 않습니다.

* **유료 앱에는 평가판 모드가 있어야**합니다. 앱은 무료 이거나 [평가판 모드](./exclude-or-limit-features-in-a-trial-version-of-your-app.md)를 포함 해야 합니다. 고객은 소매점에서 사용하는 앱에 대해 비용을 부담하려고 하지 않습니다.

### <a name="high-priority-requirements"></a>높은 우선 순위 요구 사항

이러한 높은 우선 순위 요구 사항을 충족 하지 않는 RDX 인식 앱은 수정 사항을 즉시 조사 해야 합니다. 즉시 픽스를 찾을 수 없는 경우이 앱은 모든 소매 데모 장치에서 제거 될 수 있습니다.

* **기억 하기 쉬운 오프 라인 환경**. 앱은 약 50%의 장치가 일반 정품 지역에서 오프 라인 상태 이므로 매우 오프 라인 환경을 설명 해야 합니다. 이는 앱과 상호 작용 하는 고객이 여전히 의미 있고 긍정적인 환경을 제공할 수 있도록 하기 위한 것입니다.

* **콘텐츠 환경이 업데이트 되었습니다**. 온라인으로 앱을 업데이트 하 라는 메시지를 표시 해서는 안 됩니다. 업데이트가 필요한 경우 자동으로 수행 해야 합니다.

* **익명 통신 없음**. 소매점 데모 장치를 사용 하는 고객은 익명 사용자 이므로 장치에서 콘텐츠를 메시지 하거나 공유할 수 없습니다.

* **정리 프로세스를 사용 하 여 일관 된 환경을 제공**합니다. 모든 고객은 소매 데모 장치로 이동할 때 동일한 환경을 보유 해야 합니다. 앱에서 [정리 프로세스](#cleanup-process) 를 사용 하 여 각 사용 후 동일한 기본 상태로 돌아가야 합니다. 다음 고객이 남은 최종 고객을 확인 하는 것을 원하지 않습니다. 여기에는 스코어 보드, 성과 및 잠금 해제가 포함 됩니다.

* **적절 한 콘텐츠를 사용**합니다. 모든 앱 콘텐츠에 Teen 또는 낮은 등급 범주를 할당 해야 합니다. 자세히 알아보려면 IARC 및 [ESRB 등급](https://www.esrb.org/ratings/ratings_guide.aspx) [으로 등급을 매긴 앱 가져오기](https://www.globalratings.com/for-developers.aspx) 를 참조 하세요.

### <a name="medium-priority-requirements"></a>보통 우선 순위 요구 사항

Windows 소매점 스토어 팀은 개발자에 게 직접 연락 하 여 이러한 문제를 해결 하는 방법에 대 한 설명을 설정할 수 있습니다.

* **다양 한 장치에서 성공적으로 실행할 수**있습니다. 앱은 낮은 종료 사양이 있는 장치를 비롯 한 모든 장치에서 제대로 실행 되어야 합니다. 최소 사양을 충족 하지 않는 장치에 앱이 설치 되어 있는 경우 앱은 사용자에 게이에 대 한 정보를 명확 하 게 알려 주어 야 합니다. 앱이 항상 높은 성능으로 실행 될 수 있도록 최소 장치 요구 사항을 파악 해야 합니다.

* **소매 스토어 앱 크기 요구 사항을 충족**합니다. 앱은 800MB 보다 작아야 합니다. RDX 인식 앱이 크기 요구 사항을 충족 하지 않는 경우에는 Windows 소매점 스토어 팀에 직접 문의 하세요.

## <a name="retailinfo-api-preparing-your-code-for-demo-mode"></a>RetailInfo API: 데모 모드를 위한 코드 준비

### <a name="isdemomodeenabled"></a>IsDemoModeEnabled
[**RetailInfo**](/uwp/api/Windows.System.Profile.RetailInfo) 유틸리티 클래스의 [**Isdemomodeenabled**](/uwp/api/windows.system.profile.retailinfo.isdemomodeenabled) 속성은Windows.System의 일부입니다 [. ](/uwp/api/windows.system.profile)Windows 10 SDK의 Profile 네임 스페이스는 앱이 실행 되는 코드 경로 ( _표준_ 모드 또는 _소매_ 모드)를 지정 하는 부울 표시기로 사용 됩니다.

``` csharp
using Windows.Storage;

StorageFolder folder = ApplicationData.Current.LocalFolder;

if (Windows.System.Profile.RetailInfo.IsDemoModeEnabled) 
{
    // Use the demo specific directory
    folder = await folder.GetFolderAsync("demo");
}

StorageFile file = await folder.GetFileAsync("hello.txt");
// Now read from file
```

``` cpp
using namespace Windows::Storage;

StorageFolder^ localFolder = ApplicationData::Current->LocalFolder;

if (Windows::System::Profile::RetailInfo::IsDemoModeEnabled) 
{
    // Use the demo specific directory
    create_task(localFolder->GetFolderAsync("demo").then([this](StorageFolder^ demoFolder)
    {
        return demoFolder->GetFileAsync("hello.txt");
    }).then([this](task<StorageFile^> fileTask)
    {
        StorageFile^ file = fileTask.get();
    });
    // Do something with file
}
else
{
    create_task(localFolder->GetFileAsync("hello.txt").then([this](StorageFile^ file)
    {
        // Do something with file
    });
}
```

``` javascript
if (Windows.System.Profile.retailInfo.isDemoModeEnabled) {
    console.log("Retail mode is enabled.");
} else {
    Console.log("Retail mode is not enabled.");
}
```

### <a name="retailinfoproperties"></a>RetailInfo

[**Isdemomodeenabled**](/uwp/api/windows.system.profile.retailinfo.isdemomodeenabled) 가 true를 반환 하는 경우 [**RetailInfo**](/uwp/api/windows.system.profile.retailinfo.properties) 를 사용 하 여 장치에 대 한 속성 집합을 쿼리하여 사용자 지정 소매 데모 환경을 만들 수 있습니다. 이러한 속성에는 [**ManufacturerName**](/uwp/api/windows.system.profile.knownretailinfoproperties.manufacturername), [**Screensize**](/uwp/api/windows.system.profile.knownretailinfoproperties.screensize), [**Memory**](/uwp/api/windows.system.profile.knownretailinfoproperties.memory) 등이 포함 됩니다.

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

```cpp
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

쇼핑객이 장치와의 상호 작용을 멈춘 후 2 분 후에 정리가 시작 됩니다. 소매 데모를 재생 하 고, Windows에서 연락처, 사진 및 기타 앱에 있는 샘플 데이터를 다시 설정 하기 시작 합니다. 장치에 따라 다시 정상으로 다시 설정 하는 데 1-5 분 정도 걸릴 수 있습니다. 이렇게 하면 소매점의 모든 고객이 장치를 탐색 하 고 장치와 상호 작용할 때 동일한 환경을 사용할 수 있습니다.

1 단계: 정리
* 모든 Win32 및 스토어 앱이 닫힙니다.
* __사진__, __비디오__, __음악__, __문서__, __SavedPictures__, __CameraRoll__, __데스크톱__ 및 __다운로드__ 폴더와 같은 알려진 폴더의 모든 파일이 삭제 됩니다.
* 비구조적 및 구조화 된 로밍 상태가 삭제 됩니다.
* 구조적 로컬 상태가 삭제 됨

2 단계: 설치
* 오프 라인 장치의 경우: 폴더가 비어 있는 상태로 유지 됩니다.
* 온라인 장치: 소매 데모 자산은 Microsoft Store에서 장치로 푸시할 수 있습니다.

### <a name="store-data-across-user-sessions"></a>사용자 세션 간에 데이터 저장

사용자 세션 간에 데이터를 저장 하려면 기본 정리 프로세스에서이 폴더의 데이터를 자동으로 삭제 하지 않으므로 __Applicationdata__ 에 정보를 저장할 수 있습니다. *Localstate* 를 사용 하 여 저장 된 정보는 정리 프로세스 중에 삭제 됩니다.

### <a name="customize-the-cleanup-process"></a>정리 프로세스 사용자 지정

정리 프로세스를 사용자 지정 하려면 앱에 `Microsoft-RetailDemo-Cleanup` app service를 구현 합니다.

사용자 지정 정리 논리가 필요한 시나리오에는 광범위 한 설정 실행, 데이터 다운로드 및 캐싱, 삭제할 *Localstate* 데이터를 제공 하지 않습니다.

1 단계: 앱 매니페스트에서 _RetailDemo-Cleanup_ 서비스를 선언 합니다.
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

2 단계: 아래 샘플 템플릿을 사용 하 여 _AppdataCleanup_ case 함수에서 사용자 지정 정리 논리를 구현 합니다.
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

* [앱 데이터 저장 및 검색](../design/app-settings/store-and-retrieve-app-data.md)
* [App service를 만들고 사용 하는 방법](../launch-resume/how-to-create-and-consume-an-app-service.md)
* [앱 콘텐츠 지역화](../design/globalizing/globalizing-portal.md)
* [소매 데모 환경 (RDX)](/windows-hardware/customize/desktop/retail-demo-experience)