---
author: DBirtolo
title: Bluetooth 광고
description: 이 섹션에는 AdvertisementWatcher 및 AdvertisementPublisher API 사용자를 통해 Bluetooth LE(저에너지) 광고를 UWP(유니버설 Windows 플랫폼) 앱에 통합하는 방법에 대한 문서가 포함되어 있습니다.
---

# Bluetooth 광고

\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]

** 중요 API ** 

-   [**Windows.Devices.Bluetooth.Advertisement**](https://msdn.microsoft.com/library/windows/apps/windows.devices.bluetooth.advertisement.aspx)

이 문서에서는 UWP(유니버설 Windows 플랫폼) 앱용 Bluetooth 광고(신호)의 개요를 제공합니다.  

## 개요

개발자가 광고 API를 사용하여 수행할 수 있는 두 가지 주요 기능이 있습니다.

-   [광고 감시자](https://msdn.microsoft.com/library/windows/apps/windows.devices.bluetooth.advertisement.bluetoothleadvertisementwatcher.aspx): 근처의 신호를 수신하고 페이로드 또는 근접성을 기준으로 필터링합니다.  
-   [광고 게시자](https://msdn.microsoft.com/library/windows/apps/windows.devices.bluetooth.advertisement.bluetoothleadvertisementpublisher.aspx): Windows에서 개발자 대신 광고를 수행하기 위한 페이로드를 정의합니다.  

전체 샘플 코드는 [Bluetooth 광고 샘플](http://go.microsoft.com/fwlink/p/?LinkId=619990)에서 찾을 수 있습니다.


<!--HONumber=May16_HO2-->


