---
author: mtoepke
title: "Marble Maze 샘플에 입력 및 대화형 작업 추가"
description: "UWP(유니버설 Windows 플랫폼) 앱 게임은 데스크톱 컴퓨터, 노트북, 태블릿 등의 다양한 디바이스에서 실행됩니다."
ms.assetid: b946bf62-c0ca-f9ec-1a87-8195b89a5ab4
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: ddaa13c6bf7d1bcf5a01d7525389a893a077f4f4

---

# Marble Maze 샘플에 입력 및 대화형 작업 추가


\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]


UWP(유니버설 Windows 플랫폼) 앱 게임은 데스크톱 컴퓨터, 노트북, 태블릿 등의 다양한 장치에서 실행됩니다. 한 장치에 다양한 입력 및 제어 메커니즘이 있을 수 있습니다. 게임이 고객의 광범위한 기본 설정과 기능을 수용할 수 있도록 여러 입력 장치를 지원합니다. 이 문서에서는 입력 장치 작업을 할 때 고려할 몇 가지 주요 사항을 설명하고 Marble Maze가 이러한 사례를 적용하는 방법을 보여 줍니다.

> **참고** 이 문서에 해당하는 샘플 코드는 [DirectX Marble Maze 게임 샘플](http://go.microsoft.com/fwlink/?LinkId=624011)에 있습니다.

 
게임에서 입력 작업을 하는 경우에 대해 논의하는 주요 사항은 다음과 같습니다.

-   가능한 경우 게임이 고객의 광범위한 기본 설정과 기능을 수용할 수 있도록 여러 입력 장치를 지원합니다. 게임 컨트롤러 및 센서 사용은 선택 사항이지만 플레이어 환경을 향상시키기 위해 사용하는 것이 좋습니다. 이러한 입력 장치를 더 쉽게 통합할 수 있도록 게임 컨트롤러 및 센서 API를 디자인했습니다.
-   터치를 초기화하려면 포인터가 활성화, 해제 및 이동되는 경우 등의 창 이벤트를 등록해야 합니다. 가속도계를 초기화하려면 응용 프로그램을 초기화할 때 [**Windows::Devices::Sensors::Accelerometer**](https://msdn.microsoft.com/library/windows/apps/br225687) 개체를 만듭니다. Xbox 360 컨트롤러는 초기화가 필요하지 않습니다.
-   싱글 플레이어 게임의 경우 가능한 모든 Xbox 360 컨트롤러의 입력을 결합할지 여부를 고려합니다. 이렇게 하면 어떤 입력이 어떤 컨트롤러에서 제공되었는지 추적할 필요가 없습니다.
-   입력 장치를 처리하기 전에 Windows 이벤트를 처리합니다.
-   Xbox 360 컨트롤러와 가속도계는 폴링을 지원합니다. 즉, 필요할 때 데이터를 폴링할 수 있습니다. 터치의 경우 입력 처리 코드에서 사용할 수 있는 데이터 구조에 터치 이벤트를 기록합니다.
-   입력 값을 공통 형식으로 정규화할지 여부를 고려합니다. 이렇게 하면 물리학 시뮬레이션 등 게임의 다른 구성 요소에서 입력이 해석되는 방식을 간소화할 수 있으며, 각기 다른 화면 해상도에서 작동하는 게임을 더 쉽게 작성할 수 있습니다.

## Marble Maze에서 지원하는 입력 장치


Marble Maze는 Xbox 360 일반 컨트롤러 장치, 마우스 및 터치를 통한 메뉴 항목 선택과 Xbox 360 컨트롤러, 마우스, 터치 및 가속도계를 통한 게임 플레이 제어를 지원합니다. Marble Maze는 XInput API를 사용하여 컨트롤러에서 입력을 폴링합니다. 터치를 사용하면 응용 프로그램이 손가락 입력을 추적하고 응답할 수 있습니다. 가속도계는 x, y, z축을 따라 적용되는 힘을 측정하는 센서입니다. Windows 런타임을 사용하면 가속도계 장치의 현재 상태를 폴링하고 Windows 런타임 이벤트 처리 메커니즘을 통해 터치 이벤트를 받을 수 있습니다.

> **참고** 이 문서에서는 터치를 사용하여 터치식 및 마우스 입력을 둘 다 가리키고, 포인터를 사용하여 포인터 이벤트를 사용하는 모든 디바이스를 가리킵니다. 터치 및 마우스는 표준 포인터 이벤트를 사용하므로 두 장치를 중 하나를 사용하여 메뉴 항목을 선택하고 게임 플레이를 제어할 수 있습니다.

 

> **참고** 패키지 매니페스트는 디바이스를 회전하여 구슬을 굴릴 때 방향이 변경되지 않도록 게임에 대해 지원되는 방향으로 가로를 설정합니다.

 

## 입력 장치 초기화


Xbox 360 컨트롤러는 초기화가 필요하지 않습니다. 터치를 초기화하려면 포인터가 활성화(예: 사용자가 마우스 단추를 누르거나 화면 터치), 해제 및 이동되는 경우 등의 창 이벤트를 등록해야 합니다. 가속도계를 초기화하려면 응용 프로그램을 초기화할 때 [**Windows::Devices::Sensors::Accelerometer**](https://msdn.microsoft.com/library/windows/apps/br225687) 개체를 만들어야 합니다.

다음 예제에서는 **DirectXPage** 생성자가 [**Windows::UI::Core::CoreIndependentInputSource::PointerPressed**](https://msdn.microsoft.com/library/windows/apps/dn298471), [**Windows::UI::Core::CoreIndependentInputSource::PointerReleased**](https://msdn.microsoft.com/library/windows/apps/dn298472) 및 [**Windows::UI::Core::CoreIndependentInputSource::PointerMoved**](https://msdn.microsoft.com/library/windows/apps/dn298469) 포인터 이벤트를 [**SwapChainPanel**](https://msdn.microsoft.com/library/windows/apps/dn252834)에 등록하는 방법을 보여 줍니다. 이러한 이벤트는 앱 초기화 중 및 게임 루프 전에 등록됩니다.

이러한 이벤트는 이벤트 처리기를 호출하는 별도의 스레드에서 처리됩니다.

응용 프로그램을 초기화하는 방법에 대한 자세한 내용은 [Marble Maze 응용 프로그램 구조](marble-maze-application-structure.md)를 참조하세요.

```cpp
coreInput->PointerPressed += ref new TypedEventHandler<Object^, PointerEventArgs^>(this, &DirectXPage::OnPointerPressed);
coreInput->PointerMoved += ref new TypedEventHandler<Object^, PointerEventArgs^>(this, &DirectXPage::OnPointerMoved);
coreInput->PointerReleased += ref new TypedEventHandler<Object^, PointerEventArgs^>(this, &DirectXPage::OnPointerReleased);
```

또한 MarbleMaze 클래스는 터치 이벤트를 저장할 std::map 개체를 만듭니다. 이 Map 개체의 키는 입력 포인터를 고유하게 식별하는 값입니다. 각 키는 모든 터치 지점과 화면 중심 사이의 거리에 매핑됩니다. 나중에 Marble Maze는 이러한 값을 사용하여 미로의 기울기 값을 계산합니다.

```cpp
typedef std::map<int, XMFLOAT2> TouchMap;
TouchMap        m_touches;
```

MarbleMaze 클래스는 Accelerometer 개체를 저장합니다.

```cpp
Windows::Devices::Sensors::Accelerometer^           m_accelerometer;
```

다음 예제와 같이 Accelerometer 개체는 MarbleMaze::Initialize 메서드에서 초기화됩니다. Windows::Devices::Sensors::Accelerometer::GetDefault 메서드는 기본 가속도계 인스턴스를 반환합니다. 기본 가속도계 Accelerometer::GetDefault가 없는 경우 m\_accelerometer의 값이 nullptr로 유지됩니다.

```cpp
// Returns accelerometer ref if there is one; nullptr otherwise.
m_accelerometer = Windows::Devices::Sensors::Accelerometer::GetDefault();
```

##  메뉴 탐색


###  Xbox 360 컨트롤러 입력 추적

다음과 같이 마우스, 터치 또는 Xbox 360 컨트롤러를 사용하여 메뉴를 탐색할 수 있습니다.

-   방향 패드를 사용하여 활성 메뉴 항목을 변경합니다.
-   터치, A 단추 또는 시작 단추를 사용하여 메뉴 항목을 선택하거나 최고 점수 테이블 등의 현재 메뉴를 닫습니다.
-   시작 단추를 사용하여 게임을 일시 중지하거나 다시 시작합니다.
-   마우스로 메뉴 항목을 클릭하여 해당 작업을 선택합니다.

###  터치식 및 마우스 입력 추적

Xbox 360 컨트롤러 입력을 추적하기 위해 **MarbleMaze::Update** 메서드는 입력 동작을 정의하는 단추 배열을 정의합니다. XInput은 컨트롤러의 현재 상태만 제공합니다. 따라서 **MarbleMaze::Update**는 가능한 각 Xbox 360 컨트롤러에 대해 이전 프레임에서 각 단추를 눌렀는지 여부 및 각 단추가 현재 눌려져 있는지 여부를 추적하는 배열 2개도 정의합니다.

```cpp
// This array contains the constants from XINPUT that map to the 
// particular buttons that are used by the game. 
// The index of the array is used to associate the state of that button in 
// the wasButtonDown, isButtonDown, and combinedButtonPressed variables. 

static const WORD buttons[] = {
    XINPUT_GAMEPAD_A,
    XINPUT_GAMEPAD_START,
    XINPUT_GAMEPAD_DPAD_UP,
    XINPUT_GAMEPAD_DPAD_DOWN,
    XINPUT_GAMEPAD_DPAD_LEFT,
    XINPUT_GAMEPAD_DPAD_RIGHT,
    XINPUT_GAMEPAD_BACK,
};

static const int buttonCount = ARRAYSIZE(buttons);
static bool wasButtonDown[XUSER_MAX_COUNT][buttonCount] = { false, };
bool isButtonDown[XUSER_MAX_COUNT][buttonCount] = { false, };
```

최대 4개의 Xbox 360 컨트롤러를 Windows 장치에 연결할 수 있습니다. 어떤 컨트롤러가 활성 컨트롤러인지 확인할 필요가 없도록 **MarbleMaze::Update** 메서드는 모든 컨트롤러의 입력을 결합합니다.

```cpp
bool combinedButtonPressed[buttonCount] = { false, };
```

게임이 둘 이상의 플레이어를 지원하는 경우 각 플레이어에 대한 입력을 개별적으로 추적해야 합니다.

루프에서 **MarbleMaze::Update** 메서드는 각 컨트롤러에서 입력을 폴링하고 각 단추의 상태를 읽습니다.

```cpp
// Account for input on any connected controller.
XINPUT_STATE inputState = {0};
for (DWORD userIndex = 0; userIndex < XUSER_MAX_COUNT; ++userIndex)
{
    DWORD result = XInputGetState(userIndex, &inputState);
    if(result != ERROR_SUCCESS) 
        continue;

    SHORT thumbLeftX = inputState.Gamepad.sThumbLX;
    if (abs(thumbLeftX) < XINPUT_GAMEPAD_LEFT_THUMB_DEADZONE) 
        thumbLeftX = 0;

    SHORT thumbLeftY = inputState.Gamepad.sThumbLY;
    if (abs(thumbLeftY) < XINPUT_GAMEPAD_LEFT_THUMB_DEADZONE) 
        thumbLeftY = 0;

    combinedTiltX += static_cast<float>(thumbLeftX) / 32768.0f;
    combinedTiltY += static_cast<float>(thumbLeftY) / 32768.0f;

    for (int i = 0; i < buttonCount; ++i)
        isButtonDown[userIndex][i] = (inputState.Gamepad.wButtons & buttons[i]) == buttons[i];
}
```

**MarbleMaze::Update** 메서드는 입력을 폴링한 후 결합된 입력 배열을 업데이트합니다. 결합된 입력 배열은 누르는 단추만 추적하고 이전에 누른 단추는 추적하지 않습니다. 이렇게 하면 게임이 단추를 처음 누를 때만 작업을 수행하고 단추가 눌려져 있을 때는 작업을 수행하지 않습니다.

```cpp
bool combinedButtonPressed[buttonCount] = { false, };
for (int i = 0; i < buttonCount; ++i)
{
    for (DWORD userIndex = 0; userIndex < XUSER_MAX_COUNT; ++userIndex)
    {
        bool pressed = !wasButtonDown[userIndex][i] && isButtonDown[userIndex][i];
        combinedButtonPressed[i] = combinedButtonPressed[i] || pressed;
    }
}
```

**MarbleMaze::Update** 메서드는 단추 입력을 수집한 후 발생해야 하는 작업을 수행합니다. 예를 들어 시작 단추(XINPUT\_GAMEPAD\_START)를 누르면 게임 상태가 활성에서 일시 중지됨으로 변경되거나 일시 중지됨에서 활성으로 변경됩니다.

```cpp
// Check whether the user paused or resumed the game. 
// XINPUT_GAMEPAD_START  
if (combinedButtonPressed[1] || m_pauseKeyPressed)
{
    m_pauseKeyPressed = false;
    if (m_gameState == GameState::InGameActive)
        SetGameState(GameState::InGamePaused);
    else if (m_gameState == GameState::InGamePaused)
        SetGameState(GameState::InGameActive);
}
```

주 메뉴가 활성화된 경우 방향 패드를 위 또는 아래로 누르면 활성 메뉴 항목이 변경됩니다. 사용자가 현재 선택 항목을 선택하면 해당 UI 요소가 선택된 것으로 표시됩니다.

```cpp
// Handle menu navigation. 

// XINPUT_GAMEPAD_A or XINPUT_GAMEPAD_START 
bool chooseSelection = (combinedButtonPressed[0] || combinedButtonPressed[1]);

// XINPUT_GAMEPAD_DPAD_UP 
bool moveUp = combinedButtonPressed[2];

// XINPUT_GAMEPAD_DPAD_DOWN 
bool moveDown = combinedButtonPressed[3];                                           

switch (m_gameState)
{
case GameState::MainMenu:
    if (chooseSelection)
    {
        m_audio.PlaySoundEffect(MenuSelectedEvent);

        if (m_startGameButton.GetSelected())
            m_startGameButton.SetPressed(true);

        if (m_highScoreButton.GetSelected())
            m_highScoreButton.SetPressed(true);
    }
    if (moveUp || moveDown)
    {
        m_startGameButton.SetSelected(!m_startGameButton.GetSelected());
        m_highScoreButton.SetSelected(!m_startGameButton.GetSelected());

        m_audio.PlaySoundEffect(MenuChangeEvent);
    }
    break;

case GameState::HighScoreDisplay:
    if (chooseSelection || anyPoints)
        SetGameState(GameState::MainMenu);
    break;

case GameState::PostGameResults:
    if (chooseSelection || anyPoints)
        SetGameState(GameState::HighScoreDisplay);
    break;

case GameState::InGamePaused:
    if (m_pausedText.IsPressed())
    {
        m_pausedText.SetPressed(false);
        SetGameState(GameState::InGameActive); 
    }
    break;
}
```

**MarbleMaze::Update** 메서드는 컨트롤러 입력을 처리한 후 다음 프레임을 위해 현재 입력 상태를 저장합니다.

```cpp
// Update the button state for the next frame.
memcpy(wasButtonDown, isButtonDown, sizeof(wasButtonDown));
```

### 터치식 및 마우스 입력 추적

터치식 및 마우스 입력의 경우 사용자가 터치하거나 클릭하면 메뉴 항목이 선택됩니다. 다음 예제에서는 **MarbleMaze::Update** 메서드가 포인터 입력을 처리하여 메뉴 항목을 선택하는 방법을 보여 줍니다. **m\_pointQueue** 멤버 변수는 사용자가 화면에서 터치하거나 클릭한 위치를 추적합니다. Marble Maze가 포인터 입력을 수집하는 방법은 이 문서의 뒷부분에 있는 포인터 입력 처리 섹션에서 자세히 설명합니다.

```cpp
// Check whether the user chose a button from the UI. 
bool anyPoints = !m_pointQueue.empty();
while (!m_pointQueue.empty())
{
    UserInterface::GetInstance().HitTest(m_pointQueue.front());
    m_pointQueue.pop();
}
```

**UserInterface::HitTest** 메서드는 제공된 포인트가 UI 요소의 범위 내에 있는지 확인합니다. 이 테스트를 통과하는 UI 요소는 터치된 것으로 표시됩니다. 이 메서드는 **PointInRect** 도우미 함수를 사용하여 제공된 포인트가 각 UI 요소의 범위 내에 있는지 확인합니다.

```cpp
void UserInterface::HitTest(D2D1_POINT_2F point)
{
    for (auto iter = m_elements.begin(); iter != m_elements.end(); ++iter)
    {
        if (!(*iter)->IsVisible())
            continue;

        TextButton* textButton = dynamic_cast<TextButton*>(*iter);
        if (textButton != nullptr)
        {
            D2D1_RECT_F bounds = (*iter)->GetBounds();
            textButton->SetPressed(PointInRect(point, bounds));
        }
    }
}
```

### 게임 상태 업데이트

**MarbleMaze::Update** 메서드는 컨트롤러 및 터치식 입력을 처리한 후 단추를 누른 경우 게임 상태를 업데이트합니다.

```cpp
// Update the game state if the user chose a menu option. 
if (m_startGameButton.IsPressed())
{
    SetGameState(GameState::PreGameCountdown);
    m_startGameButton.SetPressed(false);
}
if (m_highScoreButton.IsPressed())
{
    SetGameState(GameState::HighScoreDisplay);
    m_highScoreButton.SetPressed(false);
}
```

##  게임 플레이 제어


게임 루프와 **MarbleMaze::Update** 메서드는 함께 작동하여 게임 개체의 상태를 업데이트합니다. 게임이 여러 장치에서 입력을 받아들이는 경우 유지 관리하기 쉬운 코드를 작성할 수 있도록 모든 장치의 입력을 하나의 변수 집합에 누적할 수 있습니다. **MarbleMaze::Update** 메서드는 모든 장치의 움직임을 누적하는 하나의 변수 집합을 정의합니다.

```cpp
float combinedTiltX = 0.0f;
float combinedTiltY = 0.0f;
```

입력 메커니즘은 입력 장치마다 다를 수 있습니다. 예를 들어 포인터 입력은 Windows 런타임 이벤트 처리 모델을 사용하여 처리됩니다. 반대로, 필요할 때 Xbox 360 컨트롤러에서 입력 데이터를 폴링합니다. 항상 지정된 장치에 대해 규정된 입력 메커니즘을 따르는 것이 좋습니다. 이 섹션에서는 Marble Maze가 각 장치에서 입력을 읽는 방법, 결합된 입력 값을 업데이트하는 방법, 결합된 입력 값을 사용하여 게임 상태를 업데이트하는 방법 등을 설명합니다.

###  포인터 입력 처리

포인터 입력 작업을 하는 경우 [**Windows::UI::Core::CoreDispatcher::ProcessEvents**](https://msdn.microsoft.com/library/windows/apps/br208217) 메서드를 호출하여 창 이벤트를 처리합니다. 장면을 업데이트하거나 렌더링하기 전에 게임 루프에서 이 메서드를 호출합니다. Marble Maze는 이 메서드에 **CoreProcessEventsOption::ProcessAllIfPresent**를 전달하여 대기 중인 이벤트를 모두 처리하고 즉시 반환됩니다. 이벤트가 처리된 후 Marble Maze는 다음 프레임을 렌더링하고 표시합니다.

```cpp
// Process windowing events.
CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);
```

Windows 런타임은 발생한 각 이벤트에 대해 등록된 처리기를 호출합니다. **DirectXApp** 클래스는 이벤트를 등록하고 **MarbleMaze** 클래스에 포인터 정보를 전달합니다.

```cpp
void DirectXApp::OnPointerPressed(
    _In_ Windows::UI::Core::CoreWindow^ sender,
    _In_ Windows::UI::Core::PointerEventArgs^ args
    )
{
    m_renderer->AddTouch(args->CurrentPoint->PointerId, args->CurrentPoint->Position);
}

void DirectXApp::OnPointerReleased(
    _In_ Windows::UI::Core::CoreWindow^ sender,
    _In_ Windows::UI::Core::PointerEventArgs^ args
    )
{
    m_renderer->RemoveTouch(args->CurrentPoint->PointerId);
}

void DirectXApp::OnPointerMoved(
    _In_ Windows::UI::Core::CoreWindow^ sender,
    _In_ Windows::UI::Core::PointerEventArgs^ args
    )
{
    m_renderer->UpdateTouch(args->CurrentPoint->PointerId, args->CurrentPoint->Position);
}
```

**MarbleMaze** 클래스는 터치 이벤트를 저장하는 Map 개체를 업데이트하여 포인터 이벤트에 반응합니다. **MarbleMaze::AddTouch** 메서드는 포인터를 처음 누를 때(예: 사용자가 터치 사용 장치의 화면을 처음 터치할 때) 호출됩니다. **MarbleMaze::AddTouch** 메서드는 포인터 위치가 이동할 때 호출됩니다. **MarbleMaze::RemoveTouch** 메서드는 포인터를 해제할 때(예: 사용자가 화면 터치를 중지할 때) 호출됩니다.

```cpp
void MarbleMaze::AddTouch(int id, Windows::Foundation::Point point)
{
    m_touches[id] = PointToTouch(point, m_windowBounds);

    m_pointQueue.push(D2D1::Point2F(point.X, point.Y));
}

void MarbleMaze::UpdateTouch(int id, Windows::Foundation::Point point)
{
    if (m_touches.find(id) != m_touches.end())
        m_touches[id] = PointToTouch(point, m_windowBounds);
}

void MarbleMaze::RemoveTouch(int id)
{
    m_touches.erase(id);
}
```

PointToTouch 함수는 원점이 화면 중앙에 오도록 현재 포인터 위치의 좌표를 이동한 다음 약 -1.0에서 +1.0 사이가 되도록 좌표를 조정합니다. 이렇게 하면 여러 입력 방법에서 일관된 방식으로 미로의 기울기를 더 쉽게 계산할 수 있습니다.

```cpp
inline XMFLOAT2 PointToTouch(Windows::Foundation::Point point, Windows::Foundation::Rect bounds)
{
    float touchRadius = min(bounds.Width, bounds.Height);
    float dx = (point.X - (bounds.Width / 2.0f)) / touchRadius;
    float dy = ((bounds.Height / 2.0f) - point.Y) / touchRadius;

    return XMFLOAT2(dx, dy);
}
```

**MarbleMaze::Update** 메서드는 기울기 계수를 일정한 조정 값만큼 증가시켜 결합된 입력 값을 업데이트합니다. 이 조정 값은 여러 다른 값으로 실험하여 확인되었습니다.

```cpp
// Account for touch input. 
const float touchScalingFactor = 2.0f;
for (TouchMap::const_iterator iter = m_touches.cbegin(); iter != m_touches.cend(); ++iter)
{
    combinedTiltX += iter->second.x * touchScalingFactor;
    combinedTiltY += iter->second.y * touchScalingFactor;
}
```

### 가속도계 입력 처리

가속도계 입력을 처리하기 위해 **MarbleMaze::Update** 메서드는 [**Windows::Devices::Sensors::Accelerometer::GetCurrentReading**](https://msdn.microsoft.com/library/windows/apps/br225699) 메서드를 호출합니다. 이 메서드는 가속도계 읽기를 나타내는 [**Windows::Devices::Sensors::AccelerometerReading**](https://msdn.microsoft.com/library/windows/apps/br225688) 개체를 반환합니다. **Windows::Devices::Sensors::AccelerometerReading::AccelerationX** 및 **Windows::Devices::Sensors::AccelerometerReading::AccelerationY** 속성은 각각 x축과 y축을 따라 g-force 가속을 유지합니다.

다음 예제에서는 **MarbleMaze::Update** 메서드가 가속도계를 폴링하고 결합된 입력 값을 업데이트하는 방법을 보여 줍니다. 장치를 기울이면 중력으로 인해 구슬이 더 빨리 이동합니다.

```cpp
// Account for sensors. 
const float acceleromterScalingFactor = 3.5f;
if (m_accelerometer != nullptr)
{
    Windows::Devices::Sensors::AccelerometerReading^ reading =
        m_accelerometer->GetCurrentReading();

    if (reading != nullptr)
    {
        combinedTiltX += static_cast<float>(reading->AccelerationX) * acceleromterScalingFactor;
        combinedTiltY += static_cast<float>(reading->AccelerationY) * acceleromterScalingFactor;
    }
}
```

사용자 컴퓨터에 가속도계가 있는지 확신할 수 없는 경우 가속도계를 폴링하기 전에 항상 유효한 Accelerometer 개체가 있는지 확인합니다.

### Xbox 360 컨트롤러 입력 처리

다음 예제에서는 **MarbleMaze::Update** 메서드가 Xbox 360 컨트롤러에서 읽고 결합된 입력 값을 업데이트하는 방법을 보여 줍니다. **MarbleMaze::Update** 메서드는 for 루프를 사용하여 연결된 컨트롤러에서 입력을 받을 수 있도록 합니다. **XInputGetState** 메서드는 XINPUT\_STATE 개체를 컨트롤러의 현재 상태로 채웁니다. **combinedTiltX** 및 **combinedTiltY** 값은 왼쪽 엄지스틱의 x 및 y 값에 따라 업데이트됩니다.

```cpp
// Account for input on any connected controller.
XINPUT_STATE inputState = {0};
for (DWORD userIndex = 0; userIndex < XUSER_MAX_COUNT; ++userIndex)
{
    DWORD result = XInputGetState(userIndex, &inputState);
    if(result != ERROR_SUCCESS) 
        continue;

    SHORT thumbLeftX = inputState.Gamepad.sThumbLX;
    if (abs(thumbLeftX) < XINPUT_GAMEPAD_LEFT_THUMB_DEADZONE) 
        thumbLeftX = 0;

    SHORT thumbLeftY = inputState.Gamepad.sThumbLY;
    if (abs(thumbLeftY) < XINPUT_GAMEPAD_LEFT_THUMB_DEADZONE) 
        thumbLeftY = 0;

    combinedTiltX += static_cast<float>(thumbLeftX) / 32768.0f;
    combinedTiltY += static_cast<float>(thumbLeftY) / 32768.0f;

    for (int i = 0; i < buttonCount; ++i)
        isButtonDown[userIndex][i] = (inputState.Gamepad.wButtons & buttons[i]) == buttons[i];
}
```

XInput은 왼쪽 엄지스틱에 대한 **XINPUT\_GAMEPAD\_LEFT\_THUMB\_DEADZONE** 상수를 정의합니다. 이 상수는 대부분의 게임에 적합한 데드존 임계값입니다.

> **중요** Xbox 360 컨트롤러로 작업하는 경우 항상 데드존을 고려합니다. 데드존은 초기 움직임에 대한 게임 패드 간의 민감도 차이를 가리킵니다. 작은 움직임이 있을 때 읽기가 생성되지 않는 컨트롤러도 있고, 측정 가능한 읽기가 생성되는 컨트롤러도 있습니다. 게임에서 이 차이를 고려하려면 초기 엄지스틱 움직임에 대해 비이동 영역을 만듭니다. 데드존에 대한 자세한 내용은 [XInput 시작](https://msdn.microsoft.com/library/windows/desktop/ee417001)을 참조하세요.

 

###  게임 상태에 입력 적용

장치는 각기 다른 방법으로 입력 값을 보고합니다. 예를 들어 포인터 입력은 화면 좌표이고 컨트롤러 입력은 완전히 다른 형식일 수 있습니다. 여러 장치의 입력을 하나의 입력 값 집합으로 결합하는 경우의 한 가지 문제는 정규화, 즉 값을 공통 형식으로 변환하는 것입니다. Marble Maze는 값을 \[-1.0, 1.0\] 범위로 조정하여 정규화합니다. Xbox 360 컨트롤러 입력을 정규화하기 위해 Marble Maze는 엄지스틱 입력 값이 항상 -32768에서 32767 사이에 있으므로 입력 값을 32768로 나눕니다. 이 섹션의 앞부분에서 설명한 **PointToTouch** 함수는 약 -1.0에서 +1.0 사이의 정규화된 값으로 화면 좌표를 변환하여 비슷한 결과를 얻습니다.

> **팁** 응용 프로그램이 하나의 입력 방법을 사용하는 경우에도 항상 입력 값을 정규화하는 것이 좋습니다. 이렇게 하면 물리학 시뮬레이션 등 게임의 다른 구성 요소에서 입력이 해석되는 방식을 간소화할 수 있으며, 각기 다른 화면 해상도에서 작동하는 게임을 더 쉽게 작성할 수 있습니다.

 

**MarbleMaze::Update** 메서드는 입력을 처리한 후 미로의 기울기가 구슬에 미치는 영향을 나타내는 벡터를 만듭니다. 다음 예제에서는 Marble Maze가 **XMVector3Normalize** 함수를 사용하여 정규화된 중력 벡터를 만드는 방법을 보여 줍니다. *MaxTilt* 변수는 미로 기울기 양을 제한하고 미로가 옆으로 서지 않도록 합니다.

```cpp
const float maxTilt = 1.0f / 8.0f;
XMVECTOR gravity = XMVectorSet(combinedTiltX * maxTilt, combinedTiltY * maxTilt, 1.0f, 0.0f);
gravity = XMVector3Normalize(gravity);
```

장면 개체의 업데이트를 완료하기 위해 Marble Maze는 업데이트된 중력 벡터를 물리학 시뮬레이션에 전달하고, 이전 프레임 이후 경과 시간에 대한 물리학 시뮬레이션을 업데이트하고, 구슬의 위치와 방향을 업데이트합니다. 구슬이 미로에서 떨어진 경우 **MarbleMaze::Update** 메서드는 구슬이 터치한 마지막 검사점에 다시 구슬을 놓고 물리학 시뮬레이션의 상태를 다시 설정합니다.

```cpp
XMFLOAT3 g;
XMStoreFloat3(&g, gravity);
m_physics.SetGravity(g);
```

```cpp
// Only update physics when gameplay is active.
m_physics.UpdatePhysicsSimulation(timeDelta);
```

```cpp
// Check whether the marble fell off of the maze. 
const float fadeOutDepth = 0.0f;
const float resetDepth = 80.0f;
if (marblePosition.z >= fadeOutDepth)
{
    m_targetLightStrength = 0.0f;
}
if (marblePosition.z >= resetDepth)
{
    // Reset marble.
    memcpy(&marblePosition, &m_checkpoints[m_currentCheckpoint], sizeof(XMFLOAT3));
    oldMarblePosition = marblePosition;
    m_physics.SetPosition((const XMFLOAT3&)marblePosition);
    m_physics.SetVelocity(XMFLOAT3(0, 0, 0));
    m_lightStrength = 0.0f;
    m_targetLightStrength = 1.0f;

    m_resetCamera = true;
    m_resetMarbleRotation = true;
    m_audio.PlaySoundEffect(FallingEvent);
}
```

이 섹션에서는 물리학 시뮬레이션의 작동 방식에 대해 설명하지 않습니다. 자세한 내용은 Marble Maze 소스의 Physics.h 및 Physics.cpp를 참조하세요.

## 다음 단계


오디오 작업을 할 때 고려할 몇 가지 주요 사항에 대한 자세한 내용은 [Marble Maze 샘플에 오디오 추가](adding-audio-to-the-marble-maze-sample.md)를 참조하세요. 이 문서에서는 Marble Maze가 Microsoft 미디어 파운데이션 및 XAudio2를 사용하여 오디오 리소스를 로드, 믹싱 및 재생하는 방법을 설명합니다.

## 관련 항목


* [Marble Maze 샘플에 오디오 추가](adding-audio-to-the-marble-maze-sample.md)
* [Marble Maze 샘플에 시각적 콘텐츠 추가](adding-visual-content-to-the-marble-maze-sample.md)
* [C++ 및 DirectX로 UWP 게임 Marble Maze 개발](developing-marble-maze-a-windows-store-game-in-cpp-and-directx.md)

 

 







<!--HONumber=Aug16_HO3-->


