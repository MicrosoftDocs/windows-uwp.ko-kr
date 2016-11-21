---
author: awkoren
Description: "데스크톱-UWP 브리지로 변환된 UWP 앱 배포"
Search.Product: eADQiWindows 10XVcnh
title: "데스크톱-UWP 브리지로 변환된 UWP 앱 배포"
translationtype: Human Translation
ms.sourcegitcommit: fe96945759739e9260d0cdfc501e3e59fb915b1e
ms.openlocfilehash: fbe8a77e3d5735b14088efb0ff6aa01be8d4badf

---

# 데스크톱 브리지로 변환된 앱 배포

변환된 앱을 배포하는 세 가지 기본적인 방법은 Windows 스토어, 테스트용으로 로드 및 느슨한 파일을 등록입니다.  

## Windows 스토어

Windows 스토어는 고객이 앱을 다운로드할 수 있는 가장 편리한 방법입니다. 시작하려면 [데스크톱 브리지를 사용하여 기존 앱 및 게임을 Windows 스토어로 가져오기](https://developer.microsoft.com/windows/projects/campaigns/desktop-bridge)에서 양식을 작성하세요. 그러면 온보딩 프로세스를 시작하라는 메시지가 표시됩니다. 

앱이나 게임을 Windows 스토어로 가져오려면 해당 앱이나 게임의 개발자 및/또는 게시자여야 합니다. 따라서 이름과 메일 주소가 아래 URL로 제출한 웹 사이트와 일치해야 개발자이거나 게시자인지 확인할 수 있습니다.

## 테스트용으로 로드

테스트용으로 로드하는 작업은 여러 컴퓨터에 배포하는 경우 간편한 방법을 제공합니다. 배포 환경에 대해 보다 자세히 제어하고 스토어 인증서와 연관되지 않으려는 엔터프라이즈/LOB(기간 업무) 시나리오에서 특히 유용합니다.

테스트용으로 로드된 앱을 통해 배포하기 전에 먼저 인증서를 사용하여 서명해야 합니다. 인증서를 만드는 방법에 대한 자세한 내용은 [.Appx 패키지 서명](https://msdn.microsoft.com/windows/uwp/porting/desktop-to-uwp-run-desktop-app-converter#deploy-your-converted-appx)을 참조하세요. 

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

### 느슨한 파일 등록

느슨한 파일 등록은 파일을 디스크에서 쉽게 액세스하고 업데이트할 수 있는 위치에 배치하도록 디버깅하는 데 유용하며, 서명이나 인증서가 필요하지 않습니다.  

개발하는 동안 앱을 배포하려면 다음 PowerShell cmdlet을 실행합니다. 

```Add-AppxPackage –Register AppxManifest.xml```

앱의 .exe 또는 .dll 파일을 업데이트하려면 패키지의 기존 파일을 새 파일로 바꾸고, AppxManifest.xml에서 버전 번호를 늘린 다음 위의 명령을 다시 실행합니다.

다음 사항에 유의하세요. 

* 변환된 앱을 설치하는 드라이브는 NTFS 형식으로 포맷되어야 합니다.
* 변환된 앱은 항상 대화형 사용자로 실행됩니다.


<!--HONumber=Nov16_HO1-->


