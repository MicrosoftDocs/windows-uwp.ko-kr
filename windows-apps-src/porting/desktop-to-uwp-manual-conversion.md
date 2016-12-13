---
author: awkoren
Description: "UWP(유니버설 Windows 플랫폼) 앱으로 Windows 데스크톱 응용 프로그램(예: Win32, WPF 및 Windows Forms)을 수동으로 변환하는 방법을 알아봅니다."
Search.Product: eADQiWindows 10XVcnh
title: "UWP(유니버설 Windows 플랫폼) 앱으로 Windows 데스크톱 응용 프로그램을 수동으로 변환"
translationtype: Human Translation
ms.sourcegitcommit: ee697323af75f13c0d36914f65ba70f544d046ff
ms.openlocfilehash: f55f3bd6479cdf076c51cf574b07bfb5ce3a805c

---

# <a name="manually-convert-your-app-to-uwp-using-the-desktop-bridge"></a>데스크톱 브리지를 사용하여 수동으로 앱을 UWP로 변환

[DAC(Desktop App Converter)](desktop-to-uwp-run-desktop-app-converter.md)를 사용하면 작업이 편리하고 자동화되며 설치 관리자가 수행하는 작업에 대해 잘 알지 못할 때 유용합니다. 그러나 Xcopy를 사용하여 앱을 설치하거나 앱 설치 관리자의 시스템 변경에 대해 잘 아는 경우 수동으로 앱 패키지와 매니페스트를 만드는 것이 좋습니다. 이 문서에는 시작하는 단계가 포함되어 있습니다. 또한 DAC에서 다루지 않는, 판이 없는 자산을 앱에 추가하는 방법도 설명합니다. 

시작하는 방법은 다음과 같습니다.

## <a name="create-a-manifest-by-hand"></a>수동으로 매니페스트 만들기

_appxmanifest.xml_ 파일에 적어도 다음 내용이 포함되어야 합니다. \*\*\*THIS\*\*\*와 같은 서식이 지정된 자리 표시자를 응용 프로그램에 대한 실제 값으로 변경합니다.

```XML
    <?xml version="1.0" encoding="utf-8"?>
    <Package
       xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
       xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
       xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities">
      <Identity Name="***YOUR_PACKAGE_NAME_HERE***"
        ProcessorArchitecture="x64"
        Publisher="CN=***COMPANY_NAME***, O=***ORGANIZATION_NAME***, L=***CITY***, S=***STATE***, C=***COUNTRY***"
        Version="***YOUR_PACKAGE_VERSION_HERE***" />
      <Properties>
        <DisplayName>***YOUR_PACKAGE_DISPLAY_NAME_HERE***</DisplayName>
        <PublisherDisplayName>Reserved</PublisherDisplayName>
        <Description>No description entered</Description>
        <Logo>***YOUR_PACKAGE_RELATIVE_DISPLAY_LOGO_PATH_HERE***</Logo>
      </Properties>
      <Resources>
        <Resource Language="en-us" />
      </Resources>
      <Dependencies>
        <TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.14316.0" MaxVersionTested="10.0.14316.0" />
      </Dependencies>
      <Capabilities>
        <rescap:Capability Name="runFullTrust"/>
      </Capabilities>
      <Applications>
        <Application Id="***YOUR_PRAID_HERE***" Executable="***YOUR_PACKAGE_RELATIVE_EXE_PATH_HERE***" EntryPoint="Windows.FullTrustApplication">
          <uap:VisualElements
           BackgroundColor="#464646"
           DisplayName="***YOUR_APP_DISPLAY_NAME_HERE***"
           Square150x150Logo="***YOUR_PACKAGE_RELATIVE_PNG_PATH_HERE***"
           Square44x44Logo="***YOUR_PACKAGE_RELATIVE_PNG_PATH_HERE***"
           Description="***YOUR_APP_DESCRIPTION_HERE***" />
        </Application>
      </Applications>
    </Package>
```

## <a name="add-unplated-assets"></a>판이 없는 자산 추가

작업 표시줄에 표시되는 44x44 자산을 앱에 대해 구성하는 방법은 다음과 같습니다.

1. 올바른 44x44 이미지를 가져와 이미지가 들어 있는 폴더(즉, Assets)에 복사합니다.

2. 각 44x44 이미지의 복사본을 동일한 폴더에서 만들고 파일 이름에 *.targetsize-44_altform-unplated*를 추가합니다. 아이콘마다 각각 특정 방식으로 이름이 지정된 두 복사본이 있어야 합니다. 예를 들어 프로세스를 완료한 후 Assets 폴더에 *MYAPP_44x44.png* 및 *MYAPP_44x44.targetsize-44_altform-unplated.png*가 포함될 수 있습니다(참고: 전자는 appxmanifest의 VisualElements 특성 *Square44x44Logo* 아래에서 참조되는 아이콘임). 

3.  AppXManifest에서 수정할 각 아이콘의 BackgroundColor를 투명으로 설정합니다. 이 특성은 각 응용 프로그램에 대해 VisualElements 아래에서 찾을 수 있습니다.

4.  CMD를 열고 디렉터리를 패키지의 루트 폴더로 변경한 다음 ```makepri createconfig /cf priconfig.xml /dq en-US``` 명령을 실행하여 priconfig.xml 파일을 만듭니다.

5.  CMD를 통해 계속 패키지의 루트 폴더에서 ```makepri new /pr <PHYSICAL_PATH_TO_FOLDER> /cf <PHYSICAL_PATH_TO_FOLDER>\priconfig.xml``` 명령을 사용하여 resources.pri 파일을 만듭니다. 예를 들어 앱에 대한 명령은 ```makepri new /pr c:\MYAPP /cf c:\MYAPP\priconfig.xml```과 같을 수 있습니다. 

6.  다음 단계의 지침에 따라 AppX를 패키징하여 결과를 확인합니다.

## <a name="run-the-makeappx-tool"></a>MakeAppX 도구 실행

[앱 패키지 작성 도구(MakeAppx.exe)](https://msdn.microsoft.com/library/windows/desktop/hh446767(v=vs.85).aspx)를 사용하여 프로젝트의 AppX 프로젝트를 생성합니다. MakeAppx.exe는 Windows&nbsp;10 SDK에 함께 포함되어 있습니다. 

MakeAppx를 실행하려면 먼저 위에 설명된 대로 매니페스트 파일을 만들었는지 확인합니다. 

그 다음으로 매핑 파일을 만듭니다. 파일은 **[Files]**로 시작해야 하며, 그 다음 디스크의 각 원본 파일을 나열하고 이어서 패키지의 대상 경로가 나와야 합니다. 예를 들면 다음과 같습니다. 

```
[Files]
"C:\MyApp\StartPage.htm"     "default.html"
"C:\MyApp\readme.txt"        "doc\readme.txt"
"\\MyServer\path\icon.png"   "icon.png"
"MyCustomManifest.xml"       "AppxManifest.xml"
```

마지막으로 다음 명령을 실행합니다. 

```cmd
MakeAppx.exe pack /f mapping_filepath /p filepath.appx
```

## <a name="sign-your-appx-package"></a>AppX 패키지 서명

Add-appxpackage cmdlet에서는 배포 중인 응용 프로그램 패키지(.appx)가 서명되어야 합니다. .appx 패키지를 서명하려면 Microsoft Windows&nbsp;10 SDK에서 제공된 [SignTool.exe](https://msdn.microsoft.com/library/windows/desktop/aa387764(v=vs.85).aspx)를 사용합니다.

사용 예제: 

```cmd
C:\> MakeCert.exe -r -h 0 -n "CN=<publisher_name>" -eku 1.3.6.1.5.5.7.3.3 -pe -sv <my.pvk> <my.cer>
C:\> pvk2pfx.exe -pvk <my.pvk> -spc <my.cer> -pfx <my.pfx>
C:\> signtool.exe sign -f <my.pfx> -fd SHA256 -v .\<outputAppX>.appx
```

MakeCert.exe를 실행하고 암호를 입력하라는 메시지가 표시되면 **없음**을 선택합니다. 인증서와 서명에 대한 자세한 내용은 다음을 참조하세요. 

- [개발 중 사용할 임시 인증서를 만드는 방법](https://msdn.microsoft.com/library/ms733813.aspx)

- [SignTool](https://msdn.microsoft.com/library/windows/desktop/aa387764.aspx)

- [SignTool.exe(서명 도구)](https://msdn.microsoft.com/library/8s9b9yaz.aspx)




<!--HONumber=Dec16_HO1-->


