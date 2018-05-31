---
author: joannaleecy
title: 소매 데모 환경 앱 만들기
description: 소매 데모 모드 및 표준 모드에서 시작할 수 있는 단일 앱인 RDX(소매 데모 환경) 앱 만들기
ms.assetid: f83f950f-7fdd-4f18-8127-b92a8f400061
ms.author: joanlee
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 소매 데모 앱
ms.localizationpriority: medium
ms.openlocfilehash: 19a22e09484943d63988cef6bb6a7e7c09e016dd
ms.sourcegitcommit: 6618517dc0a4e4100af06e6d27fac133d317e545
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/28/2018
ms.locfileid: "1691019"
---
#  <a name="create-a-retail-demo-experience-rdx-app"></a>RDX(소매 데모 환경) 앱 만들기

소매점을 둘러보는 고객은 최신 PC와 휴대폰이 진열되어 있을 것이라고 기대합니다. 이처럼 소매점에 진열된 장치를 소매 데모 장치라고 합니다.
대개 고객은 소매 데모 장치를 오랫동안 사용하므로 어떤 장치에 어떤 콘텐츠를 설치했느냐가 소매점에서의 고객 경험을 크게 좌우합니다.

소매점에 진열하는 PC와 휴대폰에는 RDX(소매 데모 환경) 앱을 설치해야 합니다. 이 문서에서는 소매점에서 PC 및 모바일 데모 장치에 설치할 앱의 소매 데모 버전을 디자인하고 개발하는 방법을 간략하게 설명합니다.

소매 데모 환경 앱은 _일반_ 또는 _소매_의 두 가지 모드 중 하나로 시작할 수 있는 단일 빌드로 제공됩니다.
고객의 관점에서는 앱이 하나뿐이므로 고객이 두 가지 버전을 구분할 수 있도록 도와야 합니다. 소매 모드에서 실행되는 앱에서 제목 표시줄 또는 적절한 위치에 "소매"라는 단어가 분명히 표시되도록 하는 것이 좋습니다.

소매점에서 고객이 일관되게 긍정적인 경험을 하도록 지원하려면, 앱에 대한 스토어 요구 사항 외에도 RDX 앱은 소매 데모 장치와 완전히 호환되어야 합니다.

## <a name="design-principles"></a>디자인 원칙

### <a name="show-your-best"></a>최고를 표시

소매 데모 환경을 사용하여 응용 프로그램의 뛰어난 점을 보여 줍니다.  고객이 응용 프로그램을 처음 접하는 것일 수 있으므로 최고를 보여 주는 것이 중요합니다.

### <a name="show-it-fast"></a>빠르게 표시

고객은 참을성이 부족할 수 있습니다. 고객이 앱의 진정한 가치를 더 빨리 경험할수록 더 좋습니다.

### <a name="keep-the-story-simple"></a>스토리를 간단하게 유지

소매 데모 환경은 앱의 가치를 요약해서 보여 주는 것임을 기억하세요.

### <a name="focus-on-the-experience"></a>경험에 집중

사용자에게 콘텐츠를 소화할 시간을 줍니다.  최고의 부분을 빠르게 파악하도록 하는 것도 중요하지만, 적절하게 멈출 수 있는 부분을 추가하면 사용자가 충분히 즐기면서 경험할 수 있습니다.

## <a name="technical-requirements"></a>기술 요구 사항

소매 데모 환경 앱은 소매 고객에게 앱의 최고 기능을 선보이려는 것이므로 다음과 같은 기술 요구 사항을 충족하는 것은 물론 스토어의 모든 소매 데모 환경 앱에 관한 개인 정보 보호 규정을 준수해야 합니다.
이러한 요구 사항은 유효성 검사 프로세스를 준비하고 테스트 프로세스에서 명확성을 제공할 수 있도록 검사 목록으로서 사용될 수도 있습니다. 앱이 소매 데모 장치에서 실행되는 한, 유효성 검사 프로세스에서는 물론 소매 데모 환경 앱의 전체 수명 동안 이러한 요구 사항을 유지 관리해야 합니다.

### <a name="critical-level-requirements"></a>중요한 수준 요구 사항

이러한 중요한 요구 사항을 충족하지 않는 RDX 앱은 모든 소매 데모 장치에서 가능한 한 빨리 제거됩니다.

* PII(개인 식별이 가능한 정보)를 요청하지 않음

    앱은 고객에게 개인 식별이 가능한 정보를 요청할 수 없습니다.  여기에는 모든 Microsoft 계정 정보, 연락처 세부 정보 등이 포함됩니다.

* 오류 없는 환경

    앱이 오류 없이 실행되어야 합니다. 또한 소매 데모 장치를 사용하는 고객에게 오류나 알림이 표시되어서는 안 됩니다. 고객에게 최고를 선보여야 하며 최고에는 오류가 없어야 하므로 이 점은 매우 중요합니다.
    또 다른 이유는 오류가 앱 자체, 브랜드, 앱이 실행되는 장치, 장치 제조업체 브랜드는 물론 Microsoft 브랜드에도 부정적인 영향을 미친다는 점입니다.

* 유료 앱에는 평가판 모드가 있어야 합니다.

    소매 데모 장치에 앱을 설치하려면 앱이 무료 앱이거나 평가판 모드로 설정되어 있어야 합니다.  고객은 소매점에서 사용하는 앱에 대해 비용을 부담하려고 하지 않습니다. 자세한 내용은 [평가판의 기능 제외 또는 제한](https://msdn.microsoft.com/windows/uwp/monetize/exclude-or-limit-features-in-a-trial-version-of-your-app)을 참조하세요.

### <a name="high-priority-requirements"></a>높은 우선 순위 요구 사항

이러한 우선 순위가 높은 요구 사항을 충족하지 않는 RDX 앱은 즉시 조사하여 수정해야 합니다. 즉시 수정할 수 없는 경우 모든 소매 데모 장치에서 이 앱을 제거할 수 있습니다.

* 기억에 남는 오프라인 경험

    소매점에서는 장치의 50% 정도가 오프라인 상태이므로 소매 데모 환경 앱은 뛰어난 오프라인 경험을 제공해야 합니다. 이는 오프라인으로 앱을 조작하는 고객도 의미 있고 긍정적인 경험을 하도록 하려는 것입니다.

* 업데이트된 콘텐츠 환경

    뛰어난 환경을 제공하려면 앱이 항상 최신 상태여야 하며, 온라인으로 고객이 앱을 사용할 때 응용 프로그램을 업데이트하라는 메시지가 표시되어서는 안 됩니다.

* 익명 통신 없음

    소매 데모 장치를 사용하는 고객은 익명의 사용자이므로 메시지를 주고받거나 장치의 콘텐츠를 공유할 수 없습니다.

* 정리 프로세스를 사용하여 일관된 경험 제공

    소매 데모 장치를 이용하는 모든 고객은 동일한 경험을 해야 합니다. 마지막 고객이 남긴 내용을 다음 고객이 보는 것은 바람직하지 않으므로 [정리 프로세스](#clean-up-process)를 사용하여 매번 사용한 후 앱을 동일한 기본 상태로 되돌려야 합니다.  여기에는 스코어보드, 도전 과제, 잠금 해제 등이 포함됩니다.

* 연령에 맞는 콘텐츠

    모든 소매 데모 환경 앱 콘텐츠에는 청소년 이하의 평점 범주를 할당해야 합니다. 자세한 내용은 [IARC에서 앱 평점 받기](https://www.globalratings.com/for-developers.aspx) 및 [ESRB 평점](https://www.esrb.org/ratings/ratings_guide.aspx)을 참조하세요.

### <a name="medium-priority-requirements"></a>보통 우선 순위 요구 사항

이 문제의 해결 방법에 대해 논의하기 위해 Windows Retail Store 팀에서 개발자에게 직접 연락할 수 있습니다.

* 다양한 장치에서 성공적으로 실행하는 기능

    소매 데모 환경 앱은 저급 사양의 장치를 비롯한 모든 장치에서 잘 실행되어야 합니다. 앱 실행을 위한 최소 사양이 충족되지 않는 장치에 소매 데모 환경 앱을 설치하는 경우, 앱에서 이 문제에 대해 사용자에게 분명히 알려야 합니다. 앱이 항상 고성능으로 실행될 수 있도록 최소 장치 사양을 알려야 합니다.

* 소매 스토어 앱 크기 요구 사항 충족

    앱은 800MB 미만이어야 합니다. 소매 데모 환경 앱이 크기 요건을 충족하지 않는 경우 추가 설명이 필요하면 Windows Retail Store 팀에 직접 문의하세요.

## <a name="preparing-codebase-for-retail-demo-mode-development"></a>소매 데모 모드 개발을 위한 코드베이스 준비

Windows10 SDK에서 [Windows.System.Profile](https://docs.microsoft.com/uwp/api/windows.system.profile.retailinfo.isdemomodeenabled) 네임스페이스의 일부인 [**RetailInfo**](https://docs.microsoft.com/uwp/api/Windows.System.Profile.RetailInfo) 유틸리티 클래스의 [**IsDemoModeEnabled**](https://docs.microsoft.com/uwp/api/windows.system.profile) 속성이 응용 프로그램을 실행할 코드 경로(_표준_ 모드 또는 _소매_ 모드)를 지정하기 위한 부울 표시기로 사용됩니다.

[**IsDemoModeEnabled**](https://docs.microsoft.com/uwp/api/windows.system.profile.retailinfo.isdemomodeenabled)가 true를 반환하면, 보다 사용자 지정 가능한 소매 데모 환경을 구축하기 위해 [**RetailInfo.Properties**](https://docs.microsoft.com/uwp/api/windows.system.profile.retailinfo.properties)를 사용하여 장치에 대한 속성 집합을 쿼리할 수 있습니다. 이러한 속성에는 [**ManufacturerName**](https://docs.microsoft.com/uwp/api/windows.system.profile.knownretailinfoproperties.manufacturername), [**Screensize**](https://docs.microsoft.com/uwp/api/windows.system.profile.knownretailinfoproperties.screensize), [**Memory**](https://docs.microsoft.com/uwp/api/windows.system.profile.knownretailinfoproperties.memory) 등이 포함됩니다.


## <a name="clean-up-process"></a>정리 프로세스

정리 프로세스는 지정된 기간에 장치와 상호 작용이 없을 때 소매 데모 장치를 원래 기본 설정으로 자동으로 초기화하는 데 사용됩니다. 이는 소매점의 모든 사용자가 장치에 다가가서 장치와 상호 작용할 때 원래의 정확한 기본 환경을 이용할 수 있도록 하려는 것입니다. 소매 데모 환경 앱을 개발할 때는 정리 프로세스가 언제 어떻게 트리거되고 기본 정리 프로세스 중에 어떤 일이 발생하는지를 이해하고, 원래 소매 데모 환경의 요건에 따라 정리 프로세스를 사용자 지정하는 방법을 파악하는 것이 중요합니다.

### <a name="when-does-clean-up-begin"></a>정리가 시작되는 시기

지정된 장치 유휴 시간이 지난 후 정리 시퀀스가 시작됩니다. 장치에서 터치, 마우스 및 키보드의 입력이 없을 때 유휴 시간 계산이 시작됩니다.

#### <a name="desktoppc"></a>데스크톱/PC

120초 유휴 시간이 지나면 장치에서 유휴 안내 앱 동영상의 재생이 시작됩니다. 5초 후 정리 프로세스가 진행됩니다.

#### <a name="phone"></a>휴대폰

60초 유휴 시간이 지나면 장치에서 유휴 안내 앱 동영상의 재생이 시작되고 정리 프로세스가 즉시 진행됩니다.

### <a name="what-happens-during-a-default-clean-up-process"></a>기본 정리 프로세스 중에 발생하는 일

#### <a name="step-1-clean-up"></a>1단계: 정리
* 모든 Win32 및 스토어 앱이 닫힙니다.
* __사진__, __동영상__, __음악__, __문서__, __SavedPictures__, __CameraRoll__, __바탕 화면__ 및 __다운로드__와 같은 알려진 폴더의 모든 파일이 삭제됩니다.
* 구조화되지 않은 로밍 상태 및 구조화된 로밍 상태가 삭제됩니다.
* 구조화된 로컬 상태가 삭제됩니다.

#### <a name="step-2-set-up"></a>2단계: 설정
* 오프라인 장치: 폴더는 계속 비어 있습니다.
* 온라인 장치: Microsoft Store에서 장치로 소매 데모 자산이 푸시될 수 있습니다.

### <a name="how-to-store-data-across-user-sessions"></a>사용자 세션 간에 데이터를 저장하는 방법

사용자 세션 전체에서 데이터를 저장하려면 __ApplicationData.Current.TemporaryFolder__에 정보를 저장할 수 있습니다. 기본 정리 프로세스에서 이 폴더의 데이터는 자동으로 삭제되지 않기 때문입니다. *LocalState*를 사용하여 저장한 정보는 정리 프로세스 중에 삭제됩니다.

### <a name="how-to-customize-the-clean-up-process"></a>정리 프로세스를 사용자 지정하는 방법

정리 프로세스를 사용자 지정하려면 `Microsoft-RetailDemo-Cleanup` 앱 서비스를 앱에 구현해야 합니다.

사용자 지정 정리 논리가 필요한 시나리오에는 설정에 비용이 많이 드는 경우, 데이터를 다운로드하여 캐시하는 경우 또는 *LocalState* 데이터의 삭제를 원하는 않는 경우가 포함됩니다.

1단계: 응용 프로그램 매니페스트에서 _Microsoft-RetailDemo-Cleanup_ 서비스를 선언합니다.
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

2단계: 아래의 샘플 템플릿을 사용하여 _AppdataCleanup_ 사례 함수 아래에 사용자 지정 정리 논리를 구현합니다.
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


 

 
