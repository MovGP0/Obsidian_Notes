```csharp
public static void UseRecurringCareCenterJobs(this IApplicationBuilder app)
{
	var environment = app.ApplicationServices.GetRequiredService<IHostEnvironment>();
	var jobManager = app.ApplicationServices.GetRequiredService<IRecurringJobManager>();
	var timeZoneInfo = NodaTime.DateTimeZoneProviders.Tzdb["Europe/Vienna"].ToTimeZoneInfo();

	ClearJobs();
	if (environment.IsDevelopment()) return;

	var job = Job.FromExpression<IMyJob>(s => s.ExecuteAsync(null!, JobCancellationToken.Null));
	recurringJobs.AddOrUpdate("JOB DESCRIPTION", job, "*/30 * * * *", timeZoneInfo, JobQueueNames.MyQueueName);

    // TODO: schedule additional jobs here
}

private static void ClearJobs()
{
	using var connection = JobStorage.Current.GetConnection();
	foreach (var recurringJob in connection.GetRecurringJobs())
	{
		RecurringJob.RemoveIfExists(recurringJob.Id);
	}
}
```