---
title: 사용자 인터페이스 추가
description: DirectX UWP 게임을 2D 사용자 인터페이스 오버레이 추가 하는 방법에 알아봅니다.
ms.assetid: fa40173e-6cde-b71b-e307-db90f0388485
ms.date: 10/24/2017
ms.topic: article
keywords: Windows 10, uwp, 게임, 사용자 인터페이스, directx
ms.localizationpriority: medium
ms.openlocfilehash: 09005eb12997126a9cad68c388beb0473b19fda3
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57609058"
---
# <a name="add-a-user-interface"></a>사용자 인터페이스 추가


이제 게임에 3D 시각적 개체, 게임 플레이어에 게 게임 상태에 대 한 피드백을 제공할 수 있도록 몇 가지 2D 요소를 추가 하는 방법에 집중할 수 있는 시간입니다. 이 간단한 메뉴 옵션을 추가 하 여 수행할 수 있습니다 및 3 차원 그래픽 위에 (heads-up display) 구성 요소 출력을 파이프라인 합니다.

>[!Note]
>이 샘플의 최신 게임 코드를 다운로드하지 않은 경우 [Direct3D 게임 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Simple3DGameDX)로 이동합니다. 이 샘플은 UWP 기능 샘플의 큰 컬렉션의 일부입니다. 샘플을 다운로드하는 방법에 대한 지침은 [GitHub에서 UWP 샘플 가져오기](https://docs.microsoft.com/windows/uwp/get-started/get-uwp-app-samples)를 참조하세요.

## <a name="objective"></a>목표

Direct2D를 사용 하 여 포함 하는 UWP DirectX 게임에 다양 한 사용자 인터페이스 그래픽 및 동작을 추가 합니다.
- 헤드업 표시를 포함 하 여 [모양을 이동 컨트롤러](tutorial--adding-controls.md) 경계 사각형
- 게임 상태 메뉴


## <a name="the-user-interface-overlay"></a>사용자 인터페이스 오버레이


여러 가지 방법을 사용 하 여 DirectX 게임에서 텍스트 및 사용자 인터페이스 요소를 표시할 수 있습니다, 있지만 하겠습니다 포커스를 사용 하 여 [Direct2D](https://msdn.microsoft.com/library/windows/apps/dd370990.aspx)합니다. 또한 사용할 것 [DirectWrite](https://msdn.microsoft.com/library/windows/desktop/dd368038) 텍스트 요소에 대 한 합니다.


Direct2D 픽셀 기반 기본 형식 및 효과 그리는 데 사용 되는 2D 그리기 Api 집합입니다. Direct2D를 사용 하 여 시작을 하는 경우 간단 하 게 유지 하는 것이 좋습니다. 복잡한 레이아웃 및 인터페이스 동작에는 시간과 계획이 필요합니다. 게임 시뮬레이션 및 전략 게임에서 복잡 한 사용자 인터페이스를 요구 하는 경우에 대신 XAML을 사용 하는 것이 좋습니다.

> [!NOTE]
> UWP DirectX 게임에서 XAML 사용 하 여 사용자 인터페이스를 개발 하는 방법에 대 한 정보를 참조 하세요 [게임 샘플 확장](tutorial-resources.md)합니다.

Direct2D는 사용자 인터페이스 또는 HTML 및 XAML와 같은 레이아웃에 대 한 설계 되지 않습니다. 목록 상자, 단추와 같은 사용자 인터페이스 구성 요소가 제공 하지 않습니다. 또한 div, 테이블 또는 그리드와 같은 구성 요소 레이아웃도 제공 하지 않습니다.


이 게임 샘플에 대 한 두 가지 주요 UI 구성 요소가 있습니다.
1. 점수 및 게임 내 컨트롤의 헤드업 디스플레이입니다.
2. 오버레이 게임 상태 텍스트 및 일시 중지 정보 등의 옵션을 표시 하 고 수준 옵션을 시작 합니다.

### <a name="using-direct2d-for-a-heads-up-display"></a>주의 표시에 Direct2D 사용

다음 이미지에서는 샘플 게임 헤드업 디스플레이 보여 줍니다. 것이 간단 하 고 깔끔해야 플레이어 3D 세계를 탐색 하 고 대상 해결에 집중할 수 있도록 합니다. 적절 한 인터페이스 또는 헤 즈 업 디스플레이의 플레이어를 처리 하 고 게임의 이벤트에 대응할 수 복잡 해질 되지 해야 합니다.

![게임 오버레이의 스크린샷](images/simple-dx-game-ui-overlay.png)

오버레이 다음 기본 기본 형식으로 구성 됩니다.
- [**DirectWrite** ](https://msdn.microsoft.com/en-us/library/windows/desktop/dd368038) 의 플레이어에 게 알려 주는 오른쪽 위 모서리에 있는 텍스트 
    - 성공적인 적중 횟수
    - 플레이어 했습니다 샷 수
    - 수준에 남은 시간
    - 현재 수준 수 
- 십자선을 형성 하는 데 사용 되는 선 세그먼트를 교차 두
- 두 개의 사각형에 대 한 아래쪽 모서리에는 [모양을 이동 컨트롤러](tutorial--adding-controls.md) 경계입니다. 


게임에 헤 즈 업 디스플레이 상태의 오버레이 그릴지를 [ **GameHud::Render** ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameHud.cpp#L234-L358) 메서드를 [ **GameHud** ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameHud.h) 클래스. 이 메서드 내에서 UI를 나타내는 Direct2D 오버레이 적중 횟수, 시간, 남은 및 수준 번호 수의 변경 내용을 반영 하도록 업데이트 됩니다.

게임 초기화 된 경우 추가 `TotalHits()`, `TotalShots()`, 및 `TimeRemaining()` 에 [ **swprintf_s** ](https://docs.microsoft.com/cpp/c-runtime-library/reference/sprintf-s-sprintf-s-l-swprintf-s-swprintf-s-l) 버퍼링 하 고 인쇄 형식을 지정 합니다. 사용 하 여 다음 내릴 수 있습니다 합니다 [ **DrawText** ](https://msdn.microsoft.com/en-us/library/windows/desktop/dd742848) 메서드. 에서는 동일한 작업을 수행 현재 수준 표시기에 대 한 그리기 ➀를 같은 완료 되지 않은 수준을 표시 합니다. 빈 번호 및 특정 수준 완료 되었음을 표시할 ➊ 같은 채워진된 숫자입니다.


다음 코드는 과정을 안내 합니다 **GameHud::Render** 메서드의 프로세스 
- 사용 하 여 비트맵 [* * ID2D1RenderTarget::DrawBitmap * *](https://msdn.microsoft.com/en-us/library/windows/desktop/dd371880)
- 사용 하 여 사각형 UI 영역 해제 단면 [ **D2D1::RectF**](https://msdn.microsoft.com/en-us/library/windows/desktop/dd368184)
- 사용 하 여 **DrawText** 텍스트 요소를 확인 합니다.

```cpp
void GameHud::Render(_In_ Simple3DGame^ game)
{
    auto d2dContext = m_deviceResources->GetD2DDeviceContext();
    auto windowBounds = m_deviceResources->GetLogicalSize();

    if (m_showTitle)
    {
        d2dContext->DrawBitmap(
            m_logoBitmap.Get(),
            D2D1::RectF(
                GameConstants::Margin,
                GameConstants::Margin,
                m_logoSize.width + GameConstants::Margin,
                m_logoSize.height + GameConstants::Margin
                )
            );
        d2dContext->DrawTextLayout(
            Point2F(m_logoSize.width + 2.0f * GameConstants::Margin, GameConstants::Margin),
            m_titleHeaderLayout.Get(),
            m_textBrush.Get()
            );
        d2dContext->DrawTextLayout(
            Point2F(GameConstants::Margin, m_titleBodyVerticalOffset),
            m_titleBodyLayout.Get(),
            m_textBrush.Get()
            );
    }

    // Draw text for number of hits, total shots, and time remaining
    if (game != nullptr)
    {
        // This section is only used after the game state has been initialized.
        static const int bufferLength = 256;
        static char16 wsbuffer[bufferLength];
        int length = swprintf_s(
            wsbuffer,
            bufferLength,
            L"Hits:\t%10d\nShots:\t%10d\nTime:\t%8.1f",
            game->TotalHits(),
            game->TotalShots(),
            game->TimeRemaining()
            );
        
        // Draw the upper right portion of the HUD displaying total hits, shots, and time remaining
        d2dContext->DrawText(
            wsbuffer,
            length,
            m_textFormatBody.Get(),
            D2D1::RectF(
                windowBounds.Width - GameConstants::HudRightOffset,
                GameConstants::HudTopOffset,
                windowBounds.Width,
                GameConstants::HudTopOffset + (GameConstants::HudBodyPointSize + GameConstants::Margin) * 3
                ),
            m_textBrush.Get()
            );

        // Using the unicode characters starting at 0x2780 ( ➀ ) for the consecutive levels of the game.
        // For completed levels start with 0x278A ( ➊ ) (This is 0x2780 + 10).
        uint32 levelCharacter[6];
        for (uint32 i = 0; i < 6; i++)
        {
            levelCharacter[i] = 0x2780 + i + ((static_cast<uint32>(game->LevelCompleted()) == i) ? 10 : 0);
        }
        length = swprintf_s(
            wsbuffer,
            bufferLength,
            L"%lc %lc %lc %lc %lc %lc",
            levelCharacter[0],
            levelCharacter[1],
            levelCharacter[2],
            levelCharacter[3],
            levelCharacter[4],
            levelCharacter[5]
            );
        // Create a new rectangle and draw the current level info text inside
        d2dContext->DrawText(
            wsbuffer,
            length,
            m_textFormatBodySymbol.Get(),
            D2D1::RectF(
                windowBounds.Width - GameConstants::HudRightOffset,
                GameConstants::HudTopOffset + (GameConstants::HudBodyPointSize + GameConstants::Margin) * 3 + GameConstants::Margin,
                windowBounds.Width,
                GameConstants::HudTopOffset + (GameConstants::HudBodyPointSize+ GameConstants::Margin) * 4
                ),
            m_textBrush.Get()
            );

        if (game->IsActivePlay())
        {
            // Draw the move and fire rectangles
            // Draw the crosshairs
        }
    }
}
```

이 부분은 더 아래로 메서드 주요 합니다 [ **GameHud::Render** ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameHud.cpp#L320-L358) 메서드 이동 하는 과정을 그리는 데 사용 하 여 사각형을 실행 합니다. [ **ID2D1RenderTarget::DrawRectangle** ](https://msdn.microsoft.com/library/windows/desktop/dd371902), 및에 대 한 두 호출을 사용 하 여 십자 [ **ID2D1RenderTarget::DrawLine**](https://msdn.microsoft.com/library/windows/desktop/dd371895)합니다.

```cpp
        // Check if game is playing
        if (game->IsActivePlay())
        {
            // Draw a rectangle for the touch input for the move control.
            d2dContext->DrawRectangle(
                D2D1::RectF(
                    0.0f,
                    windowBounds.Height - GameConstants::TouchRectangleSize,
                    GameConstants::TouchRectangleSize,
                    windowBounds.Height
                    ),
                m_textBrush.Get()
                );
            // Draw a rectangle for the touch input of the fire control.
            d2dContext->DrawRectangle(
                D2D1::RectF(
                    windowBounds.Width - GameConstants::TouchRectangleSize,
                    windowBounds.Height - GameConstants::TouchRectangleSize,
                    windowBounds.Width,
                    windowBounds.Height
                    ),
                m_textBrush.Get()
                );

            // Draw two lines to form crosshairs
            d2dContext->DrawLine(
                D2D1::Point2F(windowBounds.Width / 2.0f - GameConstants::CrossHairHalfSize, windowBounds.Height / 2.0f),
                D2D1::Point2F(windowBounds.Width / 2.0f + GameConstants::CrossHairHalfSize, windowBounds.Height / 2.0f),
                m_textBrush.Get(),
                3.0f
                );
            d2dContext->DrawLine(
                D2D1::Point2F(windowBounds.Width / 2.0f, windowBounds.Height / 2.0f - GameConstants::CrossHairHalfSize),
                D2D1::Point2F(windowBounds.Width / 2.0f, windowBounds.Height / 2.0f + GameConstants::CrossHairHalfSize),
                m_textBrush.Get(),
                3.0f
                );
        }
```

에 **GameHud::Render** 게임 창의 논리적 크기 저장 메서드는 `windowBounds` 변수입니다. 사용 하 여이 [ `GetLogicalSize` ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Common/DeviceResources.h#L41) 메서드를 **DeviceResources** 클래스입니다. 
```cpp
auto windowBounds = m_deviceResources->GetLogicalSize();
```

 게임 창 크기를 가져오는 UI 프로그래밍에 대 한 필수적입니다. 창의 크기는 DIP를 1/96 인치로 정의 되어 있는 Dip (장치 독립적 픽셀) 라는 측정 단위로 제공 됩니다. Direct2D 조정 실제 픽셀 그리기 단위 그리기 경우 Windows에 대 한 인치당 dpi 설정 사용 하 여 이렇게 합니다. 마찬가지로, 그릴 때 사용 하 여 텍스트 [ **DirectWrite**](https://msdn.microsoft.com/en-us/library/windows/desktop/dd368038), 글꼴의 포인트 크기 보다는 Dip를 지정 합니다. DIP는 부동 소수점 수로 표현됩니다.

 

### <a name="displaying-game-state-info"></a>게임 상태 정보를 표시합니다.

헤드업 디스플레이 외에도 게임 샘플에는 6 가지 게임 상태를 나타내는 오버레이 합니다. 모든 상태를 읽는 플레이어에 대 한 텍스트를 사용 하 여 큰 검은색 사각형 기본을 기능입니다. 컨트롤러 사각형과 십자 모양을 이동 하지 그려집니다 하지 않기 때문에 현재 이러한 상태에서.

오버레이 사용 하 여 만들어집니다 합니다 [ **GameInfoOverlay** ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.h) 게임의 상태에 맞게 표시할 텍스트를 전환 하도록 허락해 클래스입니다.

![상태 및 작업의 오버레이](images/simple-dx-game-ui-finaloverlay.png)

오버레이 두 개의 섹션으로 세분화 됩니다. **상태** 하 고 **동작**합니다. 합니다 **상태** 섹션으로 세분화 추가 됩니다 **Title** 하 고 **본문** 사각형입니다. 합니다 **동작** 섹션에는 하나의 사각형만 합니다. 각 사각형 다양 한 용도가 있습니다.

-   `titleRectangle` 제목 텍스트를 포함합니다.
-   `bodyRectangle` 본문 텍스트를 포함합니다.
-   `actionRectangle` 특정 작업을 수행 하는 플레이어에 게 알려 주는 텍스트를 포함 합니다.

게임에 설정할 수 있는 6 가지 상태가 있습니다. 사용 하 여 전달 게임의 상태를 **상태** 오버레이 부분입니다. 합니다 **상태** 사각형 다양 한 상태를 사용 하 여 해당 메서드를 사용 하 여 업데이트 됩니다.

- 로드
- 초기 시작/높은 점수 통계
- 수준 시작
- 게임이 일시 중지
- 게임 종료
- 게임-이득


**작업** 부분 오버레이 사용 하 여 업데이트 되는 [ **GameInfoOverlay::SetAction** ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L522-L564) 메서드를 다음 중 하나로 설정할 동작 텍스트를 허용 합니다.
- "... 다시 재생 탭"
- "로드 수준, 잠시 기다려 주십시오."
- "계속 하려면... 탭"
- 없음

> [!NOTE]
> 두 가지이 방법에 설명 합니다에 추가 합니다 [게임 상태를 나타내는](#representing-game-state) 섹션입니다.

어떤 일이 일어나는지 게임을 따라 합니다 **상태** 하 고 **작업** 섹션 텍스트 필드를 조정 하는 합니다.
초기화 하 고 이러한 6 가지 상태에 대 한 오버레이 그리는 방법을 살펴보겠습니다.

### <a name="initializing-and-drawing-the-overlay"></a>오버레이 초기화 및 그리기

6 개의 **상태** 서로 몇 가지 공통 상태, 리소스 및 메서드 만들기와 매우 유사 해야 합니다.
    - 모두 검은 사각형 화면의 가운데에서 해당 배경으로 사용합니다.
    - 표시 된 텍스트는 **제목** 하거나 **본문** 텍스트입니다.
    - 텍스트는 Segoe UI 글꼴을 사용 하 고 백 사각형 위에 그려집니다. 


샘플 게임에 오버레이 만들 때 고려해 야 하는 네 가지 메서드가 있습니다.
 

#### <a name="gameinfooverlaygameinfooverlay"></a>GameInfoOverlay::GameInfoOverlay
합니다 [ **GameInfoOverlay::GameInfoOverlay** ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L30-L78) 생성자에서 플레이어에 게 정보를 표시를 사용 하는 비트맵 표면을 유지 오버레이 초기화 합니다. 생성자에서 팩터리를 가져옵니다 합니다 [ **ID2D1Device** ](https://msdn.microsoft.com/library/windows/desktop/hh404478) 만드는 데 사용 하기에 전달 된 개체를 [ **ID2D1DeviceContext** ](https://msdn.microsoft.com/library/windows/desktop/hh404479) 오버레이 개체 자체를 그릴 수 있습니다. [IDWriteFactory::CreateTextFormat](https://msdn.microsoft.com/en-us/library/windows/desktop/dd368203) 


#### <a name="gameinfooverlaycreatedevicedependentresources"></a>GameInfoOverlay::CreateDeviceDependentResources
[**GameInfoOverlay::CreateDeviceDependentResources** ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L82-L104) 는 브러시를 만드는 데 사용할 텍스트를 그리는 메서드입니다. 구한이 작업을 수행 하는 [ **ID2D1DeviceContext2** ](https://msdn.microsoft.com/en-us/library/windows/desktop/dn890789) 개체를 만들 수 있으며 메시 잉크 및 그라데이션 같은 기능 및 기 하 도형 그리기 렌더링 합니다. 그런 다음 일련의 색이 지정 된 브러시를 사용 하 여 만듭니다 [ **ID2D1SolidColorBrush** ](https://msdn.microsoft.com/en-us/library/windows/desktop/dd372207) folling UI 요소를 그립니다.
- 사각형 배경에 대 한 검정 브러시
- 흰색 브러시로 상태 텍스트
- 작업 텍스트 주황색 브러시

#### <a name="deviceresourcessetdpi"></a>DeviceResources::SetDpi
합니다 [ **DeviceResources::SetDpi** ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Common/DeviceResources.cpp#L514-L527) 메서드 창의 인치당을 설정 합니다. 이 메서드가 호출 되는 DPI가 변경 되 고 해야 하는 경우는 게임 창 크기를 조정할 때 발생 하는 재조정 합니다. DPI를 업데이트 한 후이 메서드 호출[**DeviceResources::CreateWindowSizeDependentResources** ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Common/DeviceResources.cpp#L214-L487) 필요한 있는지 확인 하려면 리소스 창 크기를 조정할 때마다 다시 만들어집니다.


#### <a name="gameinfooverlaycreatewindowssizedependentresources"></a>GameInfoOverlay::CreateWindowsSizeDependentResources
합니다 [ **GameInfoOverlay::CreateWindowsSizeDependentResources** ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L108-L225) 메서드는 모든 그리기 이루어지는 곳입니다. 다음은 메서드의 단계 개요입니다.
- 섹션의 UI 텍스트를 해제 하기 위해 세 개의 사각형이 만들어집니다 합니다 **제목**, **본문**, 및 **작업** 텍스트입니다.
    ```cpp 
    m_titleRectangle = D2D1::RectF(
        GameInfoOverlayConstant::SideMargin,
        GameInfoOverlayConstant::TopMargin,
        overlaySize.width - GameInfoOverlayConstant::SideMargin,
        GameInfoOverlayConstant::TopMargin + GameInfoOverlayConstant::TitleHeight
        );
    m_actionRectangle = D2D1::RectF(
        GameInfoOverlayConstant::SideMargin,
        overlaySize.height - (GameInfoOverlayConstant::ActionHeight + GameInfoOverlayConstant::BottomMargin),
        overlaySize.width - GameInfoOverlayConstant::SideMargin,
        overlaySize.height - GameInfoOverlayConstant::BottomMargin
        );
    m_bodyRectangle = D2D1::RectF(
        GameInfoOverlayConstant::SideMargin,
        m_titleRectangle.bottom + GameInfoOverlayConstant::Separator,
        overlaySize.width - GameInfoOverlayConstant::SideMargin,
        m_actionRectangle.top - GameInfoOverlayConstant::Separator
        );
    ```

- 비트맵 라는 생성 됩니다 `m_levelBitmap`를 사용 하 여 계정을 현재 DPI 고려 **CreateBitmap**합니다.
- `m_levelBitmap` 사용 하 여 2D 렌더링 대상으로 설정 됩니다 [ **ID2D1DeviceContext::SetTarget**](https://msdn.microsoft.com/en-us/library/windows/desktop/hh404533)합니다.
- 사용 하 여 검정을 수행 하는 모든 픽셀을 사용 하 여 비트맵 지워집니다 [ **ID2D1RenderTarget::Clear**](https://msdn.microsoft.com/en-us/library/windows/desktop/dd371772)합니다.
- [**ID2D1RenderTarget::BeginDraw** ](https://msdn.microsoft.com/en-us/library/windows/desktop/dd371768) 그리기를 시작 하기 위해 호출 됩니다. 
- **DrawText** 에 저장 된 텍스트를 그리기 위해 호출 됩니다 `m_titleString`, `m_bodyString`, 및 `m_actionString` 해당를 사용 하 여 approperiate 사각형에서 **ID2D1SolidColorBrush**합니다.
- [**ID2D1RenderTarget::EndDraw** ](ID2D1RenderTarget::EndDraw) 에서 모든 그리기 작업을 중지 하 라고 `m_levelBitmap`합니다.
- 사용 하 여 다른 비트 멥 만들어집니다 **CreateBitmap** 라는 `m_tooSmallBitmap` 표시 구성은 게임에 비해 너무 작은 경우에 표시에서 대체 방법으로 사용 하도록 합니다.
- 에 그리기 위한 프로세스를 반복 `m_levelBitmap` 에 대 한 `m_tooSmallBitmap`문자열을 그리기만 이번 `Paused` 본문에 있습니다.




이제는 6 개의 오버레이 상태 텍스트를 입력 하는 6 가지가 하기만 했습니다!

### <a name="representing-game-state"></a>게임 상태를 나타내는


각 게임의 6 오버레이 상태에 해당 되는 메서드를 **GameInfoOverlay** 개체입니다. 이러한 메서드는 오버레이의 변형을 그려 게임 자체에 대한 명시적 정보를 플레이어에게 전달합니다. 이 통신으로 표시 되는 **제목** 하 고 **본문** 문자열. 샘플 초기화 될 때와 해당 리소스와이 정보에 대 한 레이아웃을 이미 구성 때문에 합니다 [ **GameInfoOverlay::CreateDeviceDependentResources** ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L82-L104) 메서드만 제공 해야 오버레이 상태 관련 문자열입니다.

합니다 **상태** 오버레이에 다음 메서드 중 하나를 호출 하 여 설정 됩니다.

게임 상태 | 상태 set 메서드 | 상태 필드
:----- | :------- | :---------
로드 | [GameInfoOverlay::SetGameLoading](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L254-L306) |**제목**</br>리소스 로드 </br>**본문**</br> 증분 방식으로 인쇄 "." 로드 작업을 의미 합니다.
초기 시작/높은 점수 통계 | [GameInfoOverlay::SetGameStats](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L310-L354) |**제목**</br>최고 점수</br> **본문**</br> 수준 # 완료 </br>Total # 요소</br>Total # 장면
수준 시작 | [GameInfoOverlay::SetLevelStart](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L413-L471) |**제목**</br>수준 #</br>**본문**</br>수준 목표 설명입니다.
게임이 일시 중지 | [GameInfoOverlay::SetPause](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L475-L502) |**제목**</br>게임이 일시 중지</br>**본문**</br>없음
게임 종료 | [GameInfoOverlay::SetGameOver](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L358-L409) |**제목**</br>게임 오버</br> **본문**</br> 수준 # 완료 </br>Total # 요소</br>Total # 장면</br>수준 # 완료</br>높은 # 점수 매기기
게임-이득 | [GameInfoOverlay::SetGameOver](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L358-L409) |**제목**</br>승리 했습니다.</br> **본문**</br> 수준 # 완료 </br>Total # 요소</br>Total # 장면</br>수준 # 완료</br>높은 # 점수 매기기




사용 하 여 합니다 [ **GameInfoOverlay::CreateWindowSizeDependentResources** ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L117-L134) 메서드를 샘플 오버레이의 특정 영역에 해당 하는 세 개의 사각형 영역을 선언 합니다.



이러한 영역에 주의하면서 상태 관련 메서드 중 하나인 **GameInfoOverlay::SetGameStats**에 대해 살펴보고 오버레이를 그리는 방법을 알아보겠습니다.

```cpp
void GameInfoOverlay::SetGameStats(int maxLevel, int hitCount, int shotCount)
{
    int length;

    auto d2dContext = m_deviceResources->GetD2DDeviceContext();

    d2dContext->SetTarget(m_levelBitmap.Get());
    d2dContext->BeginDraw();
    d2dContext->SetTransform(D2D1::Matrix3x2F::Identity());
    d2dContext->FillRectangle(&m_titleRectangle, m_backgroundBrush.Get());
    d2dContext->FillRectangle(&m_bodyRectangle, m_backgroundBrush.Get());
    m_titleString = "High Score";

    d2dContext->DrawText(
        m_titleString->Data(),
        m_titleString->Length(),
        m_textFormatTitle.Get(),
        m_titleRectangle,
        m_textBrush.Get()
        );
    length = swprintf_s(
        wsbuffer,
        bufferLength,
        L"Levels Completed %d\nTotal Points %d\nTotal Shots %d",
        maxLevel,
        hitCount,
        shotCount
        );
    m_bodyString = ref new Platform::String(wsbuffer, length);
    d2dContext->DrawText(
        m_bodyString->Data(),
        m_bodyString->Length(),
        m_textFormatBody.Get(),
        m_bodyRectangle,
        m_textBrush.Get()
        );

    // We ignore D2DERR_RECREATE_TARGET here. This error indicates that the device
    // is lost. It will be handled during the next call to Present.
    HRESULT hr = d2dContext->EndDraw();
    if (hr != D2DERR_RECREATE_TARGET)
    {
        // The D2DERR_RECREATE_TARGET indicates there has been a problem with the underlying
        // D3D device.  All subsequent rendering will be ignored until the device is recreated.
        // This error will be propagated and the appropriate D3D error will be returned from the
        // swapchain->Present(...) call.   At that point, the sample will recreate the device
        // and all associated resources.  As a result, the D2DERR_RECREATE_TARGET doesn't
        // need to be handled here.
        DX::ThrowIfFailed(hr);
    }
}
```

Direct2D 장치 컨텍스트를 사용 하는 합니다 **GameInfoOverlay** 초기화 하는 개체,이 메서드는 검은색 배경 브러시를 사용 하 여를 사용 하 여 제목 및 본문 사각형을 채웁니다. 이 메서드는 흰색 텍스트 브러시를 사용하여 "High Score" 문자열에 대한 텍스트를 제목 사각형에 그리고 게임 상태 업데이트 정보를 포함하는 문자열을 본문 사각형에 그립니다.


작업 사각형에 대 한 후속 호출에 의해 업데이트 됩니다 [ **GameInfoOverlay::SetAction** ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L522-L564) 에서 메서드를 **GameMain** 필요한 게임 상태 정보를 제공 하는 개체 하 여 **GameInfoOverlay::SetAction** "계속 하려면 탭" 같은 플레이어에 게 적합 한 메시지를 확인 하려면.

지정 된 모든 상태에 대 한 오버레이 선택 합니다 [ **GameMain::SetGameInfoOverlay** ](https://github.com/Microsoft/Windows-universal-samples/blob/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/Simple3DGameXaml/cpp/GameMain.cpp#L606-L661) 다음과 같이 메서드:

```cpp
void GameMain::SetGameInfoOverlay(GameInfoOverlayState state)
{
    m_gameInfoOverlayState = state;
    switch (state)
    {
    case GameInfoOverlayState::Loading:
        m_uiControl->SetGameLoading();
        break;

    case GameInfoOverlayState::GameStats:
        m_uiControl->SetGameStats(
            m_game->HighScore().levelCompleted + 1,
            m_game->HighScore().totalHits,
            m_game->HighScore().totalShots
            );
        break;

    case GameInfoOverlayState::LevelStart:
        m_uiControl->SetLevelStart(
            m_game->LevelCompleted() + 1,
            m_game->CurrentLevel()->Objective(),
            m_game->CurrentLevel()->TimeLimit(),
            m_game->BonusTime()
            );
        break;

    case GameInfoOverlayState::GameOverCompleted:
        m_uiControl->SetGameOver(
            true,
            m_game->LevelCompleted() + 1,
            m_game->TotalHits(),
            m_game->TotalShots(),
            m_game->HighScore().totalHits
            );
        break;

    case GameInfoOverlayState::GameOverExpired:
        m_uiControl->SetGameOver(
            false,
            m_game->LevelCompleted(),
            m_game->TotalHits(),
            m_game->TotalShots(),
            m_game->HighScore().totalHits
            );
        break;

    case GameInfoOverlayState::Pause:
        m_uiControl->SetPause(
            m_game->LevelCompleted() + 1,
            m_game->TotalHits(),
            m_game->TotalShots(),
            m_game->TimeRemaining()
            );
        break;
    }
}
```

게임 플레이어 게임 상태를 기준으로 텍스트 정보를 알릴 수 있도록 설정 되었습니다 및에서는 게임 전체에 표시 되는 전환의 수입니다.

### <a name="next-steps"></a>다음 단계

다음 항목인 [컨트롤 추가](tutorial--adding-controls.md)에서는 플레이어가 게임 샘플과 상호 작용하는 방법 및 입력을 통해 게임 상태를 변경하는 방법에 대해 살펴봅니다.



 




