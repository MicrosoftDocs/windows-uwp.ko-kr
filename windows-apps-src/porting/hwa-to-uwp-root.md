---
title: "호스트된 웹앱 - UWP(유니버설 Windows 플랫폼) 앱으로 웹 응용 프로그램 변환 및 기본 Windows 10 기능 액세스"
description: "웹 사이트 URL에서 UWP(유니버설 Windows 플랫폼) 앱을 만듭니다. 웹앱 내 코드에서 Windows 10 기본 디바이스 기능에 액세스합니다. 호스트된 웹앱용 Microsoft Windows 브리지(이전의 Project Westminster)를 사용하면 Windows 스토어에 웹앱을 빠르고 쉽게 포함할 수 있습니다."
author: seksenov
translationtype: Human Translation
ms.sourcegitcommit: 765789ef83f9b6a845ab79505b1b9ecbfd987126
ms.openlocfilehash: 491665558f713dcbaae7ea20739ed72c61a12cd2

---

# 호스트된 웹앱 - 웹앱에서 Windows 10 기능 액세스

웹 응용 프로그램은 서버에 호스트된 스크립트에서 직접 Windows 런타임 API를 호출하고, Cortana 통합을 활용하고, 온라인 인증 공급자를 사용하는 등 UWP(유니버설 Windows 플랫폼)에 대한 모든 권한을 가질 수 있습니다. 호스트된 스크립트에서 호출할 로컬 코드를 포함하고 원격 페이지와 로컬 페이지 간 앱 탐색을 관리할 수 있으므로 하이브리드 앱도 지원됩니다.

## 시작

Mac을 사용하든 PC를 사용하든 상관없이 몇 분 내에 고유한 호스트된 웹앱을 만들 수 있습니다. 특히 Windows 디바이스를 사용하는 경우 가장 좋은 시작 방법은 모든 기능을 갖춘 무료 [Visual Studio Community 2015](https://www.visualstudio.com/)를 사용하는 것입니다. Visual Studio에 액세스할 수 없는 경우 몇 가지 옵션을 선택할 수 있습니다. CLI(명령줄 인터페이스) 유틸리티에 익숙한 경우 [ManifoldJS](http://manifoldjs.com/)를 확인해 보세요. 또한 코딩할 필요가 없는 무료 온라인 제작 도구인 [App Studio](http://appstudio.windows.com/)를 사용하여 Windows 10 앱을 빠르게 빌드할 수도 있습니다.

- [Windows를 사용하여 웹 응용 프로그램을 UWP(유니버설 Windows 플랫폼) 앱으로 변환하는 단계별 지침](hwa-create-windows.md)

- [Mac을 사용하여 웹 응용 프로그램을 UWP(유니버설 Windows 플랫폼) 앱으로 변환하는 단계별 지침](hwa-create-mac.md)

## 앱 성능 향상

- Windows 런타임에서 JavaScript의 [기본 Windows 기능에 액세스](hwa-access-features.md)하여 앱 성능을 향상합니다.

- CSP(콘텐츠 보안 정책) 모델과 함께 ACUR(응용 프로그램 콘텐츠 URI 규칙)을 설정하여 앱 보안을 유지합니다.
- Cortana 음성 명령과 통합하여 음성으로 앱을 실행합니다.

- 앱 접근 권한 값을 선언하여 사용자 리소스 및 디바이스 기능에 대한 액세스 권한을 프로그래밍 방식으로 부여합니다.

- OpenID 및 OAuth를 사용하여 ID를 확인함으로써 간편하고 원활한 사용자 로그인 흐름을 만듭니다.

- 패키지된 웹앱과 호스트된 웹앱 중에서 결정하지 않아도 됩니다. 하이브리드 앱을 만들어 모두 사용할 수 있습니다.

## 기존 Chrome 앱 변환

Windows 호스트된 웹앱으로 쉽게 [기존의 Chrome 호스트된 앱을 변환](hwa-chrome-conversion.md)할 수 있습니다. [ManifoldJS](http://manifoldjs.com/)는 이제 Chrome 매니페스트를 입력 형식으로 허용합니다. 또한 기존 `.zip` 또는 `.crx` 파일에서 `.appx` 패키지를 생성하는 [CLI 도구](https://github.com/MicrosoftEdge/hwa-cli)도 개발했습니다.

## 데모

- [Contoso 여행 앱](http://contosotravel.azurewebsites.net/)

- [Windows 런타임 API: JavaScript 코드 샘플](http://rjs.azurewebsites.net/)



<!--HONumber=Jul16_HO1-->


