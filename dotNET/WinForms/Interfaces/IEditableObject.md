Provides functionality to commit or rollback changes to an object that is used as a data source.

## Example

```csharp
public class YourEditableObject : IEditableObject
{
    private YourObject backup; // Keep a backup of the original object state
    private bool isEditing; // Track whether the object is currently being edited

    // Implement the required members of the IEditableObject interface

    public void BeginEdit()
    {
        if (!isEditing)
        {
            // Create a backup of the original object state
            backup = this.MemberwiseClone() as YourObject;
            isEditing = true;
        }
    }

    public void CancelEdit()
    {
        if (isEditing)
        {
            // Restore the object to its original state from the backup
            if (backup != null)
            {
                // Copy the backup properties back to the current object
                this.Property1 = backup.Property1;
                this.Property2 = backup.Property2;
                // Set other properties accordingly
            }

            isEditing = false;
        }
    }

    public void EndEdit()
    {
        if (isEditing)
        {
            // Clear the backup object and mark editing as finished
            backup = null;
            isEditing = false;
        }
    }

    // Additional members of your editable object class
}
```

## See also

- [IEditableObject](https://learn.microsoft.com/en-us/dotnet/api/system.componentmodel.ieditableobject)