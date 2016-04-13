---
ms.assetid: ba2ac5f5-1e0d-4f1d-a6f8-6a65b4cff501
이 섹션에서는 고객이 모든 유형의 디바이스에 설치할 수 있는 단일 Windows 10 앱 패키지를 만들 수 있는 UWP(유니버설 Windows 플랫폼)로 기존 앱을 포팅하는 방법을 설명합니다. 앱은 흥미로운 새 하드웨어, 엄청난 수익 창출 기회, 최신 API 집합, 적응 UI 컨트롤, 마우스/키보드, 터치 및 음성을 포함하여 다양한 입력 양식을 활용할 수 있습니다.
Windows 10으로 앱 포팅
---

# Windows 10으로 앱 포팅

\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]

이 섹션에서는 고객이 모든 유형의 디바이스에 설치할 수 있는 단일 Windows 10 앱 패키지를 만들 수 있는 UWP(유니버설 Windows 플랫폼)로 기존 앱을 포팅하는 방법을 설명합니다. 앱은 흥미로운 새 하드웨어, 엄청난 수익 창출 기회, 최신 API 집합, 적응 UI 컨트롤, 마우스/키보드, 터치 및 음성을 포함하여 다양한 입력 양식을 활용할 수 있습니다.

WinRT(Windows 런타임)는 UWP(유니버설 Windows 플랫폼) 앱을 빌드할 수 있는 기술입니다. WinRT 및 UWP 앱에 대한 배경 정보는 [UWP(유니버설 Windows 플랫폼) 앱이란?](https://msdn.microsoft.com/library/windows/apps/dn726767)을 참조할 수 있습니다.

이 포팅 가이드에서는 현재 앱 기술과 UWP(유니버설 Windows 플랫폼)의 차이점에 대해 설명합니다. 기술 간의 진행 경로를 이해한 후 UWP 앱을 개발하는 데 중요한 자료가 되는 개발자 센터의 나머지 부분을 살펴볼 수 있습니다. 그러기 위해서는 [스토어 앱 개발 방법](https://msdn.microsoft.com/library/windows/apps/dn726537)을 시작하는 것이 좋습니다.

| 항목 | 설명 |
|-------|-------------|
| [Windows Phone Silverlight에서 UWP로 이동](wpsl-to-uwp-root.md) | Windows Phone Silverlight 앱을 사용하는 개발자는 Windows 10으로 이동하는 과정에서 자신의 기술과 소스 코드를 최대한 활용할 수 있습니다. Windows 10을 사용하면 모든 종류의 디바이스에 설치할 수 있는 단일 앱 패키지인 UWP 앱을 만들 수 있습니다. |
| [Windows 런타임 8.x에서 UWP로 이동](w8x-to-uwp-root.md) | Windows 8.1, Windows Phone 8.1 또는 둘 다를 대상으로 하는지에 관계없이 유니버설 8.1 앱이 있는 경우 소스 코드 및 기술이 Windows 10으로 원활하게 포팅되는지 확인합니다. Windows 10을 사용하면 모든 종류의 디바이스에 설치할 수 있는 단일 앱 패키지인 UWP 앱을 만들 수 있습니다. |
| [RTM으로 UWP Microsoft Visual Studio 2015 RC 프로젝트 업데이트](update-your-visual-studio-2015-rc-project-to-rtm.md) | Microsoft Visual Studio 2015 RC를 사용하여 만든 Windows 10 프로젝트가 있는 경우 Visual Studio 2015 RTM에 적합한 형식으로 프로젝트 파일을 업데이트할 수 있는 두 가지 옵션이 있습니다. 권장되는 방법은 Visual Studio 2015 RTM에서 새 Windows 10 프로젝트를 만들고 해당 프로젝트에 파일을 복사하는 것입니다. 또는 고급 설명서에 따라 기존 프로젝트 파일을 편집하고 새 형식으로 이동할 수 있습니다. |
| [Android 및 iOS 개발자용 Windows 앱 개념 매핑](android-ios-uwp-map.md) | Android 또는 iOS 기술이나 코드를 사용하는 개발자로, Windows 10 및 유니버설 Windows 플랫폼으로 이동하려는 경우 이 리소스에는 세 가지 플랫폼 간에 플랫폼 기능과 지식을 매핑하는 데 필요한 모든 정보가 들어 있습니다. |
| [iOS에서 UWP로 이동](ios-to-uwp-root.md) | iOS 개발자로, Windows 10 및 UWP를 이동하는 방법이 궁금한가요? 생각만큼 겁내지 않아도 됩니다. iOS 디바이스에서와 마찬가지로 Windows에서도 원활하게 작동하는 멋진 앱을 만드는 데 필요한 도구, 기술 및 정보를 갖추었습니다. 이제 더 멋진 앱을 만들 수 있습니다. |
 
## 관련 항목

* [WPF 및 Silverlight에서 WinRT로 이동](https://msdn.microsoft.com/library/windows/apps/dn263237)
* [Android에서 WinRT로 이동](https://msdn.microsoft.com/library/windows/apps/jj945421)
* [웹에서 WinRT로 이동](https://msdn.microsoft.com/library/windows/apps/hh465151)


<!--HONumber=Mar16_HO1-->


