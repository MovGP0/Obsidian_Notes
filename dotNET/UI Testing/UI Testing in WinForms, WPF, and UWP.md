## UI Automation (UIA)

- Directly uses the Windows Automation API (**Microsoft Active Accessibility (MSAA)** and **UI Automation (UIA)** APIs) exposed by the OS
- Supported by WinForms, WPF, UWP, and native Win32 apps

## Coded UI Tests (CUIT)

> [!important]
> Deprecated since VS 2019

- Microsoft Visual Studio feature that recorded and replayed UIA and MSAA calls.
- Under the covers, it generates C# code that leverages the `System.Windows.Automation` namespace or legacy MSAA APIs to locate and act on controls

## Appium + WinAppDriver

Appium is an open-source WebDriver-based automation framework originally for mobile.

For Windows desktop, Microsoft’s WinAppDriver implements the Appium Server protocol, translating WebDriver (JSON-Wire over HTTP) calls into UIA operations.

Tests issue standard Selenium/WebDriver commands to WinAppDriver’s Appium endpoint.

### Setup Appium

- Enable [Developer Mode](https://docs.microsoft.com/en-us/windows/apps/get-started/enable-your-device-for-development) in Windows Settings.
- Download, install, and run [WinAppDriver](https://github.com/Microsoft/WinAppDriver/releases).
- When using *DevExpress*, turn on the global *WindowsFormsSettings.UseUIAutomation* property
- Install the `Appium.WebDriver` NuGet package in the unit test project.

## Ranorex Studio

> [!important]
> - Licensing costs for small teams
> - .NET only; less ideal if stack is polyglot

Ranorex communicates with applications through the Windows accessibility stack (MSAA/UIA) and, where necessary, through image-based recognition for non-standard controls.

```
[Your App (WinForms/WPF)] ←─ UIA/MSAA & GDI/GDI+ Plugins ─→ [Ranorex Core Engine] ←→ [Ranorex IDE / API]
```

## Tricentis Tosca

> [!important] 
> High cost of ownership; steeper learning curve for model-based paradigms compared to script-based tools.

Tosca uses its "TBox" engine modules (e.g., .NET, WPF) to interface with application controls via UI Automation APIs or image-based scanning.

```
[Tosca XScan] → Generates → [Tosca Modules (WPF, .NET)]  
[TestCases] → Execute → [Tosca Execution Engine] → UIA/MSAA → [Your App]
```

## FlaUI

[FlaUI](https://github.com/FlaUI/FlaUI) is a wrapper for the UI Automation libraries.

### FlaUInspect

[FlaUInspect](https://github.com/FlaUI/FlaUInspect) Visualizes what FlaUI can access of the application.

Installation
```sh
choco install FlaUInspect
```

Start
```sh
FlaUInspect
```

### Accessibility Insights for Windows

Microsoft Tool for inspecting accessibility data.

- [Accessibility Insights](https://accessibilityinsights.io/docs/windows/overview/)
