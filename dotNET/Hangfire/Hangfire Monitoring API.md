```csharp
public static class MonitoringApiExtensions
{
    public static bool HasExisting(this Storage.IMonitoringApi api, Job? inputJob, string jobQueueName, CancellationToken cancellationToken)
    {
        if (inputJob is null) return true;
        if (cancellationToken.IsCancellationRequested) return true;

        var inProcess = api.ProcessingJobs(0, int.MaxValue)
            .Select(x => x.Value.Job)
            .Any(job => job.Method.Equals(inputJob.Method)
            && job.Args.ToArray()[..^2].SequenceEqual(inputJob.Args.ToArray()[..^2]));

        if (inProcess) return true;
        if (cancellationToken.IsCancellationRequested) return true;

        var isEnqueued = api.EnqueuedJobs(jobQueueName, 0, int.MaxValue)
            .Select(x => x.Value.Job)
            .Any(job => job.Method.Equals(inputJob.Method)
            && job.Args.ToArray()[..^2].SequenceEqual(inputJob.Args.ToArray()[..^2]));

        if (isEnqueued) return true;
        if (cancellationToken.IsCancellationRequested) return true;

        var isEnqueuedInDefault = api.EnqueuedJobs("default", 0, int.MaxValue)
            .Select(x => x.Value.Job)
            .Any(job => job.Method.Equals(inputJob.Method)
            && job.Args.ToArray()[..^2].SequenceEqual(inputJob.Args.ToArray()[..^2]));

        if (isEnqueuedInDefault) return true;
        if (cancellationToken.IsCancellationRequested) return true;

        var isScheduled = api.ScheduledJobs(0, int.MaxValue)
            .Select(x => x.Value.Job)
            .Any(job => job.Method.Equals(inputJob.Method)
            && job.Args.ToArray()[..^2].SequenceEqual(inputJob.Args.ToArray()[..^2]));

        if (cancellationToken.IsCancellationRequested) return true;
        return isScheduled;
    }
}
```