## Basic Binding Syntax
```xml
<TextBox Text="{Binding Path=UserName, Mode=OneWay, UpdateSourceTrigger=PropertyChanged}" />
```

```csharp
public class MyViewModel : INotifyPropertyChanged
{
    private string _userName;
    public string UserName
    {
        get => _userName;
        set
        {
            if (_userName != value)
            {
                _userName = value;
                OnPropertyChanged(nameof(UserName));
            }
        }
    }

    public event PropertyChangedEventHandler PropertyChanged;

    protected void OnPropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

Using [[ReactiveUI]]:
```csharp
public sealed class MyViewModel : ReactiveObject
{
	[Reactive]
    public string UserName { get; set; } = string.Empty;
}
```

## Binding with `ViewModel`

In `SearchResultControl.xaml`:
```xml
<reactiveUi:ReactiveUserControl
    x:Name="SearchResultControl"
    x:TypeArguments="local:SearchResultViewModel"

    x:Class="ChoreoHelper.SearchResult.SearchResultControl"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    xmlns:reactiveUi="http://reactiveui.net"
    mc:Ignorable="d"
    d:DesignHeight="450"
    d:DesignWidth="800"

	d:DataContext="{d:DesignInstance local:MyViewModel, IsDesignTimeCreatable=True}"
	DataContext="{Binding Path=ViewModel, RelativeSource={RelativeSource Self}}">
```

**Notes:**

- the `RelativeSource` to `Self` may not work. Consider alternative approaches.
- when the `DataContext` is set programmatically, the binding is not required

## Value Converters

```xml
<TextBlock Text="{Binding UserName, Converter={StaticResource UpperCaseConverter}}" />
```

```csharp
public class UpperCaseConverter : IValueConverter
{
    public object Convert(object value, Type targetType, object parameter, CultureInfo culture)
    {
        return value?.ToString().ToUpper();
    }

    public object ConvertBack(object value, Type targetType, object parameter, CultureInfo culture)
    {
        return value?.ToString().ToLower();
    }
}
```

## Binding with `ComboBox`

**Note:** When using `ListView`, the same pattern can be used.

Declare the collection:
```csharp
public sealed class MyViewModel : ReactiveObject
{
    public IObservableCollection<CustomerViewModel> Customers { get; }  
	    = new ObservableCollectionExtended<CustomerViewModel>();

	public CustomerViewModel? SelectedCustomer { get; set; }
}
```

Wrap the collection in an `CollectionViewSource` for grouping and/or sorting:

```xml
<UserControl.Resources>  
    <ResourceDictionary>  
        <ResourceDictionary.MergedDictionaries>  
            <design:ResourceDictionaryProvider />  
            <ResourceDictionary>
                <CollectionViewSource  
                    x:Key="GroupedAndSortedCustomers"  
                    Source="{Binding Path=ViewModel.Customers, ElementName=CustomerSearchControl}">

					<!-- Grouping -->
                    <CollectionViewSource.GroupDescriptions>  
                        <PropertyGroupDescription PropertyName="Category" />  
                    </CollectionViewSource.GroupDescriptions>

					<!-- Sorting -->
                    <CollectionViewSource.SortDescriptions>  
                        <componentModel:SortDescription PropertyName="Category" Direction="Ascending" />  
                        <componentModel:SortDescription PropertyName="Name" Direction="Ascending" />  
                    </CollectionViewSource.SortDescriptions>  
                </CollectionViewSource>  
            </ResourceDictionary>  
        </ResourceDictionary.MergedDictionaries>  
    </ResourceDictionary>  
</UserControl.Resources>
```

Usage in `ComboBox`:

```xml
<ComboBox Padding="4" Width="400"  
          SelectedItem="{Binding SelectedCustomer, Mode=TwoWay, UpdateSourceTrigger=PropertyChanged}"  
          ItemsSource="{Binding Source={StaticResource GroupedAndSortedCustomers}}}">

    <!-- View for the group -->
    <ComboBox.GroupStyle>  
        <GroupStyle>  
            <GroupStyle.HeaderTemplate>  
                <DataTemplate DataType="{x:Type CollectionViewGroup}">
					<TextBlock FontWeight="Bold" Text="{Binding Name}" />
				</DataTemplate> 
            </GroupStyle.HeaderTemplate>  
        </GroupStyle>  
    </ComboBox.GroupStyle>

    <!-- View for the items -->
    <ComboBox.ItemTemplate>  
        <DataTemplate DataType="{x:Type dance:CustomerViewModel}">  
            <TextBlock Text="{Binding Name}" />  
        </DataTemplate>  
    </ComboBox.ItemTemplate>  
</ComboBox>
```

**Note:** make sure the binding is working. Note the debug output.
