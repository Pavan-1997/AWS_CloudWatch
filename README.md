# AWS CloudWatch


AWS CloudWatch is a powerful monitoring and observability service provided by Amazon Web Services. It enables you to gain insights into the performance, health, and operational aspects of your AWS resources and applications. CloudWatch collects and tracks metrics, collects and monitors log files, and sets alarms to alert you on certain conditions.

Advantages of AWS CloudWatch:

    Comprehensive Monitoring: CloudWatch allows you to monitor various AWS resources such as EC2 instances, RDS databases, Lambda functions, and more. You get a unified view of your entire AWS infrastructure.

    Real-Time Metrics: It provides real-time monitoring of metrics, allowing you to respond quickly to any issues or anomalies that might arise.

    Automated Actions: With CloudWatch Alarms, you can set up automated actions like triggering an Auto Scaling group to scale in or out based on certain conditions.

    Log Insights: CloudWatch Insights lets you analyze and search log data from various AWS services, making it easier to troubleshoot problems and identify trends.

    Dashboards and Visualization: Create custom dashboards to visualize your application and infrastructure metrics in one place, making it easier to understand the overall health of your system.

Problem Solving with AWS CloudWatch:

CloudWatch helps address several critical challenges, including:

    Resource Utilization: Tracking resource utilization and performance metrics to optimize your AWS infrastructure efficiently.
    Proactive Monitoring: Identifying and resolving issues before they impact your applications or users.
    Troubleshooting: Analyzing logs and metrics to troubleshoot problems and reduce downtime.
    Scalability: Automatically scaling resources based on demand to ensure optimal performance and cost efficiency.

Practical Use Cases of AWS CloudWatch:

    Auto Scaling: CloudWatch can trigger Auto Scaling actions based on defined thresholds. For example, you can automatically scale in or out based on CPU utilization or request counts.

    Resource Monitoring: Monitor EC2 instances, RDS databases, DynamoDB tables, and other AWS resources to gain insights into their performance and health.

    Application Insights: Track application-specific metrics to monitor the performance of your applications and identify potential bottlenecks.

    Log Analysis: Use CloudWatch Logs Insights to analyze log data, identify patterns, and troubleshoot issues in real-time.

    Billing and Cost Monitoring: CloudWatch can help you monitor your AWS billing and usage patterns, enabling you to optimize costs.

---
# Simulating a CPU Spike using a Python script, checking the CPU Utilization and sending notification using AWS SNS 


***Reducing maintenance and increased security are the main factors for users to switch from On-Prem to Cloud***


1. Now goto EC2 from AWS Console -> Click on Launch instance

    Give a name
    
    Use Ubuntu as an image
    
    Instance type as t2.micro
    
    Create a key pair if already present use the existing one
    
    Click on Launch instance
    
    Login to the EC2 instance


2. Goto CloudWatch from AWS Console -> Click on Metrics on the left pane -> Click on All metrics -> Click on EC2 -> Click on Per-Instance Metric -> Select CPUUtilization

    **By default AWS EC2 send metrics to CloudWatch every 5 mins**


3. Now go back to the EC2 -> Click on the instance that you have created -> Goto Monitoring tab under it -> Click on Manage detailed monitoring -> Select Enable -> Click on Confirm

    Now by doing the above step sends metrics to CloudWatch every 1 min


4. Now go back to the EC2 terminal -> Create a file cpu_spike.py (Present in repo - This script simulates the CPU Spike in EC2 Instance)

    Verify whether we have python3 installed using below and hit Ctrl+D to exit

   ```
   python3
   ```
    
    Run the script using below
   ``` 
   python3 cpu_spike.py
   ```


6. Go back to the CloudWatch -> Click on Metrics on the left pane -> Click on All metrics -> Click on EC2 -> Click on Per-Instance Metric -> Filter with the instanceID ->  Select CPUUtilization

    To view the metrics immediately in CloudWatch -> Goto Graphed metrics -> Change the Statistic from Average to Maximum 
    
    In organization we use the Avergae parameter


7. Now we need to act on this metric by creating Alarms -> In the CloudWatch -> Goto Alarms -> Click on Create alarm -> Click on Select metric -> Click on EC2 -> Click on Per-Instance Metric -> Filter with the instanceID ->  Select CPUUtilization -> Click on Select metric -> For demo select Statistic - Maximum -> Period - 1 minute -> Set threshold value in Conditions than... - 50 -> Click om Next -> Click on Create a new topic -> Give name to Create a new topic ... -> Give an email address -> Click on Create topic -> Click on Next -> Give name to Alarm name also if needed can give an alarm description -> Click on Next -> Click on Create alarm


8. The alarm created shows as Insufficient data -> To activate this alarm goto to the mailbox of the email address provided in the above Step and confirm the subscription


9. Now go back to the EC2 Instance -> Run the script again -> Now you should see the notification in email
