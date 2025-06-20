### TagUI

- **Description**: Cross-platform RPA tool that automates web, desktop, and command-line tasks via a human-readable scripting language.
- **UI Interaction**: Uses native UI Automation (UIA/MSAA) for standard controls and can fall back to image-based actions.
- **AI Integration**: Can invoke external LLM APIs (e.g., via `url` or `python` steps) to decide next actions, enabling rudimentary agentic behavior.

### OpenRPA

- **Description**: Enterprise-grade, MIT-licensed RPA platform with a visual flow designer and built-in Node-RED for messaging.
- **UI Interaction**: Leverages Windows UIA/MSAA and can embed Python scripts or REST calls in flows.
- **AI Integration**: Flows can call AI services or local LLMs to make decisions—effectively turning flows into agents that react based on model outputs.

### Robot Framework + FlaUI

- **Description**: Keyword-driven automation framework; `robotframework-flaui` is a wrapper around the .NET FlaUI library for WinForms/WPF.
- **UI Interaction**: Directly drives UIA through FlaUI, offering reliable element locators and actions.
- **AI Integration**: Robot Framework’s Python-based libraries (including custom ones) can integrate LLM calls, enabling test cases that adapt based on AI reasoning.

### SikuliX

- **Description**: Vision-based automation using OpenCV to locate UI elements by screenshot matching.
- **UI Interaction**: Purely image recognition; clicks, types, and reads text via OCR.
- **AI Integration**: Scripts (in Jython or Java) can embed LLM calls for decision logic—agents can choose next steps based on screen content and AI prompts.

### PyAutoGUI + LangChain/LangGraph

- **Description**: PyAutoGUI provides low-level keyboard/mouse automation in Python; LangChain/LangGraph orchestrates AI agents and tool use.
- **UI Interaction**: Coordinates-based actions or simple image matching.
- **AI Integration**: LangChain agents can use PyAutoGUI as a “tool” in their agent toolkit, issuing UI commands based on LLM planning outputs—true agentic desktop control.

### AutoHotkey + ChatGPT

- **Description**: AutoHotkey is a lightweight Windows scripting language; community examples show integrating it with ChatGPT for script generation.
- **UI Interaction**: Hotkeys and coordinate/UIA calls.
- **AI Integration**: AHK scripts can call OpenAI or local LLM APIs to compute next actions, dynamically generating AHK commands.

### UiPath Desktop Automation

> [!important] 
> Commercial offer

- **Native Desktop Recording & Scripting**
    - UiPath Studio offers a **Record App** feature that captures interactions with any Windows application—WinForms, WPF, or legacy VB.NET—by listening to UI Automation (UIA)/MSAA events and screen coordinates.
    - Developers can also write **low-code or coded** workflows in C# or VB.NET directly against native UI elements via the UiPath **.NET Activities** pack.

- **Robotic Desktop Automation (RDA)**
    - UiPath’s RDA model runs **attended** robots that execute workflows locally on a user’s machine, reacting to desktop events in real time.
    - These robots tap into the Windows accessibility APIs (UIA/MSAA) and, when needed, use image-based selectors for non-standard controls.

- **UiPath Orchestrator**
    - Deploy, schedule, and monitor desktop robots—agentic or otherwise—across your organization, ensuring consistent test execution on both desktop and web targets.
