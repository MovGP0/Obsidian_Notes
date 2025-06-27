```csharp
public sealed class CustomersCacheBehaviour : IBehaviour<OrderManagerViewModel>
{
	public void Activate(OrderManagerViewModel viewModel, CompositeDisposable d)  
	{  
		var cache = new SourceCache<CustomerViewModel, int>(e => e.Id).DisposeWith(d);  

		viewModel
			.WhenAnyValue(vm => vm.SearchCustomer)  
			.Throttle(TimeSpan.FromMilliseconds(200))  
			.Select(_ => viewModel)  
			.SelectMany(async (CustomersViewModel vm, CancellationToken ct) =>  
			{
				using (vm.Scoped<IMyDbContext>(out var db))  
				{  
					// TODO: get data
					return viewModels;
				}
				catch (Exception e)  
				{  
					Log.Here().Error(e, e.Message);  
					MessageBox.Error(e, e.Message);  
				}  

				return Array.Empty<CustomerInfoViewModel>();  
			})  
			.ObserveOn(RxApp.MainThreadScheduler)  
			.SubscribeOn(RxApp.TaskpoolScheduler)  
			.Subscribe(items =>  
			{  
				cache.Update(items, item => item.Id, true);  
			})  
			.DisposeWith(d);  

		cache  
			.Connect()  
			.Bind(viewModel.Customers)  
			.ObserveOn(RxApp.MainThreadScheduler)  
			.SubscribeOn(RxApp.MainThreadScheduler)  
			.Subscribe()  
			.DisposeWith(d);
	}
```
