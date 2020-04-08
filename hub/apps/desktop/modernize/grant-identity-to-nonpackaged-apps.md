---
Description: 패키지되지 않은 데스크톱 앱에서 최신 Windows 10 기능을 사용할 수 있도록 ID를 부여하는 방법을 알아봅니다.
title: 패키지되지 않은 데스크톱 앱에 ID 부여
ms.date: 02/28/2020
ms.topic: article
keywords: windows 10, 데스크톱, 패키지, id, MSIX, Win32
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: ae05a00cac19fdd349aa48160b88cde6b84e26b0
ms.sourcegitcommit: 620e4a51e2486ec2cb7190176b3d9bf3d7b5b6af
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/02/2020
ms.locfileid: "78222029"
---
# <a name="grant-identity-to-non-packaged-desktop-apps"></a>패키지되지 않은 데스크톱 앱에 ID 부여

<!--
> [!NOTE]
> The features described in this article require Windows 10 Insider Preview Build 10.0.19000.0 or a later release.
-->

많은 Windows 10 확장 기능은 백그라운드 작업, 알림, 라이브 타일 및 공유 대상을 포함한 비 UWP 데스크톱 앱의 [패키지 ID](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity)가 필요합니다. 이러한 시나리오에서 OS는 해당 API의 호출자를 식별하기 위해 ID가 필요합니다.

Windows 10 Insider Preview 빌드 10.0.19000.0 이전 OS 릴리스에서는 데스크톱 앱에 ID를 부여하는 유일한 방법은 [서명된 MSIX 패키지에 앱을 패키징](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-root)하는 것입니다. 이러한 앱의 ID는 패키지 매니페스트에 지정되고 ID 등록은 매니페스트의 정보에 따라 MSIX 배포 파이프라인에서 처리됩니다. 패키지 매니페스트에서 참조되는 모든 콘텐츠는 MSIX 패키지 안에 있습니다.

Windows 10 Insider Preview 빌드 10.0.19000.0부터는 *스파스 패키지*를 앱과 함께 빌드하고 등록하여 MSIX 패키지에 패키징되지 않은 데스크톱 앱에 패키지 ID를 부여할 수 있습니다. 이러한 지원이 제공되므로 아직 배포를 위한 MSIX 패키지를 도입할 수 없는 데스크톱 앱에서도 패키지 ID가 필요한 Windows 10 확장 기능을 사용할 수 있습니다. 자세한 배경 정보는 [이 블로그 게시물](https://blogs.windows.com/windowsdeveloper/2019/10/29/identity-registration-and-activation-of-non-packaged-win32-apps/#HBMFEM843XORqOWx.97)을 참조하세요.

데스크톱 앱에 패키지 ID를 부여하는 스파스 패키지를 빌드하고 등록하려면 다음 단계를 수행합니다.

1. [스파스 패키지의 패키지 매니페스트 만들기](#create-a-package-manifest-for-the-sparse-package)
2. [스파스 패키지 빌드 및 서명](#build-and-sign-the-sparse-package)
3. [데스크톱 애플리케이션 매니페스트에 패키지 ID 메타데이터 추가](#add-the-package-identity-metadata-to-your-desktop-application-manifest)
4. [런타임에 스파스 패키지 등록](#register-your-sparse-package-at-run-time)

## <a name="important-concepts"></a>중요 개념

다음 기능을 사용하면 패키지되지 않은 데스크톱 앱에서 패키지 ID를 얻을 수 있습니다.

### <a name="sparse-packages"></a>스파스 패키지

*스파스 패키지*에는 패키지 매니페스트가 포함되지만 다른 앱 이진 파일 및 콘텐츠는 포함되지 않습니다. 스파스 패키지의 매니페스트는 미리 지정된 외부 위치의 패키지 외부에 있는 파일을 참조할 수 있습니다. 따라서 아직 전체 앱에 MSIX 패키징을 도입할 수 없는 애플리케이션에서도 일부 Windows 10 확장 기능에서 요구하는 패키지 ID를 얻을 수 있습니다.

> [!NOTE]
> 스파스 패키지를 사용하는 데스크톱 앱은 MSIX 패키지를 통해 전체를 배포할 때 얻을 수 있는 이점을 누리지 못합니다. 이러한 이점으로는 변조 방지, 잠긴 위치에 설치, 배포/실행/제거 시 OS의 전체 관리가 있습니다.

### <a name="package-external-location"></a>패키지 외부 위치

스파스 패키지를 지원하기 위해 이제 패키지 매니페스트 스키마는 [ **\<Properties\>** ](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-properties) 요소 아래에서 선택적 요소인 **\<AllowExternalContent\>** 를 지원합니다. 따라서 패키지 매니페스트가 디스크의 특정 위치에서 패키지 외부의 콘텐츠를 참조할 수 있습니다.

예를 들어 앱 실행 파일과 기타 콘텐츠를 C:\Program Files\MyDesktopApp\,에 설치하는 기존의 패키지되지 않은 데스크톱 앱이 있는 경우 매니페스트에 **\<AllowExternalContent\>** 요소를 포함하는 스파스 패키지를 만들 수 있습니다. 앱의 설치 프로세스 중에 또는 앱을 처음으로 사용할 때 스파스 패키지를 설치하고 C:\Program Files\MyDesktopApp\을 앱에서 사용할 외부 위치로 선언할 수 있습니다.

## <a name="create-a-package-manifest-for-the-sparse-package"></a>스파스 패키지의 패키지 매니페스트 만들기

스파스 패키지를 만들려면 데스크톱 앱의 패키지 ID와 기타 필수 정보를 선언하는 [패키지 매니페스트](https://docs.microsoft.com/uwp/schemas/appxpackage/appx-package-manifest)(AppxManifest.xml이라는 파일)부터 만들어야 합니다. 스파스 패키지의 패키지 매니페스트를 만드는 가장 쉬운 방법은 아래 예제를 사용하되, [스키마 참조](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/schema-root)를 사용하여 앱에 맞게 사용자 지정하는 것입니다.

패키지 매니페스트에 다음 항목이 포함되어야 합니다.

* 데스크톱 앱의 ID 특성을 설명하는 [ **\<Identity\>** ](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity) 요소
* [ **\<Properties\>** ](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-properties) 요소 아래의 **\<AllowExternalContent\>** 요소. 이 요소에 `true` 값을 할당해야 합니다. 그러면 패키지 매니페스트가 디스크의 특정 위치에서 패키지 외부의 콘텐츠를 참조할 수 있습니다. 이후에 설치 관리자 또는 앱에서 실행되는 코드에서 스파스 패키지를 등록할 때 외부 위치의 경로를 지정할 것입니다. 매니페스트에서 참조하는 콘텐츠 중 패키지 자체에 없는 콘텐츠는 외부 위치에 설치해야 합니다.
* [ **\<TargetDeviceFamily\>** ](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-targetdevicefamily) 요소의 **MinVersion** 특성을 `10.0.19000.0` 이상 버전으로 설정해야 합니다.
* [ **\<Application\>** ](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-application) 요소의 **TrustLevel=mediumIL** 및 **RuntimeBehavior=Win32App** 특성은 스파스 패키지와 연결된 데스크톱 앱이 레지스트리 및 파일 시스템 가상화와 기타 런타임 변경 없이 패키지되지 않은 표준 데스크톱 앱과 유사하게 실행되도록 선언합니다.

다음 예제는 스파스 패키지 매니페스트(AppxManifest.xml)의 전체 콘텐츠를 보여줍니다. 이 매니페스트에는 패키지 ID가 필요한 `windows.sharetarget` 확장이 포함되어 있습니다.

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

패키지 매니페스트를 만든 후에는 Windows SDK의 [MakeAppx.exe 도구](https://docs.microsoft.com/windows/msix/package/create-app-package-with-makeappx-tool)를 사용하여 스파스 패키지를 빌드합니다. 스파스 패키지에는 매니페스트에서 참조되는 파일이 포함되지 않기 때문에 패키지의 의미 체계 유효성 검사를 건너뛰는 `/nv` 옵션을 지정해야 합니다.

다음 예제는 명령줄에서 스파스 패키지를 만드는 방법을 보여줍니다.  

```Console
MakeAppx.exe  pack  /d  <path to directory that contains manifest>  /p  <output path>\MyPackage.msix  /nv
```

스파스 패키지를 대상 컴퓨터에 성공적으로 설치하려면 대상 컴퓨터에서 신뢰할 수 있는 인증서로 스파스 패키지를 서명해야 합니다. Windows SDK에서 제공하는 [SignTool](https://docs.microsoft.com/windows/msix/package/sign-app-package-using-signtool)을 사용하여 개발용으로 자체 서명된 새 인증서를 만들어 스파스 패키지에 서명할 수 있습니다.

다음 예제는 명령줄에서 스파스 패키지에 서명하는 방법을 보여줍니다.

```Console
SignTool.exe sign /fd SHA256 /a /f <path to certificate>\MyCertificate.pfx  /p <certificate password>  <path to sparse package>\MyPackage.msix
```

### <a name="add-the-package-identity-metadata-to-your-desktop-application-manifest"></a>데스크톱 애플리케이션 매니페스트에 패키지 ID 메타데이터 추가

또한 데스크톱 앱과 함께 [병렬 애플리케이션 매니페스트](https://docs.microsoft.com/windows/win32/sbscs/application-manifests)를 포함하고, 앱의 ID 특성을 선언하는 특성과 함께 [&lt;msix&gt;](https://docs.microsoft.com/windows/win32/sbscs/application-manifests#msix) 요소를 포함해야 합니다. 이러한 특성의 값은 실행 파일이 시작될 때 OS에서 앱의 ID를 확인하는 데 사용됩니다.

다음 예제에서는 **\<msix\>** 요소가 포함된 병렬 애플리케이션 매니페스트를 보여줍니다.

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

**\<msix\>** 요소의 특성은 다음과 같이 스파스 패키지에 대한 패키지 매니페스트의 값과 일치해야 합니다.

* **packageName** 및 **publisher** 특성은 각각 패키지 매니페스트에 있는 [ **\<Identity\>** ](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity) 요소의 **Name** 및 **Publisher** 특성과 일치해야 합니다.
* **applicationId** 특성은 패키지 매니페스트에 있는 [ **\<Application\>** ](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-application)의 **Id** 특성과 일치해야 합니다.

병렬 애플리케이션 매니페스트는 데스크톱 앱의 실행 파일과 동일한 디렉터리에 있어야 하며, 규칙에 따라 앱의 실행 파일과 동일한 이름에 `.manifest` 확장명을 추가한 이름을 사용해야 합니다. 예를 들어 앱의 실행 파일 이름이 `ContosoPhotoStore`이면 애플리케이션 매니페스트 파일 이름은 `ContosoPhotoStore.exe.manifest`여야 합니다.

## <a name="register-your-sparse-package-at-run-time"></a>런타임에 스파스 패키지 등록

데스크톱 앱에 패키지 ID를 부여하려면 앱에서 [PackageManager](https://docs.microsoft.com/uwp/api/windows.management.deployment.packagemanager) 클래스를 사용하여 스파스 패키지를 등록해야 합니다. 앱에 코드를 추가하여 앱이 처음으로 실행될 때 스파스 패키지를 등록할 수도 있고, 데스크톱 앱을 설치할 때 패키지를 등록하는 코드를 실행할 수도 있습니다. 예를 들어 MSI를 사용하여 데스크톱 앱을 설치하는 경우 사용자 지정 작업에서 이 코드를 실행할 수 있습니다.

다음 예제에서는 스파스 패키지를 등록하는 방법을 보여줍니다. 이 코드는 패키지 매니페스트가 패키지 외부의 콘텐츠를 참조할 수 있는 외부 위치의 경로를 포함하는 **AddPackageOptions** 개체를 만듭니다. 그러면 코드에서 이 개체를 **PackageManager.AddPackageByUriAsync** 메서드에 전달하여 스파스 패키지를 등록합니다. 이 메서드는 서명된 스파스 패키지의 위치를 URI로 받습니다. 전체 예제는 관련 [샘플](#sample)의 `StartUp.cs` 코드 파일을 참조하세요.

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

스파스 패키지를 사용하여 데스크톱 앱에 패키지 ID를 부여하는 방법을 보여주는 완전한 기능을 갖춘 샘플 앱은 [https://github.com/microsoft/AppModelSamples/tree/master/Samples/SparsePackages](https://github.com/microsoft/AppModelSamples/tree/master/Samples/SparsePackages)를 참조하세요. 샘플을 빌드하고 실행하는 방법에 대한 자세한 내용은 [이 블로그 게시물](https://blogs.windows.com/windowsdeveloper/2019/10/29/identity-registration-and-activation-of-non-packaged-win32-apps/#HBMFEM843XORqOWx.97)에 설명되어 있습니다.

이 샘플에는 다음이 포함됩니다.

* PhotoStoreDemo라는 WPF 앱의 소스 코드. 시작하는 동안 앱에서는 ID를 사용하여 실행 중인지 확인합니다. ID를 사용하여 실행되고 있지 않으면 앱에서는 스파스 패키지를 등록하고 앱을 다시 시작합니다. 이 단계를 수행하는 코드는 `StartUp.cs`를 참조하세요.
* `PhotoStoreDemo.exe.manifest`라는 병렬 애플리케이션 매니페스트.
* `AppxManifest.xml`이라는 패키지 매니페스트.
