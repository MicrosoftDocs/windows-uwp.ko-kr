---
author: awkoren
Description: "데스크톱 변환 확장을 사용하여 Windows 데스크톱 응용 프로그램(Win32, WPF 및 Windows Forms)에서 변환된 UWP(유니버설 Windows 플랫폼) 앱을 배포하고 디버깅합니다."
Search.Product: eADQiWindows 10XVcnh
title: "Windows 데스크톱 응용 프로그램에서 변환된 UWP(유니버설 Windows 플랫폼) 앱 배포 및 디버깅"
translationtype: Human Translation
ms.sourcegitcommit: 2c1a8ea38081c947f90ea835447a617c388aec08
ms.openlocfilehash: 75e176f17845bdbd618c6ca63fbbb5765bef54fb

---

# 변환된 UWP 앱 배포 및 디버그

\[일부 정보는 상업용으로 출시되기 전에 상당 부분 수정될 수 있는 시험판 제품과 관련이 있습니다. Microsoft는 여기에 제공된 정보에 대해 명시적 또는 묵시적 보증을 하지 않습니다.\]

이 항목에는 앱을 변환한 후 성공적으로 배포하고 디버깅하는 데 도움이 되는 정보가 포함되어 있습니다. 또한 데스크톱 변환 확장의 내부에 대해 알고 싶은 경우 이 항목이 유용할 것입니다.

## 변환된 UWP 앱 디버그

변환된 앱 디버깅에 대한 몇 가지 옵션이 있습니다.

### 프로세스에 연결

Microsoft Visual Studio가 "관리자 권한"으로 실행되는 경우 __디버깅 시작__ 및 __디버깅하지 않고 시작__ 명령은 변환된 앱의 프로젝트에 작동하지만 시작된 앱은 [중간 무결성 수준](https://msdn.microsoft.com/library/bb625963)으로 실행됩니다. 즉, 상승된 권한을 갖지 _않습니다_. 시작된 앱에 대해 관리자 권한을 부여하려면 먼저 바로 가기 또는 타일을 통해 "관리자 권한으로" 시작해야 합니다. 앱이 일단 실행되면 "관리자 권한으로" 실행되는 Microsoft Visual Studio 인스턴스에서 __프로세스에 연결__을 호출하고 대화 상자에서 앱 프로세스를 선택합니다.

### F5 키를 사용한 디버그

Visual Studio는 이제 응용 프로그램의 설치 관리자에서 변환기를 실행할 때 응용 프로그램을 AppX 패키지로 빌드하면서 수행한 모든 업데이트를 자동으로 복사할 수 있는 새 패키지 프로젝트를 지원합니다. 패키징 프로젝트를 구성하면 이제 F5 키를 사용하여 F5 AppX 패키지로 직접 디버깅할 수 있습니다. 

시작하는 방법은 다음과 같습니다. 

1. 먼저 Desktop App Converter를 사용하도록 설정해야 합니다. 자세한 내용은 [Desktop App Converter Preview](https://msdn.microsoft.com/windows/uwp/porting/desktop-to-uwp-run-desktop-app-converter)를 참조하세요. 

2. Win32 응용 프로그램에 대한 변환기를 실행한 후 설치 관리자를 실행합니다. 변환기는 레이아웃과 레지스트리 변경 내용을 캡처하고, 매니페스트 및 registery.dat가 포함된 Appx를 출력하여 레지스트리를 가상화합니다.

![alt](images/desktop-to-uwp/debug-1.png)

3. [Visual Studio "15" Preview 2](https://www.visualstudio.com/downloads/visual-studio-next-downloads-vs.aspx)를 설치하고 시작합니다. 

4. [Visual Studio 갤러리](http://go.microsoft.com/fwlink/?LinkId=797871)에서 UWP 패키징 VSIX 프로젝트에 데스크톱을 설치합니다. 

5. 변환된 해당 Win32 솔루션을 Visual Studio에서 엽니다.
 
6. 솔루션을 마우스 오른쪽 단추로 클릭하고 "새 프로젝트 추가"를 선택하여 솔루션에 새 패키지 프로젝트를 추가합니다. 그런 다음 설치 및 배포에서 데스크톱에서 UWP 패키징 프로젝트를 선택합니다.

    ![alt](images/desktop-to-uwp/debug-2.png)

    결과 프로젝트가 솔루션에 추가됩니다.

    ![alt](images/desktop-to-uwp/debug-3.png)

    패키징 프로젝트에서 AppXFileList는 AppX 레이아웃로의 파일 매핑을 제공합니다. 참조는 처음에는 비어 있지만 빌드 순서 지정을 위해 수동으로 .exe 프로젝트로 설정해야 합니다. 

7. DesktopToUWPPackaging 프로젝트에는 AppX 패키지 루트와 실행할 타일을 구성할 수 있는 속성 페이지가 있습니다.

    ![alt](images/desktop-to-uwp/debug-4.png)

    PackageLayout을 변환기에 의해 만들어진 AppX의 루트 위치로 설정합니다(위 참조). 그런 다음 실행할 타일을 선택합니다.

8.  AppXFileList.xml을 열고 편집합니다. 이 파일은 Win32 디버그 빌드의 출력을 변환기에서 작성한 AppX 레이아웃으로 복사하는 방법을 정의합니다. 기본적으로 파일에는 예제 태그 및 설명이 있는 자리 표시자가 있습니다.

    ```XML
    <?xml version="1.0" encoding="utf-8"?>
    <Project ToolsVersion="4.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
      <ItemGroup>
    <!— Use the following syntax to copy debug output to the AppX layout
       <AppxPackagedFile Include="$(outdir)\App.exe">
          <PackagePath>App.exe</PackagePath>
        </AppxPackagedFile> 
        See http://etc...
    -->
      </ItemGroup>
    </Project>
    ```

    다음은 매핑을 만드는 방법의 예입니다. 이 경우 Win32 빌드 위치에 있는 .exe 및 .dll을 패키지 레이아웃 위치로 복사합니다. 

    ```XML
    <?xml version="1.0" encoding=utf-8"?>
    <Project ToolsVersion=14.0" xmlns="http://scehmas.microsoft.com/developer/msbuild/2003">
        <PropertyGroup>
            <MyProjectOutputPath>C:\{path}</MyProjectOutputPath>
        </PropertyGroup>
        <ItemGroup>
            <LayoutFile Include="$(MyProjectOutputPath)\ProjectTracker.exe">
                <PackagePath>$(PackageLayout)\VFS\Program Files (x86)\Contoso Software\Project Tracker\ProjectTracker.exe</PackagePath>
            </LayoutFile>
            <LayoutFile Include="$(MyProjectOutputPath)\ProjectTracker.Models.dll">
                <PackagePath>$(PackageLayout)\VFS\Program Files (x86)\Contoso Software\Project Tracker\ProjectTracker.Models.dll</PackagePath>
            </LayoutFile>
        </ItemGroup>
    </Project>
    ```

    파일은 다음과 같이 정의됩니다. 

    먼저 Win32 프로젝트가 빌드되는 위치를 가리키도록 *MyProjectOutputPath*를 정의합니다.

    ```XML
    <?xml version="1.0" encoding="utf-8"?>
    <Project ToolsVersion="14.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
        <PropertyGroup>
            <MyProjectOutputPath>C:\Users\peterfar\Desktop\ProjectTracker\ProjectTracker\bin\DesktopUWP</MyProjectOutputPath>
        </PropertyGroup>
    ```

    그런 다음 각 *LayoutFile*은 Win32 빌드 위치에서 Appx 패키지 레이아웃으로 복사할 파일을 지정합니다. 이 경우 먼저 .exe가 복사된 후 .dll이 복사됩니다. 

    ```XML
        <ItemGroup>
            <LayoutFile Include="$(MyProjectOutputPath)\ProjectTracker.exe">
                <PackagePath>$(PackageLayout)\VFS\Program Files (x86)\Contoso Software\Project Tracker\ProjectTracker.exe</PackagePath>
            </LayoutFile>
            <LayoutFile Include="$(MyProjectOutputPath)\ProjectTracker.Models.dll">
                <PackagePath>$(PackageLayout)\VFS\Program Files (x86)\Contoso Software\Project Tracker\ProjectTracker.Models.dll</PackagePath>
            </LayoutFile>
        </ItemGroup>
    </Project>
    ```

9. 패키징 프로젝트를 시작 프로젝트로 설정합니다. 이렇게 하면 Win32 파일이 AppX로 복사된 후 프로젝트가 빌드 및 실행되면 디버거가 시작됩니다.  

    ![alt](images/desktop-to-uwp/debug-5.png)

10. 마지막으로 이제 Win32 코드에 중단점을 설정하고 F5 키를 눌러 디버거를 실행할 수 있습니다. 이렇게 하면 Win32 응용 프로그램에서 업데이트한 내용이 AppX 패키지로 복사되고 Visual Studio 내에서 직접 디버깅할 수 있게 됩니다.

11. 응용 프로그램을 업데이트하는 경우 MakeAppX를 사용하여 앱을 다시 패키징해야 합니다. 자세한 내용은 [앱 패키지 작성 도구(MakeAppx.exe)](https://msdn.microsoft.com/library/windows/desktop/hh446767(v=vs.85).aspx)를 참조하세요. 

빌드 구성이 여러 개 있는 경우(예를 들어 릴리스용 및 디버깅용) AppXFileList.xml 파일에 다음을 추가하여 다른 위치에서 Win32 빌드를 복사할 수 있습니다.

```XML
<PropertyGroup>
    <MyProjectOutputPath Condition="$(Configuration) == 'DesktopUWP'">C:\Users\peterfar\Desktop\ProjectTracker\ProjectTracker\bin\DesktopUWP>
    </MyProjectOutputPath>
    <MyProjectOutputPath Condition="$(Configuration) == 'ReleaseDesktopUWP'"> C:\Users\peterfar\Desktop\ProjectTracker\ProjectTracker\bin\ReleaseDesktopUWP</MyProjectOutputPath>
</PropertyGroup>
```

또한 응용 프로그램을 UWP로 업데이트하지만 여전히 Win32용으로 빌드하려는 경우 조건부 컴파일을 사용하여 특정 코드 경로를 사용하도록 설정할 수도 있습니다. 

1.  아래 예제에서 코드는 DesktopUWP용으로만 컴파일되고 WinRT API를 사용하여 타일을 표시합니다. 

    ```C#
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

2.  구성 관리자를 사용하여 새 빌드 구성을 추가할 수 있습니다.

    ![alt](images/desktop-to-uwp/debug-6.png)

    ![alt](images/desktop-to-uwp/debug-7.png)

3.  그런 다음 프로젝트 속성에서 조건부 컴파일 기호에 대한 지원을 추가합니다.

    ![alt](images/desktop-to-uwp/debug-8.png)

4.  이제 추가한 UWP API를 대상으로 빌드하려는 경우 빌드 대상을 DesktopUWP로 전환할 수 있습니다.

### PLMDebug 

Visual Studio F5 및 프로세스에 연결은 앱이 실행되는 동안 디버깅하는 데 유용합니다. 그러나 경우에 따라 앱이 시작되기 전에 디버그하는 기능을 포함하여 디버깅 프로세스에서 세부적으로 제어할 수 있습니다. 이러한 고급 시나리오에서 [**PLMDebug**](https://msdn.microsoft.com/library/windows/hardware/jj680085%28v=vs.85%29.aspx?f=255&MSPPError=-2147217396)를 사용합니다. 이 도구를 통해 Windows 디버거를 사용하여 변환된 앱을 디버그하고 일시 중단, 다시 시작 및 종료를 비롯한 전체 앱 수명 주기를 제어할 수 있습니다. 

PLMDebug는 Windows SDK에 포함되어 있습니다. 자세한 내용은 [**PLMDebug**](https://msdn.microsoft.com/library/windows/hardware/jj680085%28v=vs.85%29.aspx?f=255&MSPPError=-2147217396)를 참조하세요. 

### 완전 신뢰 컨테이너 내 다른 프로세스 실행 

지정한 앱 패키지의 컨테이너 내에서 사용자 지정 프로세스를 호출할 수 있습니다. 이는 시나리오 테스트에 유용할 수 있습니다(예: 사용자 지정 테스트 도구가 있어 앱의 출력을 테스트할 경우). 이렇게 하려면 다음과 같이 ```Invoke-CommandInDesktopPackage``` PowerShell cmdlet을 사용합니다. 

```CMD
Invoke-CommandInDesktopPackage [-PackageFamilyName] <string> [-AppId] <string> [-Command] <string> [[-Args]
    <string>]  [<CommonParameters>]
```

## 변환된 UWP 앱 배포

변환된 앱을 배포하려면 두 가지 방법, 느슨한 파일 등록 및 appx 패키지 배포가 있습니다.  

느슨한 파일 등록은 파일을 디스크에서 쉽게 액세스하고 업데이트할 수 있는 위치에 배치하도록 디버깅하는 데 유용하며, 서명이나 인증서가 필요하지 않습니다.  

appx 패키지 배포는 여러 컴퓨터에서 응용 프로그램을 배포하고 테스트용으로 로드하는 편리한 방법을 제공하지만 서명된 패키지와 컴퓨터에서 신뢰할 수 있는 인증서가 필요합니다.

### 느슨한 파일 등록

개발하는 동안 앱을 배포하려면 다음 PowerShell cmdlet을 실행합니다. 

```Add-AppxPackage –Register AppxManifest.xml```

앱의 .exe 또는 .dll 파일을 업데이트하려면 패키지의 기존 파일을 새 파일로 바꾸고, AppxManifest.xml에서 버전 번호를 늘린 다음 위의 명령을 다시 실행합니다.

다음 사항에 유의하세요. 

* 변환된 앱을 설치하는 드라이브는 NTFS 형식으로 포맷되어야 합니다.

* 변환된 앱은 항상 대화형 사용자로 실행됩니다.

### appx 패키지 배포 

앱을 배포하기 전에 인증서로 서명해야 합니다. 인증서를 만드는 방법에 대한 자세한 내용은 [.Appx 패키지 서명](https://msdn.microsoft.com/windows/uwp/porting/desktop-to-uwp-run-desktop-app-converter#deploy-your-converted-appx)을 참조하세요. 

다음은 이전에 만든 인증서를 가져오는 방법입니다. CERTUTIL을 사용하여 직접 인증서를 가져오거나 서명한 appx에서 설치할 수 있으며 고객도 마찬가지입니다. 

CERTUTIL을 통해 인증서를 설치하려면 관리자 명령 프롬프트에서 다음 명령을 실행합니다.

```cmd
Certutil -addStore TrustedPeople <testcert.cer>
```

고객과 마찬가지로 appx에서 인증서를 가져오려면 다음을 수행합니다.

1.  파일 탐색기에서 테스트 인증서로 서명한 Appx를 마우스 오른쪽 단추로 클릭하고 상황에 맞는 메뉴에서 **속성**을 선택합니다.
2.  **디지털 서명** 탭을 클릭하거나 탭합니다.
3.  인증서를 클릭하거나 탭하고 **세부 정보**를 선택합니다.
4.  **인증서 보기**를 클릭하거나 탭합니다.
5.  **인증서 설치**를 클릭하거나 탭합니다.
6.  **저장소 위치** 그룹에서 **로컬 컴퓨터**를 선택합니다.
7.  **다음** 및 **확인**을 클릭하거나 탭하여 UAC 대화 상자를 확인합니다.
8.  인증서 가져오기 마법사의 다음 화면에서 선택한 옵션을 **모든 인증서를 다음 저장소에 저장**으로 변경합니다.
9.  **찾아보기**를 클릭하거나 탭합니다. 인증서 저장소 선택 창에서 아래로 스크롤한 후 **신뢰할 수 있는 사용자**를 선택하고 **확인**을 클릭하거나 탭합니다.
10. **다음**을 클릭하거나 탭합니다. 새 화면이 나타납니다. **마침**을 클릭하거나 탭합니다.
11. 확인 대화 상자가 나타납니다. 그러면 **확인**을 클릭합니다. 인증서에 문제가 있음을 나타내는 다른 대화 상자가 표시될 경우 인증서 문제를 해결해야 할 수도 있습니다.

참고: Windows에서 인증서를 신뢰할 수 있으려면 인증서가 **인증서(로컬 컴퓨터) &gt; 신뢰할 수 있는 루트 인증 기관 &gt; 인증서** 노드 또는 **인증서(로컬 컴퓨터) &gt; 신뢰할 수 있는 사용자 &gt; 인증서** 노드에 있어야 합니다. 이러한 두 위치에 있는 인증서만 로컬 컴퓨터의 컨텍스트에서 인증서 신뢰를 확인할 수 있습니다. 그렇지 않으면 다음 문자열과 유사한 오류 메시지가 표시됩니다.
```CMD
"Add-AppxPackage : Deployment failed with HRESULT: 0x800B0109, A certificate chain processed,
but terminated in a rootcertificate which is not trusted by the trust provider.
(Exception from HRESULT: 0x800B0109) error 0x800B0109: The root certificate of the signature
in the app package must be trusted."
```

이제 인증서를 신뢰할 수 있으며 두 가지 방법, 즉 설치할 appx 패키지 파일을 두 번 클릭하거나 Powershell을 통해 패키지를 설치할 수 있습니다.   Powershell을 통해 설치하려면 다음 cmdlet을 실행합니다.

```powershell
Add-AppxPackage <MyApp>.appx
```

## 백그라운드 작업

변환된 앱을 실행할 때 UWP 앱 패키지가 \Program Files\WindowsApps\\&lt;_패키지 이름_&gt;\\&lt;_앱 이름_&gt;.exe에서 시작됩니다. 여기서 보면 앱에 변환된 앱에 사용되는 특별한 xml 네임스페이스를 참조하는 앱 패키지 매니페스트(AppxManifest.xml)가 있는 것을 알 수 있습니다. 매니페스트 파일 내에는 완전 신뢰 앱을 참조하는 __&lt;EntryPoint&gt;__ 요소가 있습니다. 앱이 시작되면 앱 컨테이너 내에서 실행되지 않고 대신 평소와 같이 해당 사용자 권한으로 실행됩니다.

그러나 파일 시스템 및 레지스트리에 대한 앱의 액세스가 리디렉션되는 특수한 환경에서 앱이 실행됩니다. 레지스트리 리디렉션에는 Registry.dat 파일이 사용됩니다. 이 파일은 실제로 레지스트리 하이브이므로 Windows 레지스트리 편집기(Regedit)에서 볼 수 있습니다. 단, 이 메커니즘에서는 프로세스 간 통신에 레지스트리를 사용할 수 없습니다. 레지스트리는 이러한 경우에 맞게 디자인되지 않았으며 이러한 경우에 잘 맞지도 않습니다. 파일 시스템의 경우, 리디렉션되는 유일한 항목은 AppData 폴더이며, 모든 UWP 앱에 대한 앱 데이터가 저장된 동일한 위치로 리디렉션됩니다. 이 위치는 로컬 앱 데이터 저장소로 알려져 있으며 [ApplicationData.LocalFolder](https://msdn.microsoft.com/library/windows/apps/br241621) 속성을 사용하여 액세스합니다. 따라서 사용자가 아무런 작업을 수행하지 않아도 코드는 올바른 위치에서 앱 데이터를 읽고 쓸 수 있게 이미 이식됩니다. 또한 여기서 직접 작성할 수도 있습니다. 파일 시스템 리디렉션의 한 가지 이점은 보다 명확한 제거가 가능하다는 것입니다.

VFS라는 폴더 내부에는 앱이 종속되는 DLL이 들어 있는 폴더가 있습니다. 이러한 DLL은 앱의 클래식 데스크톱 버전에 대한 시스템 폴더에 설치됩니다. 그러나 UWP 앱의 경우 DLL은 앱에 로컬입니다. 따라서 UWP 앱이 설치 및 제거될 때 버전 관리 문제가 발생하지 않습니다.

### 패키지에 포함된 VFS 위치

다음 표에서 패키지의 일부로 제공된 파일이 앱에 대한 시스템에 오버레이된 위치를 보여 줍니다. 앱은 이러한 파일이 실제로 [Package Root]\VFS\ 내 리디렉션된 위치에 있을 경우 나열된 시스템 위치에 있다고 인식합니다. FOLDERID 위치는 [**KNOWNFOLDERID**](https://msdn.microsoft.com/en-us/library/windows/desktop/dd378457.aspx) 상수입니다.

시스템 위치 | 리디렉션된 위치([PackageRoot]\VFS\ 아래) | 아키텍처에서 유효
 :---- | :---- | :---
FOLDERID_SystemX86 | SystemX86 | x86, amd64 
FOLDERID_System | SystemX64 | amd64 
FOLDERID_ProgramFilesX86 | ProgramFilesX86 | x86, amd6 
FOLDERID_ProgramFilesX64 | ProgramFilesX64 | amd64 
FOLDERID_ProgramFilesCommonX86 | ProgramFilesCommonX86 | x86, amd64
FOLDERID_ProgramFilesCommonX64 | ProgramFilesCommonX64 | amd64 
FOLDERID_Windows | Windows | x86, amd64 
FOLDERID_ProgramData | Common AppData | x86, amd64 
FOLDERID_System\catroot | AppVSystem32Catroot | x86, amd64 
FOLDERID_System\catroot2 | AppVSystem32Catroot2 | x86, amd64 
FOLDERID_System\drivers\etc | AppVSystem32DriversEtc | x86, amd64 
FOLDERID_System\driverstore | AppVSystem32Driverstore | x86, amd64 
FOLDERID_System\logfiles | AppVSystem32Logfiles | x86, amd64 
FOLDERID_System\spool | AppVSystem32Spool | x86, amd64 

## 참고 항목
[UWP(유니버설 Windows 플랫폼) 앱으로 데스크톱 응용 프로그램 변환](https://msdn.microsoft.com/windows/uwp/porting/desktop-to-uwp-root)

[Desktop App Converter Preview](https://msdn.microsoft.com/windows/uwp/porting/desktop-to-uwp-run-desktop-app-converter)

[UWP(유니버설 Windows 플랫폼) 앱으로 Windows 데스크톱 응용 프로그램을 수동으로 변환](https://msdn.microsoft.com/windows/uwp/porting/desktop-to-uwp-manual-conversion)

[GitHub의 UWP에 대한 데스크톱 앱 브리지 코드 샘플](https://github.com/Microsoft/DesktopBridgeToUWP-Samples)


<!--HONumber=Sep16_HO2-->


