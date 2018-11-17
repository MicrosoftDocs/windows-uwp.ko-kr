---
author: abbycar
title: 사용자 인터페이스 추가
description: DirectX UWP 게임을 2D 사용자 인터페이스 오버레이 추가 하는 방법을 알아봅니다.
ms.assetid: fa40173e-6cde-b71b-e307-db90f0388485
ms.author: abigailc
ms.date: 10/24/2017
ms.topic: article
keywords: Windows 10, uwp, 게임, 사용자 인터페이스, directx
ms.localizationpriority: medium
ms.openlocfilehash: 9962cc9043bd650390721715ca73b2e85a219c25
ms.sourcegitcommit: 3257416aebb5a7b1515e107866806f8bd57845a8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/17/2018
ms.locfileid: "7170931"
---
# <a name="add-a-user-interface"></a>사용자 인터페이스 추가


이제 게임에는 3D 시각적 개체 위치에 게임 플레이어에 게 게임 상태에 대 한 피드백을 제공할 수 있도록 2D 일부 요소를 추가에 집중 하는 시간입니다. 이 간단한 메뉴 옵션을 추가 하 여 수행할 수 및 주의 표시 구성 요소 위에 3d 그래픽 파이프라인 출력 합니다.

>[!Note]
>이 샘플의 최신 게임 코드를 다운로드하지 않은 경우 [Direct3D 게임 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Simple3DGameDX)로 이동합니다. 이 샘플은 UWP 기능 샘플의 큰 컬렉션의 일부입니다. 샘플을 다운로드하는 방법에 대한 지침은 [GitHub에서 UWP 샘플 가져오기](https://docs.microsoft.com/windows/uwp/get-started/get-uwp-app-samples)를 참조하세요.

## <a name="objective"></a>목표

Direct2D를 사용 하 여 포함 하는 UWP DirectX 게임에 다양 한 사용자 인터페이스 그래픽 및 동작을 추가 합니다.
- [이동-보기 컨트롤러](tutorial--adding-controls.md) 경계 사각형을 포함 하는 주의 표시
- 게임 상태에 맞는 메뉴


## <a name="the-user-interface-overlay"></a>사용자 인터페이스 오버레이


DirectX 게임에서 텍스트 및 사용자 인터페이스 요소를 표시 하는 방법은 여러 가지가, 동안 하겠습니다 포커스 [Direct2D](https://msdn.microsoft.com/library/windows/apps/dd370990.aspx)를 사용 합니다. 또한 사용 하겠습니다 [DirectWrite](https://msdn.microsoft.com/library/windows/desktop/dd368038) 텍스트 요소에 대 한 합니다.


Direct2D는 픽셀 기반 기본 그래픽 및 효과 그리는 데 사용 되는 2D 그리기 Api 집합입니다. Direct2D를 사용 하 여 시작 단계 때 간단 하 게 유지 하는 것이 좋습니다. 복잡한 레이아웃 및 인터페이스 동작에는 시간과 계획이 필요합니다. 시뮬레이션 및 전략 게임 처럼는 복잡 한 사용자 인터페이스가 필요한 경우에 대신 XAML을 사용 하는 것이 좋습니다.

> [!NOTE]
> UWP DirectX 게임에서 XAML 사용 하 여 사용자 인터페이스를 개발 하는 방법에 대 한 정보를 [게임 샘플 확장](tutorial-resources.md)을 참조 하세요.

Direct2D는 사용자 인터페이스 또는 HTML 및 XAML 같은 레이아웃에 대 한 설계 되지 않습니다. 목록, 상자나 단추 같은 사용자 인터페이스 구성 요소를 제공 하지 않습니다. 또한 div, 테이블, 그리드 같은 레이아웃 구성 요소도 제공 하지 않습니다.


이 게임 샘플에 대 한 두 주요 UI 구성 했습니다.
1. 점수 및 게임에서 컨트롤에 대 한 주의 표시 합니다.
2. 게임 상태 텍스트 및 일시 중단 정보 등의 옵션을 표시 하는 데 오버레이 및 수준 시작 옵션입니다.

### <a name="using-direct2d-for-a-heads-up-display"></a>주의 표시에 Direct2D 사용

다음 이미지는 샘플에 대 한 게임 내 주의 표시를 보여 줍니다. 이 단순 하 고 깔 끔 플레이어가 3D 세계 탐색 및 슈팅 대상에 집중할 수 있습니다. 좋은 인터페이스나 주의 표시 해야 기능을의 플레이어를 처리 하 고 게임에서 이벤트에 반응 하지 복잡해 집니다.

![게임 오버레이의 스크린샷](images/simple-dx-game-ui-overlay.png)

오버레이 다음과 같은 기본 이루어져 있습니다.
- 플레이어의 오른쪽 위 모서리에 있는 [**DirectWrite**](https://msdn.microsoft.com/en-us/library/windows/desktop/dd368038) 텍스트 
    - 성공적인 적중 횟수
    - 사격 플레이어의 수
    - 레벨의 남은 시간
    - 현재 레벨 번호 
- 교차 십자 표시 하는 데 사용 하는 선 세그먼트 두
- [이동-보기 컨트롤러](tutorial--adding-controls.md) 암묵적 경계에 대 한 아래 모서리에 두 개의 사각형입니다. 


오버레이의 게임 내 주의 표시 상태 [**GameHud**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameHud.h) 클래스의 [**GameHud::Render**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameHud.cpp#L234-L358) 메서드에서 그려집니다. 이 메서드 내에서 UI를 나타내는 Direct2D 오버레이 총 수, 시간, 나머지 및 수준 번호를 반영 하도록 업데이트 됩니다.

게임 초기화 된 경우 추가 `TotalHits()`, `TotalShots()`, 및 `TimeRemaining()` [**swprintf_s**](https://docs.microsoft.com/cpp/c-runtime-library/reference/sprintf-s-sprintf-s-l-swprintf-s-swprintf-s-l) 에 버퍼 및 인쇄 형식을 지정 합니다. [**DrawText**](https://msdn.microsoft.com/en-us/library/windows/desktop/dd742848) 메서드를 사용 하 여이 그릴 수 있습니다. 수행 동일한 현재 수준 표시기에 대 한 그리기 ➀과 같은 완료 되지 않은 수준 표시 하도록 빈 숫자 및 ➊ 채워진된 번호를 특정 레벨 완료 되었음을 표시 합니다.


다음 코드 조각은 **GameHud::Render** 메서드 프로세스에 대 한 안내 
- 사용 하 여 비트맵 만들기 [* * ID2D1RenderTarget::DrawBitmap * *](https://msdn.microsoft.com/en-us/library/windows/desktop/dd371880)
- [ **D2D1::RectF** 를 사용 하 여 사각형에 단면화 UI 영역 끄기](https://msdn.microsoft.com/en-us/library/windows/desktop/dd368184)
- **DrawText** 텍스트 요소를 사용 하 여

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

메서드를 손상 시 키 지 아래쪽에 [**GameHud::Render**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameHud.cpp#L320-L358) 메서드 그립니다가이 미디어는 이동 및 실행 사각형 [**ID2D1RenderTarget::DrawRectangle**](https://msdn.microsoft.com/library/windows/desktop/dd371902) [**ID2D1RenderTarget::DrawLine**](https://msdn.microsoft.com/library/windows/desktop/dd371895)를 두 번 호출을 사용 하 여 십자 기호를 사용 하 여 합니다.

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

**GameHud::Render** 메서드에서 게임 창의 논리적 크기 저장 하 고 `windowBounds` 변수입니다. 이 사용 하는 [`GetLogicalSize`](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Common/DeviceResources.h#L41) **DeviceResources** 클래스의 메서드. 
```cpp
auto windowBounds = m_deviceResources->GetLogicalSize();
```

 게임 창 크기 가져오기 UI 프로그래밍 필수적입니다. 창 크기 DIP 인치의 1/96로 정의 되어 있는 Dip (디바이스 독립적 픽셀) 라는 측정에 지정 됩니다. Direct2D 조정 실제 픽셀에 맞춰 그리기 단위의 드로잉 발생할 때 Windows에 대 한 인치당 도트 수 (DPI) 설정 사용 하 여이 작업을 수행 합니다. 마찬가지로 [**DirectWrite**](https://msdn.microsoft.com/en-us/library/windows/desktop/dd368038)를 사용 하 여 텍스트를 그리는 경우 사용자 지정 글꼴 크기에 대 한 포인트가 아닌 Dip 합니다. DIP는 부동 소수점 수로 표현됩니다.

 

### <a name="displaying-game-state-info"></a>게임 상태 정보 표시

주의 표시 외에도 게임 샘플에는 6 가지 게임 상태를 나타내는 오버레이가 있습니다. 모든 상태 읽기 플레이어에 대 한 텍스트가 포함 된 큰 검은색 사각형 기본을 기능입니다. 되지 않기 때문에 현재 이러한 상태에서 이동-보기 컨트롤러 사각형 및 십자 기호는 그려집니다 되지 않습니다.

오버레이 게임의 상태에 부합 되도록 표시할 텍스트를 전환 하는 [**GameInfoOverlay**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.h) 클래스를 사용 하 여 만들어집니다.

![상태 및 오버레이의 동작](images/simple-dx-game-ui-finaloverlay.png)

오버레이 두 섹션으로 나눌: **상태** 와 **동작**합니다. **상태** 섹션 **제목** 및 **본문** 사각형으로 나눌 추가 됩니다. **작업** 섹션에 하나의 사각형이 있습니다. 각 사각형에 다른 목적을 가집니다.

-   `titleRectangle` 제목 텍스트를 포함합니다.
-   `bodyRectangle` 본문 텍스트를 포함합니다.
-   `actionRectangle` 플레이어 특정 작업을 수행 하도록 알리는 텍스트가 표시 됩니다.

게임에 설정할 수 있는 6 가지 상태가 있습니다. 전달 오버레이 **상태** 부분을 사용 하 여 게임의 상태입니다. **상태** 사각형은 다양 한 다음과 같은 상태에 해당 하는 메서드를 사용 하 여 업데이트 됩니다.

- 불러오는 중
- 초기 시작/최고 점수 통계
- 레벨 시작
- 게임이 일시 중지
- 게임 종료
- 게임을 성공


오버레이의 **작업** 부분 알림 텍스트를 다음 중 하나로 설정할 수 있도록 [**GameInfoOverlay::SetAction**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L522-L564) 메서드를 사용 하 여 업데이트 됩니다.
- "탭... 다시 재생"
- "로드 수준, 잠시 기다려 주십시오."
- "계속 하려면 탭"
- 없음

> [!NOTE]
> 이러한 메서드를 모두 설명 된 [게임 상태를 나타내는](#representing-game-state) 섹션에 추가 합니다.

게임, **상태** 및 **작업** 섹션에서 진행 되는 기능에 따라 텍스트 필드 조정 됩니다.
초기화 하 고 이러한 여섯 개의 상태에 대 한 오버레이 그리는 방법에 대해 살펴보겠습니다.

### <a name="initializing-and-drawing-the-overlay"></a>오버레이 초기화 및 그리기

**6 개의 상태는** 몇 가지 공통, 리소스 및 메서드가 매우 유사 필요 합니다.
    - 모두 검은색 사각형 화면 중앙에 해당 배경으로 사용합니다.
    - 표시 된 텍스트는 **제목이** 나 **본문** 텍스트입니다.
    - 텍스트는 맑은 고딕 글꼴을 사용 하 여 및 뒤로 사각형 위에 그려집니다. 


게임 샘플에 오버레이 만들 때 고려해 야 하는 네 가지 메서드가 있습니다.
 

#### <a name="gameinfooverlaygameinfooverlay"></a>GameInfoOverlay::GameInfoOverlay
[**GameInfoOverlay::GameInfoOverlay**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L30-L78) 생성자에 플레이어에 게 정보를 표시 하는 데 사용할 비트맵 표면 유지 오버레이 초기화 합니다. 생성자는 오버레이 개체 자체를 그릴 수 있는 [**ID2D1DeviceContext**](https://msdn.microsoft.com/library/windows/desktop/hh404479) 만들기를 사용 하는 것에 전달 된 [**ID2D1Device**](https://msdn.microsoft.com/library/windows/desktop/hh404478) 개체에서 팩터리를 가져옵니다. [IDWriteFactory::CreateTextFormat](https://msdn.microsoft.com/en-us/library/windows/desktop/dd368203) 


#### <a name="gameinfooverlaycreatedevicedependentresources"></a>Gameinfooverlay:: Createdevicedependentresources
[**Gameinfooverlay:: Createdevicedependentresources**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L82-L104) 우리의 방법은 만드는 브러시는 텍스트를 그리는 데 사용 됩니다. 이렇게 하려면 만들 수 있는 [**ID2D1DeviceContext2**](https://msdn.microsoft.com/en-us/library/windows/desktop/dn890789) 개체를 가져올 것과 망 잉크 및 그라데이션 등의 기능이 더하기 기 하 도형을 그리기 렌더링 합니다. 다음은 일련의 색된 브러시 [**ID2D1SolidColorBrush**](https://msdn.microsoft.com/en-us/library/windows/desktop/dd372207) 를 사용 하 여 folling UI 요소를 만듭니다.
- 사각형 배경에 검정색 브러시
- 상태 텍스트에 대 한 흰색 브러시
- 텍스트에 대 한 주황색 브러시

#### <a name="deviceresourcessetdpi"></a>DeviceResources::SetDpi
[**DeviceResources::SetDpi**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Common/DeviceResources.cpp#L514-L527) 메서드 창의 인치당 도트 수를 설정합니다. DPI가 변경 되 고 되어야 할 때이 메서드가 호출 재조정할 게임 창 크기를 조정할 때 발생 합니다. DPI를 업데이트 한 후이 메서드는 또한 창 크기를 조정할 때마다 필요한 리소스를 다시 생성 되도록[**deviceresources:: Createwindowsizedependentresources**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Common/DeviceResources.cpp#L214-L487) 호출 합니다.


#### <a name="gameinfooverlaycreatewindowssizedependentresources"></a>GameInfoOverlay::CreateWindowsSizeDependentResources
[**GameInfoOverlay::CreateWindowsSizeDependentResources**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L108-L225) 방법은 우리의 모든 그리기 이루어집니다. 다음은 메서드의 단계 개요입니다.
- 세 개의 사각형 섹션 **제목**, **본문**및 **알림** 텍스트에 대 한 UI 텍스트 해제 하도록 생성 됩니다.
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

- 비트맵 명명 된 만들어집니다 `m_levelBitmap`, 현재 DPI **CreateBitmap**를 사용 하 여 계정 고려 합니다.
- `m_levelBitmap` 우리의 2D 렌더링 대상 [**ID2D1DeviceContext::SetTarget**](https://msdn.microsoft.com/en-us/library/windows/desktop/hh404533)를 사용 하 여 설정 됩니다.
- 만든 모든 픽셀을 사용 하 여 비트맵 지워집니다 [**ID2D1RenderTarget::Clear**](https://msdn.microsoft.com/en-us/library/windows/desktop/dd371772)를 사용 하 여 검은색입니다.
- [**ID2D1RenderTarget::BeginDraw**](https://msdn.microsoft.com/en-us/library/windows/desktop/dd371768) 그리기를 시작 하 라고 합니다. 
- **DrawText** 에 저장 된 텍스트를 그리는 호출 됩니다 `m_titleString`, `m_bodyString`, 및 `m_actionString` 에서 해당 **ID2D1SolidColorBrush**를 사용 하 여 적정 사각형입니다.
- 모든 그리기 작업을 중지 하려면 [**ID2D1RenderTarget::EndDraw**](ID2D1RenderTarget::EndDraw) 이라고 `m_levelBitmap`.
- **CreateBitmap** 라는 사용 하 여 다른 비트맵을 만든 `m_tooSmallBitmap` 디스플레이 구성은 게임에 비해 너무 작은 경우에 표시 대체로 사용 하도록 합니다.
- 그리기 위한 프로세스를 반복 합니다. `m_levelBitmap` 에 대 한 `m_tooSmallBitmap`, 문자열 그리기이 이번 `Paused` 본문에 있습니다.




이제 우리 6 개의 오버레이 상태 텍스트에 맞게 6 개의 메서드는 필요한 모든!

### <a name="representing-game-state"></a>게임 상태 표시


각각의 6 개의 오버레이 상태는 게임에 해당 하는 메서드가 **GameInfoOverlay** 개체에 있습니다. 이러한 메서드는 오버레이의 변형을 그려 게임 자체에 대한 명시적 정보를 플레이어에게 전달합니다. 이 통신 **제목** 및 **본문** 문자열로 표현 됩니다. 샘플 리소스 및 레이아웃을 초기화할 때이 정보에 대 한 및 [**gameinfooverlay:: Createdevicedependentresources**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L82-L104) 메서드를 사용 하 여 이미 구성 이기 때문에 오버레이 상태 관련 문자열만 제공 해야 합니다.

오버레이의 **상태** 부분이 다음 방법 중 하나를 호출 하 여 설정 됩니다.

게임 상태 | 상태 방법을 설정 | 상태 필드
:----- | :------- | :---------
불러오는 중 | [GameInfoOverlay::SetGameLoading](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L254-L306) |**제목**</br>리소스 로드 </br>**Body**</br> 점진적으로 인쇄 "." 로드 활동은 아닙니다.
초기 시작/최고 점수 통계 | [GameInfoOverlay::SetGameStats](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L310-L354) |**제목**</br>최고 점수</br> **Body**</br> 레벨 완료 # </br>총 점수 #</br>총 샷 #
레벨 시작 | [GameInfoOverlay::SetLevelStart](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L413-L471) |**제목**</br>수준 #</br>**Body**</br>수준 목표 설명 합니다.
게임이 일시 중지 | [GameInfoOverlay::SetPause](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L475-L502) |**제목**</br>게임이 일시 중지</br>**Body**</br>없음
게임 종료 | [GameInfoOverlay::SetGameOver](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L358-L409) |**제목**</br>게임 오버</br> **Body**</br> 레벨 완료 # </br>총 점수 #</br>총 샷 #</br>레벨 완료 #</br>높은 점수 #
게임을 성공 | [GameInfoOverlay::SetGameOver](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L358-L409) |**제목**</br>성공!</br> **Body**</br> 레벨 완료 # </br>총 점수 #</br>총 샷 #</br>레벨 완료 #</br>높은 점수 #




[**GameInfoOverlay::CreateWindowSizeDependentResources**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L117-L134) 메서드를 사용 하 여 샘플 오버레이의 특정 영역에 해당 하는 세 개의 사각형 영역을 선언 합니다.



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

**GameInfoOverlay** 개체를 초기화 하는 Direct2D 디바이스 컨텍스트를 사용 하 여이 메서드는 배경 브러시를 사용 하 여 검은색으로 제목 및 본문 사각형을 채웁니다. 이 메서드는 흰색 텍스트 브러시를 사용하여 "High Score" 문자열에 대한 텍스트를 제목 사각형에 그리고 게임 상태 업데이트 정보를 포함하는 문자열을 본문 사각형에 그립니다.


작업 사각형 [**GameInfoOverlay::SetAction**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L522-L564) **GameInfoOverlay::SetAction** 적합 한 메시지를 결정 하는 데 필요한 게임 상태 정보를 제공 하는 **GameMain** 개체의 메서드에서 후속 호출 하 여 업데이트 되는 플레이어, 예: "계속 하려면 탭"입니다.

지정된 된 상태에 대 한 오버레이 같이 [**GameMain::SetGameInfoOverlay**](https://github.com/Microsoft/Windows-universal-samples/blob/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/Simple3DGameXaml/cpp/GameMain.cpp#L606-L661) 메서드에서 선택 됩니다.

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

이제 게임에 게임 상태에 따라 플레이어에 게 텍스트 정보를 전달 하는 방법 및 드디어 게임 전체에 표시 되는 전환 하는 방법.

### <a name="next-steps"></a>다음 단계

다음 항목인 [컨트롤 추가](tutorial--adding-controls.md)에서는 플레이어가 게임 샘플과 상호 작용하는 방법 및 입력을 통해 게임 상태를 변경하는 방법에 대해 살펴봅니다.



 




