---
author: normesta
Description: "패키지로 만든 앱을 실행하고 로그인할 필요 없이 어떻게 표시가 되는지 확인합니다. 그런 다음, 중단점을 설정하고 코드를 단계별로 실행합니다. 프로덕션 환경에서 앱을 이미 테스트했다면 앱에 로그인한 다음 설치합니다."
Search.Product: eADQiWindows 10XVcnh
title: "패키지로 만든 데스크톱 앱(데스크톱 브리지)을 실행, 디버그, 테스트"
ms.author: normesta
ms.date: 06/20/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: f45d8b14-02d1-42e1-98df-6c03ce397fd3
ms.openlocfilehash: c160fecc530a6366de48f4f2ecc24df2463c0469
ms.sourcegitcommit: 77bbd060f9253f2b03f0b9d74954c187bceb4a30
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/11/2017
---
# <a name="run-debug-and-test-a-packaged-desktop-app-desktop-bridge"></a>패키지로 만든 데스크톱 앱(데스크톱 브리지)을 실행, 디버그, 테스트

패키지로 만든 앱을 실행하고 로그인할 필요 없이 어떻게 표시가 되는지 확인합니다. 그런 다음, 중단점을 설정하고 코드를 단계별로 실행합니다. 프로덕션 환경에서 앱을 이미 테스트했다면 앱에 로그인한 다음 설치합니다. 이 항목에서는 이러한 작업을 수행하는 방법을 보여 줍니다.

<span id="run-app" />
## <a name="run-your-app"></a>앱 실행

인증서를 얻어서 로그인을 할 필요 없이 앱을 실행하여 로컬 테스트를 수행할 수 있습니다.

Visual Studio에서 UWP 프로젝트를 사용하여 패키지를 만든 경우, 앱을 시작 하려면 패키징 프로젝트를 시작 프로젝트로 설정한 후 CTRL+F5를 눌러 앱을 시작합니다.

Desktop App Converter를 사용했거나 앱을 수동으로 패키징하는 경우에는 Windows PowerShell 명령 프롬프트를 열고 출력 폴더의 **PacakgeFiles** 하위 폴더에서 이 cmdlet를 실행합니다.

```
Add-AppxPackage –Register AppxManifest.xml
```
앱을 시작 하려면 Windows 시작 메뉴에서 앱을 찾습니다.

![시작 메뉴의 패키지 앱](images/desktop-to-uwp/converted-app-installed.png)

> [!NOTE]
> 패키지로 만든 앱은 항상 대화형 사용자로서 실행이 되며, 패키지 앱이 설치될 모든 드라이브는 NTFS 형식으로 포맷해야 합니다.

## <a name="debug-your-app"></a>앱 디버그

세션을 시작할 때마다 패키지를 선택하지 않고, 앱을 디버그하거나, 추가 기능을 설치하고 앱을 디버그할 때마다 대화 상자에서 패키지를 선택합니다.

### <a name="debug-your-app-by-selecting-the-package"></a>패키지를 선택해 앱을 디버그

이 옵션은 설치 시간을 최소화하지만, 디버그 세션을 시작하려 할 때마다 추가 단계를 수행해야 합니다.


1. 패키지 앱을 한 번 이상 시작, 로컬 컴퓨터에 설치되어 있는지 확인합니다.

   위의 [앱 실행](#run-app) 섹션을 참조하세요.

2. Visual Studio를 시작합니다.

   승격된 권한으로 앱을 디버그하려는 경우에는 **관리자 권한으로 실행** 옵션을 사용해 Visual Studio를 시작합니다.

3. Visual Studio에서 **디버그**->**기타 디버그 대상**->**설치 된 앱 패키지 디버그**를 선택합니다.

4.  **설치된 앱 패키지** 목록에서 앱 패키지를 선택한 다음 **첨부** 버튼을 선택합니다.


### <a name="debug-your-app-without-having-to-select-the-package"></a>패키지를 선택하지 않고 앱을 디버그

이 옵션은 설정 시간은 가장 길지만, 앱을 시작할 때마다 설치된 패키지를 선택할 필요가 없습니다. [Visual Studio 2017](https://www.visualstudio.com/vs/whatsnew/)을 설치해야 이 방법을 사용할 수 있습니다.

1. 먼저, [데스크톱 브리지 디버깅 프로젝트](http://go.microsoft.com/fwlink/?LinkId=797871)를 설치합니다.

2. Visual Studio를 시작하고 데스크톱 응용 프로그램 프로젝트를 엽니다.

6. 솔루션에 **데스크톱 브리지 디버깅** 프로젝트를 추가합니다.

   설치된 템플릿의 **기타 프로젝트 형식** 그룹에서 프로젝트 템플릿을 찾을 수 있습니다.

    ![alt](images/desktop-to-uwp/debug-2.png)

    **데스크톱 브리지 디버깅** 프로젝트가 솔루션에 표시됩니다.

    ![alt](images/desktop-to-uwp/debug-3.png)

7. **데스크톱 브리지 디버깅** 프로젝트의 속성 페이지를 엽니다.

8. **패키지 레이아웃** 필드를 패키지 매니페스트 파일 (AppxManifest.xml)의 위치로 설정하고 **타일을 시작** 드롭다운 목록에서 앱의 실행 파일을 선택합니다.

     ![alt](images/desktop-to-uwp/debug-4.png)

8. 코드 편집기에서 AppXPackageFileList.xml 파일을 엽니다.

9. XML 블록 주석을 제거하고 이러한 요소에 대한 값을 추가합니다.

   **MyProjectOutputPath**: 데스크톱 응용 프로그램의 폴더를 디버그할 상대 경로입니다.

   **LayoutFile**: 데스크톱 응용 프로그램의 디버그 폴더에 있는 실행 파일입니다.

   **PackagePath**: 변환하는 동안 Windows 앱 패키지 폴더에 복사된 데스크톱 응용 프로그램의 실행 파일에 대한 정규화된 파일 이름입니다.

    예를 들면 다음과 같습니다.

    ```XML
  <?xml version="1.0" encoding="utf-8"?>
  <Project ToolsVersion="14.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
    <PropertyGroup>
     <MyProjectOutputPath>..\MyDesktopApp\bin\Debug</MyProjectOutputPath>
    </PropertyGroup>
    <ItemGroup>
      <LayoutFile Include="$(MyProjectOutputPath)\MyDesktopApp.exe">
        <PackagePath>$(PackageLayout)\MyDesktopApp.exe</PackagePath>
      </LayoutFile>
    </ItemGroup>
  </Project>
    ```

  앱이 솔루션의 다른 프로젝트에서 생성된 dll 파일을 사용 중이고, 해당 dll에 포함된 코드를 한 단계씩 실행하고 싶은 경우에는 각각의 dll 파일에 **LayoutFile** 요소를 포함시킵니다.

  ```XML
  ...
      <LayoutFile Include="$(MyProjectOutputPath)\MyDesktopApp.Models.dll">
      <PackagePath>$(PackageLayout)\MyDesktopApp.Models.dll</PackagePath>
      </LayoutFile>
  ...
  ```

10. 패키징 프로젝트를 시작 프로젝트로 설정합니다.  

    ![alt](images/desktop-to-uwp/debug-5.png)

11. 데스크톱 응용 프로그램 코드에서 중단점을 설정하고 디버거를 시작합니다.

  ![디버깅 단추](images/desktop-to-uwp/debugger-button.png)

  Visual Studio는 XML 파일에 지정한 실행 파일 및 dll 파일을 Windows 앱 패키지로 복사하고 디버거를 시작합니다.

#### <a name="handle-multiple-build-configurations"></a>여러 개의 빌드 구성을 처리

여러 개의 빌드 구성 (예: 릴리스 및 디버그)을 정의한 경우에는 디버거를 시작할 때 Visual Studio에서 선택한 빌드 구성과 일치하는 파일만 복사하도록 AppXPackageFileList.xml 파일을 수정할 수 있습니다.

예를 들면 다음과 같습니다.

```XML
<PropertyGroup>
    <MyProjectOutputPath Condition="$(Configuration) == 'Debug'">..\MyDesktopApp\bin\Debug</MyProjectOutputPath>
    <MyProjectOutputPath Condition="$(Configuration) == 'Release'"> ..\MyDesktopApp\bin\Release</MyProjectOutputPath>
</PropertyGroup>
```

#### <a name="debug-uwp-enhancements-to-your-app"></a>앱에 대한 UWP 개선 사항을 디버깅

라이브 타일 같은 최신 서비스를 통해 앱을 개선하고 싶을 수 있습니다. 이렇게 하면 특정 빌드 구성에서 코드 경로를 지원하도록 조건부 컴파일을 사용할 수 있습니다.

1. 첫째, Visual Studio에서 빌드 구성을 정의하고 "DesktopUWP"와 같은 이름을 지정합니다.

2. 프로젝트의 프로젝트 속성에 있는 **빌드** 탭에서 **조건부 컴파일 기호** 필드에 해당 이름을 추가합니다.

     ![alt](images/desktop-to-uwp/debug-8.png)

3. 조건부 코드 블록을 추가합니다. 이 코드는 **DesktopUWP** 빌드 구성에 대해서만 컴파일됩니다.

    ```csharp
    [Conditional("DesktopUWP")]
    private void showtile()
    {
        XmlDocument tileXml = TileUpdateManager.GetTemplateContent(TileTemplateType.TileSquare150x150Text01);
        XmlNodeList textNodes = tileXml.GetElementsByTagName("text");
        textNodes[0].InnerText = string.Format("Welcome to DesktopUWP!");
        TileNotification tileNotification = new TileNotification(tileXml);
        TileUpdateManager.CreateTileUpdaterForApplication().Update(tileNotification);
    }
    ```

### <a name="debug-the-entire-app-lifecycle"></a>전체 앱 수명 주기 디버깅

그러나 경우에 따라 앱이 시작되기 전에 디버깅하는 기능을 포함하여 디버깅 프로세스에서 세부적으로 제어할 수 있습니다.

[PLMDebug](https://msdn.microsoft.com/library/windows/hardware/jj680085(v=vs.85).aspx)를 사용해 일시 중지, 다시 시작, 종료를 포함해 앱 수명 주기를 완전히 제어할 수 있습니다.

[PLMDebug](https://msdn.microsoft.com/library/windows/hardware/jj680085(v=vs.85).aspx)는 Windows SDK에 포함되어 있습니다.


### <a name="modify-your-app-in-between-debug-sessions"></a>디버그 세션 간에 앱을 수정

버그를 수정하여 앱의 내용을 변경할 경우, MakeAppx 도구를 사용하여 다시 패키징합니다. [MakeAppx 도구 실행](desktop-to-uwp-manual-conversion.md#make-appx)을 참조하세요.

## <a name="test-your-app"></a>앱 테스트

앱 배포를 준비할 때 현실적인 설정에서 앱을 테스트하려면 앱에 로그인한 다음 설치하는 것이 가장 좋습니다.

Visual Studio를 사용하여 앱을 패키징한 경우에는 스크립트를 실행하여 앱에 로그인한 다음 설치할 수 있습니다. [테스트용으로 앱 패키지 로드](../packaging/packaging-uwp-apps.md#sideload-your-app-package)를 참조하세요.

Desktop App Converter를 사용하여 앱을 패키징한 경우에는 ``sign``매개 변수를 사용하면 생성된 인증서를 통해 앱에 자동으로 로그인할 수 있습니다. 해당 인증서를 설치한 다음 앱을 설치해야 합니다. [패키징된 앱 실행](desktop-to-uwp-run-desktop-app-converter.md#run-app)을 참조하세요.   

또한 앱에 수동으로 로그인할 수 있습니다. 방법

1. 인증서를 만듭니다. [인증서 만들기](../packaging/create-certificate-package-signing.md)를 참조하세요.

2. 해당 인증서를 시스템의 **신뢰할 수 있는 루트** 또는 **신뢰할 수 있는 사용자** 인증서 스토어에 설치합니다.

3. 인증서를 사용해 앱에 로그인하는 방법은 [SignTool을 사용하여 앱 패키지 로그인](../packaging/sign-app-package-using-signtool.md)을 참조하세요.

  > [!IMPORTANT]
  > 인증서의 게시자 이름이 앱 게시자 이름과 일치하는지 확인합니다.

### <a name="related-sample"></a>관련 샘플

[SigningCerts](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/SigningCerts)


### <a name="test-your-app-for-windows-10-s"></a>Windows 10 S용 앱을 테스트합니다.

앱을 게시하기 전에, Windows 10 S가 실행되는 장치에서 올바르게 작동하는지 확인하세요. 앱을 Windows 스토어에 게시할 계획을 갖고 있다면, 반드시 확인을 해야 합니다. 스토어의 요구 사항이기 때문입니다. Windows 10 S를 실행하는 장치에서 올바르게 작동하지 않는 앱은 인증이 되지 않습니다. 

[Windows 10 S용 Windows 앱 테스트](https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-test-windows-s)를 참조하세요.

### <a name="run-another-process-inside-the-full-trust-container"></a>완전 신뢰 컨테이너 내 다른 프로세스 실행

지정한 앱 패키지의 컨테이너 내에서 사용자 지정 프로세스를 호출할 수 있습니다. 이는 시나리오 테스트에 유용할 수 있습니다(예: 사용자 지정 테스트 도구가 있어 앱의 출력을 테스트할 경우). 이렇게 하려면 ```Invoke-CommandInDesktopPackage``` PowerShell cmdlet을 사용합니다.

```CMD
Invoke-CommandInDesktopPackage [-PackageFamilyName] <string> [-AppId] <string> [-Command] <string> [[-Args]
    <string>]  [<CommonParameters>]
```

## <a name="next-steps"></a>다음 단계

**특정 질문에 대한 답변 찾기**

저희 팀은 이러한 [StackOverflow 태그](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge)를 모니터링합니다.

**이 문서에 대한 피드백 제공**

아래의 설명 섹션을 사용합니다.
