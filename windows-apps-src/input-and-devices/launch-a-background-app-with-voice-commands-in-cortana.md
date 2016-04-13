---
Description: Cortana 내에서 음성 명령을 사용하여 시스템 기능에 액세스하는 것 외에, 앱 내에서 실행할 작업 또는 명령을 지정하는 음성 명령을 사용하여 백그라운드 앱의 기능으로 Cortana를 확장할 수도 있습니다.
title: Cortana에서 음성 명령으로 백그라운드 앱 시작
ms.assetid: DF5B530C-57DD-4CA5-B3BE-1A0B3695C9C6
label: Launch a background app
template: detail.hbs
---

# Cortana를 통해 음성 명령으로 백그라운드 앱 활성화


\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]


**중요 API**

-   [**Windows.ApplicationModel.VoiceCommands**](https://msdn.microsoft.com/library/windows/apps/dn706594)
-   [**VCD 요소 및 특성 v1.2**](https://msdn.microsoft.com/library/windows/apps/dn706593)

**Cortana** 내에서 음성 명령을 사용하여 시스템 기능에 액세스하는 것 외에, 앱에서 실행할 작업 또는 명령을 지정하는 음성 명령을 사용하여 백그라운드 작업의 기능으로 **Cortana**를 확장할 수도 있습니다. 앱이 백그라운드에서 음성 명령을 처리하는 경우 포커스를 받지 않습니다. 대신, **Cortana** 캔버스와 **Cortana** 음성을 통해 모든 피드백과 결과를 반환합니다.

조작의 복잡성에 따라 앱을 포그라운드로 활성화(앱에 포커스가 있음)하거나 백그라운드에서 활성화(**Cortana**에 포커스가 유지됨)할 수 있습니다. 예를 들어 추가 컨텍스트 또는 사용자 입력(예: 특정 연락처에 메시지 보내기)이 필요한 음성 명령은 포그라운드 앱에서 가장 잘 처리할 수 있지만, 기본 명령(예: 예정된 여행 목록 표시)은 백그라운드 앱을 통해 **Cortana**에서 처리할 수 있습니다.

음성 명령을 사용하여 앱을 포그라운드로 활성화하려는 경우 [Cortana를 통해 음성 명령으로 포그라운드 앱 활성화](launch-a-foreground-app-with-voice-commands-in-cortana.md)를 참조하세요.

> **참고**  
> 음성 명령은 **Cortana**를 통해 설치된 앱에 대한 특정한 의도가 있는 한 번의 말하기로, VCD(음성 명령 정의) 파일에 정의되어 있습니다.

> VCD 파일은 각각 고유한 의도가 있는 음성 명령을 하나 이상 정의합니다.

> 음성 명령 정의는 복잡성에 따라 달라질 수 있습니다. 또한 한 마디의 제한된 말하기부터 동일한 의도를 모두 나타내는 더욱 유연하고 다양한 자연어 표현에 이르기까지 다양한 말하기를 지원할 수 있습니다.

백그라운드 앱 기능을 보여 주기 위해 [Cortana 음성 명령 샘플](http://go.microsoft.com/fwlink/p/?LinkID=619899)에서 **Adventure Works**라는 여행 계획 및 관리 앱을 사용하겠습니다.

**Cortana** 캔버스에 통합된 **Adventure Works** 앱의 개요는 다음과 같습니다.

![Cortana 캔버스 개요 ](images/cortana-overview.png)

**Cortana** 없이 **Adventure Works** 여행을 보기 위해 사용자는 앱을 시작하고 **향후 여행** 페이지로 이동합니다.

**Cortana**를 통해 음성 명령을 사용하여 백그라운드에서 앱을 시작하려면 사용자는 대신 "Adventure Works, 라스베이거스 여행이 언제야?"라고 말하면 됩니다. 앱에서 명령을 처리하고 **Cortana**에 앱 아이콘 및 기타 앱 정보(제공된 경우)와 함께 결과가 표시됩니다. 다음은 기본 여행 쿼리의 예와 "다음 라스베이거스 여행은 8월 1일입니다."라고 보여 주고 말하는 **Cortana** 결과 화면입니다.

![백그라운드에서 Adventure Works 앱을 사용하는 기본 쿼리 및 결과 화면](images/cortana-backgroundapp-result.png)

음성 명령 기능을 추가하고 음성 또는 키보드 입력을 사용하여 앱의 백그라운드 기능으로 **Cortana**을(를) 확장하는 기본 단계는 다음과 같습니다.

1.  백그라운에서 **Cortana**가 호출하는 앱 서비스([**Windows.ApplicationModel.AppService**](https://msdn.microsoft.com/library/windows/apps/dn921731) 참조)를 만듭니다.
2.  VCD 파일을 만듭니다. 이 파일은 앱을 활성화할 때 작업을 시작하거나 명령을 호출하기 위해 사용자가 말할 수 있는 모든 음성 명령을 정의하는 XML 문서입니다. [
            **VCD elements and attributes v1.2**](https://msdn.microsoft.com/library/windows/apps/dn706593)을 참조하세요.
3.  앱이 시작되면 VCD 파일에 명령 집합을 등록합니다.
4.  앱 서비스의 백그라운드 활성화 및 음성 명령 실행을 처리합니다.
5.  **Cortana** 내에서 음성 명령에 대한 적절한 피드백을 표시하고 말합니다.

**필수 조건: **

UWP(유니버설 Windows 플랫폼) 앱을 처음 개발하는 경우 다음 항목을 검토하여 여기서 설명하는 기술에 대해 알아보세요.

-   [첫 번째 앱 만들기](https://msdn.microsoft.com/library/windows/apps/bg124288)
-   이벤트에 대한 자세한 내용은 [이벤트 및 라우트된 이벤트 개요](https://msdn.microsoft.com/library/windows/apps/mt185584)를 참조하세요.

**사용자 환경 지침: **

**Cortana**와 앱을 통합하는 방법에 대한 자세한 내용은 [Cortana 디자인 지침](https://msdn.microsoft.com/library/windows/apps/dn974233)을 참조하고, 유용하고 매력적인 음성 사용 앱 디자인에 도움이 되는 팁은 [음성 디자인 지침](https://msdn.microsoft.com/library/windows/apps/dn596121)을 참조하세요.

## <span id="Create_a_new_solution_with_a_primary_project_in_Visual_Studio"> </span> <span id="create_a_new_solution_with_a_primary_project_in_visual_studio"> </span> <span id="CREATE_A_NEW_SOLUTION_WITH_A_PRIMARY_PROJECT_IN_VISUAL_STUDIO"> </span>Visual Studio의 기본 프로젝트로 새 솔루션 만들기


1.  Microsoft Visual Studio 2015를 시작합니다.

    Visual Studio 2015 시작 페이지가 표시됩니다.

2.  **파일** 메뉴에서 **새로 만들기** > **프로젝트**를 선택합니다.

    **새 프로젝트** 대화 상자가 나타납니다. 대화 상자의 왼쪽 창에서 표시할 템플릿 유형을 선택할 수 있습니다.

3.  왼쪽 창에서 **설치됨 > 템플릿 > Visual C\# > Windows**를 확장하고 **유니버설** 템플릿 그룹을 선택합니다. 대화 상자의 가운데 창에 UWP(유니버설 Windows 플랫폼) 앱용 프로젝트 템플릿 목록이 표시됩니다.
4.  가운데 창에서 **비어 있는 앱(유니버설 Windows)** 템플릿을 선택합니다.

    **비어 있는 앱** 템플릿은 컴파일과 실행은 가능하지만 사용자 인터페이스 컨트롤이나 데이터는 포함되지 않은 최소한의 UWP 앱을 만듭니다. 이 자습서를 진행하면서 이 앱에 컨트롤을 추가하게 됩니다.

5.  **이름** 텍스트 상자에 프로젝트 이름을 입력합니다. 이 예제에서는 "AdventureWorks"를 사용합니다.
6.  **확인**을 클릭하여 프로젝트를 만듭니다.

    Microsoft Visual Studio에서 프로젝트를 만들고 **솔루션 탐색기**에 표시합니다.


## <span id="Add_image_assets_to_primary_project_and_specify_them_in_the_app_manifest"> </span> <span id="add_image_assets_to_primary_project_and_specify_them_in_the_app_manifest"> </span> <span id="ADD_IMAGE_ASSETS_TO_PRIMARY_PROJECT_AND_SPECIFY_THEM_IN_THE_APP_MANIFEST"> </span>기본 프로젝트에 이미지 자산 추가 및 앱 매니페스트에서 지정
      
UWP 앱에서 특정 설정 및 디바이스 기능(고대비, 유효 픽셀, 로캘 등)에 따라 가장 적절한 이미지를 자동으로 선택할 수 있습니다. 단지 이미지를 제공하고 앱 프로젝트 내에서 다른 리소스 버전에 대한 적절한 명명 규칙 및 폴더 조직을 사용하면 됩니다. 권장 리소스 버전을 제공하지 않으면 사용자의 기본 설정, 능력, 디바이스 유형 및 위치에 따라 접근성, 지역화 및 이미지 품질이 떨어질 수 있습니다.

고대비 및 배율 인수용 이미지 리소스에 대한 자세한 내용은 [타일 및 아이콘 자산에 대한 지침](https://msdn.microsoft.com/en-us/windows/uwp/controls-and-patterns/tiles-and-notifications-app-assets)을 참조하세요.

한정자를 사용하여 리소스 이름을 지정합니다. 리소스 한정자는 특정 리소스 버전을 사용할 컨텍스트를 식별하는 폴더 및 파일 이름 한정자입니다.

표준 명명 규칙은 `foldername/qualifiername-value[_qualifiername-value]/filename.qualifiername-value[_qualifiername-value].ext`입니다. 예를 들어 `images/en-US/logo.scale-100_contrast-white.png`는 `images/logo.png`라는 루트 폴더 및 파일 이름만 사용하여 코드에서 참조될 수 있습니다. [한정자를 사용하여 리소스 이름을 지정하는 방법](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/hh965324.aspx)을 참조하세요.

문자열 리소스 파일(예: "`en-US\resources.resw`")의 기본 언어 및 이미지(예: "`logo.scale-100.png`")의 기본 배율 요소를 표시하는 것이 좋습니다. 이는 지역화된 해상도 리소스 또는 여러 해상도 리소스를 제공하지 않으려는 경우에도 마찬가지입니다. 그러나 최소한 100, 200 및 400 배율 인수에 대한 자산을 제공하는 것이 좋습니다.

> *중요

> **Cortana** 캔버스의 제목 영역에서 사용되는 앱 아이콘은 "Package.appxmanifest" 파일에 지정된 Square44x44Logo 아이콘입니다. 

> 또한 **Cortana** 캔버스의 콘텐츠 영역에 있는 각 항목에 대해 아이콘을 지정할 수도 있습니다. 결과 아이콘에 대한 유효한 이미지 크기는 다음과 같습니다.

> - 68w x 68h 
> - 68w x 92h 
> - 280w x 140h 

[
            **VoiceCommandResponse**](https://msdn.microsoft.com/library/windows/apps/dn974182)가 [**VoiceCommandServiceConnection**](https://msdn.microsoft.com/library/windows/apps/dn974204)에 전달될 때까지 콘텐츠 타일의 유효성이 검사되지 않습니다. 이러한 크기 비율을 준수하지 않는 이미지가 포함된 콘텐츠 타일이 있는 **VoiceCommandResponse** 개체를 **Cortana**에 전달하는 경우 예외가 발생할 수 있습니다. 

**Adventure Works** 앱(VoiceCommandService\\AdventureWorksVoiceCommandService.cs)의 이 예제에서, [**TitleWith68x68IconAndText**](https://msdn.microsoft.com/library/windows/apps/dn974169) 타일 템플릿을 사용하여 단순한 회색 정사각형("GreyTile.png")을 [**VoiceCommandContentTile**](https://msdn.microsoft.com/library/windows/apps/dn974168)에 지정합니다. 로고 변형은 VoiceCommandService\\Images에 있으며, [**GetFileFromApplicationUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701741) 메서드를 사용하여 검색됩니다.

```CSharp
var destinationTile = new VoiceCommandContentTile();

destinationTile.ContentTileType = 
  VoiceCommandContentTileType.TitleWith68x68IconAndText;
destinationTile.Image = 
  await StorageFile.GetFileFromApplicationUriAsync(
    new Uri("ms-appx:///AdventureWorks.VoiceCommands/Images/GreyTile.png"));
```

## <span id="Create_an_app_service_project"> </span> <span id="create_an_app_service_project"> </span> <span id="CREATE_AN_APP_SERVICE_PROJECT"> </span>앱 서비스 프로젝트 만들기

<ol>
    <li>
    Right-click your Solution name, select **New > Project**.
    </li>
    <li>
    Under **Installed > Templates > Visual C# > Windows > Universal**, select **Windows Runtime Component**. This is the component that implements the app service (**[Windows.ApplicationModel.AppService](https://msdn.microsoft.com/library/windows/apps/dn921731)**).
    </li>
    <li>
    Type a name for the project (for example, "VoiceCommandService") and click **OK**.
    </li>
    <li>
    In **Solution Explorer**, select the "VoiceCommandService" project and rename the "Class1.cs" file generated by Visual Studio. For the **Adventure Works** example we use "AdventureWorksVoiceCommandService.cs".
    </li>
    <li>
    Click **Yes** when asked if you want to rename all occurrences of "Class1.cs". 
    </li>
    <li>
    In the "AdventureWorksVoiceCommandService.cs" file:
        <ol type="i">
 <li>
 다음 using 지시문을 추가합니다.  
 ```using Windows.ApplicationModel.Background;```
 </li>
 <li>
 새 프로젝트를 만들 때 프로젝트 이름은 모든 파일에서 기본 루트 네임스페이스로 사용됩니다. 네임스페이스 이름을 바꾸어 앱 서비스 코드를 기본 프로젝트 아래에 중첩합니다. 예를 들면 `namespace AdventureWorks.VoiceCommands`입니다. 
 </li>
 <li>
 솔루션 탐색기에서 앱 서비스 프로젝트 이름을 마우스 오른쪽 단추로 클릭하고 **속성**을 선택합니다. 
 </li>
 <li>
 **라이브러리** 탭에서 **기본 네임스페이스** 필드를 이 동일한 값(이 예제에서는 "AdventureWorks.VoiceCommands")으로 업데이트합니다. 
 </li>
 <li>
 [
            **IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794) 인터페이스를 구현하는 새 클래스를 만듭니다. 이 클래스에는 Cortana에서 음성 명령을 인식할 때의 진입점인 [**Run**](https://msdn.microsoft.com/library/windows/apps/br224811) 메서드가 필요합니다. 
 </li>
        </ol>
    </li>
</ol>

다음은 **Adventure Works** 앱의 기본 백그라운드 작업 클래스입니다. 나중에 세부 정보를 더 채우겠습니다.
> **참고**   
> 백그라운드 작업 클래스 자체와 백그라운드 작업 프로젝트의 다른 모든 클래스가 봉인된 public 클래스여야 합니다.
 
``` csharp
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
        /// Background tasks must respond to activation by Cortana within 0.5 seconds, and must 
        /// report progress to Cortana every 5 seconds (unless Cortana is waiting for user
        /// input). There is no execution time limit on the background task managed by Cortana,
        /// but developers should use plmdebug (https://msdn.microsoft.com/en-us/library/windows/hardware/jj680085%28v=vs.85%29.aspx)
        /// on the Cortana app package in order to prevent Cortana timing out the task during
        /// debugging.
        /// 
        /// The Cortana UI is dismissed if Cortana loses focus. 
        /// The background task is also dismissed even if being debugged. 
        /// Use of Remote Debugging is recommended in order to debug background task behaviors. 
        /// Open the project properties for the app package (not the background task project), 
        /// and enable Debug -> "Do not launch, but debug my code when it starts". 
        /// Alternatively, add a long initial progress screen, and attach to the background task process while it executes.
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

<ol start="7">
    <li>
    Declare your background task as an **AppService** in the app manifest.
    <ol type="i">
        <li>
        In **Solution Explorer**, right click the "Package.appxmanifest" file and select **View Code**. 
        </li>
        <li>
        Find the [**Application**](https://msdn.microsoft.com/library/windows/apps/dn934738) element.
        </li>
        <li>
        Add an [**Extensions**](https://msdn.microsoft.com/library/windows/apps/dn934720) element to the [**Application**](https://msdn.microsoft.com/library/windows/apps/dn934738) element.
        </li>
        <li>
        Add a [**uap:Extension**](https://msdn.microsoft.com/library/windows/apps/dn986788) element to the [**Extensions**](https://msdn.microsoft.com/library/windows/apps/dn934720) element.
        </li>
        <li>**Category** 특성을 **uap:Extension** 요소에 추가하고 **Category** 특성 값을 "windows.appService"로 설정합니다.
        </li>
        <li>
        Add an **EntryPoint** attribute to the **uap:Extension** element and set the value of the **EntryPoint** attribute to the name of the class that implements [**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794), in this case "AdventureWorks.VoiceCommands.AdventureWorksVoiceCommandService".
        </li>
        <li>
        Add a [**uap:AppService**](https://msdn.microsoft.com/library/windows/apps/dn934779) element to the **uap:Extension** element.
        </li>
        <li>
        Add a **Name** attribute to the [**uap:AppService**](https://msdn.microsoft.com/library/windows/apps/dn934779) element and set the value of the **Name** attribute to a name for the app service, in this case "AdventureWorksVoiceCommandService".
        </li>
        <li>
        Add a second [**uap:Extension**](https://msdn.microsoft.com/library/windows/apps/dn986788) element to [**Extensions**](https://msdn.microsoft.com/library/windows/apps/dn934720).
        </li>
        <li>
        Add a **Category** attribute to this [**uap:Extension**](https://msdn.microsoft.com/library/windows/apps/dn986788) element and set the value of the **Category** attribute to "windows.personalAssistantLaunch".
        </li>
    </li> 
    </ol>
    </li>    
</ol>  

다음은 Adventure Works 앱의 매니페스트입니다.
```xml
<Package>
  <Applications>
    <Application>

      <Extensions>
        <uap:Extension Category="windows.appService" 
          EntryPoint="CortanaBack1.VoiceCommands.CortanaBack1VoiceCommandService">
          <uap:AppService Name="CortanaBack1VoiceCommandService"/>
        </uap:Extension>
        <uap:Extension Category="windows.personalAssistantLaunch"/>
      </Extensions>

    <Application>
  <Applications>
</Package>
```

<ol start="8">
    <li>
    Add this app service project as a reference in the primary project. 
    <ol type="i">
        <li>
        Right click **References**. 
        </li>
        <li>
        Select **Add Reference...** 
        </li>
        <li>
        In the **Reference Manager** dialog, expand **Projects** and select the app service project. 
        </li>
        <li>
        Click OK. 
        </li>
    </ol>
    </li>
</ol>

## <span id="Create_a_VCD_file"> </span> <span id="create_a_vcd_file"> </span> <span id="CREATE_A_VCD_FILE"> </span>VCD 파일 만들기


1. Visual Studio에서 기본 프로젝트 이름을 마우스 오른쪽 단추로 클릭하고 **추가 > 새 항목**을 선택합니다. **XML 파일**을 추가합니다.
2. [
            **VCD**](https://msdn.microsoft.com/library/windows/apps/dn706593) 파일의 이름(이 예제에서는 "AdventureWorksCommands.xml")을 입력하고 추가를 클릭합니다. 
3. **솔루션 탐색기**에서 [**VCD**](https://msdn.microsoft.com/library/windows/apps/dn706593) 파일을 선택합니다.
4.  **속성** 창에서 **빌드 작업**을 **콘텐츠**로 설정한 후 **출력 디렉터리로 복사**를 **변경된 내용만 복사**로 설정합니다.

## <span id="Edit_the_VCD_file"> </span> <span id="edit_the_vcd_file"> </span> <span id="EDIT_THE_VCD_FILE"> </span>VCD 파일 편집

1. **xmlns** 특성으로 "http://schemas.microsoft.com/voicecommands/1.2"를 가리켜 **VoiceCommands** 요소를 추가합니다.

2. 앱에서 지원하는 각 언어에 대해 앱이 지원하는 음성 명령을 포함하는 [**CommandSet**](https://msdn.microsoft.com/library/windows/apps/dn722331) 요소를 만듭니다.

  각각 다른 [**xml:lang**](https://msdn.microsoft.com/library/windows/apps/dn722331) 특성이 포함된 여러 [**CommandSet**](https://msdn.microsoft.com/library/windows/apps/dn722331) 요소를 만들면 다양한 마켓에서 앱을 사용할 수 있습니다. 예를 들어 미국용 앱에는 영어에 대한 [**CommandSet**](https://msdn.microsoft.com/library/windows/apps/dn722331)와 스페인어에 대한 [**CommandSet**](https://msdn.microsoft.com/library/windows/apps/dn722331)가 포함될 수 있습니다.

  >  **주의**  
  음성 명령을 사용하여 앱을 활성화하고 동작을 시작하기 위해 앱은 사용자가 디바이스에 대해 선택한 음성 언어와 일치하는 언어의 [**CommandSet**](https://msdn.microsoft.com/library/windows/apps/dn722331)이 포함된 VCD 파일을 등록해야 합니다. 음성 언어는 **설정 > 시스템 > 음성 > 음성 언어**에 있습니다.

3. 지원하려는 각 명령에 대해 **Command** 요소를 추가합니다.

  **
            [VCD](https://msdn.microsoft.com/library/windows/apps/dn706593)** 파일에 선언된 각 **Command**에 다음 정보가 포함되어야 합니다.

  - 응용 프로그램에서 런타임에 음성 명령을 식별하는 데 사용하는 **Name** 특성 
  - 사용자가 명령을 호출할 수 있는 방법을 설명하는 구가 포함된 **Example** 요소. **Cortana**는 사용자가 "이렇게 말 하세요~", "도움말"을 말하거나 **자세히 보기**를 탭하는 경우의 이 예를 보여 줍니다.    
  -   앱이 명령으로 인식하는 단어 또는 구가 포함된 **ListenFor** 요소. 각 **ListenFor** 요소에는 명령과 관련된 특정 단어를 포함하는 하나 이상의 **PhraseList** 요소에 대한 참조가 포함될 수 있습니다.
  > **참고**  
  **ListenFor** 요소는 프로그래밍 방식으로 수정할 수 없습니다. 그러나 **ListenFor** 요소와 연결된 **PhraseList** 요소는 프로그래밍 방식으로 수정할 수 있습니다. 응용 프로그램은 사용자가 앱을 사용함에 따라 생성되는 데이터 집합을 기반으로 런타임에 **PhraseList**의 콘텐츠를 수정해야 합니다. [VCD(음성 명령 정의) 구 목록을 동적으로 수정하는 방법](dynamically-modify-voice-command-definition--vcd--phrase-lists.md)을 참조하세요.

  -   응용 프로그램이 시작되면 **Cortana**에서 표시하고 말할 텍스트가 포함된 **Feedback** 요소.

**Navigate** 요소는 음성 명령이 앱을 포그라운드로 활성화함을 나타냅니다. 이 예제에서 ```showTripToDestination``` 명령은 포그라운드 작업입니다.

**VoiceCommandService** 요소는 음성 명령이 백그라운드에서 앱을 활성화함을 나타냅니다. 이 요소의 **Target** 특성 값은 package.appxmanifest 파일에 있는 [**uap:AppService**](https://msdn.microsoft.com/library/windows/apps/dn934779) 요소의 **Name** 특성 값과 일치해야 합니다. 이 예제에서 ```whenIsTripToDestination``` 및 ```cancelTripToDestination``` 명령은 앱 서비스의 이름을 "AdventureWorksVoiceCommandService"로 지정하는 백그라운드 작업입니다.

자세한 내용은 [**VCD elements and attributes v1.2**](https://msdn.microsoft.com/library/windows/apps/dn706593) 참조를 참조하세요.

다음은 **Adventure Works** 앱에 대한 en-us 음성 명령을 정의하는 [**VCD**](https://msdn.microsoft.com/library/windows/apps/dn706593) 파일의 일부분입니다.

```xml
<?xml version="1.0" encoding="utf-8" ?>
<VoiceCommands xmlns="http://schemas.microsoft.com/voicecommands/1.2">
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

## <span id="Install_the_VCD_commands"> </span> <span id="install_the_vcd_commands"> </span> <span id="INSTALL_THE_VCD_COMMANDS"> </span>VCD 명령 설치

VCD를 설치하려면 앱을 한 번 실행해야 합니다. 

>  **참고**  
음성 명령 데이터는 앱 설치 간에 유지되지 않습니다. 앱의 음성 명령 데이터를 유지하려면 앱이 시작되거나 활성화될 때마다 VCD 파일을 초기화하거나, VCD가 현재 설치되어 있는지 여부를 나타내는 설정을 유지 관리합니다.

"app.xaml.cs" 파일에서 다음을 수행합니다.

1. 다음 using 지시문을 추가합니다.  
```csharp
using Windows.Storage;
```
2. "OnLaunched" 메서드를 비동기 한정자로 표시합니다.  
```csharp
protected async override void OnLaunched(LaunchActivatedEventArgs e)
```
3. [
            **OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335) 처리기에서 [**InstallCommandDefinitionsFromStorageFileAsync**](https://msdn.microsoft.com/library/windows/apps/dn708205)를 호출하여 시스템이 인식해야 하는 음성 명령을 등록합니다.

  Adventure Works 샘플에서 먼저 [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) 개체를 정의합니다. 

  그런 다음 [**GetFileAsync**](https://msdn.microsoft.com/library/windows/apps/br227272)를 호출하여 "AdventureWorksCommands.xml" 파일로 초기화합니다.

  그러면 이 [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) 개체가 [**InstallCommandDefinitionsFromStorageFileAsync**](https://msdn.microsoft.com/library/windows/apps/dn708205)에 전달됩니다.    
```CSharp
try
{
  // Install the main VCD. 
  StorageFile vcdStorageFile = 
    await Package.Current.InstalledLocation.GetFileAsync(
      @"AdventureWorksCommands.xml");

  await Windows.ApplicationModel.VoiceCommands.VoiceCommandDefinitionManager.
    InstallCommandDefinitionsFromStorageFileAsync(vcdStorageFile);

  // Update phrase list.
  ViewModel.ViewModelLocator locator = App.Current.Resources["ViewModelLocator"] as ViewModel.ViewModelLocator;
  if(locator != null)
  {
     await locator.TripViewModel.UpdateDestinationPhraseList();
  }
}
catch (Exception ex)
{
  System.Diagnostics.Debug.WriteLine("Installing Voice Commands Failed: " + ex.ToString());
}```

## <span id="Handle_activation_and_execute_voice_commands"></span><span id="handle_activation_and_execute_voice_commands"></span><span id="HANDLE_ACTIVATION_AND_EXECUTE_VOICE_COMMANDS"></span>Handle activation

Specify how your app responds to subsequent voice command activations (after it has been launched at least once and the voice command sets have been installed).

1.  Confirm that your app was activated by a voice command.

    Override the [**Application.OnActivated**](https://msdn.microsoft.com/library/windows/apps/br242330) event and check whether [**IActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br224727).[**Kind**](https://msdn.microsoft.com/library/windows/apps/br224728) is [**VoiceCommand**](https://msdn.microsoft.com/library/windows/apps/br224693).

2.  Determine the name of the command and what was spoken.

    Get a reference to a [**VoiceCommandActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn609755) object from the [**IActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br224727) and query the [**Result**](https://msdn.microsoft.com/library/windows/apps/dn609758) property for a [**SpeechRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/dn631432) object.

    To determine what the user said, check the value of [**Text**](https://msdn.microsoft.com/library/windows/apps/dn631441) or the semantic properties of the recognized phrase in the [**SpeechRecognitionSemanticInterpretation**](https://msdn.microsoft.com/library/windows/apps/dn631443) dictionary.

3.  Take the appropriate action in your app, such as navigating to the desired page.

For this example, we refer back to the VCD in Step 3: Edit the VCD file.

Once we get the speech-recognition result for the voice command, we get the command name from the first value in the [**RulePath**](https://msdn.microsoft.com/library/windows/apps/dn631438) array. As the VCD file defined more than one possible voice command, we need to compare the value against the command names in the VCD and take the appropriate action.

The most common action an application can take is to navigate to a page with content relevant to the context of the voice command. For this example, we navigate to a **TripPage** page and pass in the value of the voice command, how the command was input, and the recognized "destination" phrase (if applicable). Alternatively, the app could send a navigation parameter to the [**SpeechRecognitionResult**](https://msdn.microsoft.com/library/windows/apps/dn631432) when navigating to the page.

You can find out whether the voice command that launched your app was actually spoken, or whether it was typed in as text, from the [**SpeechRecognitionSemanticInterpretation.Properties**](https://msdn.microsoft.com/library/windows/apps/dn631445) dictionary using the **commandMode** key. The value of that key will be either "voice" or "text". If the value of the key is "voice", consider using speech synthesis ([**Windows.Media.SpeechSynthesis**](https://msdn.microsoft.com/library/windows/apps/dn278951)) in your app to provide the user with spoken feedback.

Use the [**SpeechRecognitionSemanticInterpretation.Properties**](https://msdn.microsoft.com/library/windows/apps/dn631445) to find out the content spoken in the **PhraseList** or **PhraseTopic** constraints of a **ListenFor** element. The dictionary key is the value of the **Label** attribute of the **PhraseList** or **PhraseTopic** element. Here, we show how to access the value of **{destination}** phrase.

``` csharp
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
protected override void OnActivated(IActivatedEventArgs args)
{
    base.OnActivated(args);

    Type navigationToPageType;
    ViewModel.TripVoiceCommand? navigationCommand = null;

    // Voice command activation.
    if (args.Kind == ActivationKind.VoiceCommand)
    {
        // Event args can represent many different activation types. 
        // Cast it so we can get the parameters we care about out.
        var commandArgs = args as VoiceCommandActivatedEventArgs;

        Windows.Media.SpeechRecognition.SpeechRecognitionResult speechRecognitionResult = commandArgs.Result;

        // Get the name of the voice command and the text spoken. 
        // See VoiceCommands.xml for supported voice commands.
        string voiceCommandName = speechRecognitionResult.RulePath[0];
        string textSpoken = speechRecognitionResult.Text;

        // commandMode indicates whether the command was entered using speech or text.
        // Apps should respect text mode by providing silent (text) feedback.
        string commandMode = this.SemanticInterpretation("commandMode", speechRecognitionResult);
        
        switch (voiceCommandName)
        {
            case "showTripToDestination":
                // Access the value of {destination} in the voice command.
                string destination = this.SemanticInterpretation("destination", speechRecognitionResult);

                // Create a navigation command object to pass to the page. 
                navigationCommand = new ViewModel.TripVoiceCommand(
                    voiceCommandName,
                    commandMode,
                    textSpoken,
                    destination);

                // Set the page to navigate to for this voice command.
                navigationToPageType = typeof(View.TripDetails);
                break;
            default:
                // If we can't determine what page to launch, go to the default entry point.
                navigationToPageType = typeof(View.TripListView);
                break;
        }
    }
    // Protocol activation occurs when a card is clicked within Cortana (using a background task).
    else if (args.Kind == ActivationKind.Protocol)
    {
        // Extract the launch context. In this case, we're just using the destination from the phrase set (passed
        // along in the background task inside Cortana), which makes no attempt to be unique. A unique id or 
        // identifier is ideal for more complex scenarios. We let the destination page check if the 
        // destination trip still exists, and navigate back to the trip list if it doesn't.
        var commandArgs = args as ProtocolActivatedEventArgs;
        Windows.Foundation.WwwFormUrlDecoder decoder = new Windows.Foundation.WwwFormUrlDecoder(commandArgs.Uri.Query);
        var destination = decoder.GetFirstValueByName("LaunchContext");

        navigationCommand = new ViewModel.TripVoiceCommand(
                                "protocolLaunch",
                                "text",
                                "destination",
                                destination);

        navigationToPageType = typeof(View.TripDetails);
    }
    else
    {
        // If we were launched via any other mechanism, fall back to the main page view.
        // Otherwise, we'll hang at a splash screen.
        navigationToPageType = typeof(View.TripListView);
    }

    // Repeat the same basic initialization as OnLaunched() above, taking into account whether
    // or not the app is already active.
    Frame rootFrame = Window.Current.Content as Frame;

    // Do not repeat app initialization when the Window already has content,
    // just ensure that the window is active.
    if (rootFrame == null)
    {
        // Create a frame to act as the navigation context and navigate to the first page.
        rootFrame = new Frame();
        App.NavigationService = new NavigationService(rootFrame);

        rootFrame.NavigationFailed += OnNavigationFailed;

        // Place the frame in the current window.
        Window.Current.Content = rootFrame;
    }

    // Since we're expecting to always show a details page, navigate even if 
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
private string SemanticInterpretation(string interpretationKey, SpeechRecognitionResult speechRecognitionResult)
{
  return speechRecognitionResult.SemanticInterpretation.Properties[interpretationKey].FirstOrDefault();
}
```

## <span id="Handle_the_voice_command_in_the_app_service"> </span> <span id="handle_the_voice_command_in_the_app_service"> </span> <span id="HANDLE_THE_VOICE_COMMAND_IN_THE_APP_SERVICE"> </span>앱 서비스의 음성 명령 처리


앱 서비스에서 음성 명령을 처리합니다.


1.  음성 명령 서비스 파일(이 예제의 경우 "AdventureWorksVoiceCommandService.cs")에 다음 using 지시문을 추가합니다.
```csharp
using Windows.ApplicationModel.VoiceCommands;
using Windows.ApplicationModel.Resources.Core;
using Windows.ApplicationModel.AppService;
```

2.  음성 명령을 처리하는 동안 앱 서비스가 종료되지 않도록 서비스 지연을 사용합니다.
3.  백그라운드 작업이 음성 명령으로 활성화된 앱 서비스로 실행되고 있는지 확인합니다.

    1.  [
            **IBackgroundTaskInstance.TriggerDetails**](https://msdn.microsoft.com/library/windows/apps/br224802)를 [**Windows.ApplicationModel.AppService.AppServiceTriggerDetails**](https://msdn.microsoft.com/library/windows/apps/dn921727)로 캐스팅합니다.
    2.  [
            **IBackgroundTaskInstance.TriggerDetails.Name**](https://msdn.microsoft.com/library/windows/apps/br224807)이 "Package.appxmanifest" 파일의 앱 서비스 이름인지 확인합니다.

4.  [
            **IBackgroundTaskInstance.TriggerDetails**](https://msdn.microsoft.com/library/windows/apps/br224802)를 사용하여 **Cortana**에 대한 [**VoiceCommandServiceConnection**](https://msdn.microsoft.com/library/windows/apps/dn974204)을 만들어 음성 명령을 검색합니다.
5.  사용자 취소로 인해 앱 서비스가 닫힐 때 알림을 받도록 [**VoiceCommandServiceConnection**](https://msdn.microsoft.com/library/windows/apps/dn974204).[**VoiceCommandCompleted**](https://msdn.microsoft.com/library/windows/apps/dn706584)에 대한 이벤트 처리기를 등록합니다.
6.  예기치 않은 오류로 인해 앱 서비스가 닫힐 때 알림을 받도록 [**IBackgroundTaskInstance.Canceled**](https://msdn.microsoft.com/library/windows/apps/br224798)에 대한 이벤트 처리기를 등록합니다.
7.  명령의 이름과 말한 내용을 확인합니다.

    1.  [
            **VoiceCommand**](https://msdn.microsoft.com/library/windows/apps/dn974162).[**CommandName**](https://msdn.microsoft.com/library/windows/apps/dn706589) 속성을 사용하여 음성 명령의 이름을 확인합니다.
    2.  사용자가 말한 내용을 확인하려면 [**Text**](https://msdn.microsoft.com/library/windows/apps/dn631441) 값 또는 [**SpeechRecognitionSemanticInterpretation**](https://msdn.microsoft.com/library/windows/apps/dn631443) 사전에서 인식되는 구의 의미 체계 속성을 확인합니다.

7.  앱 서비스에서 적절한 조치를 취합니다.
8.  **Cortana**에서 음성 명령에 대한 피드백을 표시하고 말합니다.

    1.  음성 명령에 대한 응답으로 사용자에게 **Cortana**가 표시하고 말하도록 할 문자열을 결정하고 [**VoiceCommandResponse**](https://msdn.microsoft.com/library/windows/apps/dn974182) 개체를 만듭니다. **Cortana**가 표시하고 말하는 피드백 문자열을 선택하는 방법에 대한 지침은 [Cortana 디자인 지침](https://msdn.microsoft.com/library/windows/apps/dn974233)을 참조하세요.
    2.  [
            **VoiceCommandServiceConnection**](https://msdn.microsoft.com/library/windows/apps/dn974204) 인스턴스를 사용하여 **VoiceCommandServiceConnection** 개체로 [**ReportProgressAsync**](https://msdn.microsoft.com/library/windows/apps/dn706579) 또는 [**ReportSuccessAsync**](https://msdn.microsoft.com/library/windows/apps/dn706580)를 호출하여 **Cortana**에 진행률 또는 완료를 보고합니다.

    이 예제에서는 3단계: VCD 파일 편집의 VCD를 다시 참조합니다.

```csharp
public sealed class VoiceCommandService : IBackgroundTask
    {
      private BackgroundTaskDeferral serviceDeferral;
      VoiceCommandServiceConnection voiceServiceConnection;

      public async void Run(IBackgroundTaskInstance taskInstance)
      {
      //Take a service deferral so the service isn&#39;t terminated.
        this.serviceDeferral = taskInstance.GetDeferral();

        taskInstance.Canceled += OnTaskCanceled;

        var triggerDetails = 
          taskInstance.TriggerDetails as AppServiceTriggerDetails;

        if (triggerDetails != null &amp;&amp; 
          triggerDetails.Name == "AdventureWorksVoiceServiceEndpoint")
        {
          try
          {
 voiceServiceConnection = 
   VoiceCommandServiceConnection.FromAppServiceTriggerDetails(
     triggerDetails);

 voiceServiceConnection.VoiceCommandCompleted += 
   VoiceCommandCompleted;

 VoiceCommand voiceCommand = await
 voiceServiceConnection.GetVoiceCommandAsync();

 switch (voiceCommand.CommandName)
 {
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
          finally
          {
 if (this.serviceDeferral != null)
 {
   // Complete the service deferral.
   this.serviceDeferral.Complete();
 }
          }
        }
      }

      private void VoiceCommandCompleted(
        VoiceCommandServiceConnection sender, 
        VoiceCommandCompletedEventArgs args)
      {
        if (this.serviceDeferral != null)
        {
          // Insert your code here.
          // Complete the service deferral.
          this.serviceDeferral.Complete();
        }
      }

      private async void SendCompletionMessageForDestination(
        string destination)
      {
        // Take action and determine when the next trip to destination
        // Insert code here.
        
        // Replace the hardcoded strings used here with strings 
        // appropriate for your application.

        // First, create the VoiceCommandUserMessage with the strings 
        // that Cortana will show and speak.
        var userMessage = new VoiceCommandUserMessage();
        userMessage.DisplayMessage = "Here’s your trip.";
        userMessage.SpokenMessage = "Your trip to Vegas is on August 3rd.";

        // Optionally, present visual information about the answer.
        // For this example, create a VoiceCommandContentTile with an 
        // icon and a string.
        var destinationsContentTiles = new List<VoiceCommandContentTile>();

        var destinationTile = new VoiceCommandContentTile();
        destinationTile.ContentTileType = 
          VoiceCommandContentTileType.TitleWith68x68IconAndText;
        // The user can tap on the visual content to launch the app. 
        // Pass in a launch argument to enable the app to deep link to a 
        // page relevant to the item displayed on the content tile.
        destinationTile.AppLaunchArgument = 
          string.Format("destination={0}”, “Las Vegas");
        destinationTile.Title = "Las Vegas";
        destinationTile.TextLine1 = "August 3rd 2015";
        destinationsContentTiles.Add(destinationTile);

        // Create the VoiceCommandResponse from the userMessage and list    
        // of content tiles.
        var response = 
          VoiceCommandResponse.CreateResponse(
 userMessage, destinationsContentTiles);

        // Cortana will present a “Go to app_name” link that the user 
        // can tap to launch the app. 
        // Pass in a launch to enable the app to deep link to a page 
        // relevant to the voice command.
        response.AppLaunchArgument = 
          string.Format("destination={0}”, “Las Vegas");
        
        // Ask Cortana to display the user message and content tile and 
        // also speak the user message.
        await voiceServiceConnection.ReportSuccessAsync(response);
      }

      private async void LaunchAppInForeground()
      {
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

앱 서비스는 활성화된 후 0.5초 내에 [**ReportSuccessAsync**](https://msdn.microsoft.com/library/windows/apps/dn706580)를 호출해야 합니다. **Cortana**에서는 앱에서 제공하는 데이터를 사용하여 VCD 파일에 지정된 피드백을 표시하고 말합니다. 앱이 호출하는 데 0.5초보다 오래 걸리는 경우 **Cortana**가 여기에 표시된 대로 넘기기 화면을 삽입합니다. **Cortana**는 응용 프로그램이 **ReportSuccessAsync**를 호출할 때까지 또는 최대 5초 동안 전달 화면을 표시합니다. 앱 서비스가 **ReportSuccessAsync** 또는 **Cortana**에 정보를 제공하는 [**VoiceCommandServiceConnection**](https://msdn.microsoft.com/library/windows/apps/dn974204) 메서드 중 하나를 호출하지 않으면 오류 메시지가 표시되고 앱 서비스가 취소됩니다.

![백그라운드에서 Adventure Works 앱을 사용하는 진행률 및 결과 화면이 포함된 기본 쿼리](images/cortana-backgroundapp-progress-result.png)

## <span id="related_topics"> </span>관련 문서


**개발자**
* [Cortana 조작](cortana-interactions.md)
* [사용자 지정 인식 제약 조건 정의](define-custom-recognition-constraints.md)
* [Cortana에서 백그라운드 앱 조작](interact-with-a-background-app-in-cortana.md)
* [**VCD 요소 및 특성 v1.2**](https://msdn.microsoft.com/library/windows/apps/dn706593)
* [빠른 시작: 파일 또는 이미지 리소스 사용](https://msdn.microsoft.com/library/windows/apps/xaml/hh965325)
* [한정자를 사용하여 리소스 이름을 지정하는 방법](https://msdn.microsoft.com/library/windows/apps/xaml/hh965324)

**디자이너**
* [Cortana 디자인 지침](https://msdn.microsoft.com/library/windows/apps/dn974233)
* [음성 디자인 지침](https://msdn.microsoft.com/library/windows/apps/dn596121)
* [UWP 앱용 반응형 디자인 101](https://msdn.microsoft.com/library/windows/apps/dn958435)
* [타일 및 아이콘 자산에 대한 지침](https://msdn.microsoft.com/library/windows/apps/mt412102)

**샘플**
* [Cortana 음성 명령 샘플](http://go.microsoft.com/fwlink/p/?LinkID=619899)
 

 






<!--HONumber=Mar16_HO4-->


