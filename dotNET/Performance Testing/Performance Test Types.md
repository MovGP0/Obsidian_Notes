| Type                          | Description                                                             |
| ----------------------------- | ----------------------------------------------------------------------- |
| Cold Start Tests              | First run of the function (JIT, Cache)                                  |
| Warmed Up Tests               | Consecutive runs of the function                                        |
| Asymptotic Tests              | Determines Complexity with increasing data size. ie. O(1), O(n), O(n^2) |
| Latency and Throughput        | How much requests can be processed?                                     |
| Unit/Integration/System tests | Can also provide performance estimates                                  |
| Monitoring and Telemetry      | Production performance in real-time                                     |
| Other tests                   | Stress tests, User interface test, Fuzzying, etc.                       |
| Portability                   | Teston different (slow) hardware or Operating System                    |

## Cold Start Tests

Depend on the scope of the test:
- Method
- Feature
- Application start
- Operating System cold start
- Setup in a clean Operating System

## Asymptotic Tests

Probe the function with various data sizes and build a regression model to predict how the function will behave with large data sizes.

ie. Test with many small files of various sizes to estimate how the function will behave with large files.

Drawbacks:
- Takes many execution to get confidence in the result
- Regression model might not reflect the true execution times
- Complex implementation to build the regression model

## Monitoring

| Type                     | Description / Smell                                   |
| ------------------------ | ----------------------------------------------------- |
| Degradation              | Something becomes slow that was fast before           |
| Acceleration             | Something becomes fast that was slow before           |
| Temporal Clustering      | Something changes for multiple tests at the same time |
| Spacial Clustering       | Performance depends on environment                    |
| Hudge duration           | Something takes too much time                         |
| Hudge variance           | Timing chages a lot without changes in the code       |
| Hudge Outliers           | Distribution has too many extremes                    |
| Multimodal distributions | Excecution times custer around multiple times         |
| False anomalies          | Performance is fine, but does not match expectations  |

Store performance test results in a database to observe problems with the project/test suite over a longer time.

## Performance Defence

| Type                     | Description                                                      |
| ------------------------ | ---------------------------------------------------------------- |
| Precommit Tests          | Test commits for performance before merge                        |
| Daily tests              | Test solution daily to observe performance degregation over time |
| Retrospective analysis   | Plot performance history over lifetime of the solution           |
| Checkpoint testing       | Check for performance issues in the programming lifecycle        |
| Prerelease testing       | Check performance before deploying a new release                 |
| Manual testing           | Check for perceived performance issues                           |
| Telemetry and Monitoring | Observe performance after release                                |
