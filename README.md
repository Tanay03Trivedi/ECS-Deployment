# ECS-Deployment  with faregate(autoscaling) and alb
## keypoints
- Create an ECR repository.

- Push the image to that repository.

- Create an ECS cluster.

- Create a task definition.

- Create a service in that cluster.

- Attach the ALB DNS to CloudFront (if you want to use a CDN).

- Then, you can attach the CloudFront URL to Route 53 for DNS.


## Creating the ECR Repository and Pushing the Container Image

aws ecr get-login-password --region <your-region> | docker login --username AWS --password-stdin <aws_account_id>.dkr.ecr.<your-region>.amazonaws.com
Tag the Image:


docker tag tanay/ecs-eks:latest <aws_account_id>.dkr.ecr.<your-region>.amazonaws.com/ecs-eks:latest
Push the Image to ECR:


docker push <aws_account_id>.dkr.ecr.<your-region>.amazonaws.com/ecs-eks:latest
Replace <your-region> and <aws_account_id> with your actual AWS region and account ID.

docker push (imageurl)

![Screenshot 2025-04-04 115836](https://github.com/user-attachments/assets/534ed351-a0cb-4661-bdee-c4974f1ffce9)


## Creating ECS Cluster

Using Fargate as computing

![Screenshot 2025-04-04 120044](https://github.com/user-attachments/assets/24b914ca-8810-4cd9-bb76-a5ab63f73cbb)

![Screenshot 2025-04-04 120118](https://github.com/user-attachments/assets/e7501fa6-7989-4a67-8671-58c4aeda6ebd)

## Creating TaskDefenation

![Screenshot 2025-04-04 120959](https://github.com/user-attachments/assets/1baa5585-78a2-45aa-95e6-73ba062bcf2f)

- Assign the appropriate IAM role and policies to allow ECS to pull images from ECR and manage other necessary AWS resources (e.g., CloudWatch, ELB, etc.).
- Ensure that you specify the correct ECR image URL in the task definition.
-Set the container port and host/exposed port to be the same, so traffic is correctly routed from the load balancer to the container

![Screenshot 2025-04-04 120602](https://github.com/user-attachments/assets/f32a0906-883a-49d3-9e0d-67fdcc72a81d)

- You need to allow the port used by the container in the security group associated with the ECS service or load balancer.
- (Note: Many ports may already be open because this security group is also used in other projects.)

![Screenshot 2025-04-04 121422](https://github.com/user-attachments/assets/41646407-4e55-4509-8e4c-49fc8b43f5a4)

## Creating Service for Deployment

![Screenshot 2025-04-04 121441](https://github.com/user-attachments/assets/1f12d704-7b72-465c-bf5e-67d2c4958fd2)

## Here, I set the desired capacity to 2, which acts as the minimum number of running containers (tasks) in the service.

![Screenshot 2025-04-04 121451](https://github.com/user-attachments/assets/32b330e8-4952-4fe4-988d-5bac00701c32)
![Screenshot 2025-04-04 121459](https://github.com/user-attachments/assets/d3586568-49a0-4295-9105-ab65926b06f2)
![Screenshot 2025-04-04 121511](https://github.com/user-attachments/assets/219c9502-1666-4bd9-aaf6-dfe7414c3b56)
![Screenshot 2025-04-04 121707](https://github.com/user-attachments/assets/70df11af-d8f3-4dd7-95ea-b2ba248fad74)
![Screenshot 2025-04-04 123538](https://github.com/user-attachments/assets/d0e0d86f-51e9-497f-bc7e-e76cb862403c)

![Screenshot 2025-04-04 121744](https://github.com/user-attachments/assets/de5a4554-5b98-4c2b-a5ba-e76442f28137)
![Screenshot 2025-04-04 124032](https://github.com/user-attachments/assets/846a0e98-096b-4e8c-9a05-8ad34a338364)
![Screenshot 2025-04-04 124143](https://github.com/user-attachments/assets/f9875cbc-468a-471a-a422-cb1b9bb0d3b5)
![Screenshot 2025-04-04 125314](https://github.com/user-attachments/assets/07cfc3e0-4d45-4e1d-be1f-50f16888acfe)
