---
ms.assetid: 772DEBF2-1578-4330-9C14-70BCC6F55005
description: Microsoft는 여러 광고 네트워크의 배너 광고 요청을 조정하여 앱 내 광고 수익을 최적화할 수 있는 광고 조정을 지원합니다.
title: 광고 조정을 사용하여 수익 최대화
---

#  광고 조정을 사용하여 수익 최대화


\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]

Microsoft는 여러 광고 네트워크의 배너 광고 요청을 조정하여 앱 내 광고 수익을 최적화할 수 있는 광고 조정을 지원합니다. 여러 광고 네트워크에는 1,000회 보기당 더 높은 비용(eCPM) 또는 다른 시장보다 특정 시장에서 더 높은 유효 노출률(앱에서 요청 시 광고 제공 비율) 등 각각의 장점이 있을 수 있습니다. 단일 광고 네트워크에서는 채워지지 않은 광고 요청으로 잠재적인 수익 손실이 발생할 수 있습니다. 광고 조정을 통해 항상 실시간 광고를 표시하여 광고 수익 창출을 극대화할 수 있습니다.

광고 조정 지원은 [Microsoft 스토어 참여 및 수익 창출 SDK](http://aka.ms/store-em-sdk)를 통해 사용할 수 있습니다. SDK를 설치하고 앱에 광고 조정자 컨트롤을 추가한 후 개발자 센터에서 조정 구성을 업데이트하여 각 네트워크가 사용되는 빈도를 지정할 수 있습니다. 해당 지역에서 가장 효율적인 광고 네트워크를 사용하도록 시장별로 이 값을 최적화할 수 있습니다. 그리고 앱을 다시 게시하지 않고 각 광고 네트워크가 사용되는 방식을 변경할 수 있습니다.

## 광고 조정 시작


앱에서 광고 조정을 설정 및 구성하려면 다음 단계를 따르세요.

1.  광고 조정에서 지원한 광고 네트워크 목록 및 프로젝트 형식을 검토하고, 사용하려는 광고 네트워크의 계정을 설정하고, 앱을 등록할 각 광고 네트워크의 지침을 따릅니다. 자세한 내용은 [광고 네트워크 선택 및 관리](select-and-manage-your-ad-networks.md)를 참조하세요.

2.  Visual Studio 2015 또는 Visual Studio 2013과 함께 [Microsoft 스토어 참여 및 수익 창출 SDK](http://aka.ms/store-em-sdk)를 설치합니다.

3.  Visual Studio에서 프로젝트를 열거나 새 프로젝트를 만듭니다. 광고를 호스트하려는 페이지를 열고, **AdMediatorControl**을 페이지로 끕니다. Microsoft Advertising을 사용할 경우 지원되는 광고 크기에 맞게 컨트롤의 높이와 너비를 조정합니다. 자세한 내용은 [광고 조정자 컨트롤 추가 및 사용](add-and-use-the-ad-mediator-control.md)을 참조하세요.

4.  **연결된 서비스**를 실행하여 대상으로 지정하려는 광고 네트워크를 선택하고 각 광고 네트워크의 필수 매개 변수를 구성합니다. 자세한 내용은 [광고 조정자 컨트롤 추가 및 사용](add-and-use-the-ad-mediator-control.md)을 참조하세요.

5.  앱에서 광고 조정 구현을 테스트합니다. 자세한 내용은 [광고 조정 구현 테스트](test-your-ad-mediation-implementation.md)를 참조하세요.

6.  Windows 개발자 센터 대시보드에 앱을 제출하고 대시보드에서 광고 조정 설정을 구성합니다. 자세한 내용은 [앱 제출 및 광고 조정 구성](submit-your-app-and-configure-ad-mediation.md)을 참조하세요.

7.  개발자 센터 대시보드에서 광고 조정 보고서를 검토합니다. 자세한 내용은 [광고 조정 보고서](https://msdn.microsoft.com/library/windows/apps/mt148521)를 참조하세요.

## 광고 조정 없이 Microsoft Advertising 사용


광고 조정을 사용하지 않으려는 경우 또는 프로젝트 형식이 현재 광고 조정에서 지원되지 않는 경우에도 계속 광고 조정을 사용하지 않고 Microsoft 배너 광고를 제공할 수 있습니다. 자세한 내용은 [XAML 및 .NET의 AdControl](https://msdn.microsoft.com/library/mt313186.aspx)(영문)과 [HTML 5 및 JavaScript의 AdControl](https://msdn.microsoft.com/library/mt313130.aspx)(영문)을 참조하세요.

## 관련 항목

* [광고 네트워크 선택 및 관리](select-and-manage-your-ad-networks.md)
* [광고 조정 컨트롤 추가 및 사용](add-and-use-the-ad-mediator-control.md)
* [광고 조정 구현 테스트](test-your-ad-mediation-implementation.md)
* [앱 제출 및 광고 조정 구성](submit-your-app-and-configure-ad-mediation.md)
* [광고 조정 문제 해결](troubleshoot-ad-mediation.md)
 

 


<!--HONumber=Mar16_HO5-->


