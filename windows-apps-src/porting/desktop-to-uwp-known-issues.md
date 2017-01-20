---
author: awkoren
Description: "이 문서에는 데스크톱-UWP 브리지의 알려진 문제가 포함되어 있습니다."
Search.Product: eADQiWindows 10XVcnh
title: "데스크톱 브리지의 알려진 문제"
translationtype: Human Translation
ms.sourcegitcommit: ec4c5f937e4fd133bfc4f7aa96d00cee03a13c26
ms.openlocfilehash: d3ed0af32c9a44078d0f772d7fc130121f5d4970

---
# <a name="known-issues-with-the-desktop-bridge"></a>데스크톱 브리지의 알려진 문제

이 문서에는 데스크톱-UWP 브리지의 알려진 문제가 포함되어 있습니다.

## <a name="blue-screen-with-error-code-0x139-kernelsecuritycheckfailure"></a>블루 스크린, 오류 코드 0x139 (KERNEL_SECURITY_CHECK_FAILURE)

Windows 스토어에서 특정 앱을 설치하거나 실행한 후 컴퓨터가 **0x139 (KERNEL\_SECURITY\_CHECK\_ FAILURE)** 오류로 인해 예기치 않게 다시 부팅될 수 있습니다.

영향을 받는 것으로 알려진 앱에는 Kodi, JT2Go, Ear Trumpet, Teslagrad 등이 포함됩니다.

2016년 10월 27일 릴리스된 [Windows 업데이트(버전 14393.351-KB3197954)](https://support.microsoft.com/kb/3197954)에는 이 문제를 해결하는 중요한 수정이 포함되어 있습니다. 이 문제가 발생하면 컴퓨터를 업데이트합니다. 로그인하기 전에 컴퓨터를 다시 시작해야 하기 때문에 PC를 업데이트할 수 없는 경우 시스템 복원을 사용하여 영향을 받는 앱 중 하나를 설치하기 전의 지점으로 시스템을 복구해야 합니다. 시스템 복원을 사용하는 방법에 대한 자세한 내용은 [Windows 10의 복구 옵션](https://support.microsoft.com/en-us/help/12415/windows-10-recovery-options)을 참조하세요. 

업데이트해도 문제가 해결되지 않거나 PC를 복구하는 방법을 잘 모를 경우 [Microsoft 지원](https://support.microsoft.com/contactus/)에 문의하세요. 

개발자인 경우에는 이 업데이트를 포함하지 않는 Windows 버전에서 데스크톱 브리지 앱 설치를 방지할 수 있습니다. 이렇게 하면 업데이트를 설치하지 않은 사용자는 앱을 사용할 수 없게 됩니다. 이 업데이트를 설치한 사용자에게 앱의 가용성을 제한하려면 AppxManifest.xml 파일을 다음과 같이 수정합니다.

```<TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.14393.351" MaxVersionTested="10.0.14393.351"/>```

Windows 업데이트에 대한 세부 정보는 다음 페이지에서 확인할 수 있습니다. 
* https://support.microsoft.com/kb/3197954
* https://support.microsoft.com/help/12387/windows-10-update-history


<!--HONumber=Dec16_HO3-->


