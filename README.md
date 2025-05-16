# Designing a Scalable and Highly Available Cloud Infrastructure with AWS

![image](https://github.com/user-attachments/assets/cf8beb16-2ea6-40d9-a5b0-ea30be412f99)

This traditional web application architectural solution utilizes AWS Cloud computing infrastructure to guarantee high availability, security and scalability of the application.

---

## Infrastructure ##
Amazon Route 53 offers DNS services to simplify domain management. Route 53 serves multiple functions, including domain registration, DNS routing, and health monitoring, making it a crucial component for managing traffic to web applications in the cloud.

Amazon CloudFront is utilized to deliver both static and dynamic content. CloudFront is capable of caching frequently accessed content to minimize latency. Amazon S3 is used for storing static assets such as images and videos.

Elastic Load Balancing is implemented to distribute incoming traffic across various availability zones. It enhances the accessibility, reliability, and scalability of your applications by distributing the traffic evenly and making sure that no single instance or resource becomes overloaded.

EC2 Auto Scaling Groups assist in efficiently managing your computing resources by ensuring that your application continues to perform well while reducing costs. This is accomplished through automatic scaling according to established policies, health assessments, and planned scaling actions.

AWS Auto Scaling works together with Amazon CloudWatch to collect metrics data and with Elastic Load Balancing to manage the addition and removal of hosts for traffic control. For example, if a web server consistently exceeds 80 percent CPU utilization, another web server can be swiftly launched and seamlessly incorporated into the load balancer.

## Problem Statement ## 
The promotional event increased the web traffic and overloaded the travel agency website. All requests were being timed out. The EC2 instances were also hosted in an availability zone that was affected by severe storms which caused the application to be offline for hours.

## Solution ## 
Designed and deployed an Application Load Balancer, with a minimum of three EC2 instances, across three availability zones. The AWS services I integrated in the application were Auto Scaling Group and Application Load Balancer.

Here’s a break down of the step-by-step solution explaining how I integrated each service in detail:

![image](https://github.com/user-attachments/assets/d9ae9bbe-4d6f-43e0-bfd1-43dfdeeb30d6)


I implemented an Application Load Balancer (ALB), which is specifically designed to effectively handle and direct web traffic. After configuration, the ALB registers with the Auto Scaling group, enabling it to automatically identify the instances within the group. This arrangement guarantees that the ALB acts as a single point of entry for all incoming web requests, simplifying the traffic routing process to the right instances based on their health and performance indicators. Not only is incoming web traffic distributed across several instances, but it also enhances the application’s availability and fault tolerance.

![image](https://github.com/user-attachments/assets/50d6a057-d9ae-4a6f-a9ab-6b74a8ad1c2a)

![image](https://github.com/user-attachments/assets/cc77e553-a4bc-4dce-a163-cc4e84a4eb76)

To adjust the traffic flow between the load balancer and the web servers, I established new security groups that show the permitted traffic to the load balancer and the traffic allowed to the web servers that are behind it.

![image](https://github.com/user-attachments/assets/2220519b-9455-4d17-8a2f-434066d1bbc5)

To enhance security, I modified the security group (used by EC2 instances behind the Application Load Balancer) to permit only inbound traffic from the load balancer as well.

By eliminating the 0.0.0.0/0 source and substituting it with a security group, I can manage which resources are permitted to send traffic to instances without needing to enter address ranges. Only traffic from the instances linked to the security group is permitted.

![image](https://github.com/user-attachments/assets/d985c3b0-c52f-42a8-aa6f-09485e673cae)

The security groups that I associate with the load balancer determine the rules for controlling where traffic can come from and where it can be sent.

![image](https://github.com/user-attachments/assets/670a41a3-2382-46b2-bed2-f5b3eafa0011)

I modified the load balancer health check settings to match the application performance requirements.

Once the target is registered, it needs to undergo a health check to be deemed healthy. Following each health check, the load balancer node terminates the connection that was made for the purpose of the health check.

The default settings allow 150 seconds to pass (30 second intervals * 5 unhealthy checks) before marking an instance unhealthy. The new values will cause unhealthy instances to be marked unhealthy after 10 seconds (2 failed checks, 5 seconds apart).

![image](https://github.com/user-attachments/assets/f98c7b94-88b4-42ac-877f-e294d23c2947)

Changing the subnet will not automatically cause instances to rebuild in the Auto Scaling group. So, I terminated the instance, which reduced the number of instances below the minimum for the Auto Scaling group and invoked a new instance launch in the private subnet.

If you terminate an instance in an Auto Scaling group and this causes the number of active instances to drop below the minimum threshold required by Auto Scaling, a new instance will be automatically launched.

![image](https://github.com/user-attachments/assets/f32c00fb-785a-42c0-ab91-ecf4467b4e4a)
![image](https://github.com/user-attachments/assets/462b8b53-9db0-443e-9b44-244ded14f95b)

I utilized the safety and dependability offered by geographic redundancy, spreading the Auto Scaling group across various availability zones within a region. I connected a load balancer to manage incoming traffic across three availability zones sitting in private subnets.

If one availability zone becomes unhealthy or is unavailable, EC2 Auto Scaling initiates a new instance in an unaffected availability zone. Auto Scaling tries to launch new instances in the availability zone that has the fewest instances.

## Conclusion ##
Creating a cloud infrastructure that includes auto scaling, load balancing, and health checks is crucial for enabling your application to effectively manage fluctuating traffic levels while ensuring high availability and dependability. As a certified AWS solutions architect, utilizing AWS services like Elastic Load Balancing, Auto Scaling Groups, and Health Checks enables you to build a dynamic and resilient environment that can scale effortlessly with demand.

Auto scaling allows your infrastructure to adapt in real-time to changes in traffic, thereby avoiding both over-provisioning and under-provisioning. Load balancing ensures that traffic is evenly distributed among multiple instances, preventing any single server from becoming overwhelmed and contributing to fault tolerance. Regarding health checks, they improve the application’s reliability by automatically replacing instances that are not functioning properly, thereby minimizing downtime and optimizing performance.

By implementing these approaches, you enhance the performance and cost-effectiveness of your cloud infrastructure while also following best practices for cloud-native applications. In the current digital environment, designing and deploying a cloud infrastructure that can automatically scale, ensure high availability, and recover from failures is crucial for delivering a smooth user experience and facilitating business growth.






