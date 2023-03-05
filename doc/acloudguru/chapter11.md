# Chapter 11. Monitoring

<!-- TOC -->

- [Chapter 11. Monitoring](#chapter-11-monitoring)
  - [CloudWatch Overview](#cloudwatch-overview)
    - [What Is CloudWatch?](#what-is-cloudwatch)
    - [CloudWatch Features](#cloudwatch-features)
    - [Types of Metrics](#types-of-metrics)
    - [Exam Tips](#exam-tips)
  - [Application Monitoring with CloudWatch Logs](#application-monitoring-with-cloudwatch-logs)
    - [What is CloudWatch Logs?](#what-is-cloudwatch-logs)
    - [CloudWatch Logs Terms](#cloudwatch-logs-terms)
    - [Features](#features)
    - [Exam Tips](#exam-tips)
  - [Monitoring with Amazon Managed Services for Prometheus and Amazon Managed Grafana](#monitoring-with-amazon-managed-services-for-prometheus-and-amazon-managed-grafana)
    - [What is Amazon Managed Grafana?](#what-is-amazon-managed-grafana)
    - [Amazon Managed Grafana Overview](#amazon-managed-grafana-overview)
    - [Use Cases](#use-cases)
    - [What is Amazon Managed Prometheus?](#what-is-amazon-managed-prometheus)
    - [Amazon Managed Prometheus Overview](#amazon-managed-prometheus-overview)
  - [Monitoring Exam Tips](#monitoring-exam-tips)
    - [Questions to Ask in the Exam](#questions-to-ask-in-the-exam)
    - [What to Know for the Exam](#what-to-know-for-the-exam)
  - [Lab 11.1. CloudWatch Logs Monitoring for a Web Server](#lab-111-cloudwatch-logs-monitoring-for-a-web-server)
    - [Introduction](#introduction)
    - [Download and Run the CloudWatch Logs Installer](#download-and-run-the-cloudwatch-logs-installer)
    - [Configure CloudWatch Logs](#configure-cloudwatch-logs)
    - [Log in to the CloudWatch Logs Website](#log-in-to-the-cloudwatch-logs-website)
  - [Lab 11.2. AWS VPC Flow Logs for Network Monitoring](#lab-112-aws-vpc-flow-logs-for-network-monitoring)
    - [Introduction](#introduction)
    - [Create CloudWatch Log Group and VPC Flow Log to CloudWatch](#create-cloudwatch-log-group-and-vpc-flow-log-to-cloudwatch)
      - [Create VPC Flow Log to S3](#create-vpc-flow-log-to-s3)
      - [CloudWatch Log Group](#cloudwatch-log-group)
      - [Create VPC Flow Log to CloudWatch](#create-vpc-flow-log-to-cloudwatch)
    - [Create CloudWatch Filters and Alerts](#create-cloudwatch-filters-and-alerts)
      - [Create CloudWatch Log Metric Filter](#create-cloudwatch-log-metric-filter)
      - [Create Alarm Based on the Metric Filter](#create-alarm-based-on-the-metric-filter)
      - [Generate Traffic for Alerts](#generate-traffic-for-alerts)
    - [Use CloudWatch Insights](#use-cloudwatch-insights)
    - [Analyze VPC Flow Logs Data in Athena](#analyze-vpc-flow-logs-data-in-athena)
      - [S3 Bucket](#s3-bucket)
      - [Athena](#athena)
      - [Create Partitions to be Able to Read the Data](#create-partitions-to-be-able-to-read-the-data)
      - [Display Formatted Data](#display-formatted-data)

<!-- /TOC -->

---
## CloudWatch Overview

### What Is CloudWatch?

CloudWatch is a *monitoring* and *observability* platform that was designed to give us insight into our AWS architecture. It allows us to monitor multiple levels of our applications and *identify* potential issues.

### CloudWatch Features

* System Metrics - These are metrics that you get out of the box. The more managed the service is, the more you get.

* Application Metrics - By installing the CloudWatch agent, you can get information from inside your EC2 instances.

* Alarms - Alarms can alert you when something goes wrong, and additional actions can be taken, such as auto-scaling, EC2, system manager. For custom metrics, you can set the alarm to check every 10 seconds, while the minimum period is every 1 minute for default metrics.

### Types of Metrics

* Default metrics - These metrics are provided out of the box and do not require any additional configuration, e.g. CPU Utilization, Network Throughput.

* Custom statistics - These stats will need to be provided by using the CloudWatch agent installed on the host, e.g. EC2 Memory Utilization, EBS Storage Capacity.

### Exam Tips

* Default vs Custom - You need to know which metrics are default and custom. AWS can't see past the hypervisor level for EC2 instances.

* Alarms - There are no default alarms. You should create your own with additional actions.

* Managed Services - The more managed a service is, the more metrics you get out of the box.

* Standard vs Detailed - Standard is 5-minute intervals, whereas detailed is 1-minute for default metrics. There is a small additional cost for detailed intervals.

* Period - The default period is 1-minute for custom statistics, however you can set this period to a minimum of 10 seconds.

---
## Application Monitoring with CloudWatch Logs

### What is CloudWatch Logs?

CloudWatch Logs is a tool that allows you to *monitor*, *store*, and *access* log files (from CloudWatch Agent) from a variety of different sources. It gives you the ability to query your logs to look for *potential issues* or data that is relevant to you.

### CloudWatch Logs Terms

* Log Event - This is the record of what happend. It contains a timestamp and the data.

* Log Stream - A collection of **log events** from the same source create a **log stream**. For example, a continuous set of logs from a single instance.

* Log Group - This is a collection of **log streams**. For example, you may group all your web server instances together.

### Features

* Filter Patterns - You can look for specific terms in your logs, e.g. 400 errors.

* CloudWatch Logs Insights - This allows you to query your logs using a SQL-like interactive solution.

* Alarms - Once you've identified your trends or patterns, it's time to alert on them.

### Exam Tips

* CloudWatch Logs - Logs should go to **CloudWatch Logs**, except for situations where we don't need to process them. Then, they should go straight to S3.

* Go-To Tool - Generally, favour **CloudWatch Logs**, unless the exam asks for a real-time solution, i.e. **Kinesis**.

* Agent-Based - The **CloudWatch Agent** must be installed and configured.

* SQL - If the exam mentions SQL, think **CloudWatch Logs Insights**.

---
## Monitoring with Amazon Managed Services for Prometheus and Amazon Managed Grafana

### What is Amazon Managed Grafana?

**Amazon Managed Grafana** is a fully managed AWS service allowing secure data visualizations for instantly querying, correlating, and visualizing your operational metrics, logs, and traces from different sources.

### Amazon Managed Grafana Overview

* Grafana Made Easy - Easily deploy, operate and scale **Grafana** within your AWS accounts.

* Logical Separation - Workspaces (logical Grafana servers) allow for separation of data visualizations and querying.

* AWS Managed - AWS handles scaling, setup, and maintenance of all workspaces. You only worry about tasks.

* Secure - Built-in security features, such as SSO, data control, VPC endpoints, audit reporting, help you meet corporate governance and compliance requirements.

* Pricing - Pricing is based per active user in a workspace.

* Data Sources - Integrate with several sources, such as CloudWatch, Prometheus, OpenSearch, TimeStream, X-Ray.

### Use Cases

* Container Metric Visualizations - Connect to data sources, such as Prometheus, for visualizing EKS, ECS or your own K8s cluster metrics.

* Internet of Things (IoT) - Vast data plugins make the service a perfect fit for monitoring IoT and edge device data.

* Troubleshooting - Centralizing dashboards allows for more efficient operational issue troubleshooting.

### What is Amazon Managed Prometheus?

Serverless, Prometheus-compatible service used for securely monitoring container metrics at scale.

### Amazon Managed Prometheus Overview

* Open-source Prometheus - Leverage the open-source Prometheus with AWS-managed scaling and availability.

* Automatic Scaling - Let AWS manage automatic scaling on ingestion, storage, and querying of metrics.

* Designed for High Availability (HA) - AWS replicates data across three Availability Zones (AZs) in the same region.

* Choose Your Kubernetes - Works with clusters running on Amazon EKS or self-managed K8s clusters.

* PromQL - Leverage the open-source PromQL query language for exploring and extracting data.

* Data Retention - Data is stored in workspaces for 150 days (automatically deleted afterwards).

---
## Monitoring Exam Tips

### Questions to Ask in the Exam

1. What is the best tool to monitor with?

2. Is that metric available by default?

3. Where can I find those logs?

4. Do I need to adjust my alarm threshold?

### What to Know for the Exam

* **CloudWatch** is the main tool for anything alarm related.

* Not everything should go through **CloudWatch**. For example, AWS standards should be watched by **AWS Config**.

* Know your intervals. The standard metric is delivered every five minutes, while detailed monitoring delivers data every one minute.

* **CloudWatch Logs** is the place for logs. It's important to know EC2, on-premises, RDS, Lambda, and CloudTrail can all integrate with this service.

* SQL. Think **CloudWatch Logs Insights**.

* Real-time. Thinsk **Kinesis** for Big Data.

* **Grafana**. Best for visualizition and correlation of container or IoT metrics.

* **Prometheus**. Leverage this for any Kubernetes-based metrics monitoring at scale.

* **Managed Services**. AWS handles scaling and HA for you.

---
## Lab 11.1. CloudWatch Logs Monitoring for a Web Server

<details>
<summary>Click here to start Lab 11.1.</summary>

### Introduction

In this lab, you will configure an EC2 instance to stream its Apache web server error logs to **CloudWatch Logs**. You will configure the agent and then log in to the CloudWatch Logs console to make sure the logs are streamed correctly.

By the end of this lab, you will understand how to install the **CloudWatch Logs Agent** and configure it to stream a log to the service.

### Download and Run the CloudWatch Logs Installer

1. Navigate to EC2 > Instances. Connect to the `webserver-01` instance.

2. In the EC2 terminal, run the following command to install the **CloudWatch Logs Agent**.

```sh
wget -O awslogs-agent-setup.py https://s3.amazonaws.com/aws-cloudwatch/downloads/latest/awslogs-agent-setup.py
sudo python ./awslogs-agent-setup.py --region us-east-1
```

### Configure CloudWatch Logs

1. Navigate to IAM > Users > `cloud_user` > `Security credentials`.
2. Click **Create access key**. 
3. Select Command Line Interface (CLI). Click **Create access key**.
4. Go back to the EC2 terminal, and paste the **AWS Access Key ID**.
5. Copy and paste the **Secret access key**.
6. Enter the default region `us-east-1` and output format.
7. Copy and paste the log file path `/var/log/apache2/error.log`.
8. Enter the Log Group name.
9. Enter the Log Stream name to use **EC2 instance id**. 
10. Leave all other entries as default values.
11. Enter `N` to complete the configuration.

### Log in to the CloudWatch Logs Website

If successful, you will see the following configuration output:

```sh
------------------------------------------------------
- Configuration file successfully saved at: /var/awslogs/etc/awslogs.conf
- You can begin accessing new log events after a few moments at https://console.aws.amazon.com/cloudwatch/home?region=us-east-1#logs:
- You can use 'sudo service awslogs start|stop|status|restart' to control the daemon.
- To see diagnostic information for the CloudWatch Logs Agent, see /var/log/awslogs.log
- You can rerun interactive setup using 'sudo python ./awslogs-agent-setup.py --region us-east-1 --only-generate-config'
------------------------------------------------------
```

1. Click on the link provided in the CLI output to begin accessing new log events (make sure to include the ending `:`).
2. Click `/var/log/apache2/error.log`.
3. Under Log streams, click the instance id. You will see the contents of your error log with two events logged.
4. In your EC2 terminal, type the following command to view the contents of the error log.

```sh
sudo cat /var/log/apache2/error.log
```

</details>

---
## Lab 11.2. AWS VPC Flow Logs for Network Monitoring

<details>
<summary>Click here to start Lab 11.2.</summary>

### Introduction

In this hands-on lab, we will set up and use **VPC Flow Logs** published to Amazon CloudWatch, create custom metrics and alerts based on the CloudWatch logs to understand trends and receive notifications for potential security issues, and use **Amazon Athena** to query and analyze VPC Flow Logs stored in S3.

### Create CloudWatch Log Group and VPC Flow Log to CloudWatch

#### Create VPC Flow Log to S3

1. Navigate to VPC > `A Cloud Guru` VPC > Flow logs tab.
2. Click **Create flow log** and set the following values:
  - Filter: **All**
  - Maximum aggregation interval: **1 minute**
  - Destination: **Send to an Amazon S3 bucket**
  - S3 bucket ARN: <S3://vpcflowlogsbucket.ARN>
  - Leave the rest as their defaults and click **Create flow log**
3. Verify the flow log shows an **Active** status.
4. Navigate to S3 > `vpcflowlogsbucket` > Permissions tab > Bucket policy.
5. Check the bucket path in the policy includes **AWSLogs**.

#### CloudWatch Log Group

1. Navigate to CloudWatch > Logs > Log groups.
2. Click **Create log group**.
3. In Log group name, enter **VPCFlowLogs**.
4. Click **Create**.

#### Create VPC Flow Log to CloudWatch

1. Navigate to VPC > `A Cloud Guru` VPC > Flow logs tab.
2. Click **Create flow log** and set the following values:
  - Filter: **All**
  - Maximum aggregation interval: **1 minute**
  - Destination: **Send to an CloudWatch Logs**
  - Destination log group: **VPCFlowLogs**
  - IAM role: Select the role with **DeliverVPCLogsRole** in the name.
  - Leave the rest as their defaults and click **Create flow log**
3. Verify the flow log shows an **Active** status.
4. Navigate to CloudWatch > `VPCFlowLogs` log group to open it.

> Note: It can take 5-15 minutes before logs start to show up.

### Create CloudWatch Filters and Alerts

#### Create CloudWatch Log Metric Filter

1. Navigate to CloudWatch > Logs > Log groups > `VPCFlowLogs` log group. You should now see a log stream.
2. Click the listed log string starting with `eni`.
3. Navigate to `VPCFlowLogs` log group > Metrics filters tab.
4. Click **Create metric filter**.
5. Enter the following in the Filter pattern field to track failed SSH attempts on port 22.

```
[version, account, eni, source, destination, srcport, destport="22", protocol="6", packets, bytes, windowstart, windowend, action="REJECT", flowlogstatus]
```

6. In the Select log data to test dropdown, select **Custom log data**.
7. Replace the existing log data with the following:

```
2 086112738802 eni-0d5d75b41f9befe9e 61.177.172.128 172.31.83.158 39611 22 6 1 40 1563108188 1563108227 REJECT OK
2 086112738802 eni-0d5d75b41f9befe9e 182.68.238.8 172.31.83.158 42227 22 6 1 44 1563109030 1563109067 REJECT OK
2 086112738802 eni-0d5d75b41f9befe9e 42.171.23.181 172.31.83.158 52417 22 6 24 4065 1563191069 1563191121 ACCEPT OK
2 086112738802 eni-0d5d75b41f9befe9e 61.177.172.128 172.31.83.158 39611 80 6 1 40 1563108188 1563108227 REJECT OK
```

8. Click **Test pattern**. Click **Next**, then set the following values:
- Filter name: **dest-port-22-rejects**
- Metric namespace: **VPC Flow Logs**
- Metric name: **SSH-rejects**
- Metric value: **1**
9. Click **Next**. Click **Create metric filter**.

#### Create Alarm Based on the Metric Filter

1. Click the checkbox in the top right corner of the new `SSH-rejects` metric filter.
2. Click **Create alarm**.
3. In the Metric section, change Period to **1 minute**.
4. In the Conditions section, set Whenever SSH-rejects is... to **Greater/Equal** than **1**.
5. Click **Next**. In the Notification section, set the following values:
  - Select an SNS topic: **Create new topic**
  - Create a new topic: **Leave default**
  - Email endpoints that will receive the notification**: **user@example.com**
6. Click **Create topic**. Click **Next**.
7. In Alarm name, type **SSH Rejects**.
8. Click **Next**. Click **Create alarm**.

#### Generate Traffic for Alerts

1. Navigate to EC2 > `Web Server` instance.
2. Click Actions > Security > Change security groups.
3. Under Associated security groups, click **Remove** to remove the attached security group.
4. In the search bar, search for and select the **HTTPOnly** security group.
5. Click **Add security group**. Click **Save**.
6. Navigate to EC2 terminal and attempt to connect via SSH.

> Note: We expect this to time out since we just selected a security group with no SSH access.

7. Press **Ctrl-C** to cancel the SSH command.
8. Return to `Web Server` instance, click Actions > Security > Change security groups.
9. Click **Remove** to remove the **HTTPOnly** security group.
10. Select again the **HTTPAndSSH** security group and click **Add security group**.
11. Click **Save**.
12. Navigate to CloudWatch > Alarms. We should see our **SSH Rejects** alarm enter an **In alarm** state shortly.

### Use CloudWatch Insights

1. Navigate to CloudWatch > Logs > **Logs Insights**.
2. In the Select log group(s) search bar, select **VPCFlowLogs**.
3. In the right-hand pane, select **Queries** > Sample queries > VPC Flow Logs > **Top 20 source IP addresses with highest number of rejected requests**.
4. Click **Apply**. Observe the query changes.
5. Click **Run query**. After a few moments, we'll see some data start to populate.

### Analyze VPC Flow Logs Data in Athena

#### S3 Bucket

Before attempting to run a query in Athena, you have to specify an S3 bucket for the results to be saved.

1. Navigate to S3 > `vpcflowlogsbucket` bucket.
2. Navigate through the bucket folder structure AWSLogs > <ACCOUNT_ID> > vpcflowlogs > us-east-1 > <YEAR> > <MONTH> > <DAY>.
3. At the top right, click **Copy S3 URI**. We'll need this shortly.

#### Athena

1. Navigate to Athena.
2. Click **Launch query editor**.
3. Under Settings, click **Manage**.
4. In Location of query result, paste the **S3 URI**, making sure it has an ending `/`.
5. Click **Save**.
6. Under Editor, paste the following DDL code in the Query1 window, replacing <YOUR_LOG_BUCKET> and <ACCOUNT_ID> with your values.

```sql
CREATE EXTERNAL TABLE IF NOT EXISTS default.vpc_flow_logs (
  version int,
  account string,
  interfaceid string,
  sourceaddress string,
  destinationaddress string,
  sourceport int,
  destinationport int,
  protocol int,
  numpackets int,
  numbytes bigint,
  starttime int,
  endtime int,
  action string,
  logstatus string
)
PARTITIONED BY (dt string)
ROW FORMAT DELIMITED
FIELDS TERMINATED BY ' '
LOCATION 's3://{your_log_bucket}/AWSLogs/{account_id}/vpcflowlogs/us-east-1/'
TBLPROPERTIES ("skip.header.line.count"="1");
```

7. Click **Run**. Once executed, a Query successful message should display.

#### Create Partitions to be Able to Read the Data

1. Click the **+** icon to open a new query window.
2. Paste the following code, replacing <YEAR>-<MONTH>-<DAY>, <YOUR_LOG_BUCKET>, <ACCOUNT_ID> with today's date, your S3 bucket and account like before:

```sql
ALTER TABLE default.vpc_flow_logs
    ADD PARTITION (dt='{Year}-{Month}-{Day}')
    location 's3://{your_log_bucket}/AWSLogs/{account_id}/vpcflowlogs/us-east-1/{Year}/{Month}/{Day}/';
```

3. Click **Run**. A Query successful message should display.

#### Display Formatted Data

1. Open a new query window and paste the following code:

```sql
SELECT day_of_week(from_iso8601_timestamp(dt)) AS
     day,
     dt,
     interfaceid,
     sourceaddress,
     destinationport,
     action,
     protocol
   FROM vpc_flow_logs
   WHERE action = 'REJECT' AND protocol = 6
   order by sourceaddress
   LIMIT 100;
```

2. Click **Run**. Our formatted data should appear underneath.

</details>