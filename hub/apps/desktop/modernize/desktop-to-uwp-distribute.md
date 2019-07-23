---
Description: 배포 패키지에 포함 된 데스크톱 응용 프로그램 (데스크톱 브리지)
title: Microsoft Store 또는 테스트용으로 로드 하려면 패키지에 포함 된 데스크톱 응용 프로그램을 게시할 하나 이상의 장치에 해당 합니다.
ms.date: 05/18/2018
ms.topic: article
keywords: windows 10, uwp
ms.assetid: edff3787-cecb-4054-9a2d-1fbefa79efc4
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.openlocfilehash: 2d10836da46cce4d862f7f727890b0c9c107df5a
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67317067"
---
# <a name="distribute-your-packaged-desktop-app"></a>패키지에 포함 된 데스크톱 앱 배포

하려는 경우 [MSIX 패키지에서 데스크톱 앱 패키지](/windows/msix/desktop/desktop-to-uwp-root), Microsoft Store 또는 테스트용으로 로드 패키지 응용 프로그램을 게시할 수 있습니다 하나 이상의 장치에 해당 합니다.

> [!NOTE]
> 패키지에 포함 된 응용 프로그램에 사용자를 전환 될 수 있습니다 하는 방법에 대 한 계획 했습니까? 앱을 배포하기 전에 몇 가지 아이디어를 얻고 싶다면 이 가이드의 [패키지된 앱으로 사용자 전환](#transition-users) 섹션을 참조하세요.

## <a name="distribute-your-application-by-publishing-it-to-the-microsoft-store"></a>Microsoft Store 게시 하 여 응용 프로그램을 배포 합니다.

[Microsoft Store](https://www.microsoft.com/store/apps)는 고객이 앱을 다운로드할 수 있는 편리한 방법입니다.

가장 광범위 한 대상에 연결할 Microsoft Store 응용 프로그램을 게시 합니다. 또한 조직 고객 응용 프로그램을 통해 조직에 내부적으로 배포를 얻을 수 있습니다 합니다 [비즈니스용 Microsoft Store](https://businessstore.microsoft.com/store)합니다.

Microsoft Store에 게시하려는 경우 제출 프로세서의 일환으로 몇 가지 추가 질문을 받게 됩니다. 패키지 매니페스트가 **runFullTrust**라는 제한된 접근 권한 값을 선언하기 때문이며, 해당 접근 권한 값을 사용하여 응용 프로그램을 승인해야 합니다. 자세한 내용은이 요구 사항에 대 한 합니다. [기능 제한](/windows/uwp/packaging/app-capability-declarations#restricted-capabilities)합니다.

스토어에 제출 하기 전에 응용 프로그램에 서명 필요가 없습니다.

>[!IMPORTANT]
> Microsoft Store 응용 프로그램을 게시 하려는 경우 S. Windows 10을 실행 하는 장치에 응용 프로그램이 올바르게 작동 하는지 확인 저장소 요구 사항입니다. [Windows 10 S용 Windows 앱 테스트](/windows/msix/desktop/desktop-to-uwp-test-windows-s)를 참조하세요.

<a id="side-load" />

## <a name="distribute-your-application-without-placing-it-onto-the-microsoft-store"></a>Microsoft Store 배치 하지 않고 응용 프로그램을 배포 합니다.

대신에 배포 하면 응용 프로그램 저장소를 사용 하지 않고 하나 이상의 장치에 앱을 수동으로 배포할 수 있습니다.

배포 경험에 대한 강력한 제어를 원하거나 Microsoft Store 인증 프로세스에 참여하고 싶지 않은 경우에 적합한 방법입니다.

저장소에 배치 하지 않고 다른 장치에 응용 프로그램을 배포 하려면 인증서 가져오기, 응용 프로그램을 해당 장치는 인증서 및 다음 테스트용으로 로드를 사용 하 여 응용 프로그램에 서명 해야 합니다.

[인증서를 만들거나](/windows/uwp/packaging/create-certificate-package-signing)[Verisign](https://www.verisign.com/) 같은 인기 공급자로부터 구입할 수 있습니다.

Windows 10 S를 실행 하는 장치에 응용 프로그램을 배포 하려는 경우 응용 프로그램이 해당 장치에 응용 프로그램을 배포 하기 전에 스토어 제출 프로세스를 통과 해야 하므로 Microsoft Store 통해 로그인 합니다.

인증서를 만드는 경우에는 앱을 실행하는 각 장치의 **신뢰할 수 있는 루트** 또는 **신뢰할 수 있는 사용자** 인증서 저장소에 이를 설치해야 합니다. 인기 공급자로부터 인증서를 구입하는 경우에는 다른 시스템에 앱 이외에 어떤 것도 설치할 필요가 없습니다.  

> [!IMPORTANT]
> 인증서의 게시자 이름이 앱 게시자 이름과 일치하는지 확인합니다.

인증서를 사용 하 여 응용 프로그램에 서명, 참조 [SignTool을 사용 하 여 응용 프로그램 패키지를 서명](/windows/uwp/packaging/sign-app-package-using-signtool)합니다.

다른 장치에 응용 프로그램을 사이드 로드 참조 [Windows 10에서 테스트용으로 로드 LOB 앱](/windows/application-management/sideload-apps-in-windows-10)합니다.

<a id="transition-users" />

## <a name="transition-users-to-your-packaged-app"></a>패키지된 앱으로 사용자 전환

앱을 배포하기 전에 사용자가 패키지된 앱을 사용하는 습관을 갖도록 패키지 매니페스트에 몇 가지 확장을 추가하는 것이 좋습니다. 다음과 같이 몇 가지 방법이 가능합니다.

* 기존의 시작 타일 및 작업 표시줄 단추가 패키지 앱을 가리키도록 지정합니다.
* 파일 형식 집합이 포함 된 패키지에 포함 된 응용 프로그램을 연결 합니다.
* 기본적으로 특정 형식의 파일을 열려면 응용 프로그램 패키지를 확인 합니다.

전체 확장 목록과 사용 방법에 대한 지침은 [사용자를 앱으로 전환](desktop-to-uwp-extensions.md#transition-users-to-your-app)을 참조하세요.

또한 이러한 작업을 수행 하는 패키지에 포함 된 응용 프로그램에 코드를 추가 하는 것이 좋습니다.

* 패키지 된 앱의 적절 한 폴더 위치를 데스크톱 응용 프로그램과 연결 된 사용자 데이터를 마이그레이션합니다.
* 사용자에게 데스크톱 버전의 앱을 제거하는 옵션을 제공합니다.

이러한 작업들을 하나씩 살펴보겠습니다. 사용자 데이터 마이그레이션부터 시작하겠습니다.

### <a name="migrate-user-data"></a>사용자 데이터 마이그레이션

사용자 데이터를 마이그레이션하는 코드를 추가 하려는 경우 응용 프로그램을 처음 시작 하는 경우에 해당 코드를 실행 하는 것이 좋습니다. 사용자 데이터를 마이그레이션하기 전에 대화 상자를 표시하여 사용자에게 마이그레이션 과정과 이를 권장하는 이유, 기존 데이터에 미치는 영향 등을 설명합니다.

여기에 .NET 기반의 패키지된 앱에서 이를 수행할 수 있는 방법이 예로 나와 있습니다.

```csharp
private void MigrateUserData()
{
    String sourceDir = Environment.GetFolderPath
        (Environment.SpecialFolder.ApplicationData) + "\\AppName";

    if (sourceDir != null)
    {
        DialogResult migrateResult = MessageBox.Show
            ("Would you like to migrate your data from the previous version of this app?",
             "Data Migration", MessageBoxButtons.YesNo);

        if (migrateResult.Equals(DialogResult.Yes))
        {
            String destinationDir =
                Windows.Storage.ApplicationData.Current.LocalFolder.Path + "\\AppName";

            Process process = new Process();
            process.StartInfo.FileName = "robocopy.exe";
            process.StartInfo.Arguments = "%LOCALAPPDATA%\\AppName " + destinationDir + " /move";
            process.StartInfo.CreateNoWindow = true;
            process.Start();
            process.WaitForExit();

            if (process.ExitCode > 1)
            {
                //Migration was unsuccessful -- you can choose to block/retry/other action
            }
        }
    }
}
```

### <a name="uninstall-the-desktop-version-of-your-app"></a>데스크톱 버전의 앱 제거

첫 번째을 요청 하지 않고 해당 사용 권한에 대 한 사용자가 데스크톱 응용 프로그램을 제거할 필요가 것이 좋습니다. 해당 사용 권한을 요청하는 대화 상자를 표시합니다. 사용자가 데스크톱 버전의 앱을 제거하지 않을 수 있습니다. 이 경우 데스크톱 응용 프로그램의 사용을 차단 하거나 앱을 모두 side-by-side-사용을 지원 하려고 하는지 여부를 결정 해야 합니다.

여기에 .NET 기반의 패키지된 앱에서 이를 수행할 수 있는 방법이 예로 나와 있습니다.

이 조각의 전체 컨텍스트를 보려면 이 샘플의 **MainWindow.cs**([전환/마이그레이션/제거와 WPF 사진 뷰어](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/DesktopAppTransition))를 참조하세요.

```csharp
private void RemoveDesktopApp()
{              
    //Typically, you can find your uninstall string at this location.
    String uninstallString = (String)Microsoft.Win32.Registry.GetValue
        (@"HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Microsoft\Windows\CurrentVersion" +
         @"\Uninstall\{7AD02FB8-B85E-44BC-8998-F4803BA5A0E3}\", "UninstallString", null);

    //Detect if the previous version of the Desktop application is installed.
    if (uninstallString != null)
    {
        DialogResult uninstallResult = MessageBox.Show
            ("To have the best experience, consider uninstalling the "
              + " previous version of this app. Would you like to do that now?",
              "Uninstall the previous version", MessageBoxButtons.YesNo);

        if (uninstallResult.Equals(DialogResult.Yes))
        {
                    string[] uninstallArgs = uninstallString.Split(' ');

            Process process = new Process();
            process.StartInfo.FileName = uninstallArgs[0];
            process.StartInfo.Arguments = uninstallArgs[1];
            process.StartInfo.CreateNoWindow = true;

            process.Start();
            process.WaitForExit();

            if (process.ExitCode != 0)
            {
                //Uninstallation was unsuccessful - You can choose to block the application here.
            }
        }
    }

}
```

## <a name="next-steps"></a>다음 단계

**질문에 답변**

질문이 있으세요? Stack Overflow에서 질문해 주세요. 저희 팀은 이러한 [태그](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge)를 모니터링합니다. [여기](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D)에서 Microsoft에 문의할 수도 있습니다.

Microsoft Store에 응용 프로그램을 게시할 때 문제가 발생하는 경우 이 [블로그 게시물](https://blogs.msdn.microsoft.com/appconsult/2017/09/25/preparing-a-desktop-bridge-application-for-the-store-submission/)에는 몇 가지 유용한 팁을 제공합니다.

**피드백 제공 또는 기능 제안**

[UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial)를 참조하세요.