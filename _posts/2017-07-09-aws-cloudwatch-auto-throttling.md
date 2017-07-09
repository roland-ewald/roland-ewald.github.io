---
title:  "Query AWS CloudWatch in Java"
date:   2017-07-09
tags: java aws programming api cloudwatch
---

If you run a service on the [T2 tier of AWS](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/t2-instances.html), your machines have a specific compute budget. So, for example, you may want to delay certain CPU-intensive background tasks to a later time, throttling them by the `CPUCreditBalance` your machine currently has.

The current (averaged) `CPUCreditBalance` can be retrieved via [CloudWatch](https://aws.amazon.com/cloudwatch/), but I found the Java API rather unforgiving[^1] -- it fails silently when certain elements like the unit (`StandardUnit.Count`) are missing from the API call. Here is a snippet that retrieves the last (average) `CPUCreditBalance`, as a starting point:

{% highlight Java %}
String ec2InstanceId = EC2MetadataUtils.getInstanceId();
LocalDateTime endTime = LocalDateTime.now();
LocalDateTime startTime = endTime.minusMinutes(120);
GetMetricStatisticsRequest cpuCreditBalanceRequest = new GetMetricStatisticsRequest()
  .withNamespace("AWS/EC2")
  .withStatistics(Statistic.Average)
  .withMetricName("CPUCreditBalance")
  .withPeriod(60)
  .withUnit(StandardUnit.Count)
  .withStartTime(Date.from(startTime.toInstant(ZoneOffset.UTC)))
  .withEndTime(Date.from(endTime.toInstant(ZoneOffset.UTC)))
  .withDimensions(new Dimension().withName("InstanceId").withValue(ec2InstanceId));
GetMetricStatisticsResult cpuCreditBalanceResult = getAmazonCloudWatch().get().getMetricStatistics(cpuCreditBalanceRequest);
List<Datapoint> metricData = cpuCreditBalanceResult.getDatapoints();
if (metricData == null || metricData.isEmpty()) {
  LOG.warn("Metric 'CPUCreditBalance' is not available.");  
} else {
  Datapoint mostRecentData = metricData.get(metricData.size() - 1);
  Double result = mostRecentData.getAverage();
  LOG.info("Latest average of 'CPUCreditBalance': " + result);
}
{% endhighlight %}

---

[^1]: In any case, before using the Java API, I would always suggest to try out the request via the [AWS CLI](https://aws.amazon.com/cli/) first; this is a complex API with many features.