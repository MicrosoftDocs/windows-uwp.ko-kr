---
title: 음성 명령을 사용 하 여 Cortana에서 백그라운드 앱 활성화 | Cortana UWP 디자인 및 개발
description: 음성 명령을 사용 하 여 앱의 기능 (백그라운드 작업으로)으로 Cortana를 확장 합니다.
ms.assetid: e2c7eae3-6beb-4156-92a5-474bba53451e
ms.date: 09/24/2019
ms.topic: article
keywords: 나
ms.openlocfilehash: 9331a87eb6a7f2f8a09beb57f82540518993806a
ms.sourcegitcommit: d7efd35c1749f695aebbc0db99d8b62b70fb72da
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99057865"
---
# <a name="activate-a-background-app-in-cortana-using-voice-commands"></a>음성 명령을 사용 하 여 Cortana에서 백그라운드 앱 활성화  

>[!WARNING]
> 이 기능은 Windows 10 5 월 2020 업데이트 (버전 2004, 코드명 "20H1")에서 더 이상 지원 되지 않습니다.

**Cortana** 내에서 음성 명령을 사용 하 여 시스템 기능에 액세스 하는 것 외에도 실행 하는 작업 또는 명령을 지정 하는 음성 명령을 사용 하 여 앱의 기능 및 **기능을 백그라운드 작업으로 확장할 수** 있습니다. 앱이 백그라운드에서 음성 명령을 처리 하는 경우에는 포커스를 사용 하지 않습니다. 대신 **cortana** 캔버스 및 **cortana** 음성을 통해 모든 피드백과 결과를 반환 합니다.  

> [!NOTE]
> **중요 API**
>
> - [**VoiceCommands**](/uwp/api/Windows.ApplicationModel.VoiceCommands)
> - [**VCD 요소 및 특성 v 1.2**](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2)

앱은 작업의 복잡성에 따라 포그라운드 (앱에서 포커스를 취하는 경우) 또는 백그라운드에서 활성화 (**Cortana** 는 포커스를 유지 함) 할 수 있습니다. 예를 들어, 추가 컨텍스트 또는 사용자 입력이 필요한 음성 명령 (예: 특정 연락처에 메시지 보내기)은 포그라운드 앱에서 가장 잘 처리 되지만, 기본 명령 (예: 예정 된 여행 나열)은 백그라운드 앱을 통해 **Cortana** 에서 처리 될 수 있습니다.  

음성 명령을 사용 하 여 응용 프로그램을 포그라운드로 활성화 하려면 [Cortana를 통해 음성 명령을 사용 하 여 포그라운드 앱 활성화](cortana-launch-a-foreground-app-with-voice-commands.md)를 참조 하세요.  

> [!NOTE]
> 음성 명령은 설치 된 앱에서 **Cortana** 를 통해 전달 되는 특정 의도가 포함 된 단일 Utterance (오디오 명령 정의) 파일에 정의 되어 있습니다.
>
> VCD 파일은 각각 고유한 의도를 가진 하나 이상의 음성 명령을 정의 합니다.
>
> 음성 명령 정의는 복잡성이 다를 수 있습니다. 단일 제한 된 utterance의 모든 항목을 동일한 의도를 나타내는 보다 유연 하 고 자연 언어 길이 발언 컬렉션으로 지원할 수 있습니다.

여기에 나와 있는 **놀이 Works** 라는 여행 계획 및 관리 앱을 사용  하 여 설명 하는 다양 한 개념 및 기능을 보여 줍니다. 자세한 내용은 [Cortana 음성 명령 샘플](https://go.microsoft.com/fwlink/p/?LinkID=619899)을 참조 하세요.

:::image type="content" source="images/cortana/cortana-overview.png" alt-text="Cortana 시작 포그라운드 앱 스크린샷":::

**Cortana** 없이 **놀이 동산** 여행을 보려면 사용자가 앱을 시작 하 고 **예정 된 여행** 페이지로 이동 합니다.  

**Cortana** 를 통해 음성 명령을 사용 하 여 백그라운드에서 앱을 시작 하는 것은 사용자가 대신 하는 것 `Adventure Works, when is my trip to Las Vegas?` 입니다. 앱에서 명령을 처리 하 고 **Cortana** 가 앱 아이콘 및 다른 앱 정보 (제공 된 경우)와 함께 결과를 표시 합니다.  

:::image type="content" source="images/cortana/cortana-backgroundapp-result.png" alt-text="백그라운드에서 AdventureWorks 앱을 사용 하 여 기본 쿼리 및 결과 화면을 포함 하는 Cortana의 스크린샷":::

다음 기본 단계는 음성 명령 기능을 추가 하 고 음성 또는 키보드 입력을 사용 하 여 앱에서 백그라운드 기능으로 **Cortana** 를 확장 합니다.

1. **Cortana** 가 백그라운드에서 호출 하는 app Service ( [**AppService**](/uwp/api/Windows.ApplicationModel.AppService)참조)를 만듭니다.  
2. VCD 파일을 만듭니다. VCD 파일은 사용자가 앱을 활성화할 때 작업을 시작 하거나 명령을 호출할 수 있는 모든 음성 명령을 정의 하는 XML 문서입니다. [**VCD 요소 및 특성 v 1.2**](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2)를 참조 하세요.  
3. 앱이 시작 될 때 VCD 파일에 명령 집합을 등록 합니다.  
4. App service의 백그라운드 활성화와 음성 명령 실행을 처리 합니다.  
5. **Cortana** 내 음성 명령에 대 한 적절 한 피드백을 표시 하 고 말합니다.  

> [!TIP]
> **전제 조건**
>
> UWP (유니버설 Windows 플랫폼) 앱을 처음 개발 하는 경우 여기에 설명 된 기술에 익숙해질 수 있도록 다음 항목을 참조 하세요.
>
> - [첫 번째 앱 만들기](/windows/uwp/get-started/your-first-app)
> - [이벤트 및 라우트된 이벤트](/windows/uwp/xaml-platform/events-and-routed-events-overview) 를 사용 하 여 이벤트에 대해 알아보기 개요
>
> **사용자 환경 지침**
>
> Cortana 및 [음성 상호 작용](speech-interactions.md) **을 사용 하** 여 앱을 통합 하는 방법에 대 한 정보는 [cortana 디자인 지침](cortana-design-guidelines.md) 을 참조 하세요.

## <a name="create-a-new-solution-with-a-primary-project-in-visual-studio"></a>Visual Studio에서 주 프로젝트를 사용 하 여 새 솔루션 만들기  

1. 2015 Microsoft Visual Studio 시작 합니다.  
    Visual Studio 2015 시작 페이지가 나타납니다.  

2. **파일** 메뉴에서 **새로 만들기** > **프로젝트** 를 선택합니다.  
    **새 프로젝트** 대화 상자가 나타납니다. 대화 상자의 왼쪽 창에서 표시할 템플릿 형식을 선택할 수 있습니다.  

3. 왼쪽 창에서 **설치 된 > 템플릿 > Visual C \# > Windows** 를 확장 한 다음 **유니버설** 템플릿 그룹을 선택 합니다. 대화 상자의 가운데 창에 UWP (유니버설 Windows 플랫폼) 앱에 대 한 프로젝트 템플릿 목록이 표시 됩니다.  
4. 가운데 창에서 **비어 있는 앱 (유니버설 Windows)** 템플릿을 선택 합니다.  
    **비어 있는 앱** 템플릿은 컴파일 및 실행 되는 최소 UWP 앱을 만듭니다. **비어 있는 앱** 템플릿에는 사용자 인터페이스 컨트롤이 나 데이터가 포함 되지 않습니다. 이 페이지를 가이드로 사용 하 여 앱에 컨트롤을 추가 합니다.  

5. **이름** 텍스트 상자에 프로젝트 이름을 입력 합니다. 예: `AdventureWorks` 를 사용 합니다.  
6. **확인** 단추를 클릭 하 여 프로젝트를 만듭니다.  
    Microsoft Visual Studio 프로젝트를 만들고 **솔루션 탐색기** 에 표시 합니다.  

## <a name="add-image-assets-to-primary-project-and-specify-them-in-the-app-manifest"></a>주 프로젝트에 이미지 자산을 추가 하 고 응용 프로그램 매니페스트에서 지정  

UWP 앱은 가장 적합 한 이미지를 자동으로 선택 해야 합니다. 선택은 특정 설정 및 장치 기능 (고대비, 유효 픽셀, 로캘 등)을 기반으로 합니다. 이미지를 제공 하 고 응용 프로그램 프로젝트 내에서 적절 한 이름 지정 규칙 및 폴더를 사용 하 여 다른 리소스 버전을 확인 해야 합니다.  
권장 리소스 버전을 제공 하지 않으면 다음과 같은 방법으로 사용자 환경이 저하 될 수 있습니다.

- 접근성
- 지역화  
- 이미지 품질  
리소스 버전은 사용자 환경에서 다음과 같은 변경 내용을 조정 하는 데 사용 됩니다.  
- 사용자 기본 설정  
- 기능은  
- 디바이스 유형  
- 위치  

고대비 및 배율 인수에 대 한 이미지 리소스에 대 한 자세한 내용은 [msdn.microsoft.com/windows/uwp/controls-and-patterns/tiles-and-notifications-app-assets](/windows/uwp/app-resources/images-tailored-for-scale-theme-contrast)에 있는 타일 및 아이콘 자산에 대 한 지침을 참조 하세요.  

한정자를 사용 하 여 리소스에 이름을 사용 해야 합니다. 리소스 한정자는 특정 버전의 리소스를 사용 해야 하는 컨텍스트를 식별 하는 폴더 및 파일 이름 한정자입니다.  

표준 명명 규칙은 `foldername/qualifiername-value[_qualifiername-value]/filename.qualifiername-value[_qualifiername-value].ext` 입니다.  
예:는 `images/logo.scale-100_contrast-white.png` 루트 폴더와 파일 이름를 사용 하는 코드를 참조할 수 있습니다 `images/logo.png` .  
자세한 내용은 [msdn.microsoft.com/library/windows/apps/xaml/hh965324.aspx](/previous-versions/windows/apps/hh965324(v=win.10))에 있는 한정자를 사용 하 여 리소스를 이름으로 만드는 방법 페이지를 참조 하세요.  

`en-US\resources.resw` `logo.scale-100.png` 현재 지역화 된 또는 여러 해결 리소스를 제공 하지 않는 경우에도 문자열 리소스 파일 (예:) 및 기본 배율 인수 (예:)에 대 한 기본 언어를 표시 하는 것이 좋습니다. 그러나 Microsoft는 최소한 100, 200 및 400 크기 조정 요소에 대 한 자산을 제공 하는 것이 좋습니다.  

>[!IMPORTANT]
> **Cortana** 캔버스의 제목 영역에 사용 되는 앱 아이콘은 파일에 지정 된 Square44x44Logo 아이콘입니다 `Package.appxmanifest` .  
> **Cortana** 캔버스의 콘텐츠 영역에 있는 각 항목에 대 한 아이콘을 지정할 수도 있습니다. 결과 아이콘의 올바른 이미지 크기는 다음과 같습니다.
>
> - 68w x 68h
> - 68w x 92h  
> - 280w x 140h  

[**VoiceCommandResponse**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandResponse) 개체가 [**VoiceCommandServiceConnection**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection) 클래스에 전달 될 때까지 콘텐츠 타일의 유효성은 검사 되지 않습니다. 이러한 크기 비율을 따르지 않는 이미지를 사용 하 여 콘텐츠 타일이 포함 된 **Cortana** 에 [**VoiceCommandResponse**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandResponse) 개체를 전달 하는 경우 예외가 발생할 수 있습니다.  

예: **어드벤처 Works** 앱 ( `VoiceCommandService\\AdventureWorksVoiceCommandService.cs` )은 `GreyTile.png` [**TitleWith68x68IconAndText**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandContentTileType) 타일 템플릿을 사용 하 여 [**VoiceCommandContentTile**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandContentTile) 클래스에 간단한 회색 사각형 ()을 지정 합니다. 로고 변형은에 있으며 `VoiceCommandService\\Images` [**GetFileFromApplicationUriAsync**](/uwp/api/Windows.Storage.StorageFile) 메서드를 사용 하 여 검색 됩니다.

```csharp
var destinationTile = new VoiceCommandContentTile();  

destinationTile.ContentTileType = VoiceCommandContentTileType.TitleWith68x68IconAndText;
destinationTile.Image = await StorageFile.GetFileFromApplicationUriAsync(
    new Uri("ms-appx:///AdventureWorks.VoiceCommands/Images/GreyTile.png")
);  
```

## <a name="create-an-app-service-project"></a>App Service 프로젝트 만들기

1. 솔루션 이름을 마우스 오른쪽 단추로 클릭 하 고 **새로 만들기 > 프로젝트** 를 선택 합니다.  
2. **설치 된 > 템플릿 > Visual C \# > Windows > 유니버설** 에서 **Windows 런타임 구성 요소** 를 선택 합니다. **Windows 런타임 구성 요소** 는 app Service ([**AppService**](/uwp/api/Windows.ApplicationModel.AppService))를 구현 하는 구성 요소입니다.  
3. 프로젝트의 이름을 입력 하 고 **확인** 단추를 클릭 합니다.  
    예: `VoiceCommandService`.  
4. **솔루션 탐색기** 에서 프로젝트를 선택 `VoiceCommandService` 하 고 `Class1.cs` Visual Studio에서 생성 된 파일의 이름을 바꿉니다.
    예: **어드벤처 Works** 는를 사용 `AdventureWorksVoiceCommandService.cs` 합니다.  
5. **예** 단추를 클릭 합니다. 모든 항목의 이름을 바꿀지 묻는 메시지가 표시 되 면 `Class1.cs` 입니다.  
6. 파일 `AdventureWorksVoiceCommandService.cs`에서:
    1. 다음 using 지시문을 추가 합니다.  
        `using Windows.ApplicationModel.Background;`  
    2. 새 프로젝트를 만들 때 프로젝트 이름은 모든 파일에서 기본 루트 네임 스페이스로 사용 됩니다. 기본 프로젝트 아래에서 app service 코드를 중첩 하려면 네임 스페이스의 이름을 바꿉니다.
        예: `namespace AdventureWorks.VoiceCommands`.  
    3. 솔루션 탐색기에서 app service 프로젝트 이름을 마우스 오른쪽 단추로 클릭 하 고 **속성** 을 선택 합니다.  
    4. **라이브러리** 탭에서 **기본 네임 스페이스** 필드를 동일한 값으로 업데이트 합니다.  
        예: `AdventureWorks.VoiceCommands` ).  
    5. [**IBackgroundTask**](/uwp/api/Windows.ApplicationModel.Background.IBackgroundTask) 인터페이스를 구현 하는 새 클래스를 만듭니다. 이 클래스에는 Cortana에서 음성 명령을 인식할 때 진입점 인 [**Run**](/uwp/api/Windows.ApplicationModel.Background.IBackgroundTask) 메서드가 필요 합니다.  

    예: **놀이 Works** 앱의 기본 백그라운드 작업 클래스입니다.  

    >[!NOTE]
    > 백그라운드 작업 클래스 자체 및 백그라운드 작업 프로젝트의 모든 클래스는 봉인 된 공용 클래스 여야 합니다.  

    ```csharp
    namespace AdventureWorks.VoiceCommands
    {
        ...
        
        /// <summary>
        /// The VoiceCommandService implements the entry point for all voice commands.
        /// The individual commands supported are described in the VCD xml file. 
        /// The service entry point is defined in the appxmanifest.
        /// </summary>
        public sealed class AdventureWorksVoiceCommandService : IBackgroundTask
        {
            ...

            /// <summary>
            /// The background task entrypoint. 
            /// 
            /// Background tasks must respond to activation by Cortana within 0.5 second, and must 
            /// report progress to Cortana every 5 seconds (unless Cortana is waiting for user
            /// input). There is no running time limit on the background task managed by Cortana,
            /// but developers should use plmdebug (https://msdn.microsoft.com/library/windows/hardware/jj680085%28v=vs.85%29.aspx)
            /// on the Cortana app package in order to prevent Cortana timing out the task during
            /// debugging.
            /// 
            /// The Cortana UI is dismissed if Cortana loses focus. 
            /// The background task is also dismissed even if being debugged. 
            /// Use of Remote Debugging is recommended in order to debug background task behaviors. 
            /// Open the project properties for the app package (not the background task project), 
            /// and enable Debug -> "Do not launch, but debug my code when it starts". 
            /// Alternatively, add a long initial progress screen, and attach to the background task process while it runs.
            /// </summary>
            /// <param name="taskInstance">Connection to the hosting background service process.</param>
            public void Run(IBackgroundTaskInstance taskInstance)
            {
              //
              // TODO: Insert code 
              //
              //
        }
      }
    }
    ```  

7. 응용 프로그램 매니페스트에서 백그라운드 작업을 **AppService** 로 선언 합니다.  
    1. **솔루션 탐색기** 에서 파일을 마우스 오른쪽 단추로 클릭 `Package.appxmanifest` 하 고 **코드 보기** 를 선택 합니다.  
    2. 요소를 찾습니다 [`Application`](/uwp/schemas/appxpackage/uapmanifestschema/element-application) .  
    3. 요소 [`Extensions`](/uwp/schemas/appxpackage/uapmanifestschema/element-1-extensions) 를 요소에 추가 [`Application`](/uwp/schemas/appxpackage/uapmanifestschema/element-application) 합니다.  
    4. 요소 [`uap:Extension`](/uwp/schemas/appxpackage/uapmanifestschema/element-1-extension) 를 요소에 추가 [`Extensions`](/uwp/schemas/appxpackage/uapmanifestschema/element-1-extensions) 합니다.  
    5. 특성을 `Category` 요소에 추가 하 `uap:Extension` 고 특성의 값을 `Category` 로 설정 `windows.appService` 합니다.  
    6. `EntryPoint`특성을 요소에 추가 `uap: Extension` 하 고 특성의 값을 `EntryPoint` 를 구현 하는 클래스의 이름으로 설정 합니다 [`IBackgroundTask`](/uwp/api/Windows.ApplicationModel.Background.IBackgroundTask) .  
        예: `AdventureWorks.VoiceCommands.AdventureWorksVoiceCommandService`.  
    7. 요소 [`uap:AppService`](/uwp/schemas/appxpackage/uapmanifestschema/element-uap-appservice) 를 요소에 추가 `uap:Extension` 합니다.  
    8. 특성을 `Name` 요소에 추가 [`uap:AppService`](/uwp/schemas/appxpackage/uapmanifestschema/element-uap-appservice) 하 고 특성 값을 `Name` app service의 이름 (이 경우)으로 설정 `AdventureWorksVoiceCommandService` 합니다.  
    9. 요소에 두 번째 요소를 추가 [`uap:Extension`](/uwp/schemas/appxpackage/uapmanifestschema/element-1-extension) [`Extensions`](/uwp/schemas/appxpackage/uapmanifestschema/element-1-extensions) 합니다.  
    10. `Category`이 요소에 특성을 추가 하 [`uap:Extension`](/uwp/schemas/appxpackage/uapmanifestschema/element-1-extension) 고 특성의 값을 `Category` 로 설정 `windows.personalAssistantLaunch` 합니다.  

    예: 어드벤처 Works 앱의 매니페스트입니다.

    ```xml
    <Package>
        <Applications>
            <Application>
            
                <Extensions>
                    <uap:Extension Category="windows.appService" EntryPoint="CortanaBack1.VoiceCommands.AdventureWorksVoiceCommandService">
                        <uap:AppService Name="AdventureWorksVoiceCommandService"/>
                    </uap:Extension>
                    <uap:Extension Category="windows.personalAssistantLaunch"/>
                </Extensions>
                
            <Application>
        <Applications>
    </Package>
    ```  

8. 이 app service 프로젝트를 기본 프로젝트에 참조로 추가 합니다.  
    1. **참조** 를 마우스 오른쪽 단추로 클릭 합니다.  
    2. **참조 추가**...를 선택 합니다.  
    3. **참조 관리자** 대화 상자에서 **프로젝트** 를 확장 하 고 app service 프로젝트를 선택 합니다.  
    4. **확인** 단추를 클릭 합니다.  

## <a name="create-a-vcd-file"></a>VCD 파일 만들기

1. Visual Studio에서 기본 프로젝트 이름을 마우스 오른쪽 단추로 클릭 하 고 **추가 > 새 항목** 을 선택 합니다. **XML 파일** 을 추가 합니다.  
2. [**VCD**](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2) 파일의 이름을 입력 합니다.  
    예: `AdventureWorksCommands.xml`.
3. **추가** 단추를 클릭 합니다.  
4. **솔루션 탐색기** 에서 [**VCD**](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2) 파일을 선택 합니다.  
5. **속성** 창에서 **빌드 작업** 을 **내용** 으로 설정 하 고 **출력 디렉터리로 복사** 를 **새 버전이 면 복사** 로 설정 합니다.  

## <a name="edit-the-vcd-file"></a>VCD 파일 편집  

1. `VoiceCommands`을 가리키는 특성이 있는 요소를 추가 `xmlns` `https://schemas.microsoft.com/voicecommands/1.2` 합니다.  
2. 앱에서 지 원하는 각 언어에 대해 [`CommandSet`](/previous-versions/windows/dn722331(v=win.10)) 앱에서 지 원하는 음성 명령을 포함 하는 요소를 만듭니다.  
    서로 다른 특성을 사용 하 [`CommandSet`](/previous-versions/windows/dn722331(v=win.10)) 여 여러 요소를 선언 하 여 [`xml:lang`](/previous-versions/windows/dn722331(v=win.10)) 앱을 다른 시장에서 사용할 수 있습니다. 예를 들어 미국의 앱에는 [`CommandSet`](/previous-versions/windows/dn722331(v=win.10)) 영어와 스페인어 용이 있을 수 있습니다 [`CommandSet`](/previous-versions/windows/dn722331(v=win.10)) .  

    >[!IMPORTANT]
    > 앱을 활성화 하 고 음성 명령을 사용 하 여 작업을 시작 하려면 앱은 [`CommandSet`](/previous-versions/windows/dn722331(v=win.10)) 사용자의 장치에 표시 된 음성 언어와 일치 하는 언어의 요소가 포함 된 VCD 파일을 등록 해야 합니다. 음성 언어는 **설정 > System > speech > 음성 언어** 에 있습니다.  

3. 지원 하려는 `Command` 각 명령에 대 한 요소를 추가 합니다.  
    `Command` [**VCD**](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2) 파일에 선언 된 각에는 다음 정보가 포함 되어야 합니다.  
    - `Name`런타임에 음성 명령을 식별 하기 위해 응용 프로그램에서 사용 하는 특성입니다.  
    - `Example`사용자가 명령을 호출 하는 방법을 설명 하는 구가 포함 된 요소입니다. **Cortana** 는 사용자가 `What can I say?` , `Help` 또는 탭을 **자세히 볼** 때 예제를 보여 줍니다.  
    - `ListenFor`앱이 명령으로 인식 하는 단어 또는 구를 포함 하는 요소입니다. 각 `ListenFor` 요소에는 `PhraseList` 명령과 관련 된 특정 단어를 포함 하는 하나 이상의 요소에 대 한 참조가 포함 될 수 있습니다.  

       >[!NOTE]
       > `ListenFor` 요소를 프로그래밍 방식으로 수정 해서는 안 됩니다. 그러나 `PhraseList` 요소와 연결 된 요소는 `ListenFor` 프로그래밍 방식으로 수정할 수 있습니다. 응용 프로그램은 `PhraseList` 사용자가 앱을 사용할 때 생성 되는 데이터 집합에 따라 런타임에 요소의 콘텐츠를 수정 해야 합니다.
       >
       > 자세한 내용은 [CORTANA VCD 문구 목록 동적 수정](cortana-dynamically-modify-voice-command-definition-vcd-phrase-lists.md)을 참조 하세요.  

    - `Feedback`응용 프로그램이 시작 될 때 **Cortana** 에서 표시 하 고 말할 텍스트를 포함 하는 요소입니다.  

`Navigate`요소는 음성 명령이 앱을 포그라운드로 활성화 함을 나타냅니다. 이 예제에서 ```showTripToDestination``` 명령은 전경 작업입니다.  

`VoiceCommandService`요소는 음성 명령이 백그라운드에서 앱을 활성화 함을 나타냅니다. `Target`이 요소의 특성 값은 `Name` [`uap:AppService`](/uwp/schemas/appxpackage/uapmanifestschema/element-uap-appservice) appxmanifest.xml 파일에 있는 요소의 특성 값과 일치 해야 합니다. 이 예제에서 `whenIsTripToDestination` 및 `cancelTripToDestination` 명령은 app service의 이름을로 지정 하는 백그라운드 작업입니다 `AdventureWorksVoiceCommandService` .  

자세한 내용은 [**VCD 요소 및 특성 v 1.2**](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2) 참조를 참조 하세요.  

예: [](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2) `en-us` **놀이 Works** 앱에 대 한 음성 명령을 정의 하는 VCD 파일의 일부입니다.  

```xml
<?xml version="1.0" encoding="utf-8" ?>
<VoiceCommands xmlns="https://schemas.microsoft.com/voicecommands/1.2">
<CommandSet xml:lang="en-us" Name="AdventureWorksCommandSet_en-us">
    <AppName> Adventure Works </AppName>
    <Example> Show trip to London </Example>
    
    <Command Name="showTripToDestination">
        <Example> Show trip to London </Example>
        <ListenFor RequireAppName="BeforeOrAfterPhrase"> show [my] trip to {destination} </ListenFor>
        <ListenFor RequireAppName="ExplicitlySpecified"> show [my] {builtin:AppName} trip to {destination} </ListenFor>
        <Feedback> Showing trip to {destination} </Feedback>
        <Navigate />
    </Command>
      
    <Command Name="whenIsTripToDestination">
        <Example> When is my trip to Las Vegas?</Example>
        <ListenFor RequireAppName="BeforeOrAfterPhrase"> when is [my] trip to {destination}</ListenFor>
        <ListenFor RequireAppName="ExplicitlySpecified"> when is [my] {builtin:AppName} trip to {destination} </ListenFor>
        <Feedback> Looking for trip to {destination}</Feedback>
        <VoiceCommandService Target="AdventureWorksVoiceCommandService"/>
    </Command>
    
    <Command Name="cancelTripToDestination">
        <Example> Cancel my trip to Las Vegas </Example>
        <ListenFor RequireAppName="BeforeOrAfterPhrase"> cancel [my] trip to {destination}</ListenFor>
        <ListenFor RequireAppName="ExplicitlySpecified"> cancel [my] {builtin:AppName} trip to {destination} </ListenFor>
        <Feedback> Cancelling trip to {destination}</Feedback>
        <VoiceCommandService Target="AdventureWorksVoiceCommandService"/>
    </Command>

    <PhraseList Label="destination">
        <Item>London</Item>
        <Item>Las Vegas</Item>
        <Item>Melbourne</Item>
        <Item>Yosemite National Park</Item>
    </PhraseList>
</CommandSet>
```  

## <a name="install-the-vcd-commands"></a>VCD 명령 설치  

앱은 VCD를 설치 하기 위해 한 번 실행 해야 합니다.  

>[!NOTE]
> 음성 명령 데이터는 앱 설치 간에 유지 되지 않습니다. 앱에 대 한 음성 명령 데이터가 그대로 유지 되도록 하려면 앱이 시작 되거나 활성화 될 때마다 VCD 파일을 초기화 하거나, 현재 VCD가 설치 되어 있는지 여부를 나타내는 설정을 유지 관리 하는 것이 좋습니다.  

파일 `app.xaml.cs`에서:  

1. 다음 using 지시문을 추가합니다.

   ```csharp
   using Windows.Storage;
   ```

2. `OnLaunched`비동기 한정자를 사용 하 여 메서드를 표시 합니다.  

   ```csharp
   protected async override void OnLaunched(LaunchActivatedEventArgs e)
   ```  

3. [`InstallCommandDefinitionsFromStorageFileAsync`](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandDefinitionManager)처리기에서 메서드를 호출 [`OnLaunched`](/uwp/api/Windows.UI.Xaml.Application) 하 여 인식 해야 하는 음성 명령을 등록 합니다.  
    예: 어드벤처 Works 앱은 개체를 정의 합니다 [`StorageFile`](/uwp/api/Windows.Storage.StorageFile) .  
    예: 메서드를 호출 하 여 파일을 사용 하 여 [`GetFileAsync`](/uwp/api/Windows.Storage.StorageFolder) 개체를 초기화 [`StorageFile`](/uwp/api/Windows.Storage.StorageFile) `AdventureWorksCommands.xml` 합니다.  
    [`StorageFile`](/uwp/api/Windows.Storage.StorageFile)그런 다음 개체는 메서드에 전달 됩니다 [`InstallCommandDefinitionsFromStorageFileAsync`](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandDefinitionManager) .  

   ```csharp
   try {
      // Install the main VCD. 
      StorageFile vcdStorageFile = await Package.Current.InstalledLocation.GetFileAsync(
            @"AdventureWorksCommands.xml"
      );
       
      await Windows.ApplicationModel.VoiceCommands.VoiceCommandDefinitionManager.InstallCommandDefinitionsFromStorageFileAsync(vcdStorageFile);
     
      // Update phrase list.
      ViewModel.ViewModelLocator locator = App.Current.Resources["ViewModelLocator"] as ViewModel.ViewModelLocator;
      if(locator != null) {
            await locator.TripViewModel.UpdateDestinationPhraseList();
        }
    }
    catch (Exception ex) {
        System.Diagnostics.Debug.WriteLine("Installing Voice Commands Failed: " + ex.ToString());
    }
    ```  

## <a name="handle-activation"></a>활성화 처리  

앱이 후속 음성 명령 활성화에 응답 하는 방법을 지정 합니다.

>[!NOTE]
> 음성 명령 집합이 설치 된 후에는 앱을 한 번 이상 시작 해야 합니다.  

1. 음성 명령으로 앱을 활성화 했는지 확인 합니다.  

    이벤트를 재정의 [`Application.OnActivated`](/uwp/api/Windows.UI.Xaml.Application) 하 고 [**IActivatedEventArgs**](/uwp/api/Windows.ApplicationModel.Activation.IActivatedEventArgs)여부를 확인 합니다.[****](/uwp/api/Windows.ApplicationModel.Activation.IActivatedEventArgs) [**VoiceCommand**](/uwp/api/Windows.ApplicationModel.Activation.ActivationKind)종류입니다.  

2. 명령의 이름과 음성 정보를 확인 합니다.  

    [`VoiceCommandActivatedEventArgs`](/uwp/api/Windows.ApplicationModel.Activation.VoiceCommandActivatedEventArgs) [**IActivatedEventArgs**](/uwp/api/Windows.ApplicationModel.Activation.IActivatedEventArgs) 에서 개체에 대 한 참조를 가져오고 [`Result`](/uwp/api/Windows.ApplicationModel.Activation.VoiceCommandActivatedEventArgs) 개체에 대 한 속성을 쿼리 합니다 [`SpeechRecognitionResult`](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionResult) .  

    사용자가 수행한 작업을 확인 하려면 사전에 있는 인식 된 구의 [**텍스트**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionResult) 값 또는 의미 체계 속성을 확인 합니다 [`SpeechRecognitionSemanticInterpretation`](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionSemanticInterpretation) .  

3. 원하는 페이지로 이동 하는 것과 같이 앱에서 적절 한 작업을 수행 합니다.  

    >[!NOTE]
    > VCD를 참조 해야 하는 경우에는 [Vcd 파일 편집](#edit-the-vcd-file) 섹션을 방문 하세요.

    음성 명령의 음성 인식 결과를 받은 후에는 배열의 첫 번째 값에서 명령 이름을 가져옵니다 [`RulePath`](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionResult) . VCD 파일은 둘 이상의 가능한 음성 명령을 정의 하므로 값이 VCD의 명령 이름과 일치 하는지 확인 하 고 적절 한 조치를 취해야 합니다.  

    응용 프로그램에 대 한 가장 일반적인 작업은 음성 명령의 컨텍스트와 관련 된 콘텐츠가 있는 페이지로 이동 하는 것입니다.  
    예: **TripPage** 페이지를 열고 음성 명령의 값, 명령이 입력 된 방법 및 인식 되는 대상 구 (해당 하는 경우)를 전달 합니다. 또는 **TripPage** 페이지로 이동할 때 앱이 [**SpeechRecognitionResult**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionResult) 에 탐색 매개 변수를 보낼 수 있습니다.  

    앱을 실행 한 음성 명령이 실제로 음성으로 전달 되었는지 아니면 [`SpeechRecognitionSemanticInterpretation.Properties`](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionSemanticInterpretation) **commandmode** 키를 사용 하 여 사전에서 텍스트로 입력 되었는지 확인할 수 있습니다. 이 키의 값은 `voice` 또는 `text` 입니다. 키 값이 이면 `voice` 앱에서 음성 합성 ([**SpeechSynthesis**](/uwp/api/Windows.Media.SpeechSynthesis))을 사용 하 여 사용자에 게 음성 피드백을 제공 하는 것이 좋습니다.  

    [**SpeechRecognitionSemanticInterpretation**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionSemanticInterpretation) 를 사용 하 여 `PhraseList` `PhraseTopic` 요소의 또는 제약 조건에서 말하는 콘텐츠를 찾을 수 있습니다. `ListenFor` 사전 키는 `Label` 또는 요소의 특성에 대 한 값입니다 `PhraseList` `PhraseTopic` .
    예: **{destination}** 구 값에 액세스 하는 방법에 대 한 다음 코드입니다.  

    ```csharp
    /// <summary>
    /// Entry point for an application activated by some means other than normal launching. 
    /// This includes voice commands, URI, share target from another app, and so on. 
    /// 
    /// NOTE:
    /// A previous version of the VCD file might remain in place 
    /// if you modify it and update the app through the store. 
    /// Activations might include commands from older versions of your VCD. 
    /// Try to handle these commands gracefully.
    /// </summary>
    /// <param name="args">Details about the activation method.</param>
    protected override void OnActivated(IActivatedEventArgs args) {
        base.OnActivated(args);
        
        Type navigationToPageType;
        ViewModel.TripVoiceCommand? navigationCommand = null;
        
        // Voice command activation.
        if (args.Kind == ActivationKind.VoiceCommand) {
            // Event args may represent many different activation types. 
            // Cast the args so that you only get useful parameters out.
            var commandArgs = args as VoiceCommandActivatedEventArgs;
            
            Windows.Media.SpeechRecognition.SpeechRecognitionResult speechRecognitionResult = commandArgs.Result;
            
            // Get the name of the voice command and the text spoken.
            // See VoiceCommands.xml for supported voice commands.
            string voiceCommandName = speechRecognitionResult.RulePath[0];
            string textSpoken = speechRecognitionResult.Text;
            
            // commandMode indicates whether the command was entered using speech or text.
            // Apps should respect text mode by providing silent (text) feedback.
            string commandMode = this.SemanticInterpretation("commandMode", speechRecognitionResult);
            
            switch (voiceCommandName) {
                case "showTripToDestination":
                    // Access the value of {destination} in the voice command.
                    string destination = this.SemanticInterpretation("destination", speechRecognitionResult);
                    
                    // Create a navigation command object to pass to the page.
                    navigationCommand = new ViewModel.TripVoiceCommand(
                        voiceCommandName,
                        commandMode,
                        textSpoken,
                        destination
                    );
              
                    // Set the page to navigate to for this voice command.
                    navigationToPageType = typeof(View.TripDetails);
                    break;
                default:
                    // If not able to determine what page to launch, then go to the default entry point.
                    navigationToPageType = typeof(View.TripListView);
                    break;
            }
        }
        // Protocol activation occurs when a card is selected within Cortana (using a background task).
        else if (args.Kind == ActivationKind.Protocol) {
            // Extract the launch context. In this case, use the destination from the phrase set (passed
            // along in the background task inside Cortana), which makes no attempt to be unique. A unique id or 
            // identifier is ideal for more complex scenarios. The destination page is left to check if the 
            // destination trip still exists, and navigate back to the trip list if it does not.
            var commandArgs = args as ProtocolActivatedEventArgs;
            Windows.Foundation.WwwFormUrlDecoder decoder = new Windows.Foundation.WwwFormUrlDecoder(commandArgs.Uri.Query);
            var destination = decoder.GetFirstValueByName("LaunchContext");
            
            navigationCommand = new ViewModel.TripVoiceCommand(
                "protocolLaunch",
                "text",
                "destination",
                destination
            );
            
            navigationToPageType = typeof(View.TripDetails);
        }
        else {
            // If launched using any other mechanism, fall back to the main page view.
            // Otherwise, the app will freeze at a splash screen.
            navigationToPageType = typeof(View.TripListView);
        }
        
        // Repeat the same basic initialization as OnLaunched() above, taking into account whether
        // or not the app is already active.
        Frame rootFrame = Window.Current.Content as Frame;
        
        // Do not repeat app initialization when the Window already has content,
        // just ensure that the window is active.
        if (rootFrame == null) {
            // Create a frame to act as the navigation context and navigate to the first page.
            rootFrame = new Frame();
            App.NavigationService = new NavigationService(rootFrame);
            
            rootFrame.NavigationFailed += OnNavigationFailed;
            
            // Place the frame in the current window.
            Window.Current.Content = rootFrame;
        }
        
        // Since the expectation is to always show a details page, navigate even if 
        // a content frame is in place (unlike OnLaunched).
        // Navigate to either the main trip list page, or if a valid voice command
        // was provided, to the details page for that trip.
        rootFrame.Navigate(navigationToPageType, navigationCommand);
        
        // Ensure the current window is active
        Window.Current.Activate();
    }

    /// <summary>
    /// Returns the semantic interpretation of a speech result. 
    /// Returns null if there is no interpretation for that key.
    /// </summary>
    /// <param name="interpretationKey">The interpretation key.</param>
    /// <param name="speechRecognitionResult">The speech recognition result to get the semantic interpretation from.</param>
    /// <returns></returns>
    private string SemanticInterpretation(string interpretationKey, SpeechRecognitionResult speechRecognitionResult) {
        return speechRecognitionResult.SemanticInterpretation.Properties[interpretationKey].FirstOrDefault();
    }
    ```  

## <a name="handle-the-voice-command-in-the-app-service"></a>App Service에서 음성 명령을 처리 합니다.  

App service에서 음성 명령을 처리 합니다.  

1. 음성 명령 서비스 파일에 다음 using 지시문을 추가 합니다.  
    예: `AdventureWorksVoiceCommandService.cs`.  

    ```csharp
        using Windows.ApplicationModel.VoiceCommands;
        using Windows.ApplicationModel.Resources.Core;
        using Windows.ApplicationModel.AppService;
    ```  

2. 음성 명령을 처리 하는 동안 app service가 종료 되지 않도록 서비스를 지연 합니다.  
3. 백그라운드 작업이 음성 명령으로 활성화 된 app service로 실행 중인지 확인 합니다.  
    1. [**IBackgroundTaskInstance**](/uwp/api/Windows.ApplicationModel.Background.IBackgroundTaskInstance) 에 [**AppService를 AppServiceTriggerDetails**](/uwp/api/Windows.ApplicationModel.AppService.AppServiceTriggerDetails)로 캐스팅 합니다.  
    2. [**IBackgroundTaskInstance.TriggerDetails.Name**](/uwp/api/Windows.ApplicationModel.Background.IBackgroundTaskRegistration) 이 파일의 app service 이름 인지 확인 `Package.appxmanifest` 합니다.  
4. [**IBackgroundTaskInstance**](/uwp/api/Windows.ApplicationModel.Background.IBackgroundTaskInstance) 를 사용 하 여 **Cortana** 에서 음성 명령을 검색 하는 [**VoiceCommandServiceConnection**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection) 를 만듭니다.
5. [**VoiceCommandServiceConnection**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection)에 대 한 이벤트 처리기를 등록 합니다.  사용자 취소로 인해 app service가 닫힐 때 알림을 [**VoiceCommandCompleted**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection) .  
6. 예기치 않은 오류로 인해 app service가 닫힐 때 알림을 받도록 IBackgroundTaskInstance에 대 한 이벤트 처리기를 등록 [**합니다**](/uwp/api/Windows.ApplicationModel.Background.IBackgroundTaskInstance) .  
7. 명령의 이름과 음성 정보를 확인 합니다.  
    1. [**VoiceCommand**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommand)를 사용 합니다. [**CommandName**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommand) 속성을 지정 하 여 음성 명령의 이름을 결정 합니다.  
    2. 사용자가 수행한 작업을 확인 하려면 사전에 있는 인식 된 구의 [**텍스트**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionResult) 값 또는 의미 체계 속성을 확인 합니다 [`SpeechRecognitionSemanticInterpretation`](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionSemanticInterpretation) .  
8. App service에서 적절 한 작업을 수행 합니다.  
9. **Cortana** 를 사용 하 여 음성 명령에 대 한 피드백을 표시 하 고 말합니다.  
    1. **Cortana** 에서 음성 명령에 대 한 응답으로 사용자에 게 표시 하 고 말할 문자열을 결정 하 고 개체를 만듭니다 [`VoiceCommandResponse`](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandResponse) . **Cortana** 에서 표시 하 고 말하는 피드백 문자열을 선택 하는 방법에 대 한 지침은 [cortana 디자인 지침](cortana-design-guidelines.md)을 참조 하세요.  
    2. [**VoiceCommandServiceConnection**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection) 인스턴스를 사용 하 여 개체에 [**ReportProgressAsync**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection) 또는 [**ReportSuccessAsync**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection) 를 호출 하 여 **Cortana** 에 진행률 또는 완료를 보고 합니다 `VoiceCommandServiceConnection` .  

    >[!NOTE]
    > VCD를 참조 해야 하는 경우에는 [Vcd 파일 편집](#edit-the-vcd-file) 섹션을 방문 하세요.  

    ```csharp
    public sealed class VoiceCommandService : IBackgroundTask {
        private BackgroundTaskDeferral serviceDeferral;
        VoiceCommandServiceConnection voiceServiceConnection;
        
        public async void Run(IBackgroundTaskInstance taskInstance) {
            //Take a service deferral so the service isn&#39;t terminated.
            this.serviceDeferral = taskInstance.GetDeferral();
            
            taskInstance.Canceled += OnTaskCanceled;
            
            var triggerDetails = taskInstance.TriggerDetails as AppServiceTriggerDetails;
    
            if (triggerDetails != null &amp;&amp; 
                triggerDetails.Name == "AdventureWorksVoiceServiceEndpoint") {
                try {
                    voiceServiceConnection = 
                    VoiceCommandServiceConnection.FromAppServiceTriggerDetails(
                        triggerDetails);
                    voiceServiceConnection.VoiceCommandCompleted += 
                    VoiceCommandCompleted;
                    
                    VoiceCommand voiceCommand = await 
                    voiceServiceConnection.GetVoiceCommandAsync();
                    
                    switch (voiceCommand.CommandName) {
                        case "whenIsTripToDestination":
                            {
                                var destination = 
                                voiceCommand.Properties["destination"][0];
                                SendCompletionMessageForDestination(destination);
                                break;
                            }
                            
                            // As a last resort, launch the app in the foreground.
                        default:
                            LaunchAppInForeground();
                            break;
                    }
                }
                finally {
                    if (this.serviceDeferral != null) {
                        // Complete the service deferral.
                        this.serviceDeferral.Complete();
                    }
                }
            }
        }
        
        private void VoiceCommandCompleted(VoiceCommandServiceConnection sender,
            VoiceCommandCompletedEventArgs args) {
            if (this.serviceDeferral != null) {
                // Insert your code here.
                // Complete the service deferral.
                this.serviceDeferral.Complete();
            }
        }
        
        private async void SendCompletionMessageForDestination(
            string destination) {
            // Take action and determine when the next trip to destination
            // Insert code here.
            
            // Replace the hardcoded strings used here with strings 
            // appropriate for your application.
            
            // First, create the VoiceCommandUserMessage with the strings 
            // that Cortana will show and speak.
            var userMessage = new VoiceCommandUserMessage();
            userMessage.DisplayMessage = "Here's your trip.";
            userMessage.SpokenMessage = "Your trip to Vegas is on August 3rd.";
            
            // Optionally, present visual information about the answer.
            // For this example, create a VoiceCommandContentTile with an 
            // icon and a string.
            var destinationsContentTiles = new List<VoiceCommandContentTile>();
            
            var destinationTile = new VoiceCommandContentTile();
            destinationTile.ContentTileType = 
                VoiceCommandContentTileType.TitleWith68x68IconAndText;
            // The user taps on the visual content to launch the app. 
            // Pass in a launch argument to enable the app to deep link to a 
            // page relevant to the item displayed on the content tile.
            destinationTile.AppLaunchArgument = 
                string.Format("destination={0}", "Las Vegas");
            destinationTile.Title = "Las Vegas";
            destinationTile.TextLine1 = "August 3rd 2015";
            destinationsContentTiles.Add(destinationTile);
            
            // Create the VoiceCommandResponse from the userMessage and list    
            // of content tiles.
            var response = VoiceCommandResponse.CreateResponse(
                userMessage, destinationsContentTiles);
            
            // Cortana displays a "Go to app_name" link that the user 
            // taps to launch the app. 
            // Pass in a launch to enable the app to deep link to a page 
            // relevant to the voice command.
            response.AppLaunchArgument = string.Format(
                "destination={0}", "Las Vegas");
            
            // Ask Cortana to display the user message and content tile and 
            // also speak the user message.
            await voiceServiceConnection.ReportSuccessAsync(response);
        }
        
        private async void LaunchAppInForeground() {
            var userMessage = new VoiceCommandUserMessage();
            userMessage.SpokenMessage = "Launching Adventure Works";
            
            var response = VoiceCommandResponse.CreateResponse(userMessage);
            
            // When launching the app in the foreground, pass an app 
            // specific launch parameter to indicate what page to show.
            response.AppLaunchArgument = "showAllTrips=true";
            
            await voiceServiceConnection.RequestAppLaunchAsync(response);
        }
    }
    ```  

활성화 되 면 app service에서 [**ReportSuccessAsync**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection)를 호출 하는 데 0.5 초가 발생 합니다. **Cortana** 는 피드백 문자열을 표시 하 고 표시 합니다.  

>[!NOTE]
> VCD 파일에서 **피드백** 문자열을 선언할 수 있습니다. 이 문자열은 Cortana 캔버스에 표시 되는 UI 텍스트에 영향을 주지 않고 **cortana** 에서 말하는 텍스트에만 영향을 줍니다.  

앱이 전화를 거는 데 0.5 초 보다 오래 걸리면 **Cortana** 는 여기에 표시 된 것 처럼 손을 끌 화면을 삽입 합니다. **Cortana** 는 응용 프로그램이 **ReportSuccessAsync** 를 호출할 때까지 또는 최대 5 초 동안 백오프 화면을 표시 합니다. App service가 **ReportSuccessAsync** 를 호출 하지 않거나 [`VoiceCommandServiceConnection`](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection) 정보를 사용 하 여 **Cortana** 를 제공 하는 메서드를 호출 하지 않으면 사용자에 게 오류 메시지가 수신 되 고 app service가 취소 됩니다.  

:::image type="content" source="images/cortana/cortana-backgroundapp-progress-result.png" alt-text="백그라운드에서 AdventureWorks 앱을 사용 하 여 진행률 및 결과 화면을 사용 하는 기본 쿼리 및 Cortana의 스크린샷":::

## <a name="related-articles"></a>관련된 문서

- [Windows 앱에서 Cortana 상호 작용](cortana-interactions.md)
- [Cortana를 통해 음성 명령으로 포그라운드 앱 활성화](cortana-launch-a-foreground-app-with-voice-commands.md)
- [VCD 요소 및 특성 v 1.2](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2)
- [Cortana 디자인 지침](cortana-design-guidelines.md)
- [Cortana 음성 명령 샘플](https://go.microsoft.com/fwlink/p/?LinkID=619899)
