---
title: 사용자 인터페이스 추가
description: DirectX UWP 게임에 2D 사용자 인터페이스 오버레이를 추가 하는 방법에 대해 알아봅니다.
ms.assetid: fa40173e-6cde-b71b-e307-db90f0388485
ms.date: 10/24/2017
ms.topic: article
keywords: windows 10, uwp, 게임, 사용자 인터페이스, directx
ms.localizationpriority: medium
ms.openlocfilehash: b6b59bd4f42d31e1f29cc1af298199b42cce6781
ms.sourcegitcommit: 20969781aca50738792631f4b68326f9171a3980
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/26/2020
ms.locfileid: "85409542"
---
# <a name="add-a-user-interface"></a>사용자 인터페이스 추가

> [!NOTE]
> 이 항목은 DirectX 자습서 시리즈 [를 사용 하 여 단순 유니버설 Windows 플랫폼 (UWP) 게임 만들기](tutorial--create-your-first-uwp-directx-game.md) 의 일부입니다. 해당 링크의 항목은 계열의 컨텍스트를 설정 합니다.

이제 게임에 3D 시각적 개체가 준비 되었으므로 일부 2D 요소를 추가 하는 데 집중할 수 있습니다. 그러면 게임에서 플레이어에 게임 상태에 대 한 피드백을 제공할 수 있습니다. 이 작업을 수행 하려면 3 차원 그래픽 파이프라인 출력 위에 간단한 메뉴 옵션과 준비 디스플레이 구성 요소를 추가 합니다.

>[!Note]
>이 샘플에 대 한 최신 게임 코드를 다운로드 하지 않은 경우 [Direct3D sample game](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Simple3DGameDX)로 이동 합니다. 이 샘플은 여러 UWP 기능 샘플 컬렉션의 일부입니다. 샘플을 다운로드 하는 방법에 대 한 지침은 [GitHub에서 UWP 샘플 가져오기](/windows/uwp/get-started/get-uwp-app-samples)를 참조 하세요.

## <a name="objective"></a>Objective

Direct2D를 사용 하 여 다음을 포함 하 여 UWP DirectX 게임에 많은 사용자 인터페이스 그래픽 및 동작을 추가 합니다.
- [이동-보기 컨트롤러](tutorial--adding-controls.md) boundry 사각형을 포함 한 준비 표시
- 게임 상태 메뉴


## <a name="the-user-interface-overlay"></a>사용자 인터페이스 오버레이


DirectX 게임에서 텍스트 및 사용자 인터페이스 요소를 표시 하는 방법에는 여러 가지가 있지만 [Direct2D](/windows/desktop/Direct2D/direct2d-portal)를 사용 하는 데 집중 하겠습니다. 또한 텍스트 요소에 대해 [DirectWrite](/windows/desktop/DirectWrite/direct-write-portal) 를 사용 합니다.


Direct2D는 픽셀 기반 기본 형식 및 효과를 그리는 데 사용 되는 2D 그리기 Api 집합입니다. Direct2D를 사용 하 여 시작 하는 경우 간단 하 게 유지 하는 것이 좋습니다. 복잡 한 레이아웃 및 인터페이스 동작에는 시간과 계획이 필요 합니다. 시뮬레이션 및 전략 게임에 있는 것과 같은 복잡 한 사용자 인터페이스가 게임에 필요한 경우 XAML을 대신 사용 하는 것이 좋습니다.

> [!NOTE]
> UWP DirectX 게임에서 XAML을 사용 하 여 사용자 인터페이스를 개발 하는 방법에 대 한 정보는 [샘플 게임 확장](tutorial-resources.md)을 참조 하세요.

Direct2D은 사용자 인터페이스 또는 HTML, XAML 등의 레이아웃을 위해 특별히 설계 되지 않았습니다. 목록, 상자 또는 단추와 같은 사용자 인터페이스 구성 요소는 제공 하지 않습니다. 또한 div, 테이블 또는 표와 같은 레이아웃 구성 요소는 제공 하지 않습니다.


이 샘플 게임의 경우 두 가지 주요 UI 구성 요소가 있습니다.
1. 점수 및 게임 내 컨트롤에 대 한 헤드 표시입니다.
2. 게임 상태 텍스트를 표시 하는 데 사용 되는 오버레이 및 일시 중지 정보 및 수준 시작 옵션 등의 옵션입니다.

### <a name="using-direct2d-for-a-heads-up-display"></a>Direct2D를 사용 하 여 헤드 표시

다음 이미지는 샘플의 게임 내 헤드 표시를 보여 줍니다. 이는 간단 하 고 깔끔하게 볼 수 있으므로 플레이어에서 3D 세계와 촬영 대상의 탐색에 집중할 수 있습니다. 좋은 인터페이스 또는 준비 디스플레이는 플레이어에서 게임 이벤트를 처리 하 고 대응할 수 있는 능력을 복잡 하 게 하지 않아야 합니다.

![게임 오버레이의 스크린샷](images/simple-dx-game-ui-overlay.png)

오버레이는 다음과 같은 기본 기본 형식으로 구성 됩니다.
- 플레이어에 게 알리는 텍스트를 오른쪽 위 모퉁이에 [**DirectWrite**](/windows/desktop/DirectWrite/direct-write-portal) . 
    - 성공한 적중
    - 플레이어가 만든 샷 수
    - 수준에서 남은 시간
    - 현재 수준 번호 
- 십자 머리를 형성 하는 데 사용 되는 두 개의 교차 선 세그먼트
- [이동 지점 컨트롤러](tutorial--adding-controls.md) 경계의 아래쪽 모퉁이에 두 개의 사각형이 있습니다. 


오버레이의 게임 내 헤드 표시 상태는 [**GameHud**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameHud.h) 클래스의 [**GameHud:: Render**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameHud.cpp#L234-L358) 메서드에 그려집니다. 이 메서드 내에서 UI를 나타내는 Direct2D 오버레이는 적중 횟수, 남은 시간 및 수준 번호의 변경 내용을 반영 하도록 업데이트 됩니다.

게임을 초기화 한 후에는 `TotalHits()` , 및를 `TotalShots()` `TimeRemaining()` [**swprintf_s**](/cpp/c-runtime-library/reference/sprintf-s-sprintf-s-l-swprintf-s-swprintf-s-l) 버퍼에 추가 하 고 인쇄 형식을 지정 합니다. 그런 다음 [**DrawText**](/windows/desktop/Direct2D/id2d1rendertarget-drawtext) 메서드를 사용 하 여 그릴 수 있습니다. 현재 수준 표시기에 대해 동일한 작업을 수행 하 고, ➀와 같은 완료 되지 않은 수준을 표시 하는 빈 숫자를 그리고 특정 수준이 완료 되었음을 표시 하는 ➊와 같은 값을 채웁니다.


다음 코드 조각에서는에 대 한 **GameHud:: Render** 메서드의 프로세스를 안내 합니다. 
- [* * ID2D1RenderTarget::D rawbitmap * *](/windows/desktop/api/d2d1/nf-d2d1-id2d1rendertarget-drawbitmap(id2d1bitmap_constd2d1_rect_f__float_d2d1_bitmap_interpolation_mode_constd2d1_rect_f_)) 를 사용 하 여 비트맵 만들기
- [ **D2D1:: RectF** 를 사용 하 여 UI 영역을 사각형으로 단면화 합니다.](/windows/desktop/api/dcommon/ns-dcommon-d2d_rect_f)
- **DrawText** 를 사용 하 여 텍스트 요소 만들기

```cppwinrt
void GameHud::Render(_In_ std::shared_ptr<Simple3DGame> const& game)
{
    auto d2dContext = m_deviceResources->GetD2DDeviceContext();
    auto windowBounds = m_deviceResources->GetLogicalSize();

    if (m_showTitle)
    {
        d2dContext->DrawBitmap(
            m_logoBitmap.get(),
            D2D1::RectF(
                GameUIConstants::Margin,
                GameUIConstants::Margin,
                m_logoSize.width + GameUIConstants::Margin,
                m_logoSize.height + GameUIConstants::Margin
                )
            );
        d2dContext->DrawTextLayout(
            Point2F(m_logoSize.width + 2.0f * GameUIConstants::Margin, GameUIConstants::Margin),
            m_titleHeaderLayout.get(),
            m_textBrush.get()
            );
        d2dContext->DrawTextLayout(
            Point2F(GameUIConstants::Margin, m_titleBodyVerticalOffset),
            m_titleBodyLayout.get(),
            m_textBrush.get()
            );
    }

    // Draw text for number of hits, total shots, and time remaining
    if (game != nullptr)
    {
        // This section is only used after the game state has been initialized.
        static const int bufferLength = 256;
        static wchar_t wsbuffer[bufferLength];
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
            m_textFormatBody.get(),
            D2D1::RectF(
                windowBounds.Width - GameUIConstants::HudRightOffset,
                GameUIConstants::HudTopOffset,
                windowBounds.Width,
                GameUIConstants::HudTopOffset + (GameUIConstants::HudBodyPointSize + GameUIConstants::Margin) * 3
                ),
            m_textBrush.get()
            );

        // Using the unicode characters starting at 0x2780 ( ➀ ) for the consecutive levels of the game.
        // For completed levels start with 0x278A ( ➊ ) (This is 0x2780 + 10).
        uint32_t levelCharacter[6];
        for (uint32_t i = 0; i < 6; i++)
        {
            levelCharacter[i] = 0x2780 + i + ((static_cast<uint32_t>(game->LevelCompleted()) == i) ? 10 : 0);
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
            m_textFormatBodySymbol.get(),
            D2D1::RectF(
                windowBounds.Width - GameUIConstants::HudRightOffset,
                GameUIConstants::HudTopOffset + (GameUIConstants::HudBodyPointSize + GameUIConstants::Margin) * 3 + GameUIConstants::Margin,
                windowBounds.Width,
                GameUIConstants::HudTopOffset + (GameUIConstants::HudBodyPointSize + GameUIConstants::Margin) * 4
                ),
            m_textBrush.get()
            );

        if (game->IsActivePlay())
        {
            // Draw the move and fire rectangles
            ...
            // Draw the crosshairs
            ...
        }
    }
}
```

이 방법의 [**GameHud:: Render**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameHud.cpp#L320-L358) 메서드는 [**ID2D1RenderTarget::D rawrectangle**](/windows/desktop/api/d2d1/nf-d2d1-id2d1rendertarget-drawrectangle(constd2d1_rect_f__id2d1brush_float_id2d1strokestyle))및 십자 기호를 사용 하 여 [**ID2D1RenderTarget::D rawline**](/windows/desktop/api/d2d1/nf-d2d1-id2d1rendertarget-drawline)에 대 한 두 호출을 사용 하 여 이동 및 발생 하는 사각형을 그립니다.

```cppwinrt
// Check if game is playing
if (game->IsActivePlay())
{
    // Draw a rectangle for the touch input for the move control.
    d2dContext->DrawRectangle(
        D2D1::RectF(
            0.0f,
            windowBounds.Height - GameUIConstants::TouchRectangleSize,
            GameUIConstants::TouchRectangleSize,
            windowBounds.Height
            ),
        m_textBrush.get()
        );
    // Draw a rectangle for the touch input for the fire control.
    d2dContext->DrawRectangle(
        D2D1::RectF(
            windowBounds.Width - GameUIConstants::TouchRectangleSize,
            windowBounds.Height - GameUIConstants::TouchRectangleSize,
            windowBounds.Width,
            windowBounds.Height
            ),
        m_textBrush.get()
        );

    // Draw the cross hairs
    d2dContext->DrawLine(
        D2D1::Point2F(windowBounds.Width / 2.0f - GameUIConstants::CrossHairHalfSize,
            windowBounds.Height / 2.0f),
        D2D1::Point2F(windowBounds.Width / 2.0f + GameUIConstants::CrossHairHalfSize,
            windowBounds.Height / 2.0f),
        m_textBrush.get(),
        3.0f
        );
    d2dContext->DrawLine(
        D2D1::Point2F(windowBounds.Width / 2.0f, windowBounds.Height / 2.0f -
            GameUIConstants::CrossHairHalfSize),
        D2D1::Point2F(windowBounds.Width / 2.0f, windowBounds.Height / 2.0f +
            GameUIConstants::CrossHairHalfSize),
        m_textBrush.get(),
        3.0f
        );
}
```

**GameHud:: Render** 메서드에서 게임 창의 논리적 크기를 변수에 저장 합니다 `windowBounds` . 이는 [`GetLogicalSize`](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Common/DeviceResources.h#L41) **DeviceResources** 클래스의 메서드를 사용 합니다. 

```cppwinrt
auto windowBounds = m_deviceResources->GetLogicalSize();
```

게임 창의 크기를 가져오는 것은 UI 프로그래밍에 필수적입니다. 창의 크기는 dip (장치 독립적 픽셀) 라는 단위로 지정 되며 DIP는 1/96 인치로 정의 됩니다. Direct2D는 드로잉이 발생할 때 그리기 단위의 크기를 실제 픽셀로 조정 하 고, DPI (인치당 도트 수) 설정을 사용 하 여이 작업을 수행 합니다. 마찬가지로 [**DirectWrite**](/windows/desktop/DirectWrite/direct-write-portal)를 사용 하 여 텍스트를 그리면 글꼴 크기에 대 한 점이 아니라 dip를 지정 합니다. Dip는 부동 소수점 숫자로 표현 됩니다. 

### <a name="displaying-game-state-info"></a>게임 상태 정보 표시

샘플 게임에는 헤드 표시 외에도 6 개의 게임 상태를 나타내는 오버레이가 있습니다. 모든 상태 기능은 플레이어에서 읽을 텍스트를 포함 하는 긴 검은색 사각형 기본 형식입니다. 이동 보기 컨트롤러 사각형과 십자 기호는 이러한 상태에서 활성화 되지 않기 때문에 그려지지 않습니다.

오버레이는 [**GameInfoOverlay**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.h) 클래스를 사용 하 여 생성 됩니다 .이를 통해 게임 상태를 맞추기 위해 표시 되는 텍스트를 전환할 수 있습니다.

![오버레이 상태 및 동작](images/simple-dx-game-ui-finaloverlay.png)

오버레이는 **상태** 및 **동작**의 두 섹션으로 구분 됩니다. **상태** secton **제목** 및 **본문** 사각형으로 세분화 됩니다. **작업** 섹션에는 하나의 사각형만 있습니다. 각 사각형의 용도는 서로 다릅니다.

-   `titleRectangle`제목 텍스트를 포함 합니다.
-   `bodyRectangle`본문 텍스트를 포함 합니다.
-   `actionRectangle`플레이어에 게 특정 작업을 수행 하도록 알리는 텍스트를 포함 합니다.

게임에는 6 가지 상태를 설정할 수 있습니다. 오버레이의 **상태** 부분을 사용 하 여 전달 된 게임의 상태입니다. **상태** 사각형은 다음 상태에 해당 하는 여러 메서드를 사용 하 여 업데이트 됩니다.

- 로드
- 초기 시작/높은 점수 통계
- 수준 시작
- 게임 일시 중지 됨
- 게임 종료
- 게임 승리


오버레이의 **동작** 부분은 [**GameInfoOverlay:: setaction**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L522-L564) 메서드를 사용 하 여 업데이트 되므로 작업 텍스트를 다음 중 하나로 설정할 수 있습니다.
- "다시 재생 하려면 탭 하세요 ..."
- "수준 로드 중 ... 잠시 기다려 주십시오."
- "계속 하려면 탭 ..."
- 없음

> [!NOTE]
> 이러한 방법에 대해서는 [게임 상태 표시](#representing-game-state) 섹션에서 자세히 설명 합니다.

게임의 진행 상황에 따라 **상태** 및 **동작** 섹션 텍스트 필드가 조정 됩니다.
이러한 6 가지 상태에 대 한 오버레이를 초기화 하 고 그리는 방법을 살펴보겠습니다.

### <a name="initializing-and-drawing-the-overlay"></a>오버레이 초기화 및 그리기

6 가지 **상태** 상태에는 몇 가지 공통적인 내용이 있으며, 리소스와 메서드는 매우 유사 하 게 수행 됩니다.
    - 화면 가운데에 있는 검정색 사각형을 배경으로 사용 합니다.
    - 표시 되는 텍스트는 **제목** 또는 **본문** 텍스트입니다.
    - 텍스트는 맑은 고딕 글꼴을 사용 하 고 뒤쪽 사각형 위에 그려집니다. 


샘플 게임에는 오버레이를 만들 때 재생 되는 네 가지 메서드가 있습니다.
 

#### <a name="gameinfooverlaygameinfooverlay"></a>GameInfoOverlay::GameInfoOverlay
[**GameInfoOverlay:: GameInfoOverlay**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L30-L78) 생성자는 오버레이를 초기화 하 여 플레이어에 게 정보를 표시 하는 데 사용할 비트맵 화면을 유지 합니다. 생성자는 여기에 전달 된 [**ID2D1Device**](/windows/desktop/api/d2d1_1/nn-d2d1_1-id2d1device) 개체에서 팩터리를 가져와 오버레이 개체 자체가 그릴 수 있는 [**ID2D1DeviceContext**](/windows/desktop/api/d2d1_1/nn-d2d1_1-id2d1devicecontext) 를 만드는 데 사용 합니다. [IDWriteFactory::CreateTextFormat](/windows/desktop/api/dwrite/nf-dwrite-idwritefactory-createtextformat) 


#### <a name="gameinfooverlaycreatedevicedependentresources"></a>GameInfoOverlay::CreateDeviceDependentResources
[**GameInfoOverlay:: CreateDeviceDependentResources**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L82-L104) 는 텍스트를 그리는 데 사용 되는 브러시를 만드는 데 사용 되는 메서드입니다. 이렇게 하려면 기 하 도형 만들기 및 그리기를 가능 하 게 하는 [**ID2D1DeviceContext2**](/windows/desktop/api/d2d1_3/nn-d2d1_3-id2d1devicecontext2) 개체와 잉크 및 그라데이션 메시 렌더링과 같은 기능을 얻습니다. 그런 다음 [**ID2D1SolidColorBrush**](/windows/desktop/api/d2d1/nn-d2d1-id2d1solidcolorbrush) 를 사용 하 여 folling UI 요소를 그리는 색이 지정 된 일련의 브러시를 만듭니다.
- 사각형 배경의 검정 브러시
- 상태 텍스트에 대 한 흰색 브러시
- 작업 텍스트에 대 한 주황색 브러시

#### <a name="deviceresourcessetdpi"></a>DeviceResources:: SetDpi

[**DeviceResources:: SetDpi**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Common/DeviceResources.cpp#L514-L527) 메서드는 창의 dpi를 설정 합니다. 이 메서드는 DPI가 변경 될 때 호출 되며 게임 창의 크기를 조정할 때 발생 하는 다시 조정 해야 합니다. DPI를 업데이트 한 후에도이 메서드는[**DeviceResources:: CreateWindowSizeDependentResources**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Common/DeviceResources.cpp#L214-L487) 를 호출 하 여 창의 크기를 조정할 때마다 필요한 리소스가 다시 만들어지도록 합니다.

#### <a name="gameinfooverlaycreatewindowssizedependentresources"></a>GameInfoOverlay::CreateWindowsSizeDependentResources

[**GameInfoOverlay:: CreateWindowsSizeDependentResources**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L108-L225) 메서드는 모든 그리기가 발생 하는 위치입니다. 다음은 메서드 단계에 대 한 개요입니다.
- **제목**, **본문**및 **동작** 텍스트에 대 한 UI 텍스트에서 섹션에 세 개의 사각형이 생성 됩니다.
    ```cppwinrt 
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

- 라는 비트맵이 생성 되 `m_levelBitmap` 고 **createbitmap**을 사용 하 여 현재 DPI를 고려 합니다.
- `m_levelBitmap`는 [**ID2D1DeviceContext:: SetTarget**](/windows/desktop/api/d2d1_1/nf-d2d1_1-id2d1devicecontext-settarget)를 사용 하 여 2d 렌더링 대상으로 설정 됩니다.
- 비트맵은 [**ID2D1RenderTarget:: Clear**](/windows/desktop/api/d2d1/nf-d2d1-id2d1rendertarget-clear)를 사용 하 여 모든 픽셀이 검정색으로 지워집니다.
- [**ID2D1RenderTarget:: BeginDraw**](/windows/desktop/api/d2d1/nf-d2d1-id2d1rendertarget-begindraw) 를 호출 하 여 그리기를 시작 합니다. 
- **DrawText** 는 `m_titleString` `m_bodyString` `m_actionString` 해당 **ID2D1SolidColorBrush**를 사용 하 여, 및에 저장 된 텍스트를 approperiate 사각형에 그리도록 호출 합니다.
- [**ID2D1RenderTarget:: enddraw**](ID2D1RenderTarget::EndDraw) 은에서 모든 그리기 작업을 중지 하기 위해 호출 됩니다 `m_levelBitmap` .
- 다른 비트맵은로 사용 하기 위해 라는 **Createbitmap** 을 사용 하 여 생성 됩니다 .이는 `m_tooSmallBitmap` 디스플레이 구성이 게임에 너무 작은 경우에만 표시 됩니다.
- 에 대 한 그리기 프로세스를 반복 합니다 `m_levelBitmap` `m_tooSmallBitmap` . 이번에는 `Paused` 본문의 문자열만 그립니다.




이제 여섯 가지 방법으로 6 가지 오버레이 상태 텍스트를 채울 수 있습니다.

### <a name="representing-game-state"></a>게임 상태 표시


게임의 각 여섯 가지 오버레이 상태는 **GameInfoOverlay** 개체에 해당 하는 메서드가 있습니다. 이러한 메서드는 게임 자체에 대해 플레이어에 게 명시적 정보를 전달 하기 위해 오버레이 변형을 그립니다. 이 통신은 **제목과** **본문** 문자열로 표시 됩니다. 이 샘플은 초기화 될 때이 정보에 대 한 리소스 및 레이아웃을 이미 구성 했으므로 [**GameInfoOverlay:: CreateDeviceDependentResources**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L82-L104) 메서드를 사용 하 여 오버레이 상태 관련 문자열만 제공 하면 됩니다.

오버레이의 **상태** 부분은 다음 방법 중 하나를 호출 하 여 설정 됩니다.

게임 상태 | 상태 집합 방법 | 상태 필드
:----- | :------- | :---------
로드 | [GameInfoOverlay::SetGameLoading](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L254-L306) |**제목**</br>리소스 로드 중 </br>**본문**</br> 로드 작업을 의미 하는 "."를 증분 방식으로 출력 합니다.
초기 시작/높은 점수 통계 | [GameInfoOverlay::SetGameStats](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L310-L354) |**제목**</br>높은 점수</br> **본문**</br> 완료 된 수준 # </br>총 점수 #</br>총 샷 #
수준 시작 | [GameInfoOverlay:: SetLevelStart](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L413-L471) |**제목**</br>저수준 #</br>**본문**</br>수준 목표 설명입니다.
게임 일시 중지 됨 | [GameInfoOverlay:: SetPause](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L475-L502) |**제목**</br>게임 일시 중지 됨</br>**본문**</br>없음
게임 종료 | [GameInfoOverlay::SetGameOver](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L358-L409) |**제목**</br>게임 오버</br> **본문**</br> 완료 된 수준 # </br>총 점수 #</br>총 샷 #</br>완료 된 수준 #</br>높은 점수 #
게임 승리 | [GameInfoOverlay::SetGameOver](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L358-L409) |**제목**</br>이겼습니다!</br> **본문**</br> 완료 된 수준 # </br>총 점수 #</br>총 샷 #</br>완료 된 수준 #</br>높은 점수 #

[**GameInfoOverlay:: CreateWindowSizeDependentResources**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L117-L134) 메서드를 사용 하 여 샘플은 오버레이의 특정 영역에 해당 하는 세 개의 사각형 영역을 선언 했습니다.

이러한 영역을 염두에 두면 상태별 **GameInfoOverlay:: SetGameStats**메서드 중 하나를 살펴보고 오버레이가 그려지는 방식을 확인 합니다.

```cppwinrt
void GameInfoOverlay::SetGameStats(int maxLevel, int hitCount, int shotCount)
{
    int length;

    auto d2dContext = m_deviceResources->GetD2DDeviceContext();

    d2dContext->SetTarget(m_levelBitmap.get());
    d2dContext->BeginDraw();
    d2dContext->SetTransform(D2D1::Matrix3x2F::Identity());
    d2dContext->FillRectangle(&m_titleRectangle, m_backgroundBrush.get());
    d2dContext->FillRectangle(&m_bodyRectangle, m_backgroundBrush.get());
    m_titleString = L"High Score";

    d2dContext->DrawText(
        m_titleString.c_str(),
        m_titleString.size(),
        m_textFormatTitle.get(),
        m_titleRectangle,
        m_textBrush.get()
        );
    length = swprintf_s(
        wsbuffer,
        bufferLength,
        L"Levels Completed %d\nTotal Points %d\nTotal Shots %d",
        maxLevel,
        hitCount,
        shotCount
        );
    m_bodyString = std::wstring(wsbuffer, length);
    d2dContext->DrawText(
        m_bodyString.c_str(),
        m_bodyString.size(),
        m_textFormatBody.get(),
        m_bodyRectangle,
        m_textBrush.get()
        );

    // We ignore D2DERR_RECREATE_TARGET here. This error indicates that the device
    // is lost. It will be handled during the next call to Present.
    HRESULT hr = d2dContext->EndDraw();
    if (hr != D2DERR_RECREATE_TARGET)
    {
        // The D2DERR_RECREATE_TARGET indicates there has been a problem with the underlying
        // D3D device. All subsequent rendering will be ignored until the device is recreated.
        // This error will be propagated and the appropriate D3D error will be returned from the
        // swapchain->Present(...) call. At that point, the sample will recreate the device
        // and all associated resources. As a result, the D2DERR_RECREATE_TARGET doesn't
        // need to be handled here.
        winrt::check_hresult(hr);
    }
}
```

이 메서드는 **GameInfoOverlay** 개체가 초기화 한 Direct2D 장치 컨텍스트를 사용 하 여 배경 브러시를 사용 하 여 제목 및 본문 사각형을 검은색으로 채웁니다. "높은 점수" 문자열의 텍스트를 제목 사각형에 그리고 업데이트 게임 상태 정보를 포함 하는 문자열을 흰색 텍스트 브러시를 사용 하 여 본문 사각형에 그립니다.


작업 사각형은 **GameMain** 개체의 메서드에서 [**GameInfoOverlay:: setaction**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L522-L564) 에 대 한 후속 호출로 업데이트 됩니다 .이 작업은 **GameInfoOverlay:: setaction** 에서 플레이어에 게 올바른 메시지를 결정 하는 데 필요한 게임 상태 정보를 제공 합니다 (예: "계속 하려면 탭").

지정 된 상태의 오버레이는 [**GameMain:: SetGameInfoOverlay**](https://github.com/Microsoft/Windows-universal-samples/blob/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/Simple3DGameXaml/cpp/GameMain.cpp#L606-L661) 메서드에서 다음과 같이 선택 됩니다.

```cppwinrt
void GameMain::SetGameInfoOverlay(GameInfoOverlayState state)
{
    m_gameInfoOverlayState = state;
    switch (state)
    {
    case GameInfoOverlayState::Loading:
        m_uiControl->SetGameLoading(m_loadingCount);
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

이제 게임 상태에 따라 플레이어에 게 텍스트 정보를 전달 하는 방법을 제공 하 고 게임 전체에 표시 되는 항목을 전환 하는 방법을 제공 합니다.

### <a name="next-steps"></a>다음 단계

다음 항목인 [컨트롤 추가](tutorial--adding-controls.md)에서는 플레이어가 샘플 게임과 상호 작용 하는 방법 및 입력이 게임 상태를 변경 하는 방법을 살펴봅니다.
