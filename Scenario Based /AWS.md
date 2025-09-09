<p align="center">
  <img src="https://capsule-render.vercel.app/api?type=waving&color=0:FF6F61,100:FFB347&height=140&section=header&text=AWS%20SCENARIO%20BASED%20QUESTIONS&fontSize=28&fontColor=fff" />
</p>

## Question 1. Suppose your application is hosted on an EC2 instance. Suddenly, there is a huge traffic spike and your application becomes slow. As a DevOps Engineer, what steps will you take?

<Details>
  
- ✅ First, check **EC2 instance metrics** in CloudWatch (CPU, Memory, Network).  
- ✅ If the instance is overloaded, use an **Auto Scaling Group (ASG)** so that more EC2 instances are automatically created during high traffic.  
- ✅ Place an **Elastic Load Balancer (ELB)** in front of the EC2 instances to distribute traffic.  
- ✅ Store static files (images, CSS, JS) in **S3 + CloudFront CDN** to reduce load on EC2.  
- ✅ In the long term, optimize code, database queries, and consider containerization with ECS/EKS for better scaling.  
</Details>


## Question 2. Suppose your EC2 instance crashed unexpectedly. As a DevOps Engineer, what steps will you take to handle this situation?

<Details>

- ✅ **Check AWS CloudWatch logs/metrics** to find the reason (CPU, Memory, Disk, etc.).  
- ✅ **Verify system logs** from the EC2 console (boot errors, kernel issues).  
- ✅ If it’s a one-time issue → **Reboot** the instance from the AWS console.  
- ✅ If the instance does not recover:  
  - Detach the **root EBS volume**, attach it to another instance, and check logs or recover important data.  
  - Launch a **new EC2 instance** with the same AMI, attach the old volume, and restore configuration.  
- ✅ For high availability → Use **Auto Scaling Group with Load Balancer**, so even if one instance crashes, traffic shifts automatically.  
  
</Details>

## Question 3. Suppose you are using an EC2 instance and trying to pull Docker images or access data over the internet, but it is not working. What could be the possible reasons?

<Details>

- ✅ **Internet Gateway not attached**  
  If your EC2 is in a **public subnet**, the VPC must have an **Internet Gateway (IGW)** attached. Without it, no outbound internet access.  

- ✅ **No Elastic IP / Public IP**  
  If the EC2 does not have a **public IP or Elastic IP**, it cannot access the internet directly.  

- ✅ **Incorrect Route Table**  
  The route table of the subnet must have a **0.0.0.0/0 route pointing to IGW** (for public subnets) or **NAT Gateway** (for private subnets).  

- ✅ **Private Subnet without NAT Gateway**  
  If the instance is in a **private subnet**, it needs a **NAT Gateway or NAT instance** to reach the internet.  

- ✅ **Security Group Restrictions**  
  The **outbound rules** in the EC2 Security Group may be too restrictive (e.g., blocking HTTP/HTTPS traffic).  

- ✅ **NACL (Network ACL) restrictions**  
  If Network ACL is denying outbound/inbound HTTP/HTTPS traffic.  
</Details>


## Question 4: How can you restore data from an S3 bucket if it was accidentally deleted?

<Details>
  
- ✅ **Check S3 Versioning**  
  - If **versioning is enabled**, you can simply restore the previous version of the object.  
  - Deleted objects are marked with a "delete marker," but old versions still exist.  

- ✅ **Check S3 Replication (Cross-Region or Same-Region)**  
  - If replication was enabled, you may recover data from the **replica bucket**.  

- ✅ **Check Backup Services**  
  - If AWS **Backup** or a custom backup solution was in place, restore from backup.  

- ✅ **If Versioning/Backup Not Enabled**  
  - Unfortunately, **you cannot recover deleted data** once it's permanently removed.  
  - That’s why AWS recommends enabling **S3 Versioning + Lifecycle Policies** for safety.
    

</Details>


## Question 5: How can you connect your on-premises data center to AWS?
<Details>

- ✅ **VPN Connection (Site-to-Site VPN)**  
  - Set up an **IPSec VPN** between your on-premises network and AWS VPC.  
  - Secure and quick, but depends on internet bandwidth.  

- ✅ **AWS Direct Connect**  
  - A **dedicated private network connection** from your data center to AWS.  
  - Offers low latency and high bandwidth.  
  - Best for large-scale or sensitive data transfers.  

- ✅ **Hybrid Approach**  
  - Use **VPN for quick setup** and later switch to **Direct Connect** for better performance.  

- ✅ **Data Transfer Services**  
  - For bulk data, use **AWS Snowball/Snowmobile** to physically move data into AWS.  

</Details>
