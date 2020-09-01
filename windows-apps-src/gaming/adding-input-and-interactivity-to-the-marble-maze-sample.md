---
title: Marble Maze 샘플에 입력 및 대화형 작업 추가
description: 입력 장치를 사용 하 여 작업할 때 염두에 두어야 하는 주요 방법에 대해 알아봅니다.
ms.assetid: b946bf62-c0ca-f9ec-1a87-8195b89a5ab4
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 게임, 입력, 샘플
ms.localizationpriority: medium
ms.openlocfilehash: d4c3742ed843deca9d7d8edba033addd2e4888fe
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172077"
---
# <a name="adding-input-and-interactivity-to-the-marble-maze-sample"></a>Marble Maze 샘플에 입력 및 대화형 작업 추가




UWP (유니버설 Windows 플랫폼) 게임은 데스크톱 컴퓨터, 랩톱, 태블릿 등 다양 한 장치에서 실행 됩니다. 장치는 입력 및 제어 메커니즘을 다양 한 수 있습니다. 이 문서에서는 입력 장치를 사용할 때 유의 해야 하는 주요 관행을 설명 하 고, 대리석에서 이러한 방법을 적용 하는 방법을 보여 줍니다.

> [!NOTE]
> 이 문서에 해당 하는 샘플 코드는 [DirectX 대리석 미로 game 샘플](https://github.com/microsoft/Windows-appsample-marble-maze)에서 찾을 수 있습니다.

 
게임에서 입력 작업을 수행할 때이 문서에서 설명 하는 주요 사항은 다음과 같습니다.

-   가능 하면 여러 입력 장치를 지원 하 여 고객 들에 게 광범위 한 기본 설정 및 기능을 수용할 수 있도록 합니다. 게임 컨트롤러 및 센서 사용은 선택 사항 이지만 플레이어 환경을 개선 하는 것이 좋습니다. 이러한 입력 장치를 보다 쉽게 통합 하는 데 도움이 되는 게임 컨트롤러 및 센서 Api를 설계 했습니다.

-   터치를 초기화 하려면 포인터가 활성화, 해제 및 이동 되는 경우와 같은 창 이벤트를 등록 해야 합니다. 가 속도계를 초기화 하려면 응용 프로그램을 초기화할 때 [Windows::D evices:: 센서인::가 속도계](/uwp/api/Windows.Devices.Sensors.Accelerometer) 개체를 만듭니다. Xbox 컨트롤러를 초기화할 필요가 없습니다.

-   단일 플레이어 게임의 경우 모든 가능한 Xbox 컨트롤러의 입력을 결합할 것인지 여부를 고려 합니다. 이러한 방식으로 어떤 컨트롤러에서 어떤 입력이 제공 되는지 추적할 필요가 없습니다. 또는이 샘플에서와 같이 가장 최근에 추가한 컨트롤러 에서만 입력을 추적 하면 됩니다.

-   입력 장치를 처리 하기 전에 Windows 이벤트를 처리 합니다.

-   Xbox 컨트롤러 및가 속도계는 폴링을 지원 합니다. 즉, 필요할 때 데이터를 폴링할 수 있습니다. 터치의 경우 입력 처리 코드에서 사용할 수 있는 데이터 구조로 터치 이벤트를 기록 합니다.

-   입력 값을 공통 형식으로 정규화 할지 여부를 고려 합니다. 이렇게 하면 게임의 다른 구성 요소 (예: 물리학 시뮬레이션)에서 입력을 해석 하는 방법이 간단해 지 며, 다른 화면 해상도에서 작동 하는 게임을 보다 쉽게 작성할 수 있습니다.

## <a name="input-devices-supported-by-marble-maze"></a>대리석 미로에서 지원 되는 입력 장치


대리석 미로는 Xbox 컨트롤러, 마우스 및 터치를 선택 하 여 메뉴 항목을 선택 하 고, Xbox 컨트롤러, 마우스, 터치 및가 속도계 게임 플레이를 제어할 수 있도록 지원 합니다. 대리석 메 이즈는 [Windows:: 게이밍:: input](/uwp/api/windows.gaming.input) api를 사용 하 여 입력을 위해 컨트롤러를 폴링합니다. 터치를 통해 응용 프로그램은 fingertip 입력을 추적 하 고 응답할 수 있습니다. 가 속도계은 X, Y 및 Z 축을 따라 적용 되는 힘을 측정 하는 센서입니다. Windows 런타임를 사용 하 여가 속도계 장치의 현재 상태를 폴링하고 Windows 런타임 이벤트 처리 메커니즘을 통해 터치 이벤트를 받을 수 있습니다.

> [!NOTE]
> 이 문서에서는 터치와 마우스 입력 및 포인터를 모두 참조 하 여 포인터 이벤트를 사용 하는 장치를 참조 합니다. 터치 및 마우스가 표준 포인터 이벤트를 사용 하기 때문에 두 장치 중 하나를 사용 하 여 메뉴 항목을 선택 하 고 게임 재생을 제어할 수 있습니다.

 

> [!NOTE]
> 패키지 매니페스트는 게임에 대해 지원 되는 유일한 회전으로 **가로** 를 설정 하 여 대리석을 롤백하여 장치를 회전할 때 방향이 변경 되지 않도록 합니다. 패키지 매니페스트를 보려면 Visual Studio의 **솔루션 탐색기** 에서 **appxmanifest.xml** 를 엽니다.

 

## <a name="initializing-input-devices"></a>입력 장치 초기화


Xbox 컨트롤러는 초기화할 필요가 없습니다. 터치를 초기화 하려면 포인터가 활성화 될 때 (예: 플레이어에서 마우스 단추를 누르거나 화면 터치), 해제 및 이동 하는 등의 창 고 이벤트를 등록 해야 합니다. 가 속도계를 초기화 하려면 응용 프로그램을 초기화할 때 [Windows::D evices:: 센서인::가 속도계](/uwp/api/Windows.Devices.Sensors.Accelerometer) 개체를 만들어야 합니다.

다음 예제에서는 **App:: setwindow** 메서드가 [WINDOWS:: Ui:: Core:: CoreWindow::P ointerpressed](/uwp/api/windows.ui.core.corewindow.PointerPressed), [windows:: ui:: core:: CoreWindow::P ointerpressed](/uwp/api/windows.ui.core.corewindow.PointerReleased)및 [windows:: ui:: Core:: CoreWindow::P ointerpressed](/uwp/api/windows.ui.core.corewindow.PointerMoved) 이벤트를 등록 하는 방법을 보여 줍니다. 이러한 이벤트는 응용 프로그램 초기화 중과 게임 루프 이전에 등록 됩니다.

이러한 이벤트는 이벤트 처리기를 호출 하는 별도의 스레드에서 처리 됩니다.

응용 프로그램을 초기화 하는 방법에 대 한 자세한 내용은 [대리석 미로 응용 프로그램 구조](marble-maze-application-structure.md)를 참조 하세요.

```cpp
window->PointerPressed += ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(
    this, 
    &App::OnPointerPressed);

window->PointerReleased += ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(
    this, 
    &App::OnPointerReleased);

window->PointerMoved += ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(
    this, 
    &App::OnPointerMoved);
```

**MarbleMazeMain** 클래스는 또한 touch 이벤트를 포함 하는 **std:: map** 개체를 만듭니다. 이 map 개체의 키는 입력 포인터를 고유 하 게 식별 하는 값입니다. 각 키는 모든 터치 지점과 화면 중심 사이의 거리에 매핑됩니다. 대리석은 나중에 이러한 값을 사용 하 여 미로가 기울어진 정도를 계산 합니다.

```cpp
typedef std::map<int, XMFLOAT2> TouchMap;
TouchMap        m_touches;
```

**MarbleMazeMain** 클래스에는 [가 속도계](/uwp/api/Windows.Devices.Sensors.Accelerometer) 개체도 포함 되어 있습니다.

```cpp
Windows::Devices::Sensors::Accelerometer^           m_accelerometer;
```

**가 속도계** 개체는 다음 예제와 같이 **MarbleMazeMain** 생성자에서 초기화 됩니다. [Windows::D evices:: 센서인::가 속도계:: getdefault](/uwp/api/Windows.Devices.Sensors.Accelerometer.GetDefault) 메서드는 기본가 속도계의 인스턴스를 반환 합니다. 기본가 속도계 없는 경우 **가 속도계:: GetDefault** 는 **nullptr**를 반환 합니다.

```cpp
// Returns accelerometer ref if there is one; nullptr otherwise.
m_accelerometer = Windows::Devices::Sensors::Accelerometer::GetDefault();
```

##  <a name="navigating-the-menus"></a>메뉴 탐색

마우스, 터치 또는 Xbox 컨트롤러를 사용 하 여 다음과 같이 메뉴를 탐색할 수 있습니다.

-   방향 패드를 사용 하 여 활성 메뉴 항목을 변경 합니다.
-   터치, A 단추 또는 메뉴 단추를 사용 하 여 메뉴 항목을 선택 하거나 현재 메뉴 (예: 높은 점수 표)를 닫습니다.
-   메뉴 단추를 사용 하 여 게임을 일시 중지 하거나 다시 시작 합니다.
-   마우스를 사용 하 여 메뉴 항목을 클릭 하 여 해당 작업을 선택 합니다.

###  <a name="tracking-xbox-controller-input"></a>Xbox 컨트롤러 입력 추적

현재 장치에 연결 된 gamepads을 추적 하기 위해 **MarbleMazeMain** 은 [Windows:: 게이밍:: Input:: 게이밍](/uwp/api/windows.gaming.input.gamepad) 개체의 컬렉션인 멤버 변수 **m_myGamepads**정의 합니다. 이는 생성자에서 다음과 같이 초기화 됩니다.

```cpp
m_myGamepads = ref new Vector<Gamepad^>();

for (auto gamepad : Gamepad::Gamepads)
{
    m_myGamepads->Append(gamepad);
}
```

또한 **MarbleMazeMain** 생성자는 gamepads를 추가 하거나 제거 하는 경우에 대 한 이벤트를 등록 합니다.

```cpp
Gamepad::GamepadAdded += 
    ref new EventHandler<Gamepad^>([=](Platform::Object^, Gamepad^ args)
{
    m_myGamepads->Append(args);
    m_currentGamepadNeedsRefresh = true;
});

Gamepad::GamepadRemoved += 
    ref new EventHandler<Gamepad ^>([=](Platform::Object^, Gamepad^ args)
{
    unsigned int indexRemoved;

    if (m_myGamepads->IndexOf(args, &indexRemoved))
    {
        m_myGamepads->RemoveAt(indexRemoved);
        m_currentGamepadNeedsRefresh = true;
    }
});
```

추가 된 게임 패드는 **m_myGamepads**에 추가 됩니다. 게임 패드를 제거 하는 경우에는 게임 패드를 **m_myGamepads**있는지 확인 하 고, 있는 경우 제거 합니다. 두 경우 모두 **m_currentGamepadNeedsRefresh** 을 **true**로 설정 하 여 **m_gamepad**를 다시 할당 해야 함을 나타냅니다.

마지막으로 **m_gamepad** 에 게임 패드를 할당 하 고 **m_currentGamepadNeedsRefresh** 을 **false**로 설정 합니다.

```cpp
m_gamepad = GetLastGamepad();
m_currentGamepadNeedsRefresh = false;
```

**업데이트** 메서드에서 **m_gamepad** 를 다시 할당 해야 하는지 확인 합니다.

```cpp
if (m_currentGamepadNeedsRefresh)
{
    auto mostRecentGamepad = GetLastGamepad();

    if (m_gamepad != mostRecentGamepad)
    {
        m_gamepad = mostRecentGamepad;
    }

    m_currentGamepadNeedsRefresh = false;
}
```

**M_gamepad** 를 다시 할당 해야 하는 경우 다음과 같이 정의 된 **GetLastGamepad**를 사용 하 여 가장 최근에 추가 된 게임 패드에 할당 합니다.

```cpp
Gamepad^ MarbleMaze::MarbleMazeMain::GetLastGamepad()
{
    Gamepad^ gamepad = nullptr;

    if (m_myGamepads->Size > 0)
    {
        gamepad = m_myGamepads->GetAt(m_myGamepads->Size - 1);
    }

    return gamepad;
}
```

이 메서드는 단순히 **m_myGamepads**의 마지막 게임 패드를 반환 합니다.

최대 4 개의 Xbox 컨트롤러를 Windows 10 장치에 연결할 수 있습니다. 활성 컨트롤러인 컨트롤러를 파악 하지 않으려면 가장 최근에 추가 된 게임 패드만 추적 하면 됩니다. 게임에서 둘 이상의 플레이어를 지 원하는 경우 각 플레이어에 대해 개별적으로 입력을 추적 해야 합니다.

**MarbleMazeMain:: Update** 메서드는 입력에 대 한 게임 패드를 폴링합니다.

```cpp
if (m_gamepad != nullptr)
{
    m_oldReading = m_newReading;
    m_newReading = m_gamepad->GetCurrentReading();
}
```

**M_oldReading**를 사용 하 여 마지막 프레임에서 가져온 입력 읽기와 **m_newReading**로 최신 입력 읽기를 추적 합니다 .이는 [게임 패드:: GetCurrentReading](/uwp/api/windows.gaming.input.gamepad.GetCurrentReading)를 호출 하 여 가져옵니다. 그러면 게임 패드의 현재 상태에 대 한 정보를 포함 하는 [GamepadReading](/uwp/api/windows.gaming.input.gamepadreading) 개체를 반환 합니다.

단추가 눌러져 있거나 해제 되었는지 확인 하려면이 프레임과 마지막 프레임의 단추 판독값을 비교 하는 **MarbleMazeMain:: ButtonJustPressed** 및 **MarbleMazeMain:: ButtonJustReleased**를 정의 합니다. 이러한 방식으로 단추를 처음으로 눌렀다 놓았을 때만 작업을 수행할 수 있으며, 유지 되는 경우에는 수행할 수 없습니다.

```cpp
bool MarbleMaze::MarbleMazeMain::ButtonJustPressed(GamepadButtons selection)
{
    bool newSelectionPressed = (selection == (m_newReading.Buttons & selection));
    bool oldSelectionPressed = (selection == (m_oldReading.Buttons & selection));
    return newSelectionPressed && !oldSelectionPressed;
}

bool MarbleMaze::MarbleMazeMain::ButtonJustReleased(GamepadButtons selection)
{
    bool newSelectionReleased = 
        (GamepadButtons::None == (m_newReading.Buttons & selection));

    bool oldSelectionReleased = 
        (GamepadButtons::None == (m_oldReading.Buttons & selection));

    return newSelectionReleased && !oldSelectionReleased;
}
```

[GamepadButtons](/uwp/api/windows.gaming.input.gamepadbuttons) 판독값은 비트 연산을 사용 하 여 비교 되며, &mdash; *비트* and (&)를 사용 하 여 단추를 눌렀는지 여부를 확인 합니다. 이전 읽기와 새 읽기를 비교 하 여 단추를 눌렀다 놓았음을 여부를 확인 합니다.

위의 방법을 사용 하 여 특정 단추가 눌러져 있는지 확인 하 고 수행 해야 하는 해당 작업을 수행 합니다. 예를 들어 메뉴 단추 (**GamepadButtons:: menu**)를 누르면 게임 상태가 활성에서 일시 중지 됨 또는 일시 중지 됨에서 활성으로 변경 됩니다.

```cpp
if (ButtonJustPressed(GamepadButtons::Menu) || m_pauseKeyPressed)
{
    m_pauseKeyPressed = false;

    if (m_gameState == GameState::InGameActive)
    {
        SetGameState(GameState::InGamePaused);
    }  
    else if (m_gameState == GameState::InGamePaused)
    {
        SetGameState(GameState::InGameActive);
    }
}
```

또한 플레이어가 보기 단추를 누르는 지 확인 합니다 .이 경우 게임을 다시 시작 하거나 높은 점수 표를 지웁니다.

```cpp
if (ButtonJustPressed(GamepadButtons::View) || m_homeKeyPressed)
{
    m_homeKeyPressed = false;

    if (m_gameState == GameState::InGameActive ||
        m_gameState == GameState::InGamePaused ||
        m_gameState == GameState::PreGameCountdown)
    {
        SetGameState(GameState::MainMenu);
        m_inGameStopwatchTimer.SetVisible(false);
        m_preGameCountdownTimer.SetVisible(false);
    }
    else if (m_gameState == GameState::HighScoreDisplay)
    {
        m_highScoreTable.Reset();
    }
}
```

주 메뉴가 활성화 되어 있으면 방향 패드를 누르면 활성 메뉴 항목이 변경 됩니다. 사용자가 현재 선택 항목을 선택 하면 적절 한 UI 요소가 선택 된 것으로 표시 됩니다.

```cpp
// Handle menu navigation.
bool chooseSelection = 
    (ButtonJustPressed(GamepadButtons::A) 
    || ButtonJustPressed(GamepadButtons::Menu));

bool moveUp = ButtonJustPressed(GamepadButtons::DPadUp);
bool moveDown = ButtonJustPressed(GamepadButtons::DPadDown);

switch (m_gameState)
{
case GameState::MainMenu:
    if (chooseSelection)
    {
        m_audio.PlaySoundEffect(MenuSelectedEvent);
        if (m_startGameButton.GetSelected())
        {
            m_startGameButton.SetPressed(true);
        }
        if (m_highScoreButton.GetSelected())
        {
            m_highScoreButton.SetPressed(true);
        }
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
    {
        SetGameState(GameState::MainMenu);
    }
    break;

case GameState::PostGameResults:
    if (chooseSelection || anyPoints)
    {
        SetGameState(GameState::HighScoreDisplay);
    }
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

### <a name="tracking-touch-and-mouse-input"></a>터치 및 마우스 입력 추적

터치 및 마우스 입력의 경우 메뉴 항목이 사용자가 클릭 하거나 클릭 하면 선택 됩니다. 다음 예제에서는 **MarbleMazeMain:: Update** 메서드가 포인터 입력을 처리 하 여 메뉴 항목을 선택 하는 방법을 보여 줍니다. **M \_ pointqueue** 멤버 변수는 사용자가 화면에서 작업 하거나 클릭 한 위치를 추적 합니다. 대리석 포인터를 수집 하는 방법에 대 한 자세한 내용은이 문서의 뒷부분에 나오는 [포인터 입력 처리](#processing-pointer-input)단원에서 설명 합니다.

```cpp
// Check whether the user chose a button from the UI. 
bool anyPoints = !m_pointQueue.empty();
while (!m_pointQueue.empty())
{
    UserInterface::GetInstance().HitTest(m_pointQueue.front());
    m_pointQueue.pop();
}
```

**Userinterface:: system.windows.media.visualtreehelper.hittest** 메서드는 제공 된 점이 UI 요소의 범위에 있는지 여부를 확인 합니다. 이 테스트를 통과 하는 모든 UI 요소는 작업 중인 것으로 표시 됩니다. 이 메서드는 **Pointinrect** 도우미 함수를 사용 하 여 제공 된 점이 각 UI 요소의 범위에 있는지 여부를 확인 합니다.

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

### <a name="updating-the-game-state"></a>게임 상태 업데이트

**MarbleMazeMain:: Update** 메서드는 컨트롤러 및 터치식 입력을 처리 한 후 단추를 누른 경우 게임 상태를 업데이트 합니다.

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

##  <a name="controlling-game-play"></a>게임 플레이 제어


게임 루프 및 **MarbleMazeMain:: Update** 메서드는 함께 작동 하 여 게임 개체의 상태를 업데이트 합니다. 게임에서 여러 장치에 대 한 입력을 허용 하는 경우, 유지 관리 하기 쉬운 코드를 작성할 수 있도록 모든 장치의 입력을 하나의 변수 집합에 누적 할 수 있습니다. **MarbleMazeMain:: Update** 메서드는 모든 장치에서 이동을 누적 하는 변수 집합 하나를 정의 합니다.

```cpp
float combinedTiltX = 0.0f;
float combinedTiltY = 0.0f;
```

입력 메커니즘은 입력 장치 마다 다를 수 있습니다. 예를 들어 포인터 입력은 Windows 런타임 이벤트 처리 모델을 사용 하 여 처리 됩니다. 반대로, 필요할 때 Xbox 컨트롤러에서 입력 데이터를 폴링합니다. 항상 지정 된 장치에 대해 지정 된 입력 메커니즘을 따르는 것이 좋습니다. 이 섹션에서는 대리석이 각 장치에서 입력을 읽는 방법, 조합 된 입력 값을 업데이트 하는 방법 및 결합 된 입력 값을 사용 하 여 게임 상태를 업데이트 하는 방법을 설명 합니다.

###  <a name="processing-pointer-input"></a>포인터 입력 처리

포인터 입력 작업을 하는 경우 [Windows::UI::Core::CoreDispatcher::ProcessEvents](/uwp/api/windows.ui.core.coredispatcher.processevents) 메서드를 호출하여 창 이벤트를 처리합니다. 장면을 업데이트 하거나 렌더링 하기 전에 게임 루프에서이 메서드를 호출 합니다. 대리석은 **App:: Run** 메서드에서이를 호출 합니다. 

```cpp
while (!m_windowClosed)
{
    if (m_windowVisible)
    {
        CoreWindow::GetForCurrentThread()->
            Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);

        m_main->Update();

        if (m_main->Render())
        {
            m_deviceResources->Present();
        }
    }
    else
    {
        CoreWindow::GetForCurrentThread()->
            Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);
    }
}
```

창이 표시 되 면 **CoreProcessEventsOption::P rocessallifpresent** 를 **processevents** 에 전달 하 여 대기 중인 모든 이벤트를 처리 하 고 즉시 반환 합니다. 그렇지 않으면 **CoreProcessEventsOption::P rocessoneandallpending** 를 전달 하 여 대기 중인 모든 이벤트를 처리 하 고 다음 새 이벤트를 기다립니다. 이벤트가 처리 된 후에는 대리석으로 렌더링 하 여 다음 프레임을 표시 합니다.

Windows 런타임는 발생 한 각 이벤트에 대해 등록 된 처리기를 호출 합니다. **App:: SetWindow** 메서드는 이벤트를 등록 하 고 포인터 정보를 **MarbleMazeMain** 클래스에 전달 합니다.

```cpp
void App::OnPointerPressed(
    Windows::UI::Core::CoreWindow^ sender, 
    Windows::UI::Core::PointerEventArgs^ args)
{
    m_main->AddTouch(args->CurrentPoint->PointerId, args->CurrentPoint->Position);
}

void App::OnPointerReleased(
    Windows::UI::Core::CoreWindow^ sender, 
    Windows::UI::Core::PointerEventArgs^ args)
{
    m_main->RemoveTouch(args->CurrentPoint->PointerId);
}

void App::OnPointerMoved(
    Windows::UI::Core::CoreWindow^ sender, 
    Windows::UI::Core::PointerEventArgs^ args)
{
    m_main->UpdateTouch(args->CurrentPoint->PointerId, args->CurrentPoint->Position);
}
```

**MarbleMazeMain** 클래스는 터치 이벤트를 보유 하는 맵 개체를 업데이트 하 여 포인터 이벤트에 반응 합니다. **MarbleMazeMain:: AddTouch** 메서드는 사용자가 터치 사용 장치에서 처음으로 화면을 터치 하는 경우 처럼 포인터를 처음 누를 때 호출 됩니다. **MarbleMazeMain:: UpdateTouch** 메서드는 포인터 위치가 이동 될 때 호출 됩니다. **MarbleMazeMain:: RemoveTouch** 메서드는 포인터를 놓을 때 (예: 사용자가 화면 터치를 중지할 때) 호출 됩니다.

```cpp
void MarbleMazeMain::AddTouch(int id, Windows::Foundation::Point point)
{
    m_touches[id] = PointToTouch(point, m_deviceResources->GetLogicalSize());

    m_pointQueue.push(D2D1::Point2F(point.X, point.Y));
}

void MarbleMazeMain::UpdateTouch(int id, Windows::Foundation::Point point)
{
    if (m_touches.find(id) != m_touches.end())
        m_touches[id] = PointToTouch(point, m_deviceResources->GetLogicalSize());
}

void MarbleMazeMain::RemoveTouch(int id)
{
    m_touches.erase(id);
}
```

**Pointtotouch** 함수는 현재 포인터 위치를 변환 하 여 원점이 화면 가운데에 오도록 한 다음 좌표 크기를 조정 하 여 대략-1.0에서 + 1.0 사이의 범위에 오도록 좌표를 조정 합니다. 이렇게 하면 여러 입력 메서드에서 일관 된 방식으로 미로의 기울기를 보다 쉽게 계산할 수 있습니다.

```cpp
inline XMFLOAT2 PointToTouch(Windows::Foundation::Point point, Windows::Foundation::Size bounds)
{
    float touchRadius = min(bounds.Width, bounds.Height);
    float dx = (point.X - (bounds.Width / 2.0f)) / touchRadius;
    float dy = ((bounds.Height / 2.0f) - point.Y) / touchRadius;

    return XMFLOAT2(dx, dy);
}
```

**MarbleMazeMain:: Update** 메서드는 기울기 계수를 상수 배율 값 만큼 증가 시켜 결합 된 입력 값을 업데이트 합니다. 이 크기 조정 값은 여러 다른 값을 시험해 보면 결정 됩니다.

```cpp
// Account for touch input.
for (TouchMap::const_iterator iter = m_touches.cbegin(); 
    iter != m_touches.cend(); 
    ++iter)
{
    combinedTiltX += iter->second.x * m_touchScaleFactor;
    combinedTiltY += iter->second.y * m_touchScaleFactor;
}
```

### <a name="processing-accelerometer-input"></a>가 속도계 입력 처리

가 속도계 입력을 처리 하기 위해 **MarbleMazeMain:: Update** 메서드는 [Windows::D Evices:: 센서인::가 속도계:: GetCurrentReading](/uwp/api/windows.devices.sensors.accelerometer.getcurrentreading) 메서드를 호출 합니다. 이 메서드는가 속도계 읽기를 나타내는 [Windows::D evices:: 센서인:: AccelerometerReading](/uwp/api/Windows.Devices.Sensors.AccelerometerReading) 개체를 반환 합니다. **Windows::D evices:: 센서인:: AccelerometerReading:: AccelerationX** and **Windows::D Evices:: 센서인:: AccelerometerReading:: AccelerationY** 속성은 각각 X 및 Y 축을 따라 g-force 가속을 보유 합니다.

다음 예제에서는 **MarbleMazeMain:: Update** 메서드에서가 속도계를 폴링하고 결합 된 입력 값을 업데이트 하는 방법을 보여 줍니다. 장치를 기울이는 경우 무게는 대리석을 더 빠르게 이동 시킵니다.

```cpp
// Account for sensors.
if (m_accelerometer != nullptr)
{
    Windows::Devices::Sensors::AccelerometerReading^ reading =
        m_accelerometer->GetCurrentReading();

    if (reading != nullptr)
    {
        combinedTiltX += 
            static_cast<float>(reading->AccelerationX) * m_accelerometerScaleFactor;

        combinedTiltY += 
            static_cast<float>(reading->AccelerationY) * m_accelerometerScaleFactor;
    }
}
```

사용자의 컴퓨터에가 속도계이 있는지 확인할 수 없기 때문에가 속도계를 폴링하기 전에 항상 유효한 [가 속도계](/uwp/api/Windows.Devices.Sensors.Accelerometer) 개체가 있는지 확인 해야 합니다.

### <a name="processing-xbox-controller-input"></a>Xbox 컨트롤러 입력 처리

**MarbleMazeMain:: Update** 메서드에서 **m_newReading** 를 사용 하 여 왼쪽 아날로그 스틱의 입력을 처리 합니다.

```cpp
float leftStickX = static_cast<float>(m_newReading.LeftThumbstickX);
float leftStickY = static_cast<float>(m_newReading.LeftThumbstickY);

auto oppositeSquared = leftStickY * leftStickY;
auto adjacentSquared = leftStickX * leftStickX;

if ((oppositeSquared + adjacentSquared) > m_deadzoneSquared)
{
    combinedTiltX += leftStickX * m_controllerScaleFactor;
    combinedTiltY += leftStickY * m_controllerScaleFactor;
}
```

왼쪽 아날로그 스틱의 입력이 데드 영역 외부에 있는지 확인 하 고, 있는 경우 **combinedTiltX** 및 **combinedTiltY** (배율 계수를 곱하여)에 추가 하 여 스테이지를 기울입니다.

> [!IMPORTANT]
> Xbox 컨트롤러를 사용 하는 경우 항상 데드 영역을 고려 합니다. 데드 영역은 초기 이동에 대 한 민감도에서 gamepads 간의 차이를 나타냅니다. 일부 컨트롤러에서는 약간의 이동으로 인해 읽기가 생성 되지 않을 수 있지만, 그 밖의 경우에는 측정 가능한 읽기가 생성 될 수도 있습니다. 게임에서이를 고려 하 여 초기 엄지 스틱 움직임에 대해 이동 하지 않는 영역을 만듭니다. 데드 영역에 대 한 자세한 내용은 [Thumbsticks 읽기](gamepad-and-vibration.md#reading-the-thumbsticks)를 참조 하십시오.

 

###  <a name="applying-input-to-the-game-state"></a>게임 상태에 입력 적용

장치는 입력 값을 다양 한 방식으로 보고 합니다. 예를 들어 포인터 입력은 화면 좌표에 있을 수 있고 컨트롤러 입력은 완전히 다른 형식일 수 있습니다. 여러 장치의 입력을 하나의 입력 값 집합으로 결합 하는 한 가지 문제는 정규화 또는 값을 공통 형식으로 변환 하는 것입니다. 대리석은-1.0, 1.0 범위로 크기를 조정 하 여 값을 정규화 \[ \] 합니다. 이 섹션에서 설명 하는 **Pointtotouch** 함수는 화면 좌표를 대략-1.0에서 + 1.0 사이의 정규화 된 값으로 변환 합니다.

> [!TIP]
> 응용 프로그램에서 하나의 입력 방법을 사용 하는 경우에도 항상 입력 값을 정규화 하는 것이 좋습니다. 이렇게 하면 게임의 다른 구성 요소 (예: 물리학 시뮬레이션)에서 입력을 해석 하는 방법이 간단해 지 며, 다른 화면 해상도에서 작동 하는 게임을 보다 쉽게 작성할 수 있습니다.

 

**MarbleMazeMain:: Update** 메서드는 입력을 처리 한 후에는 대리석의 미로의 기울기 효과를 나타내는 벡터를 만듭니다. 다음 예제에서는 Marble Maze가 [XMVector3Normalize](/windows/desktop/api/directxmath/nf-directxmath-xmvector3normalize) 함수를 사용하여 정규화된 중력 벡터를 만드는 방법을 보여 줍니다. **Maxtilt** 변수는 미로가 tilts 하는 양을 제한 하 고 그 쪽에서 미로의 tilting을 방지 합니다.

```cpp
const float maxTilt = 1.0f / 8.0f;

XMVECTOR gravity = XMVectorSet(
    combinedTiltX * maxTilt, 
    combinedTiltY * maxTilt, 
    1.0f, 
    0.0f);

gravity = XMVector3Normalize(gravity);
```

장면 개체의 업데이트를 완료 하기 위해 대리석 무늬 메 이즈는 업데이트 된 중력 벡터를 물리학 시뮬레이션으로 전달 하 고, 이전 프레임 이후 경과 된 시간에 대 한 물리 시뮬레이션을 업데이트 하 고, 대리석의 위치와 방향을 업데이트 합니다. 대리석이 미로를 통해 떨어져 있는 경우 **MarbleMazeMain:: Update** 메서드는 대리석에 해당 하는 마지막 검사점에서 대리석을 다시 배치 하 고 물리학 시뮬레이션의 상태를 다시 설정 합니다.

```cpp
XMFLOAT3A g;
XMStoreFloat3(&g, gravity);
m_physics.SetGravity(g);

if (m_gameState == GameState::InGameActive)
{
    // Only update physics when gameplay is active.
    m_physics.UpdatePhysicsSimulation(static_cast<float>(m_timer.GetElapsedSeconds()));

    // ...Code omitted for simplicity...

}

// ...Code omitted for simplicity...

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

이 섹션에서는 물리학 시뮬레이션의 작동 방식에 대해 설명 하지 않습니다. 이에 대 한 자세한 내용은 **대리석의** 대리석 및 물리학의 물리학를 **참조 하십시오.**

## <a name="next-steps"></a>다음 단계


오디오를 사용할 때 염두에 두어야 하는 몇 가지 주요 방법에 대 한 자세한 내용은 [대리석으로 오디오 추가를](adding-audio-to-the-marble-maze-sample.md) 참조 하세요. 이 문서에서는 Marble Maze가 Microsoft 미디어 파운데이션 및 XAudio2를 사용하여 오디오 리소스를 로드, 믹싱 및 재생하는 방법을 설명합니다.

## <a name="related-topics"></a>관련 항목


* [Marble Maze 샘플에 오디오 추가](adding-audio-to-the-marble-maze-sample.md)
* [Marble Maze 샘플에 시각적 콘텐츠 추가](adding-visual-content-to-the-marble-maze-sample.md)
* [C + + 및 c + +의 UWP 게임, 대리석](developing-marble-maze-a-windows-store-game-in-cpp-and-directx.md)

 

 