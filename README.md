# AWS Cloud Monitoring & Notification System

## Project Overview

Implemented an AWS monitoring and notification workflow using Amazon SNS, Amazon SQS, Amazon EC2, and Amazon CloudWatch to demonstrate centralized message delivery, infrastructure monitoring, and automated alert notifications.

The project uses a single Amazon SNS topic to deliver notifications to two endpoints: an Email subscriber and an Amazon SQS queue.

---

## Objective

To understand how Amazon SNS, Amazon SQS, Amazon EC2, and Amazon CloudWatch work together to monitor infrastructure events and distribute automated notifications to multiple subscribers.

---

## Architecture

![AWS SNS, SQS & CloudWatch Monitoring Architecture](architecture/AWS_SNS_SQS_CloudWatch_Monitoring_Architecture.png)

### Architecture Flow

```text
                         Amazon EC2
                       (Monitor Server)
                              |
                              | CPU Utilization Metric
                              v
                     Amazon CloudWatch
                              |
                              | Alarm Evaluation
                              v
                      CloudWatch Alarm
                       (CRITICAL ALARM)
                              |
                              | IN ALARM Notification
                              v
                       Amazon SNS Topic
                           NK-Topic
                         /         \
                        /           \
                       v             v
              Email Subscriber    Amazon SQS
               (Email-JSON)      My-NK-Queue
```

---

## AWS Services Used

- **Amazon EC2** — EC2 instance used for infrastructure monitoring.
- **Amazon CloudWatch** — Monitoring of EC2 CPU Utilization and alarm configuration.
- **Amazon SNS** — Central notification service and message distribution.
- **Amazon SQS** — Queue subscriber used to receive SNS messages and alarm notifications.
- **AWS IAM** — Access and permission configuration used during the lab workflow.
- **Email Subscription** — Notification endpoint configured using the Email-JSON protocol.

---

## SNS Notification Workflow

### Amazon SNS Topic

An SNS topic named **`NK-Topic`** was created as the central notification topic.

The topic was configured with two subscribers:

- Email subscriber
- Amazon SQS subscriber

### Email Subscription

The first subscriber was created using the **Email-JSON** protocol.

The subscription workflow was:

```text
Amazon SNS Topic
        |
        v
Email-JSON Subscription
        |
        v
Subscription Confirmation Email
        |
        v
SubscribeURL Confirmation
        |
        v
Confirmed Subscription
```

After confirmation, messages published to `NK-Topic` were delivered to the configured email endpoint in JSON format.

### Amazon SQS Subscription

An Amazon SQS queue named **`My-NK-Queue`** was created and configured as the second subscriber endpoint.

The SNS-to-SQS workflow was:

```text
Amazon SNS Topic
        |
        v
SNS Subscription
        |
        v
Amazon SQS Queue
        |
        v
Poll for Messages
        |
        v
View Received Message
```

The queue received both test messages published through SNS and CloudWatch alarm notifications.

---

## Message Delivery Validation

A test message was published to `NK-Topic`.

The message was successfully delivered to:

- The configured Email-JSON subscriber
- The `My-NK-Queue` Amazon SQS queue

This validated the SNS publish/subscribe workflow across multiple notification endpoints.

---

## EC2 Monitoring

An EC2 instance named **`Monitor Server`** was launched for the monitoring task.

Amazon CloudWatch Classic Metrics was used to select and monitor the EC2 **CPU Utilization** metric.

The monitoring workflow was:

```text
Amazon EC2
     |
     v
CPU Utilization Metric
     |
     v
Amazon CloudWatch
     |
     v
Metric Visualization
```

---

## CloudWatch Alarm Configuration

A CloudWatch alarm named **`CRITICAL ALARM`** was created using the EC2 CPU Utilization metric.

### Alarm Configuration

- **Metric:** CPU Utilization
- **Statistic:** Average
- **Period:** 5 minutes
- **Threshold Type:** Static threshold
- **Condition:** Greater than or equal to the configured threshold
- **Alarm Action:** Notify the Amazon SNS topic `NK-Topic`
- **Lab Threshold:** `0`

> **Note:** The threshold of `0` was used for lab testing so that the alarm could be triggered and the notification workflow could be validated. In a real production environment, the threshold should be selected based on workload behavior and operational requirements.

---

## Alarm Notification Workflow

```text
Amazon EC2
     |
     v
CloudWatch CPU Metric
     |
     v
CRITICAL ALARM
     |
     | IN ALARM State
     v
Amazon SNS (NK-Topic)
     |
     +------------------+
     |                  |
     v                  v
Email-JSON         Amazon SQS
Subscriber          My-NK-Queue
     |                  |
     v                  v
Email Alert       Queue Message
```

When the CloudWatch alarm entered the **IN ALARM** state, Amazon SNS delivered the notification to both configured subscribers.

---

## Validation

The project successfully validated the following:

- SNS topic created successfully.
- Email-JSON subscription confirmed successfully.
- Amazon SQS queue configured as an SNS subscriber.
- Test message delivered to both Email and SQS.
- EC2 CPU Utilization metric selected in CloudWatch.
- CloudWatch alarm `CRITICAL ALARM` created successfully.
- Alarm entered the **IN ALARM** state during the lab test.
- Alarm notification received through Email.
- Alarm notification received in the Amazon SQS queue.

---

## Key Learning Outcomes

- Amazon SNS topics and subscriptions
- SNS publish/subscribe model
- Email-JSON notification subscriptions
- Subscription confirmation workflow
- Amazon SQS as an SNS notification endpoint
- Message polling and validation in SQS
- EC2 infrastructure monitoring
- CloudWatch metrics
- CPU Utilization monitoring
- CloudWatch alarms
- Alarm states and notification actions
- SNS integration with CloudWatch
- Multi-endpoint notification delivery
- End-to-end monitoring and alert validation

---

## Project Documentation

All implementation screenshots and validation evidence are included directly in the complete project documentation. The screenshots are not required as a separate repository folder.

### Documentation Files

- `AWS_Cloud_Monitoring_and_Notification_Project_Documentation.docx` — Editable Microsoft Word project documentation containing the project architecture, implementation workflow, validation steps, and embedded screenshots.
- `AWS_Cloud_Monitoring_and_Notification_Project_Documentation.pdf` — PDF version of the complete project documentation for sharing and review.

> **Note:** All project screenshots are embedded in the documentation and arranged according to the implementation workflow.

---

## Final Result

Successfully implemented and validated an AWS cloud monitoring and notification workflow using Amazon EC2, Amazon CloudWatch, Amazon SNS, Email, and Amazon SQS.

The project demonstrated centralized notification delivery and verified that both manually published SNS messages and CloudWatch alarm notifications could be delivered to multiple subscriber endpoints.
