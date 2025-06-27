### Binding to the Current `DataContext`

**Description**: Binds to the current `DataContext` of the element unless explicitly overridden.

```xml
<TextBox Text="{Binding Path=Name}" />
```
- The `Path=Name` refers to a property of the object set as the `DataContext` of the control.

**Notes:**
- If no `DataContext` is set, the binding won't work unless another source is provided.

### Binding to the Current Element (`Self`)

Binds to the element itself (its properties or methods).

Used to bind a property of the element to another property of the same element.

```xml
<TextBox Text="{Binding RelativeSource={RelativeSource Self}, Path=Tag}" Tag="Hello" />
```

### Binding to an Ancestor Element

Finds a parent element of a specified type in the visual tree.

```xml
<TextBlock Text="{Binding RelativeSource={RelativeSource AncestorType=Window}, Path=Title}" />
```

Variations:
- **AncestorType**: The type of ancestor to search for.
- **AncestorLevel**: Specifies which occurrence of the ancestor to find (e.g., the first or second).

Example with Multiple Levels:
```xml
<TextBox Text="{Binding RelativeSource={RelativeSource AncestorType=Grid, AncestorLevel=2}, Path=Tag}" />
```

### Binding to an Element by Name (`ElementName`)

References a specific element by its `x:Name` in the same XAML namescope.

```xml
<TextBox x:Name="SourceTextBox" Text="Hello" />
<TextBlock Text="{Binding ElementName=SourceTextBox, Path=Text}" />
```

**Notes:**

- Requires that both elements be in the same namescope.
- Commonly used to bind properties between sibling elements.

### Binding to a Static Resource

**Description**: Binds to a resource defined in the `Resources` section.

```xml
<TextBlock Text="{Binding Source={StaticResource MyStringResource}}" />
```

**Notes:**
- This is useful for global, reusable resources like strings, brushes, or styles.

### Binding to a Static Property (`x:Static`)

**Description**: Binds to a static property or field in a class.

```xml
<TextBox Text="{Binding Source={x:Static local:MyClass.MyStaticProperty}}" />
```

- The `Source` points to a static property using `{x:Static}`.

### Binding to a `Freezable` (Inherited Context)

**Description**: WPF's `Freezable` objects (e.g., `Brush`, `Transform`) do not automatically inherit `DataContext`. You can use a `BindingProxy` to enable data binding.

**Example**
```xml
<Window.Resources>
    <local:BindingProxy x:Key="Proxy" Data="{Binding}" />
</Window.Resources>

<Ellipse Fill="{Binding Data.BrushColor, Source={StaticResource Proxy}}" />
```

### Binding to an Object Relative to the Data Context

**Description**: Use the `Path` to traverse nested properties in the `DataContext`.

```xml
<TextBox Text="{Binding Path=Address.Street}" />
```

**Notes:**
- Allows traversal through complex object hierarchies.

### Binding with an Explicit `Source`

**Description**: Provides an explicit data source that overrides the `DataContext`.

```xml
<TextBox Text="{Binding Source={local:MyStaticObject}, Path=Name}" />
```

**Notes:**
- Commonly used for global singletons or explicitly defined objects.

### Binding to a Parent Data Template or ItemsControl

**Description**: Bind to the data context of a parent `DataTemplate`, `ControlTemplate`, or `ItemsControl`.

Example (Inside `DataTemplate`):
```xml
<DataTemplate>
    <TextBlock Text="{Binding RelativeSource={RelativeSource TemplatedParent}, Path=Content}" />
</DataTemplate>
```

### Binding with `FindAncestor` in Multi-level Templates

**Description**: Useful when dealing with nested templates (e.g., `ItemsControl` inside `DataTemplate`).

```xml
<TextBlock Text="{Binding RelativeSource={RelativeSource AncestorType=ListView}, Path=SelectedItem}" />
```

### Binding to `Application` or Singleton Instances

Bind to a property or object accessible via the `Application` instance.

```xml
<TextBlock Text="{Binding Source={x:Static Application.Current}, Path=MainWindow.Title}" />
```

**Notes:**
- Used for global state management.

### Binding to a CollectionView

Use `CollectionViewSource` or `View` to bind to sorted, grouped, or filtered data.

```xml
<ListView ItemsSource="{Binding Source={StaticResource MyCollectionViewSource}}" />
```

### Binding to an Attached Property

Allows to bind to attached properties.

```xml
<TextBox Text="{Binding Path=(local:AttachedProperty.MyAttachedProperty)}" />
```

### `MultiBinding`

Combines multiple sources into one binding using a `Converter`.

```xml
<TextBlock>
    <TextBlock.Text>
        <MultiBinding StringFormat="{}{0}, {1}">
            <Binding Path="FirstName" />
            <Binding Path="LastName" />
        </MultiBinding>
    </TextBlock.Text>
</TextBlock>
```

### `PriorityBinding`

Uses the first available source from a list.

```xml
<TextBox Text="{PriorityBinding}">
    <PriorityBinding.Bindings>
        <Binding Path="FastSource" />
        <Binding Path="SlowSource" />
    </PriorityBinding.Bindings>
</TextBox>
```
