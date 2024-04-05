# Percentile

Percentiles tell you how a value ranks compared to the rest of the data set. It essentially divides the data into 100 equal parts. Here's a breakdown to understand it better:

* **Imagine a set of scores on a test:** There are 100 students and 100 percentiles.
* **The 50th percentile, also known as the median:** This is the middle score. If you arrange the scores in ascending order, the 50th percentile score will have 50% of the scores lower than it and 50% higher or equal to it.
* **Higher percentiles indicate higher values:** For instance, the 90th percentile score means 90% of the scores fall below it and only 10% are higher or equal.
* **Lower percentiles indicate lower values:** Conversely, the 10th percentile score signifies 10% of the scores are lower and 90% are higher or equal.

Here are specific examples for the percentiles you mentioned:

* **50th percentile:** In the test score example, this is the median score, dividing the class exactly in half.
* **90th percentile:** This score would be higher than 90% of the test scores. Only a small fraction of students (top 10%) scored higher than this.
* **95th percentile:** An even more exclusive group! This score represents the performance better than 95% of the test takers.
* **99th percentile:** This is a very high score, exceeding 99% of the students on the test.

Percentiles are useful in various situations, like comparing student performance, income levels, or housing prices in a particular area. They provide a quick and easy way to understand where a value stands relative to others in the data set.

***

Here's how percentiles offer valuable insights when monitoring system performance based on CPU metrics:

**Typical CPU Metrics:**

* **CPU Usage (%):** The percentage of processing power a system is currently utilizing.

**Example Use Cases:**

1. **Understanding Response Times:**
   * **90th percentile of CPU usage:** If your 90th percentile consistently remains high (e.g., above 80%), it suggests that 10% of the time, your system is heavily loaded and may be experiencing delays.
   * **99th percentile of CPU usage:** If your 99th percentile stays close to 100%, it's a strong indicator that your system might be reaching capacity limits and likely results in significant slowdowns for users.
2. **Capacity Planning:**
   * **Trend of 95th or 99th percentile CPU usage:** If the 95th/99th percentiles show an upward trend, it signals that your system might be outgrowing its current capacity. This helps proactively plan upgrades or scaling strategies.
3. **Identifying Performance Bottlenecks:**
   * **Comparing CPU percentiles with other metrics:** Correlate CPU percentiles with metrics like request latency or response times. If high CPU usage percentiles coincide with spikes in latency, it points to the CPU being a likely performance bottleneck.
4. **Establishing Baseline & Anomaly Detection:**
   * **Historical percentiles as a baseline:** Establish a baseline of normal CPU usage percentiles. Deviations from this baseline (e.g., sudden jumps in 95th percentile) can alert you to potential performance issues or unexpected spikes in activity.

**Advantages of Using Percentiles:**

* **Resilient to outlier data:** Percentiles are less affected by extreme spikes compared to averages. They give you a better sense of typical load levels.
* **Intuitive to interpret:** Users can easily understand that, for example, a 90th percentile value means the system operates below that value 90% of the time.
* **Applicable to many metrics:** Percentiles work well beyond CPU usage. You can apply them to memory utilization, disk I/O, network latency, etc.

**Important Notes:**

* Percentiles are not a magic fix to all performance problems. Use them in conjunction with other metrics and in the context of your application's expected behavior.
* The time period you analyze is important. For example, percentiles calculated every minute will showcase different behavior than those calculated hourly or daily.







