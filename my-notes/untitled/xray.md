# Xray

AWS X-Ray is a service that **collects data about requests that your application serves**, and provides tools that you can use to **view, filter, and gain insights into that data to identify issues and opportunities for optimization.**&#x20;

For any traced request to your application, you can see `detailed information not only about the request and response,` but also about **calls that your application makes to downstream AWS resources, microservices, databases, and web APIs**.



### Daemon

Instead of sending trace data directly to X-Ray, each client SDK sends JSON segment documents to a daemon process listening for UDP traffic.&#x20;

The [X-Ray daemon](https://docs.aws.amazon.com/xray/latest/devguide/xray-daemon.html) buffers segments in a queue and uploads them to X-Ray in batches. The daemon is available for Linux, Windows, and macOS, and is included on AWS Elastic Beanstalk and AWS Lambda platforms.
