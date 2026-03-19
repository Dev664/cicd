

Create A Ec2 instance for jenkins , I'm assuning we alredy have a K8s cluster setup from previous lab with one Control plane and one Worker node . 

Total server we required  for our setup  :
1 EC2 - Kubernetes Control Plane  (Alredy present)
1 EC2 - Worker Node (Alredy present)
1 EC2 - Jenkins Server  (We will build)


use the same Security group as k8s cluster else open the  below ports on your SG 
22    → SSH
8080  → Jenkins UI
30000–32767 → NodePort (for K8s apps)

instance Type : t2.medium
Storage : Minimum: 20 GB
