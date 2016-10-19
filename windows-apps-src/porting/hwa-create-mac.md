---
author: seksenov
title: "호스트된 웹앱 - Mac을 사용하여 Windows 앱으로 웹 응용 프로그램 변환"
description: "Mac을 사용하여 웹 사이트를 Windows 10용 UWP(유니버설 Windows 플랫폼) 앱으로 변환합니다."
kw: Hosted Web Apps with a Mac, Porting to Windows 10 with a Mac, Convert website to Windows with Mac, Packaging web application with ManfoldJS for Windows Store, Add website to Windows Store with App Studio
translationtype: Human Translation
ms.sourcegitcommit: 0458dcd2aab862ccdecf1ebbc51e883405a929a6
ms.openlocfilehash: 3ba820e2ec8a3556874c0c7c7e328831bab783ca

---

# Mac을 사용하여 호스트된 웹앱 만들기

웹 사이트 URL만으로 시작되는 Windows 10용 유니버설 Windows 플랫폼 앱을 빠르게 만듭니다. 

> [!NOTE]
> 다음 지침은 Mac 개발 플랫폼에서 사용하기 위한 것입니다. Windows 사용자는 [Windows 개발 플랫폼을 사용하는 경우의 지침](/hwa-create-windows.md)을 참조하세요.

## Mac에서 개발하는 데 필요한 사항

- 웹 브라우저.
- 명령 프롬프트.

## 옵션 1: ManifoldJS

[ManifoldJS](http://manifoldjs.com/)는 NPM에서 쉽게 설치하는 Node.js 앱입니다. 웹 사이트에 대한 메타 데이터를 가져와 Android, iOS 및 Windows에서 기본 호스트 앱을 생성합니다. 사이트에 [웹앱 매니페스트](https://www.w3.org/TR/appmanifest/)가 없는 경우 자동으로 생성됩니다.

1. NPM(노드 패키지 관리자)이 포함된 [NodeJS](https://nodejs.org/)를 설치합니다. <br>

2. 명령 프롬프트를 열고 다음과 같이 NPM으로 ManifoldJS를 설치합니다.
```
npm install -g manifoldjs
```

3. 다음과 같이 웹 사이트 URL에서 `manifoldjs` 명령을 실행합니다.
```
manifoldjs http://codepen.io/seksenov/pen/wBbVyb/?editors=101
```

4. 아래 동영상의 단계에 따라 패키징을 완료하고 Windows 스토어에 호스트된 웹앱을 게시합니다.

[![ManifoldJS를 사용하여 Mac에 UWP 웹앱 게시](images/hwa-to-uwp/mac_manifoldjs_video.png)](https://sec.ch9.ms/ch9/0a67/9b06e5c7-d7aa-478d-b30d-f99e145a0a67/ManifoldJS_high.mp4 "ManifoldJS를 사용하여 Mac에 UWP 웹앱 게시")

## 옵션 2: App Studio

[App Studio](http://appstudio.windows.com/)는 Windows 10 앱을 빠르게 빌드할 수 있게 하는 무료 온라인 앱 제작 도구입니다.

1. 웹 브라우저에서 [App Studio](http://appstudio.windows.com/)를 엽니다.

2. **지금 시작하세요!**를 클릭합니다.

3. **웹앱 템플릿**에서 **호스트된 웹앱**을 클릭합니다.

4. 화면상의 지침에 따라 Windows 스토어에 게시할 패키지를 생성합니다.

## 관련 항목

- [UWP(유니버설 Windows 플랫폼) 기능에 액세스하여 웹앱 향상](/hwa-access-features.md)
- [UWP(유니버설 Windows 플랫폼) 앱 지침](http://go.microsoft.com/fwlink/p/?LinkID=397871)
- [Windows 스토어 앱용 디자인 자산 다운로드](https://msdn.microsoft.com/library/windows/apps/xaml/bg125377.aspx)



<!--HONumber=Aug16_HO3-->


