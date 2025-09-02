# AWS VPC Network Monitoring and EC2 Metrics

## Project Overview
This project demonstrates the deployment and monitoring of an AWS infrastructure using a **Virtual Private Cloud (VPC)**, **EC2 instance**, **security groups**, **network ACLs**, **CloudWatch metrics and logs**, **alarms**, and **Grafana dashboards**.  

The primary goal is to create a secure network environment, monitor key performance metrics of an EC2 instance, visualize them in Grafana, and configure real-time alerts for critical conditions.

---

## Architecture Overview

### VPC Configuration
- **VPC CIDR:** 10.0.0.0/24
- **Public Subnet:** 10.0.0.0/25, Availability Zone: eu-west-3a
- **Internet Gateway:** Connected to VPC `VPC_Notes_App`
<br><img src="https://github.com/Barbarossa01/AWS-VPC-and-EC2-Monitoring-with-CloudWatch-and-Grafana/blob/main/images/VPC_1.PNG" alt="VPC">
<br><img src="https://github.com/Barbarossa01/AWS-VPC-and-EC2-Monitoring-with-CloudWatch-and-Grafana/blob/main/images/Subnet_Details.PNG" alt="VPC">

### Network ACLs
- **Inbound Rules:**  
  - Rule 100: Allow all traffic from 0.0.0.0/0
<br><img src="https://github.com/Barbarossa01/AWS-VPC-and-EC2-Monitoring-with-CloudWatch-and-Grafana/blob/main/images/VPC_NACL_Inbound.PNG" alt="NACL">
- **Outbound Rules:**  
  - Allow all traffic to 0.0.0.0/0
<br><img src="https://github.com/Barbarossa01/AWS-VPC-and-EC2-Monitoring-with-CloudWatch-and-Grafana/blob/main/images/VPC_NACL_Outbound.PNG" alt="NACL">
### Security Groups
- **Inbound Rules:**
  - HTTPS (443) → 0.0.0.0/0
  - HTTP (80) → 0.0.0.0/0
  - SSH (22) → 0.0.0.0/0
  - Custom TCP Port 3000 → 89.64.19.47/52
- **Outbound Rules:**
  - Allow all traffic → 0.0.0.0/0

### Route Table
- Local route for `10.0.0.0/24` associated with the public subnet
<br><img src="https://github.com/Barbarossa01/AWS-VPC-and-EC2-Monitoring-with-CloudWatch-and-Grafana/blob/main/images/Route_Table.PNG" alt="RT">
---

## Monitoring Setup

### CloudWatch Logs
- `/ec2/nginx/access` → Tracks all HTTP access requests
- `/ec2/nginx/error` → Tracks Nginx errors for debugging
<br><img src="https://github.com/Barbarossa01/AWS-VPC-and-EC2-Monitoring-with-CloudWatch-and-Grafana/blob/main/images/Logs.PNG" alt="logs">
### CloudWatch Metrics
Monitored metrics for EC2 instance include:
- **CPU Metrics:** CPU utilization, CPU credit usage, CPU credit balance, CPU high usage
- **Memory Metrics:** RAM usage percentage
- **Disk Metrics:** Disk usage
- **Network Metrics:** Network in/out, network packets in/out, established TCP connections
- **EC2 Status Metrics:** StatusCheckFailed
<br><img src="https://github.com/Barbarossa01/AWS-VPC-and-EC2-Monitoring-with-CloudWatch-and-Grafana/blob/main/images/CW_1.PNG" alt="CW">
<br><img src="https://github.com/Barbarossa01/AWS-VPC-and-EC2-Monitoring-with-CloudWatch-and-Grafana/blob/main/images/CW_2.PNG" alt="CW">
<br><img src="https://github.com/Barbarossa01/AWS-VPC-and-EC2-Monitoring-with-CloudWatch-and-Grafana/blob/main/images/CW_3.PNG" alt="CW">
<br><img src="https://github.com/Barbarossa01/AWS-VPC-and-EC2-Monitoring-with-CloudWatch-and-Grafana/blob/main/images/CW_4.PNG" alt="CW">
<br><img src="https://github.com/Barbarossa01/AWS-VPC-and-EC2-Monitoring-with-CloudWatch-and-Grafana/blob/main/images/CW_5.PNG" alt="CW">
<br><img src="https://github.com/Barbarossa01/AWS-VPC-and-EC2-Monitoring-with-CloudWatch-and-Grafana/blob/main/images/CW_6.PNG" alt="CW">
<br><img src="https://github.com/Barbarossa01/AWS-VPC-and-EC2-Monitoring-with-CloudWatch-and-Grafana/blob/main/images/CW_7_Add_to_Dashboard.PNG" alt="CW">

### CloudWatch Alarms
Two alarms were created:
1. **EC2 Status Check Failed Alarm**  
   - Condition: `StatusCheckFailed > 0` for 1 datapoint within 5 minutes  
   - Action: Send notification email

2. **High CPU Usage Alarm**  
   - Condition: `CPU utilization > 70%` for 1 datapoint within 5 minutes  
   - Action: Send notification email <br>
<br><img src="https://github.com/Barbarossa01/AWS-VPC-and-EC2-Monitoring-with-CloudWatch-and-Grafana/blob/main/images/Alarms.PNG" alt="CW">
---

## Grafana Dashboards
Custom dashboards visualize real-time metrics from CloudWatch:

- **CPU Metrics**: CPU usage, CPU credit balance
<br><img src="https://github.com/Barbarossa01/AWS-VPC-and-EC2-Monitoring-with-CloudWatch-and-Grafana/blob/main/images/Grafana_1.PNG" alt="grafana">
- **Memory Metrics**: RAM usage %
<br><img src="https://github.com/Barbarossa01/AWS-VPC-and-EC2-Monitoring-with-CloudWatch-and-Grafana/blob/main/images/Grafana_2.PNG" alt="grafana">
- **Network Metrics**: Network traffic, packets, established TCP connections
<br><img src="https://github.com/Barbarossa01/AWS-VPC-and-EC2-Monitoring-with-CloudWatch-and-Grafana/blob/main/images/Grafana_3.PNG" alt="grafana">
- **Disk Metrics**: Disk usage
- **Status Metrics**: EC2 StatusCheckFailed


---

## Deployment Steps

1. **VPC & Subnet Setup**
   - Create a VPC with CIDR `10.0.0.0/24`
   - Create a public subnet `10.0.0.0/25` in `eu-west-3a`
   - Attach Internet Gateway

2. **Configure Security**
   - Set up Security Groups and NACLs as described above

3. **EC2 Deployment**
   - Launch an EC2 instance in the public subnet
   - Assign Security Groups

4. **CloudWatch Configuration**
   - Create log groups: `/ec2/nginx/access` and `/ec2/nginx/error`
   - Configure custom metrics (CPU, memory, network)
   - Create alarms and link email notifications

5. **Grafana Setup**
   - Connect Grafana to CloudWatch as a data source
   - Import dashboards JSON from `grafana-dashboards/`
   - Visualize metrics and alerts

---

## Screenshots
Screenshots are included in the `screenshots/` folder

---

## Notes
- This project demonstrates secure and monitored AWS infrastructure.
- Provides insights into EC2 instance performance and network traffic.
- Shows how CloudWatch and Grafana can be used together for real-time monitoring.
- Alerts ensure immediate notification for critical system failures.

---

## Optional Enhancements
- Automate deployment using **Terraform** or **AWS CloudFormation**
- Expand monitoring for additional EC2 instances or services
- Integrate with SNS topics for more advanced notifications
- Configure log retention and centralized log analysis
