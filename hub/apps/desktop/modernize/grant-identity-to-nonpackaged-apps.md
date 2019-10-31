---
Description: 패키지 되지 않은 데스크톱 앱에 id를 부여 하 여 해당 앱에서 최신 Windows 10 기능을 사용할 수 있도록 하는 방법을 알아봅니다.
title: 패키지 되지 않은 데스크톱 앱에 id 부여
ms.date: 10/25/2019
ms.topic: article
keywords: windows 10, 데스크톱, 패키지, id, MSIX, Win32
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: f355bba3087f58ed20800052371804048bc0006c
ms.sourcegitcommit: d7eccdb27c22bccac65bd014e62b6572a6b44602
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/30/2019
ms.locfileid: "73145618"
---
# <a name="grant-identity-to-non-packaged-desktop-apps"></a>패키지 되지 않은 데스크톱 앱에 id 부여

<!--
> [!NOTE]
> The features described in this article require Windows 10 Insider Preview Build 10.0.19000.0 or a later release.
-->

많은 Windows 10 확장성 기능에는 백그라운드 작업, 알림, 라이브 타일 및 공유 대상을 비롯 하 여 UWP 이외의 데스크톱 앱에서 [패키지 id](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity) 를 사용 해야 합니다. 이러한 시나리오의 경우 OS는 해당 API의 호출자를 식별할 수 있도록 id가 필요 합니다.

Windows 10 Insider preview 빌드 10.0.19000.0 이전 OS 릴리스에서는 데스크톱 앱에 id를 부여 하는 유일한 방법은 [서명 된 MSIX 패키지에 패키지](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-root)하는 것입니다. 이러한 앱에 대해 id는 패키지 매니페스트에 지정 되 고 id 등록은 매니페스트의 정보에 따라 MSIX 배포 파이프라인에서 처리 됩니다. 패키지 매니페스트에서 참조 된 모든 콘텐츠는 MSIX 패키지 내에 있습니다.

Windows 10 Insider Preview 빌드 10.0.19000.0 부터는 앱과 함께 *스파스 패키지* 를 빌드하여 등록 하 여 msix 패키지에 패키지 되지 않은 데스크톱 앱에 패키지 id를 부여할 수 있습니다. 이 지원을 통해 패키지 id가 필요한 Windows 10 확장성 기능을 사용할 수 있도록 아직 배포를 위한 MSIX 패키지를 도입할 수 없는 데스크톱 앱을 사용할 수 있습니다. 자세한 배경 정보는 [이 블로그 게시물](https://blogs.windows.com/windowsdeveloper/2019/10/29/identity-registration-and-activation-of-non-packaged-win32-apps/#HBMFEM843XORqOWx.97)을 참조 하세요.

패키지 id를 데스크톱 앱에 부여 하는 스파스 패키지를 빌드하고 등록 하려면 다음 단계를 수행 합니다.

1. [스파스 패키지에 대 한 패키지 매니페스트 만들기](#create-a-package-manifest-for-the-sparse-package)
2. [스파스 패키지 빌드 및 서명](#build-and-sign-the-sparse-package)
3. [데스크톱 응용 프로그램 매니페스트에 패키지 id 메타 데이터 추가](#add-the-package-identity-metadata-to-your-desktop-application-manifest)
4. [런타임에 스파스 패키지 등록](#register-your-sparse-package-at-run-time)

## <a name="important-concepts"></a>중요 한 개념

다음 기능을 사용 하 여 패키지 id를 가져올 수 없습니다.

### <a name="sparse-packages"></a>스파스 패키지

*스파스 패키지* 는 패키지 매니페스트를 포함 하지만 다른 응용 프로그램 이진 파일 및 콘텐츠는 포함 하지 않습니다. 스파스 패키지의 매니페스트는 미리 지정 된 외부 위치에서 패키지 외부의 파일을 참조할 수 있습니다. 이를 통해 일부 Windows 10 확장성 기능에서 요구 하는 것 처럼 전체 앱에 대 한 패키지 id를 얻기 위해 아직 전체 앱에 대 한 MSIX 패키지를 도입할 수 없는 응용 프로그램도 가능 합니다

> [!NOTE]
> 스파스 패키지를 사용 하는 데스크톱 앱은 MSIX 패키지를 통해 완전 하 게 배포 될 수 있는 몇 가지 이점을 제공 하지 않습니다. 이러한 이점에는 변조 방지, 잠긴 위치에 설치, 배포 시 OS의 전체 관리, 실행 시간 및 제거 등이 포함 됩니다.

### <a name="package-external-location"></a>외부 위치 패키지

스파스 패키지를 지원 하기 위해 패키지 매니페스트 스키마는 이제 [ **\<속성\>** ](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-properties) 요소 아래에서 선택적 **\<allowexternalcontent\>** 요소를 지원 합니다. 이렇게 하면 패키지 매니페스트가 디스크의 특정 위치에서 패키지 외부의 콘텐츠를 참조할 수 있습니다.

예를 들어, C:\Program Files\MyDesktopApp\,에 앱 실행 파일 및 기타 콘텐츠를 설치 하는 기존 패키지 되지 않은 데스크톱 앱이 있는 경우 **\<AllowExternalContent** 를 포함 하는 스파스 패키지를 만들 수 있습니다\>매니페스트의 요소입니다. 앱에 대 한 설치 프로세스 또는 앱을 처음으로 실행 하는 동안 스파스 패키지를 설치 하 고 앱이 사용할 외부 위치로 C:\Program Files\MyDesktopApp\를 선언할 수 있습니다.

## <a name="create-a-package-manifest-for-the-sparse-package"></a>스파스 패키지에 대 한 패키지 매니페스트 만들기

스파스 패키지를 작성 하려면 먼저 데스크톱 앱 및 기타 필요한 세부 정보에 대 한 패키지 id 메타 데이터를 선언 하는 [패키지 매니페스트](https://docs.microsoft.com/uwp/schemas/appxpackage/appx-package-manifest) (appxmanifest.xml 라는 파일)를 만들어야 합니다. 스파스 패키지에 대 한 패키지 매니페스트를 만드는 가장 쉬운 방법은 아래 예제를 사용 하 고 [스키마 참조](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/schema-root)를 사용 하 여 앱에 대해 사용자 지정 하는 것입니다.

패키지 매니페스트에 다음 항목이 포함 되어 있는지 확인 합니다.

* 데스크톱 응용 프로그램의 id 특성을 설명 하는 [ **\<id\>** ](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity) 요소입니다.
* [ **\<속성\>** ](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-properties) 요소의 **\<allowexternalcontent\>** 요소입니다. 이 요소에는 `true`값이 할당 되어야 합니다 .이를 통해 패키지 매니페스트는 디스크의 특정 위치에서 패키지 외부의 콘텐츠를 참조할 수 있습니다. 이후 단계에서는 설치 관리자 또는 앱에서 실행 되는 코드에서 스파스 패키지를 등록할 때 외부 위치의 경로를 지정 합니다. 패키지 자체에 없는 매니페스트에서 참조 하는 모든 콘텐츠는 외부 위치에 설치 해야 합니다.
* [ **\<TargetDeviceFamily\>** ](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-targetdevicefamily) 요소의 **MinVersion** 특성을 `10.0.19000.0` 이상 버전으로 설정 해야 합니다.
* [ **\<응용 프로그램\>** ](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-application) 요소의 **TrustLevel = mediumil** 및 **runtimebehavior = Win32App** 특성은 스파스 패키지와 연결 된 데스크톱 앱이 표준 패키지 되지 않은 데스크톱과 유사 하 게 실행 되도록 선언 합니다. 레지스트리 및 파일 시스템 가상화와 기타 런타임 변경 없이 앱을 사용 합니다.

다음 예제에서는 스파스 패키지 매니페스트 (Appxmanifest.xml)의 전체 내용을 보여 줍니다. 이 매니페스트에는 패키지 id가 필요한 `windows.sharetarget` 확장이 포함 되어 있습니다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<Package 
  xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:uap2="http://schemas.microsoft.com/appx/manifest/uap/windows10/2"
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities"
  xmlns:desktop="http://schemas.microsoft.com/appx/manifest/desktop/windows10"
  xmlns:uap10="http://schemas.microsoft.com/appx/manifest/uap/windows10/10"
  IgnorableNamespaces="uap uap2 uap3 rescap desktop uap10">
  <Identity Name="ContosoPhotoStore" ProcessorArchitecture="x64" Publisher="CN=Contoso" Version="1.0.0.0" />
  <Properties>
    <DisplayName>ContosoPhotoStore</DisplayName>
    <PublisherDisplayName>Contoso</PublisherDisplayName>
    <Logo>Assets\storelogo.png</Logo>
    <uap10:AllowExternalContent>true</uap10:AllowExternalContent>
  </Properties>
  <Resources>
    <Resource Language="en-us" />
  </Resources>
  <Dependencies>
    <TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.19000.0" MaxVersionTested="10.0.19000.0" />
  </Dependencies>
  <Capabilities>
    <rescap:Capability Name="runFullTrust" />
    <rescap:Capability Name="unvirtualizedResources"/>
  </Capabilities>
  <Applications>
    <Application Id="ContosoPhotoStore" Executable="ContosoPhotoStore.exe" uap10:TrustLevel="mediumIL" uap10:RuntimeBehavior="win32App"> 
      <uap:VisualElements AppListEntry="none" DisplayName="Contoso PhotoStore" Description="Demonstrate photo app" BackgroundColor="transparent" Square150x150Logo="Assets\Square150x150Logo.png" Square44x44Logo="Assets\Square44x44Logo.png">
        <uap:DefaultTile Wide310x150Logo="Assets\Wide310x150Logo.png" Square310x310Logo="Assets\LargeTile.png" Square71x71Logo="Assets\SmallTile.png"></uap:DefaultTile>
        <uap:SplashScreen Image="Assets\SplashScreen.png" />
      </uap:VisualElements>
      <Extensions>
        <uap:Extension Category="windows.shareTarget">
          <uap:ShareTarget Description="Send to ContosoPhotoStore">
            <uap:SupportedFileTypes>
              <uap:FileType>.jpg</uap:FileType>
              <uap:FileType>.png</uap:FileType>
              <uap:FileType>.gif</uap:FileType>
            </uap:SupportedFileTypes>
            <uap:DataFormat>StorageItems</uap:DataFormat>
            <uap:DataFormat>Bitmap</uap:DataFormat>
          </uap:ShareTarget>
        </uap:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

## <a name="build-and-sign-the-sparse-package"></a>스파스 패키지 빌드 및 서명

패키지 매니페스트를 만든 후 Windows SDK에서 [Makeappx 도구](https://docs.microsoft.com/windows/msix/package/create-app-package-with-makeappx-tool) 를 사용 하 여 스파스 패키지를 빌드합니다. 스파스 패키지에는 매니페스트에서 참조 된 파일이 포함 되어 있지 않기 때문에 패키지에 대 한 의미 체계 유효성 검사를 생략 하는 `/nv` 옵션을 지정 해야 합니다.

다음 예제에서는 명령줄에서 스파스 패키지를 만드는 방법을 보여 줍니다.  

```Console
MakeAppx.exe  pack  /d  <path to directory that contains manifest>  /p  <output path>\MyPackage.msix  /nv
```

스파스 패키지를 대상 컴퓨터에 성공적으로 설치 하려면 대상 컴퓨터에서 신뢰할 수 있는 인증서로 서명 해야 합니다. 개발 목적으로 자체 서명 된 새 인증서를 만들고 Windows SDK에서 사용할 수 있는 [SignTool](https://docs.microsoft.com/windows/msix/package/sign-app-package-using-signtool)를 사용 하 여 스파스 패키지에 서명할 수 있습니다.

다음 예제에서는 명령줄에서 스파스 패키지에 서명 하는 방법을 보여 줍니다.

```Console
SignTool.exe sign /fd SHA256 /a /f <path to certificate>\MyCertificate.pfx  /p <certificate password>  <path to sparse package>\MyPackage.msix
```

### <a name="add-the-package-identity-metadata-to-your-desktop-application-manifest"></a>데스크톱 응용 프로그램 매니페스트에 패키지 id 메타 데이터 추가

또한 데스크톱 앱과 함께 [side-by-side 응용 프로그램 매니페스트](https://docs.microsoft.com/windows/win32/sbscs/application-manifests) 를 포함 하 고 앱의 id 특성을 선언 하는 특성을 사용 하 여 **\<.msix\>** 요소를 포함 해야 합니다. 이러한 특성의 값은 실행 파일이 시작 될 때 OS에서 앱의 id를 확인 하는 데 사용 됩니다.

다음 예제에서는 **\<.msix\>** 요소가 포함 된 side-by-side 응용 프로그램 매니페스트를 보여 줍니다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<assembly manifestVersion="1.0" xmlns="urn:schemas-microsoft-com:asm.v1">
  <assemblyIdentity version="1.0.0.0" name="Contoso.PhotoStoreApp"/>
  <msix xmlns="urn:schemas-microsoft-com:msix.v1"
          publisher="CN=Contoso"
          packageName="ContosoPhotoStore"
          applicationId="ContosoPhotoStore"
        />
</assembly>
```

**\<.msix\>** 요소의 특성은 스파스 패키지에 대 한 패키지 매니페스트의 이러한 값과 일치 해야 합니다.

* **PackageName** 및 **게시자** 특성은 각각 패키지 매니페스트의 [ **\<Identity\>** ](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity) 요소에 있는 **Name** 및 **publisher** 특성과 일치 해야 합니다.
* **ApplicationId** 특성은 패키지 매니페스트의 [ **\<응용 프로그램\>** ](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-application) 요소에 대 한 **Id** 특성과 일치 해야 합니다.

Side-by-side 응용 프로그램 매니페스트는 데스크톱 앱에 대 한 실행 파일과 동일한 디렉터리에 있어야 하며, 규칙에 따라 `.manifest` 확장을 추가 하 여 앱의 실행 파일과 동일한 이름을 사용 해야 합니다. 예를 들어 응용 프로그램의 실행 파일 이름이 `ContosoPhotoStore`경우 응용 프로그램 매니페스트 파일 이름을 `ContosoPhotoStore.exe.manifest`해야 합니다.

## <a name="register-your-sparse-package-at-run-time"></a>런타임에 스파스 패키지 등록

패키지 id를 데스크톱 앱에 부여 하려면 앱에서 [PackageManager](https://docs.microsoft.com/uwp/api/windows.management.deployment.packagemanager) 클래스를 사용 하 여 스파스 패키지를 등록 해야 합니다. 앱이 처음으로 실행 될 때 스파스 패키지를 등록 하는 코드를 앱에 추가 하거나 데스크톱 앱을 설치 하는 동안 코드를 실행 하 여 패키지를 등록할 수 있습니다. 예를 들어 MSI를 사용 하 여 데스크톱 앱을 설치 하는 경우입니다. 사용자 지정 작업에서이 코드를 실행할 수 있습니다.

다음 예에서는 스파스 패키지를 등록 하는 방법을 보여 줍니다. 이 코드는 패키지 매니페스트가 패키지 외부의 콘텐츠를 참조할 수 있는 외부 위치의 경로를 포함 하는 **Addpackageoptions** 개체를 만듭니다. 그런 다음이 개체를 **PackageManager AddPackageByUriAsync** 메서드에 전달 하 여 스파스 패키지를 등록 합니다. 또한이 메서드는 서명 된 스파스 패키지의 위치를 URI로 받습니다. 자세한 예제는 관련 [샘플](#sample)에서 `StartUp.cs` 코드 파일을 참조 하세요.

```csharp
private static bool registerSparsePackage(string externalLocation, string sparsePkgPath)
{
    bool registration = false;
    try
    {
        Uri externalUri = new Uri(externalLocation);
        Uri packageUri = new Uri(sparsePkgPath);

        Console.WriteLine("exe Location {0}", externalLocation);
        Console.WriteLine("msix Address {0}", sparsePkgPath);

        Console.WriteLine("  exe Uri {0}", externalUri);
        Console.WriteLine("  msix Uri {0}", packageUri);

        PackageManager packageManager = new PackageManager();

        // Declare use of an external location
        var options = new AddPackageOptions();
        options.ExternalLocationUri = externalUri;

        Windows.Foundation.IAsyncOperationWithProgress<DeploymentResult, DeploymentProgress> deploymentOperation = packageManager.AddPackageByUriAsync(packageUri, options);

        // Other progress and error-handling code omitted for brevity...
    }
}
```

## <a name="sample"></a>샘플

스파스 패키지를 사용 하 여 데스크톱 앱에 패키지 id를 부여 하는 방법을 보여 주는 완전히 작동 하는 샘플 앱은 [https://aka.ms/sparsepkgsample](https://aka.ms/sparsepkgsample)를 참조 하세요. 샘플 빌드 및 실행에 대 한 자세한 내용은 [이 블로그 게시물](https://blogs.windows.com/windowsdeveloper/2019/10/29/identity-registration-and-activation-of-non-packaged-win32-apps/#HBMFEM843XORqOWx.97)에 제공 되어 있습니다.

이 샘플에는 다음이 포함 됩니다.

* 사진 이름이 지정 된 WPF 앱에 대 한 소스 코드입니다. 시작 하는 동안 앱은 id로 실행 중인지 확인 합니다. Id를 사용 하 여 실행 되 고 있지 않으면 스파스 패키지를 등록 한 다음 앱을 다시 시작 합니다. 이러한 단계를 수행 하는 코드는 `StartUp.cs`를 참조 하세요.
* `PhotoStoreDemo.exe.manifest`라는 side-by-side 응용 프로그램 매니페스트입니다.
* 이름이 `AppxManifest.xml`패키지 매니페스트입니다.
