### Key Components of Routed Commands

1. **Command**: Represents the action to be performed (e.g., `Copy`, `Cut`, `Save`).
2. **Command Binding**: Associates the command with the handler logic.
3. **Command Source**: The UI element that invokes the command (e.g., `Button`, `MenuItem`).
4. **Command Target**: The element that handles the command (e.g., a `TextBox` for `Copy`).

### How Routed Commands Work

1. **User Interaction**:
    - A user interacts with a command source, like clicking a `Button`.
2. **Command Execution**:
    - The `Command` specified in the `Button.Command` property is invoked.
3. **Routing**:
    - The command routing system looks for the appropriate `CommandBinding` in the visual or logical tree.
4. **Handler Invocation**:
    - If a `CommandBinding` with a matching command is found, its `Executed` handler is invoked.
5. **Enabled/Disabled State**:
    - The `CanExecute` handler determines if the command source (e.g., the `Button`) is enabled.

### Example

#### Step 1: Define the Routed Command

WPF provides a set of predefined commands (e.g., `ApplicationCommands`, `EditingCommands`). You can also create your own custom commands.

```csharp
public static class CustomCommands
{
    public static readonly RoutedUICommand SayHello = new RoutedUICommand(
        "Say Hello",  // Text
        "SayHello",   // Name
        typeof(CustomCommands) // Owner type
    );
}
```

#### Step 2: Create Command Bindings

Bind the command to the logic that handles its execution.

```csharp
public partial class MainWindow : Window
{
    public MainWindow()
    {
        InitializeComponent();

        CommandBinding helloCommandBinding = new CommandBinding(
            CustomCommands.SayHello,
            ExecuteSayHello,
            CanExecuteSayHello);

        this.CommandBindings.Add(helloCommandBinding);
    }

    private void ExecuteSayHello(object sender, ExecutedRoutedEventArgs e)
    {
        MessageBox.Show("Hello, World!");
    }

    private void CanExecuteSayHello(object sender, CanExecuteRoutedEventArgs e)
    {
        e.CanExecute = true; // Command is always enabled
    }
}
```

#### Step 3: Add Command Source in XAML

Associate the command with a UI element, such as a `Button`.

```xml
<Window x:Class="WpfApp.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:local="clr-namespace:WpfApp"
        Title="Routed Command Example" Height="200" Width="300">
    <Grid>
        <Button Command="{x:Static local:CustomCommands.SayHello}"
                Content="Say Hello"
                HorizontalAlignment="Center"
                VerticalAlignment="Center" />
    </Grid>
</Window>
```

### How Commands Are Routed

1. **Route Traversal**:
    - Commands are routed through the visual and logical tree until a matching `CommandBinding` is found.
    - Routing follows the **bubbling** mechanism by default.
2. **Routing Order**:
    - The search starts from the `CommandTarget` (if specified).
    - If no `CommandTarget` is specified, it starts from the `CommandSource` (e.g., `Button`) and moves up the tree.

### Predefined Commands

WPF provides several built-in commands organized into categories:

1. **Application Commands**:
    - `ApplicationCommands.New`, `ApplicationCommands.Open`, `ApplicationCommands.Save`
2. **Editing Commands**:
    - `EditingCommands.Copy`, `EditingCommands.Cut`, `EditingCommands.Paste`
3. **Media Commands**:
    - `MediaCommands.Play`, `MediaCommands.Stop`, `MediaCommands.Record`
4. **Navigation Commands**:
    - `NavigationCommands.BrowseBack`, `NavigationCommands.BrowseForward`

These commands already have predefined `Text`, `Name`, and `InputGestures`.

### Handling Command Enable/Disable

The `CanExecute` handler determines whether a command can currently execute. This is particularly useful for enabling or disabling buttons or other UI elements dynamically.

#### Example:

```csharp
private void CanExecuteSayHello(object sender, CanExecuteRoutedEventArgs e)
{
    e.CanExecute = someCondition; // Enable based on a condition
}
```

In XAML, the command source (e.g., `Button`) will automatically update its `IsEnabled` property based on `CanExecute`.

### Customizing Input Gestures

Commands can have custom input gestures like keyboard shortcuts.

#### Example: Add Custom Gesture

```csharp
CustomCommands.SayHello.InputGestures.Add(new KeyGesture(Key.H, ModifierKeys.Control));
```

This allows the command to be executed with `Ctrl+H`.

### Command Target

The `CommandTarget` property explicitly specifies the element that should handle the command.

#### Example:

```xml
<Button Command="{x:Static ApplicationCommands.Copy}" CommandTarget="{Binding ElementName=MyTextBox}" />
<TextBox x:Name="MyTextBox" />
```

In this case, the `TextBox` becomes the target for the `Copy` command, and the command routes directly to it.

### Advantages of Routed Commands

1. **Separation of Concerns**:
    - The command logic is decoupled from the UI element that triggers it.
2. **Reusable Commands**:
    - Commands can be reused across multiple controls and views.
3. **Automatic UI Updates**:
    - The `CanExecute` logic automatically enables or disables the associated controls.
4. **Input Gesture Support**:
    - Easily add keyboard shortcuts or gestures to commands.
5. **Routing Flexibility**:
    - Commands can bubble up the visual/logical tree, enabling dynamic command handling.
