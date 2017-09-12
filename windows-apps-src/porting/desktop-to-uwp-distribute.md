---
author: normesta
Description: "패키지 데스크톱 앱(데스크톱 브리지) 배포"
Search.Product: eADQiWindows 10XVcnh
title: "Windows 스토어에 패키지 앱을 게시하거나, 이를 하나 이상의 장치에 테스트용으로 로드합니다."
ms.author: normesta
ms.date: 05/25/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: edff3787-cecb-4054-9a2d-1fbefa79efc4
ms.openlocfilehash: 24a005309271a91d669322787fb8d341e1a6d6ad
ms.sourcegitcommit: 77bbd060f9253f2b03f0b9d74954c187bceb4a30
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/11/2017
---
# <a name="distribute-a-packaged-desktop-app-desktop-bridge"></a>패키지 데스크톱 앱(데스크톱 브리지) 배포

Windows 스토어에 패키지 앱을 게시하거나, 이를 하나 이상의 장치에 테스트용으로 로드합니다.  

> [!NOTE]
> 사용자를 패키지 앱으로 전환할 수 있는 방법에 대해 계획이 있으십니까? 앱을 배포하기 전에 몇 가지 아이디어를 얻고 싶다면 이 가이드의 [데스크톱 브리지 앱으로 사용자 전환](#transition-users) 섹션을 참조하세요.

## <a name="distribute-your-app-by-publishing-it-to-the-windows-store"></a>Windows 스토어에 게시하여 앱을 배포합니다.

[Windows 스토어](https://www.microsoft.com/store/apps)는 고객이 앱을 다운로드할 수 있는 편리한 방법입니다.

<div style="float: left; padding: 10px">
    ![스토어 아이콘](images/desktop-to-uwp/store.png)
</div>
가장 광범위한 사용자에 도달하려면 스토어에 앱을 게시합니다. 또한 기업 고객은은 [비즈니스용 Windows 스토어](https://www.microsoft.com/business-store)를 통해 회사 내부에 배포할 앱을 구입할 수 있습니다.

Windows 스토어에 게시할 계획이지만 아직 우리에게 연락을 하지 않으신 경우에는 [이 양식](https://developer.microsoft.com/windows/projects/campaigns/desktop-bridge)을 작성하면 온보딩 프로세스를 시작하라는 메시지가 표시됩니다.

스토어에 앱을 제출하기 전에 로그인할 필요가 없습니다.

>[!IMPORTANT]
> 앱을 Windows 스토어에 게시할 계획이 있다면, Windows 10 S가 실행되는 장치에서 올바르게 작동하는지 확인하세요. 이는 스토어의 요구 사항입니다. [Windows 10 S용 Windows 앱 테스트](desktop-to-uwp-test-windows-s.md)를 참조하세요.

## <a name="distribute-your-app-without-placing-it-onto-the-windows-store"></a>Windows 스토어에 배치하지 않고 앱을 배포

스토어를 사용하지 않고 앱을 배포하는 경우에는 수동으로 하나 이상의 장치에 앱을 배포할 수 있습니다.

배포 경험에 대한 강력한 제어를 원하거나 Windows 스토어 인증 프로세스에 참여하지 않고 싶은 경우에 적합한 방법입니다.

스토어에 배치하지 않고 다른 장치에 앱을 배포하려면 인증서를 얻고 이를 통해 앱에 로그인한 다음, 앱을 이들 장치에 테스트용으로 로드해야 합니다.

[인증서를 만들거나](../packaging/create-certificate-package-signing.md) [Verisign](https://www.verisign.com/) 같은 인기 공급자로부터 구입할 수 있습니다.

Windows 10 S를 실행하는 장치에 앱을 배포하려는 경우에는 Windows 스토어에서 앱에 로그인해야 합니다. 스토어 제출 프로세스를 거쳐야만 장치에 앱을 배포할 수 있기 때문입니다.

인증서를 만드는 경우에는 앱을 실행하는 각 장치의 **신뢰할 수 있는 루트** 또는 **신뢰할 수 있는 사용자** 인증서 스토어에 이를 설치해야 합니다. 인기 공급자로부터 인증서를 구입하는 경우에는 다른 시스템에 앱 이외에 어떤 것도 설치할 필요가 없습니다.  

> [!IMPORTANT]
> 인증서의 게시자 이름이 앱 게시자 이름과 일치하는지 확인합니다.

인증서를 사용해 앱에 로그인하는 방법은 [SignTool을 사용하여 앱 패키지 로그인](../packaging/sign-app-package-using-signtool.md)을 참조하세요.

앱을 다른 장치에 테스트용으로 로드하는 방법은 [Windows 10에서 LOB 앱을 테스트용으로 로드](https://technet.microsoft.com/itpro/windows/deploy/sideload-apps-in-windows-10)를 참조하세요.

**비디오**

|Windows 스토어에 앱을 게시합니다. |엔터프라이즈 앱을 배포합니다.  |
|---|---|
|<iframe src="https://mva.microsoft.com/en-US/training-courses-embed/developers-guide-to-the-desktop-bridge-17373/Demo-Windows-Store-Publication-3cWyG5WhD_5506218965"      width="426" height="472" allowFullScreen frameBorder="0"></iframe>|<iframe src="https://mva.microsoft.com/en-US/training-courses-embed/developers-guide-to-the-desktop-bridge-17373/Video-Distribution-for-Enterprise-Apps-XJ5Hd5WhD_1106218965" width="426" height="472" allowFullScreen frameBorder="0"></iframe>|

<span id="transition-users" />
## <a name="transition-users-to-your-desktop-bridge-app"></a>데스크톱 브리지 앱으로 사용자를 전환

앱을 배포하기 전에 사용자가 데스크톱 브리지 앱을 사용하는 습관을 갖도록 패키지 매니페스트에 몇 가지 확장을 추가하는 것이 좋습니다. 다음과 같이 몇 가지 방법이 가능합니다.

* 기존의 시작 타일 및 작업 표시줄 단추가 데스크톱 브리지 앱을 가리키도록 합니다.
* 파일 형식 별로 패키징된 앱을 연결합니다.
* 데스크톱 브리지 앱이 특정 파일 형식을 열도록 기본 설정합니다.

전체 확장 목록과 사용 방법에 대한 지침은 [사용자를 앱으로 전환](desktop-to-uwp-extensions.md#transition-users-to-your-app)을 참조하세요.

또한 이러한 작업을 수행하는 데스크톱 브리지 앱에 코드를 추가하는 것이 좋습니다.

* 데스크톱 앱와 관련된 사용자 데이터를 데스크톱 브리지 앱의 해당 폴더 위치로 마이그레이션합니다.
* 사용자에게 데스크톱 버전의 앱을 제거하는 옵션을 제공합니다.

이러한 작업들을 하나씩 살펴보겠습니다. 사용자 데이터 마이그레이션부터 시작하겠습니다.

### <a name="migrate-user-data"></a>사용자 데이터 마이그레이션

사용자 데이터를 마이그레이션하는 코드를 추가하려는 경우에는 앱이 처음 시작될 때만 해당 코드를 실행하는 것이 좋습니다. 사용자 데이터를 마이그레이션하기 전에 대화 상자를 표시하여 사용자에게 마이그레이션 과정과 이를 권장하는 이유, 기존 데이터에 미치는 영향 등을 설명합니다.

여기에 .NET 기반의 데스크톱 브리지 앱에서 이를 수행할 수 있는 방법이 예로 나와 있습니다.

```csharp
private void MigrateUserData()
{
    String sourceDir = Environment.GetFolderPath
        (Environment.SpecialFolder.ApplicationData) + "\\AppName";

    if (sourceDir != null)
    {
        String migrateMessage =
            "Would you like to migrate your data from the previous version of this app?";

        DialogResult migrateResult = MessageBox.Show
            (migrateMessage, "Data Migration", MessageBoxButtons.YesNo);

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

권한을 먼저 요청하지 않은 상태에서는 사용자 데스크톱 앱을 제거하지 않도록 합니다. 해당 사용 권한을 요청하는 대화 상자를 표시합니다. 사용자가 데스크톱 버전의 앱을 제거하지 않을 수 있습니다. 이 경우에는 데스크톱 앱의 사용을 차단하고 싶은지, 아니면 두 앱을 동시에 사용하도록 지원하고 싶은지 판단해야 합니다.

여기에 .NET 기반의 데스크톱 브리지 앱에서 이를 수행할 수 있는 방법이 예로 나와 있습니다.

이 조각의 전체 컨텍스트를 보려면 이 샘플의 **MainWindow.cs**([전환/마이그레이션/제거와 WPF 사진 뷰어](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/DesktopAppTransition))를 참조하세요.

```csharp
private void RemoveDesktopApp()
{              
    //Typically, you can find your uninstall string at this location.
    String uninstallString = (String)Microsoft.Win32.Registry.GetValue
        (@"HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Microsoft\Windows\CurrentVersion" +
         @"\Uninstall\{7AD02FB8-B85E-44BC-8998-F4803BA5A0E3}\", "UninstallString", null);

    //Detect if the previous version of the Desktop App is installed.
    if (uninstallString != null)
    {
        String uninstallMessage = "To have the best experience, consider uninstalling the "
            +" previous version of this app. Would you like to do that now?";

        DialogResult uninstallResult = MessageBox.Show
            (uninstallMessage, "Uninstall the previous version", MessageBoxButtons.YesNo);

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
                //Uninstallation was unsuccessful - You can choose to block the app here.
            }
        }
    }

}
```

### <a name="video"></a>비디오

<iframe src="https://mva.microsoft.com/en-US/training-courses-embed/developers-guide-to-the-desktop-bridge-17373/Demo-Transition-Taskbar-Pins-Start-Tiles-File-Type-Associations-and-Protocol-Handlers-MD5mv5WhD_2406218965" width="636" height="480" allowFullScreen frameBorder="0"></iframe>

## <a name="next-steps"></a>다음 단계

**특정 질문에 대한 답변 찾기**

저희 팀은 이러한 [StackOverflow 태그](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge)를 모니터링합니다.

**이 문서에 대한 피드백 제공**

아래의 설명 섹션을 사용합니다.
