Create a `BindingSource` and `XPServerCollectionSource` (or `XPInstantFeedbackSource` for large data sources):
```csharp
// Initialize a new UnitOfWork
UnitOfWork uow = new UnitOfWork();

// Create a new XPServerCollectionSource
XPServerCollectionSource xpServerCollectionSource = new XPServerCollectionSource(uow, typeof(Person));

// Create and configure a new BindingSource
BindingSource bindingSource = new BindingSource
{
    DataSource = xpServerCollectionSource
};
```

Bind the `BindingSource` to the `GridControl`
```csharp
XtraGrid gridControl = // ...
gridControl.DataSource = bindingSource;
```

Save changes in the database
```csharp
uow.CommitChanges();
```

Reload data using the `XPServerCollectionSource`
```csharp
xpServerCollectionSource.Reload();
```