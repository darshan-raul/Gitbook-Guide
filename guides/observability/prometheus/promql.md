# PromQL

## PromQL Cheatsheet: From Basics to Advanced

### Basics

#### Instant Vector Selectors

*   Select all time series with the metric name `http_requests_total`:

    ```
    http_requests_total
    ```
*   Select with a label matcher:

    ```
    http_requests_total{status="200"}
    ```

#### Range Vector Selectors

*   Select the last 5 minutes of data:

    ```
    http_requests_total[5m]
    ```

#### Offset Modifier

*   Select data from 1 hour ago:

    ```
    http_requests_total offset 1h
    ```

### Operators

#### Arithmetic Operators

* Addition: `+`
* Subtraction: `-`
* Multiplication: `*`
* Division: `/`
* Modulo: `%`
* Power: `^`

Example:

```
(node_memory_total - node_memory_free) / node_memory_total * 100
```

#### Comparison Operators

* Equal: `==`
* Not equal: `!=`
* Greater than: `>`
* Less than: `<`
* Greater or equal: `>=`
* Less or equal: `<=`

Example:

```
http_requests_total > 100
```

#### Logical Operators

* and
* or
* unless

Example:

```
http_requests_total > 100 and http_errors_total > 5
```

### Aggregation Operators

* Sum: `sum(http_requests_total)`
* Average: `avg(http_requests_total)`
* Min: `min(http_requests_total)`
* Max: `max(http_requests_total)`
* Count: `count(http_requests_total)`

Grouping:

```
sum(http_requests_total) by (status)
```

### Functions

#### Rate and Increase

*   Rate of increase per second:

    ```
    rate(http_requests_total[5m])
    ```
*   Total increase:

    ```
    increase(http_requests_total[1h])
    ```

#### Time Functions

* Current timestamp: `time()`
* Time range: `time() - 3600`

#### Label Manipulation

* Replace label: `label_replace()`
* Join: `label_join()`

#### Histograms

*   Histogram quantile:

    ```
    histogram_quantile(0.95, rate(http_request_duration_seconds_bucket[5m]))
    ```

### Advanced Concepts

#### Subqueries

```
max_over_time(rate(http_requests_total[5m])[1h:])
```

#### Recording Rules

```
record: job:http_requests_total:rate5m
expr: rate(http_requests_total[5m])
```

#### Staleness

*   Handling stale data:

    ```
    http_requests_total unless on(instance) (up == 0)
    ```

#### Binary Operator Matching

*   One-to-one:

    ```
    method_code:http_errors:rate5m{code="500"} / ignoring(code) method:http_requests:rate5m
    ```

#### Vector Matching

*   Many-to-one:

    ```
    sum(http_requests_total) by (job) / group_left count(up) by (job)
    ```

#### Complex Aggregations

```
topk(3, sum(rate(http_requests_total[5m])) by (path))
```

#### Delta and Deriv

*   For gauge metrics:

    ```
    delta(cpu_temp_celsius{host="zeus"}[2h])
    deriv(cpu_temp_celsius{host="zeus"}[2h])
    ```

#### Predict Linear

```
predict_linear(node_filesystem_free{job="node"}[1h], 4 * 3600)
```

#### Time-based Thresholds

```
sum(rate(http_requests_total[5m])) > sum(rate(http_requests_total[5m] offset 1d)) * 1.5
```

Remember, PromQL is powerful but can be resource-intensive. Always optimize your queries for performance, especially in production environments.
