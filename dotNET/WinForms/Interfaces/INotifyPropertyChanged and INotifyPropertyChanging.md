## Example

```csharp
using System.ComponentModel;

public class Person : INotifyPropertyChanged, INotifyPropertyChanging
{
    private string firstName;
    private string lastName;

    public string FirstName
    {
        get { return firstName; }
        set
        {
            if (firstName != value)
            {
                OnPropertyChanging(nameof(FirstName));
                firstName = value;
                OnPropertyChanged(nameof(FirstName));
            }
        }
    }

    public string LastName
    {
        get { return lastName; }
        set
        {
            if (lastName != value)
            {
                OnPropertyChanging(nameof(LastName));
                lastName = value;
                OnPropertyChanged(nameof(LastName));
            }
        }
    }

    public event PropertyChangedEventHandler PropertyChanged;
    public event PropertyChangingEventHandler PropertyChanging;

    protected virtual void OnPropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }

    protected virtual void OnPropertyChanging(string propertyName)
    {
        PropertyChanging?.Invoke(this, new PropertyChangingEventArgs(propertyName));
    }
}
```

Using `ReactiveUI` + `Fody.ReactiveUI`
```csharp
using ReactiveUI;
using ReactiveUI.Fody.Helpers;

[AddINotifyPropertyChangedInterface]
[AddReactiveNotifyPropertyChangedInterface]
public class Person : ReactiveObject
{
    [Reactive] public string FirstName { get; set; }
    [Reactive] public string LastName { get; set; }
}
```
