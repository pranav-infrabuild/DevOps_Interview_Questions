<p align="center">
  <img src="https://capsule-render.vercel.app/api?type=waving&color=0:FF6F61,100:FFB347&height=140&section=header&text=AWS%20SCENARIO%20BASED%20QUESTIONS&fontSize=28&fontColor=fff" />
</p>

## Question1. Suppose your application is hosted on an EC2 instance. Suddenly, there is a huge traffic spike and your application becomes slow. As a DevOps Engineer, what steps will you take?

<Details>
  
- ✅ First, check **EC2 instance metrics** in CloudWatch (CPU, Memory, Network).  
- ✅ If the instance is overloaded, use an **Auto Scaling Group (ASG)** so that more EC2 instances are automatically created during high traffic.  
- ✅ Place an **Elastic Load Balancer (ELB)** in front of the EC2 instances to distribute traffic.  
- ✅ Store static files (images, CSS, JS) in **S3 + CloudFront CDN** to reduce load on EC2.  
- ✅ In the long term, optimize code, database queries, and consider containerization with ECS/EKS for better scaling.  
</Details>


