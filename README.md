# AWS-LOAD-BALANCER


<h1>AIM</h1>
To use Elastic Load Balancing (ELB) and Auto Scaling services to load balance and automatically scale an AWS infrastructure.

<h1>PROBLEM STATEMENT</h1>
As web applications receive varying levels of traffic, a single EC2 instance cannot reliably handle sudden spikes in demand, nor can it provide fault tolerance if that instance fails. Elastic Load Balancing (ELB) solves this by automatically distributing incoming application traffic across multiple EC2 instances and Availability Zones, ensuring high availability and fault tolerance.

However, load balancing alone does not solve the problem of capacity — if traffic increases beyond what the current instances can handle, more instances are needed; when traffic decreases, running excess instances wastes cost. AWS Auto Scaling addresses this by automatically launching or terminating EC2 instances based on defined conditions (such as CPU utilization), maintaining application performance while minimizing cost.

This experiment demonstrates how to create a reusable Amazon Machine Image (AMI) from a running server, configure an Application Load Balancer to distribute traffic, set up a Launch Template and Auto Scaling group to automatically manage instance capacity, and use CloudWatch alarms to trigger and verify scaling behavior under load.

<h1>ALGORITHM</h1>
Step 1: Create an AMI for Auto Scaling
Open the EC2 console, confirm that Web Server 1 is running (2/2 status checks passed), select the instance, and choose Actions → Image and templates → Create image. Name it "WebServerAMI" and create it. This AMI will be used to launch identical instances later.

Step 2: Create a Target Group and Load Balancer
Create a Target Group named "LabGroup" (type: Instances, VPC: Lab VPC) without registering targets yet. Then create an Application Load Balancer named "LabELB" under Lab VPC, mapped to Public Subnet 1 and Public Subnet 2, using the Web Security Group, with the HTTP:80 listener forwarding to LabGroup.

Step 3: Create a Launch Template and Auto Scaling Group
Create a Launch Template named "LabConfig" using the WebServerAMI, instance type t2.micro, key pair "vockey", the Web Security Group, and Detailed CloudWatch monitoring enabled. Using this template, create an Auto Scaling group named "Lab Auto Scaling Group" attached to Private Subnet 1 and Private Subnet 2, linked to the LabGroup target group, with desired/minimum/maximum capacity of 2/2/6 and a target tracking scaling policy set to maintain 60% average CPU utilization.

Step 4: Verify Load Balancing
Confirm that two new "Lab Instance" EC2 instances were launched by Auto Scaling and that both show a "healthy" status in the LabGroup target group. Copy the Load Balancer's DNS name and open it in a browser to confirm the application is being served correctly through the load balancer.

Step 5: Test Auto Scaling
Lower the scaling policy's target CPU value to 50% to make scaling trigger sooner, then use the application's "Load Test" feature to generate high CPU load across the instances. Monitor the CloudWatch alarms (AlarmLow/AlarmHigh) until AlarmHigh enters the "In alarm" state, then verify in the EC2 console that additional instances were automatically launched to handle the load.

Step 6: Terminate the Original Web Server
Select Web Server 1 (the original instance used to create the AMI) and terminate it, since it is no longer needed once the Auto Scaling group is managing instances independently.

<h1>COMMANDS</h1>
No CLI commands are used in this experiment, as it is performed entirely through the AWS Management Console (GUI-based setup) using EC2, Elastic Load Balancing, Auto Scaling, and CloudWatch services.

```
K SUVETHA
212225040444
```

<img width="1919" height="808" alt="Screenshot 2026-08-20 141508" src="https://github.com/user-attachments/assets/31512e2f-6b72-4806-9ecb-47b6518c6e58" />
<img width="1888" height="620" alt="Screenshot 2026-08-20 141542" src="https://github.com/user-attachments/assets/a8d02826-d4cb-44f7-b681-1a55e0150aca" />
<img width="1736" height="782" alt="Screenshot 2026-08-20 141601" src="https://github.com/user-attachments/assets/47cdba85-95d4-4e31-bc91-47ec783aa109" />
<img width="1918" height="733" alt="Screenshot 2026-08-20 141615" src="https://github.com/user-attachments/assets/974924e1-a186-45a7-9a48-9c7494185a36" />
<img width="1874" height="780" alt="Screenshot 2026-08-20 141643" src="https://github.com/user-attachments/assets/4c342b87-41bc-45f4-83e3-5652f4720f3c" />
<img width="1594" height="620" alt="Screenshot 2026-08-20 141705" src="https://github.com/user-attachments/assets/75981ba9-17ba-4c1d-9f35-6ea79e437473" />
<img width="1902" height="799" alt="Screenshot 2026-08-20 141719" src="https://github.com/user-attachments/assets/814ad262-44bd-479a-9f06-9e6a04d09fe7" />


<h1>RESULT</h1>
Thus, an AMI was created from a running EC2 instance, a Load Balancer was configured to distribute traffic across multiple instances, an Auto Scaling group was set up with a target tracking scaling policy, and the infrastructure was verified to automatically scale out under increased load using CloudWatch alarms.
