---
title: 소매 데모 (RDX) 기능을 앱에 추가
description: 소매 매장에서 앱을 소개 하는 데 도움이 소매 데모 모드에서는 앱을 준비 합니다.
ms.assetid: f83f950f-7fdd-4f18-8127-b92a8f400061
ms.date: 10/02/2018
ms.topic: article
keywords: windows 10, uwp, 소매 데모 앱
ms.localizationpriority: medium
ms.openlocfilehash: b66435dd7c94762874461b48e19e9a60224f287b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57596758"
---
# <a name="add-retail-demo-rdx-features-to-your-app"></a>소매 데모 (RDX) 기능을 앱에 추가

Pc와 매장에서 장치를 사용해 고객의 오른쪽을 이동할 수 있도록 Windows 앱에는 소매 데모 모드를 포함 합니다.

고객은 소매점에 있는 경우 Pc 및 장치의 데모를 사용해 수 있으려면 하기를 원합니다. 상당한 시간을 통해 앱을 사용 하 여 생길 청크 보내는 경우가 많습니다 합니다 [소매 데모 환경 (RDX)](https://docs.microsoft.com/windows-hardware/customize/desktop/retail-demo-experience)합니다.

하는 동안 다른 환경을 제공 하려면 앱을 설정할 수 있습니다 _정상적인_ 하거나 _소매_ 모드입니다. 예를 들어 앱에서 설치 프로세스를 시작 하는 경우 수도 소매 모드로 건너뛰어야 있으며에서 오른쪽을 이동할 수 있도록 샘플 데이터 및 기본 설정 사용 하 여 앱을 미리 채웁니다.

고객의 관점에서 하나의 앱만 있습니다. 고객이 두 가지 모드를 구분할 수 있도록, 것이 좋습니다 앱 소매 모드일 때를 표시 하는 단어 "일반 정품" 눈에 띄게 제목 표시줄에 또는 적합 한 위치입니다.

앱에 대 한 Microsoft Store의 요구 사항과 더불어 RDX 인식 앱은 RDX 설치, 정리 및 고객에 게 소매 상점에서 일관 되 게 긍정적인 경험을 있는지 확인 하는 업데이트 프로세스를 사용 하 여 호환 수도 있어야 합니다.

## <a name="design-principles"></a>디자인 원칙

* **최대한 표시**합니다. 소매 데모 환경 쇼케이스 앱이 최고인 이유를 사용 합니다. 이 경우 처음으로 고객에 게 앱에 따라서 최상의 조각 표시 나타납니다 있습니다!

* **빠른 표시**합니다. 고객은 참을성이 부족할 수 있습니다. 고객이 앱의 진정한 가치를 더 빨리 경험할수록 더 좋습니다.

* **스토리를 간단 하 게 유지**합니다. 소매 데모 환경은 앱의 값을 요약 합니다.

* **환경에 집중**합니다. 사용자에게 콘텐츠를 소화할 시간을 줍니다. 최고의 부분을 빠르게 파악하도록 하는 것도 중요하지만, 적절하게 멈출 수 있는 부분을 추가하면 사용자가 충분히 즐기면서 경험할 수 있습니다.

## <a name="technical-requirements"></a>기술 요구 사항

RDX 인식 앱 소매 고객에 게 앱의 가장을 보여 주기 위해 제공 되며,으로 기술 요구 사항을 충족 하 고 모든 소매 데모 환경 앱에 대 한 Microsoft Store 있는 개인 정보 보호 규정을 준수 되어야 합니다.

이 유효성 검사 프로세스에 대 한 준비를 지원 하 고 테스트 프로세스에서 명확 하 게 제공 하는 검사 목록으로 사용할 수 있습니다. 앱이 소매 데모 장치에서 실행되는 한, 유효성 검사 프로세스에서는 물론 소매 데모 환경 앱의 전체 수명 동안 이러한 요구 사항을 유지 관리해야 합니다.

### <a name="critical-requirements"></a>중요 한 요구 사항

RDX 인식 앱의 이러한 중요 한 요구 사항을 충족 하지 않는 모든 소매 데모 장치에서 가능한 한 빨리 제거 됩니다.

* **개인 식별이 가능한 정보 (PII)에 대해 묻지 않음**합니다. 여기에 로그인 정보, Microsoft 계정 정보 또는 연락처 세부 정보입니다.

* **오류가 없는 환경을**합니다. 앱이 오류 없이 실행되어야 합니다. 또한 소매 데모 장치를 사용하는 고객에게 오류나 알림이 표시되어서는 안 됩니다. 오류 자체, 브랜드, 장치의 브랜드, 장치 제조업체의 브랜드 및 Microsoft의 브랜드가 앱의 부정적인 반영합니다.

* **유료 앱 평가 모드 있어야**합니다. 앱을 무료 또는 포함 하거나 해야는 [평가 모드](https://msdn.microsoft.com/windows/uwp/monetize/exclude-or-limit-features-in-a-trial-version-of-your-app)합니다. 고객은 소매점에서 사용하는 앱에 대해 비용을 부담하려고 하지 않습니다.

### <a name="high-priority-requirements"></a>우선 순위가 높은 요구 사항

이러한 우선 순위가 높은 요구 사항을 충족 하지 않는 RDX 인식 앱 문제 해결을 위한 즉시 조사 해야 합니다. 즉시 수정할 수 없는 경우 모든 소매 데모 장치에서 이 앱을 제거할 수 있습니다.

* **기억 하기 쉬운 오프 라인 환경**합니다. 앱이 장치의 약 50%에서 오프 라인으로 오프 라인 경험을 보여 주기 위해 필요 합니다. 이는 오프라인으로 앱을 조작하는 고객도 의미 있고 긍정적인 경험을 하도록 하려는 것입니다.

* **업데이트 된 콘텐츠 환경**합니다. 앱 온라인일 때 업데이트에 대 한 프롬프트 되어서는 안 됩니다. 업데이트가 필요한 경우 자동으로 수행 되어야 합니다.

* **익명 통신이 없습니다**합니다. 소매 데모 장치를 사용 하는 고객은 익명 사용자 이므로 장치에서 메시지 또는 공유 콘텐츠 수 있어야 합니다.

* **정리 프로세스를 사용 하 여 일관 된 환경을 제공**합니다. 소매 데모 장치를 이용하는 모든 고객은 동일한 경험을 해야 합니다. 앱을 사용할지 [정리 프로세스](#cleanup-process) 각 사용 후 동일한 기본 상태로 돌아갑니다. 남아 있는 어떠한는 마지막 고객을 확인 하려면 다음 고객을 않으려 합니다. 여기에는 스코어보드, 도전 과제, 잠금 해제 등이 포함됩니다.

* **적절 한 콘텐츠 age**합니다. 모든 앱 콘텐츠 해야 청소년 또는 낮은 등급 범주를 할당 합니다. 자세한 내용은를 참조 하세요. [IARC 평가한 앱 가져오기](https://www.globalratings.com/for-developers.aspx) 하 고 [ESRB 등급](https://www.esrb.org/ratings/ratings_guide.aspx)합니다.

### <a name="medium-priority-requirements"></a>중간 우선 요구 사항

이 문제의 해결 방법에 대해 논의하기 위해 Windows Retail Store 팀에서 개발자에게 직접 연락할 수 있습니다.

* **다양 한 장치를 통해 성공적으로 실행 하는 기능**합니다. 저사양 사양 사용 하 여 장치를 포함 하 여, 모든 장치에서 앱 실행 해야 합니다. 앱의 최소 사양을 충족 하지 않는 장치에 설치 되어 있으면 앱이에 대 한 사용자에 게 알릴 명확 하 게 해야 합니다. 앱이 항상 고성능으로 실행될 수 있도록 최소 장치 사양을 알려야 합니다.

* **소매 스토어 앱 크기 요구 사항을 충족할**합니다. 앱은 800MB 미만이어야 합니다. RDX 인식 앱 크기 요구 사항에 맞지 않는 경우 직접 대 한 자세한 내용은 Windows 소매점 팀에 문의 합니다.

## <a name="retailinfo-api-preparing-your-code-for-demo-mode"></a>RetailInfo API: 데모 모드에 대 한 코드를 준비 하는 중

### <a name="isdemomodeenabled"></a>IsDemoModeEnabled
합니다 [ **IsDemoModeEnabled** ](https://docs.microsoft.com/uwp/api/windows.system.profile.retailinfo.isdemomodeenabled) 속성에는 [ **RetailInfo** ](https://docs.microsoft.com/uwp/api/Windows.System.Profile.RetailInfo) 참가 하는 유틸리티 클래스의는 [ Windows.System.Profile](https://docs.microsoft.com/uwp/api/windows.system.profile) Windows 10 SDK에서 네임 스페이스에서 앱이 실행 되는 코드 경로 지정 하려면 부울 표시기로 사용 됩니다 합니다 _normal_ 모드 또는 _소매_ 모드입니다.

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

[  **IsDemoModeEnabled**](https://docs.microsoft.com/uwp/api/windows.system.profile.retailinfo.isdemomodeenabled)가 true를 반환하면, 보다 사용자 지정 가능한 소매 데모 환경을 구축하기 위해 [**RetailInfo.Properties**](https://docs.microsoft.com/uwp/api/windows.system.profile.retailinfo.properties)를 사용하여 장치에 대한 속성 집합을 쿼리할 수 있습니다. 이러한 속성에는 [**ManufacturerName**](https://docs.microsoft.com/uwp/api/windows.system.profile.knownretailinfoproperties.manufacturername), [**Screensize**](https://docs.microsoft.com/uwp/api/windows.system.profile.knownretailinfoproperties.screensize), [**Memory**](https://docs.microsoft.com/uwp/api/windows.system.profile.knownretailinfoproperties.memory) 등이 포함됩니다.

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

정리는 구매자의 장치와 상호 작용을 중지 한 후 2 분을 시작 합니다. 소매 데모 재생 Windows 연락처, 사진 및 기타 앱에 샘플 데이터를 다시 시작 됩니다. 장치에 따라 완전히 정상으로 돌아 다시 설정 하는 모든 항목 1 ~ 5 분 정도 걸릴 수 있습니다. 있는지 여부를 확인는 소매 상점에서 모든 고객이 수는 장치에 게 동일한 환경을 장치와 상호 작용 하는 경우.

1단계: 정리
* 모든 Win32 및 스토어 앱이 닫힙니다.
* __사진__, __동영상__, __음악__, __문서__, __SavedPictures__, __CameraRoll__, __바탕 화면__ 및 __다운로드__와 같은 알려진 폴더의 모든 파일이 삭제됩니다.
* 구조화되지 않은 로밍 상태 및 구조화된 로밍 상태가 삭제됩니다.
* 구조화된 로컬 상태가 삭제됩니다.

2단계:  설치
* 오프 라인 장치의 경우: 폴더는 비어 있는 상태로 유지
* 온라인 장치의 경우: 소매 데모 자산 Microsoft Store 장치에 푸시할 수

### <a name="store-data-across-user-sessions"></a>사용자 세션 간에 데이터를 저장 합니다.

사용자 세션 간에 데이터를 저장 하려면 정보를 저장할 수 있습니다 __ApplicationData.Current.TemporaryFolder__ 기본적으로 정리 프로세스는 자동으로 삭제 되지이 폴더에 있는 데이터입니다. 사용 하 여 저장 하는 정보를 확인 *LocalState* 정리 프로세스 중에 삭제 됩니다.

### <a name="customize-the-cleanup-process"></a>정리 프로세스를 사용자 지정

정리 프로세스를 사용자 지정 하려면 구현 합니다 `Microsoft-RetailDemo-Cleanup` 앱으로 app service입니다.

사용자 지정 정리 논리를 필요한 시나리오 다운로드 하 고 데이터를 캐시 하거나 것을 피하기 위해 광범위 한 설치 프로그램을 실행 하는 포함 되어 있습니다 *LocalState* 데이터를 삭제 합니다.

1단계: 선언 된 _Microsoft RetailDemo 정리_ 앱 매니페스트에 서비스입니다.
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

2단계: 사용자 지정 정리 논리를 구현 합니다 _AppdataCleanup_ 아래 샘플 템플릿을 사용 하 여 case 함수입니다.
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
* [만들기 및 app service를 사용 하는 방법](https://msdn.microsoft.com/windows/uwp/launch-resume/how-to-create-and-consume-an-app-service)
* [앱 콘텐츠 지역화](https://msdn.microsoft.com/windows/uwp/globalizing/globalizing-portal)
* [소매 데모 환경 (RDX)](https://docs.microsoft.com/windows-hardware/customize/desktop/retail-demo-experience)
