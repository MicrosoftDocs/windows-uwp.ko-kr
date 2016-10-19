---
Description: "UWP(유니버설 Windows 플랫폼) 앱으로 Windows 데스크톱 응용 프로그램(예: Win32, WPF 및 Windows Forms)을 수동으로 변환하는 방법을 알아봅니다."
Search.Product: eADQiWindows 10XVcnh
title: "UWP(유니버설 Windows 플랫폼) 앱으로 Windows 데스크톱 응용 프로그램을 수동으로 변환"
translationtype: Human Translation
ms.sourcegitcommit: 2c1a8ea38081c947f90ea835447a617c388aec08
ms.openlocfilehash: 646a5b88cb7ca97f18bf4552950979a2ceead398

---

# UWP(유니버설 Windows 플랫폼) 앱으로 Windows 데스크톱 응용 프로그램을 수동으로 변환

\[일부 정보는 상업용으로 출시되기 전에 상당 부분 수정될 수 있는 시험판 제품과 관련이 있습니다. Microsoft는 여기에 제공된 정보에 대해 명시적 또는 묵시적 보증을 하지 않습니다.\]

변환기를 사용하면 작업이 편리하고 자동화되며 설치 관리자가 수행하는 작업에 대해 잘 알지 못할 때 유용합니다. Xcopy를 사용하여 앱을 설치하거나 앱의 설치 관리자 시스템 변경에 익숙한 경우 수동으로 앱 패키지와 매니페스트를 만들도록 선택할 수 있습니다.

패키지를 수동으로 만드는 단계는 다음과 같습니다.

## 매니페스트를 직접 만듭니다.

_appxmanifest.xml_ 파일 콘텐츠가 최소로 필요 합니다. \*\*\*THIS\*\*\* 와 같은 서식이 지정된 자리 표시자를 응용 프로그램에 대한 실제 값으로 변경합니다.

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

## MakeAppX 도구 실행

[앱 패키지 작성 도구(MakeAppx.exe)](https://msdn.microsoft.com/library/windows/desktop/hh446767(v=vs.85).aspx)를 사용하여 프로젝트의 AppX 프로젝트를 생성합니다. MakeAppx.exe는 Windows 10 SDK에 함께 포함되어 있습니다. 

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

## AppX 패키지 서명

Add-appxpackage cmdlet에서는 배포 중인 응용 프로그램 패키지(.appx)가 서명되어야 합니다. .appx 패키지를 서명하려면 Microsoft Windows 10 SDK에서 제공된 [SignTool.exe](https://msdn.microsoft.com/library/windows/desktop/aa387764(v=vs.85).aspx)를 사용합니다.

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




<!--HONumber=Sep16_HO2-->


