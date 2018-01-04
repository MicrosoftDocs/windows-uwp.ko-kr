---
author: laurenhughes
title: "앱 설치 관리자 파일을 사용하여 관련 집합 설치"
description: "이 섹션에서는 앱 설치 관리자를 통해 관련 집합을 설치하는 데 필요한 단계를 검토합니다. 또한 관련 집합을 정의하는 *.appinstaller 파일을 만드는 단계를 수행합니다."
ms.author: lahugh
ms.date: 10/10/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "Windows 10, uwp 앱 설치 관리자, AppInstaller, 테스트용으로 로드, 관련 집합, 선택적 패키지"
localizationpriority: medium
ms.openlocfilehash: 2601d1ec2cbd36ca2869b030cb0a185a284f67ea
ms.sourcegitcommit: 44a24b580feea0f188c7eae36e72e4a4f412802b
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/31/2017
---
# <a name="install-a-related-set-using-an-app-installer-file"></a>앱 설치 관리자 파일을 사용하여 관련 집합 설치

UWP 옵션 패키지 또는 관련 집합을 처음 시작한 경우 다음 문서를 참고하는 것이 좋습니다. 

1.  [선택적 패키지를 사용하여 응용 프로그램 확장](https://blogs.msdn.microsoft.com/appinstaller/2017/04/05/uwpoptionalpackages/)
2.  [첫 번째 선택적 패키지 빌드](https://blogs.msdn.microsoft.com/appinstaller/2017/05/09/build-your-first-optional-package/)
3.  [선택적 패키지에서 코드 로드](https://blogs.msdn.microsoft.com/appinstaller/2017/05/11/loading-code-from-an-optional-package/)
4.  [관련 집합을 만드는 도구](https://blogs.msdn.microsoft.com/appinstaller/2017/05/12/tooling-to-create-a-related-set/)
5.  [선택적 패키지 및 관련 집합 제작](https://docs.microsoft.com/windows/uwp/packaging/optional-packages)

Windows 10 Fall Creators Update를 통해 앱 설치 관리자로 관련 집합을 설치할 수 있습니다. 이렇게 하면 관련 집합 앱 패키지를 사용자에게 배포할 수 있습니다. 

## <a name="how-to-install-a-related-set"></a>관련 집합 설치 방법 
관련 집합은 하나의 엔터티가 아니라 주 패키지와 선택적 패키지의 조합입니다. 관련 집합을 하나의 엔터티로 설치할 수 있으려면 주 패키지와 선택적 패키지를 하나의 엔터티로 지정할 수 있어야 합니다. 이렇게 하려면 관련 집합을 정의하기 위해 ".appinstaller" 확장명을 가진 XML 파일을 만들어야 합니다. 앱 설치 관리자는 *.appinstaller 파일을 사용하며 사용자가 한 번의 클릭으로 정의된 모든 패키지를 설치할 수 있게 허용합니다. 

좀 더 자세하게 진행하기 전에 전체 샘플 *.appinstaller 파일을 살펴보겠습니다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<AppInstaller
    xmlns="http://schemas.microsoft.com/appx/appinstaller/2017"
    Version="1.0.0.0" 
    Uri="http://mywebservice.azurewebsites.net/appset.appinstaller" > 
   
    <MainBundle 
        Name="Contoso.MainApp"
        Publisher="CN=Contoso"
        Version="2.23.12.43"
        Uri="http://mywebservice.azurewebsites.net/mainapp.appxbundle" />
        
    <OptionalPackages>
        <Bundle
            Name="Contoso.OptionalApp1"
            Publisher="CN=Contoso"
            Version="2.23.12.43"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp1.appxbundle" />

        <Bundle
            Name="Contoso.OptionalApp2"
            Publisher="CN=Contoso"
            Version="2.23.12.43"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp2.appxbundle" />

        <Package
            Name="Fabrikam.OptionalApp3"
            Publisher="CN=Fabrikam"
            Version="10.34.54.23"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp3.appx" />

    </OptionalPackages>

</AppInstaller>
```

배포하는 동안 앱 설치 관리자 파일은 `Uri` 요소에서 참조된 앱 패키지에 대해 유효성이 검사됩니다. 따라서 `Name`, `Publisher` 및 `Version`은 앱 패키지 매니페스트의 [패키지/ID](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity) 요소와 일치해야 합니다. 

## <a name="how-to-create-an-app-installer-file"></a>앱 설치 관리자 파일을 만드는 방법 

관련 집합을 하나의 엔터티로 배포하려면 [appinstaller 스키마](https://docs.microsoft.com/uwp/schemas/appinstallerschema/app-installer-file)에 필요한 요소를 포함하는 앱 설치 관리자 파일을 만들어야 합니다.

### <a name="step-1-create-the-appinstaller-file"></a>1단계: *.appinstaller 파일 만들기
텍스트 편집기를 사용하여 XML을 포함할 파일을 만들고 이름을 &lt;filename&gt;.appinstaller로 지정합니다. 

### <a name="step-2-add-the-basic-template"></a>2단계: 기본 템플릿 추가
기본 템플릿에는 앱 설치 관리자 파일 정보가 포함됩니다. 
```xml
<?xml version="1.0" encoding="utf-8"?>
<AppInstaller
    xmlns="http://schemas.microsoft.com/appx/appinstaller/2017"
    Version="1.0.0.0" 
    Uri="http://mywebservice.azurewebsites.net/appset.appinstaller" > 
</AppInstaller>
```

### <a name="step-3-add-the-main-package-information"></a>3단계: 주 패키지 정보 추가 
주 앱 패키지가 .appxbundle 파일인 경우 아래에 표시된 `<MainBundle>`을 사용합니다. 주 앱 패키지가 .appx 파일인 경우 조각의 `<MainBundle>` 자리에 `<MainPackage>`를 사용합니다. 

```xml
<?xml version="1.0" encoding="utf-8"?>
<AppInstaller
    xmlns="http://schemas.microsoft.com/appx/appinstaller/2017"
    Version="1.0.0.0" 
    Uri="http://mywebservice.azurewebsites.net/appset.appinstaller" > 
   
    <MainBundle 
        Name="Contoso.MainApp"
        Publisher="CN=Contoso"
        Version="2.23.12.43"
        Uri="http://mywebservice.azurewebsites.net/mainapp.appxbundle" />

</AppInstaller>
```
`<MainBundle>` 또는 `<MainPackage>` 특성은 각각 앱 번들 매니페스트 또는 앱 패키지 매니페스트의 [패키지/ID](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity) 요소와 일치해야 합니다. 

### <a name="step-4-add-the-optional-packages"></a>4단계: 선택적 패키지 추가 
주 앱 패키지 특성과 마찬가지로 선택적 패키지가 앱 패키지 또는 앱 번들일 수 있는 경우 `<OptionalPackages>` 특성의 자식 요소는 각각 `<Package>` 또는 `<Bundle>`이어야 합니다. 자식 요소의 패키지 정보는 번들 또는 패키지 매니페스트의 ID 요소와 일치해야 합니다. 

``` xml
<?xml version="1.0" encoding="utf-8"?>
<AppInstaller
    xmlns="http://schemas.microsoft.com/appx/appinstaller/2017"
    Version="1.0.0.0" 
    Uri="http://mywebservice.azurewebsites.net/appset.appinstaller" > 
   
    <MainBundle 
        Name="Contoso.MainApp"
        Publisher="CN=Contoso"
        Version="2.23.12.43"
        Uri="http://mywebservice.azurewebsites.net/mainapp.appxbundle" />
        
    <OptionalPackages>
        <Bundle
            Name="Contoso.OptionalApp1"
            Publisher="CN=Contoso"
            Version="2.23.12.43"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp1.appxbundle" />

        <Bundle
            Name="Contoso.OptionalApp2"
            Publisher="CN=Contoso"
            Version="2.23.12.43"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp2.appxbundle" />

        <Package
            Name="Fabrikam.OptionalApp3"
            Publisher="CN=Fabrikam"
            Version="10.34.54.23"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp3.appx" />

    </OptionalPackages>

</AppInstaller>
```

### <a name="step-5-add-dependencies"></a>5단계: 종속성 추가 
종속성 요소에서 주 패키지 또는 선택적 패키지에 필요한 프레임워크 패키지를 지정할 수 있습니다. 

``` xml
<?xml version="1.0" encoding="utf-8"?>
<AppInstaller
    xmlns="http://schemas.microsoft.com/appx/appinstaller/2017"
    Version="1.0.0.0" 
    Uri="http://mywebservice.azurewebsites.net/appset.appinstaller" > 
   
    <MainBundle 
        Name="Contoso.MainApp"
        Publisher="CN=Contoso"
        Version="2.23.12.43"
        Uri="http://mywebservice.azurewebsites.net/mainapp.appxbundle" />
        
    <OptionalPackages>
        <Bundle
            Name="Contoso.OptionalApp1"
            Publisher="CN=Contoso"
            Version="2.23.12.43"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp1.appxbundle" />

        <Bundle
            Name="Contoso.OptionalApp2"
            Publisher="CN=Contoso"
            Version="2.23.12.43"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp2.appxbundle" />

        <Package
            Name="Fabrikam.OptionalApp3"
            Publisher="CN=Fabrikam"
            Version="10.34.54.23"
            ProcessorArchitecture="x86"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp3.appx" />

    </OptionalPackages>

    <Dependencies>
        <Package Name="Microsoft.VCLibs.140.00" Publisher="CN=Microsoft Corporation, O=Microsoft Corporation, L=Redmond, S=Washington, C=US" Version="14.0.24605.0" ProcessorArchitecture="x86" Uri="http://foobarbaz.com/fwkx86.appx" />
        <Package Name="Microsoft.VCLibs.140.00" Publisher="CN=Microsoft Corporation, O=Microsoft Corporation, L=Redmond, S=Washington, C=US" Version="14.0.24605.0" ProcessorArchitecture="x64" Uri="http://foobarbaz.com/fwkx64.appx" />
    </Dependencies>

</AppInstaller>
```

### <a name="step-6-add-update-setting"></a>6단계: 업데이트 설정 추가 
앱 설치 관리자 파일은 업데이트 설정을 지정하여 최신 앱 설치 관리자 파일이 게시될 때 관련 집합이 자동으로 업데이트되도록 할 수 있습니다. **<UpdateSettings>**는 선택 요소입니다. 
``` xml
<?xml version="1.0" encoding="utf-8"?>
<AppInstaller
    xmlns="http://schemas.microsoft.com/appx/appinstaller/2017"
    Version="1.0.0.0" 
    Uri="http://mywebservice.azurewebsites.net/appset.appinstaller" > 
   
    <MainBundle 
        Name="Contoso.MainApp"
        Publisher="CN=Contoso"
        Version="2.23.12.43"
        Uri="http://mywebservice.azurewebsites.net/mainapp.appxbundle" />
        
    <OptionalPackages>
        <Bundle
            Name="Contoso.OptionalApp1"
            Publisher="CN=Contoso"
            Version="2.23.12.43"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp1.appxbundle" />

        <Bundle
            Name="Contoso.OptionalApp2"
            Publisher="CN=Contoso"
            Version="2.23.12.43"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp2.appxbundle" />

        <Package
            Name="Fabrikam.OptionalApp3"
            Publisher="CN=Fabrikam"
            Version="10.34.54.23"
            ProcessorArchitecture="x86"
            Uri="http://mywebservice.azurewebsites.net/OptionalApp3.appx" />

    </OptionalPackages>

    <Dependencies>
        <Package Name="Microsoft.VCLibs.140.00" Publisher="CN=Microsoft Corporation, O=Microsoft Corporation, L=Redmond, S=Washington, C=US" Version="14.0.24605.0" ProcessorArchitecture="x86" Uri="http://foobarbaz.com/fwkx86.appx" />
        <Package Name="Microsoft.VCLibs.140.00" Publisher="CN=Microsoft Corporation, O=Microsoft Corporation, L=Redmond, S=Washington, C=US" Version="14.0.24605.0" ProcessorArchitecture="x64" Uri="http://foobarbaz.com/fwkx64.appx" />
    </Dependencies>
    
    <UpdateSettings>
        <OnLaunch />
    </UpdateSettings>

</AppInstaller>
```

XML 스키마에 대한 모든 세부 사항은 [앱 설치 관리자 파일 참조](https://docs.microsoft.com/uwp/schemas/appinstallerschema/app-installer-file)를 참조하세요.

> [!NOTE]
> 
> 앱 설치 관리자 파일 형식은 Windows 10 Fall Creators Update의 새로운 기능입니다. 이전 버전의 Windows 10에서 앱 설치 관리자 파일을 사용하여 UWP 앱을 배포할 수는 없습니다. 