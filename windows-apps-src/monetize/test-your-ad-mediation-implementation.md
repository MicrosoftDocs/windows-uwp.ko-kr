---
ms.assetid: 54ECD653-7FC2-4A95-AC5A-972C4FB5A54B
description: 앱을 제출하기 전에 광고 조정 구현을 테스트하는 것이 좋습니다.
title: 광고 조정 구현 테스트
---

# 광고 조정 구현 테스트


\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]

앱을 제출하기 전에 광고 조정 구현을 테스트하는 것이 좋습니다.

## 테스트 광고 네트워크 구성 값으로 테스트


Visual Studio에서 프로젝트의 **연결된 서비스**를 시작하여 광고 네트워크 구성을 입력하지 않고 앱을 실행하는 경우, 개발 컴퓨터(UWP(Universal Windows Platform) 및 Windows 8.1 XAML 앱) 또는 에뮬레이터나 장치(Windows Phone 앱)에서 앱을 실행할 때 광고 조정에서 자동으로 테스트 구성 값을 사용합니다. 그러면 광고 네트워크의 필수 매개 변수를 입력하기 전에 신속하게 앱을 테스트하여 올바르게 코딩되었는지 확인할 수 있습니다.

광고 네트워크가 순차적으로 회전하면서 동일한 시간 동안 차례로 표시됩니다. 모든 광고 네트워크를 확인하여 일시적인 연결 문제가 발생할 가능성을 줄일 수 있도록 여러 주기 동안 충분히 오래 실행해야 합니다.

지원하는 광고 네트워크에 대한 테스트 광고가 표시됩니다. 테스트 광고가 오류처럼 보이는 경우도 있습니다. 이벤트를 검토하여 오류가 발생했는지 확인해야 합니다.

**참고** Windows Phone Silverlight 앱에서 테스트할 경우 Google AdMob에서는 테스트 메타데이터를 사용하지 않기 때문에 항상 **잘못된 요청** 오류를 반환합니다. Google AdMob 구현을 확인하려면 다음 섹션에 설명된 대로 필수 매개 변수를 입력해야 합니다.

 

[광고 중재자 컨트롤 추가 및 사용](add-and-use-the-ad-mediator-control.md)에 표시된 이벤트 처리 코드를 사용한 경우 콘솔 출력에 오류가 표시됩니다.

## 광고 네트워크 구성 값으로 테스트


테스트 구성 데이터로 앱을 테스트하고 나면 Windows 스토어에 게시하는 앱 버전에 사용할 광고 네트워크 구성 값으로 앱을 테스트합니다.

먼저 **연결된 서비스 추가** 창(Visual Studio 2015) 또는 **서비스 관리자** 창(Visual Studio 2013)을 열고 [광고 중재자 컨트롤 추가 및 사용](add-and-use-the-ad-mediator-control.md)에 설명된 대로 각 광고 네트워크를 구성합니다. 각 광고 네트워크에 대한 필수 매개 변수를 입력합니다.

이제 앱을 테스트할 준비가 되었습니다. 각 광고 네트워크에 광고가 제대로 표시되도록 앱을 충분히 오래 실행해야 합니다. 앱을 제출하기 전에 예외를 확인하고 코딩 오류를 수정합니다.

Windows 개발자 센터 대시보드에 앱 패키지를 제출하면 Visual Studio에 입력하는 구성 값이 **광고로 수익 창출** 대시보드 페이지에 자동으로 입력됩니다. 이러한 값을 수정하여 Windows 스토어에서 앱의 광고 네트워크 동작을 구성할 수 있습니다. 자세한 내용은 [앱 제출 및 광고 조정 구성](submit-your-app-and-configure-ad-mediation.md)을 참조하세요.

## 관련 항목

* [광고 네트워크 선택 및 관리](select-and-manage-your-ad-networks.md)
* [광고 조정 컨트롤 추가 및 사용](add-and-use-the-ad-mediator-control.md)
* [앱 제출 및 광고 조정 구성](submit-your-app-and-configure-ad-mediation.md)
* [광고 조정 문제 해결](troubleshoot-ad-mediation.md)
 

 





<!--HONumber=Mar16_HO1-->


