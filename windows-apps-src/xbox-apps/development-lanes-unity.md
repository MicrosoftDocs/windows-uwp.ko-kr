---
title: "Xbox One으로 Unity 게임 가져오기"
author: JordanEllis6809
ms.sourcegitcommit: 008ff2566b17a05b52dee0a8cd6c070d841b1f62
ms.openlocfilehash: cc854bc707a9c08687d3c6d92a704f5099d52d5b

---

# Xbox One으로 Unity 게임 가져오기

이 단계별 자습서에서는 빌드하여 배포할 준비가 된 게임이 Unity에 이미 있다고 가정합니다.

[이 자습서의 비디오 버전.](https://www.youtube.com/watch?v=f0Ptvw7k-CE)

## 0단계: Unity가 올바르게 설치되었는지 확인

Unity를 설치할 때 다음과 같은 구성 요소를 선택해야 합니다.

![Unity 설치 구성 요소](images/unity-install-components.png)

## 1단계: UWP 솔루션 빌드

Unity 게임 프로젝트에서 `File -> Build Settings...`에 있는 빌드 설정 창을 열고 아래에 표시된 Windows 스토어 옵션 메뉴로 이동합니다.

![빌드 설정 창](images/build-settings.png)

`SDK` 설정이 `Universal 10`으로 설정되어 있는지 확인합니다. 그런 다음 메뉴 아래쪽의 빌드 단추를 누르면 대상 폴더를 묻는 탐색기 창이 시작됩니다. 프로젝트의 `Assets` 디렉터리에 `UWP`라는 폴더를 만들고 이 폴더를 빌드의 대상 폴더로 선택합니다.

![빌드 대상 폴더](images/build-destination.png)

Unity에서 이제 새 Visual Studio 솔루션을 만들었습니다. 다음 단계에서 이 솔루션을 사용하여 UWP 게임을 배포합니다.

![UWP VS 솔루션](images/uwp-vs-solution.png)

## 2단계: 게임 배포

`Assets/UWP` 폴더에서 새로 생성된 솔루션을 엽니다.  솔루션이 열리면 대상 플랫폼을 X64로 변경합니다.

![x64 빌드 플랫폼](images/x64-build-platform.png)

게임에 대한 UWP Visual Studio 솔루션이 마련되었으므로 [이 단계를 수행하여](https://msdn.microsoft.com/en-us/windows/uwp/xbox-apps/getting-started) 정품 Xbox One에 게임을 성공적으로 배포할 수 있습니다.

## 개발자 참고 사항

- 버전 제어에서 UWP 폴더를 무시하는 것이 좋습니다. 프로젝트에 다른 XAML 요소를 추가하려는 경우 UWP 폴더 내의 일부 자산에 버전을 지정해야 하면 [이 작업을 수행하는 샘플](https://bitbucket.org/Unity-Technologies/windowsstoreappssamples/overview)을 참조하세요.

- Unity에서 게임의 빌드에 포함된 콘텐츠(스크립트 제외)를 변경하는 경우 다음에 배포할 때 해당 변경 사항을 적용하려면 UWP 솔루션을 다시 빌드해야 합니다. 이는 Unity의 빌드 단계 중에 프로젝트의 모든 자산이 하나의 리소스 파일로 컴파일되기 때문입니다. UWP 솔루션은 게임을 배포할 때 생성된 해당 리소스 파일을 참조합니다.




<!--HONumber=Jun16_HO5-->


