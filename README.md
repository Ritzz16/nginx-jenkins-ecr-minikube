# Jenkins → Amazon ECR → Minikube

## 🚀 Flow
1. Jenkins builds Docker image from Dockerfile
2. Pushes image to Amazon ECR
3. Deploys/updates app in Minikube

## 🔧 Setup
prerequsite : 1.Linux 64bit
              2.Docker& K8s Installed on it.
              3.Aws Cli configured on it
              4.Jenkins installed with plugins - AWS ECR,Docker,Kubernetes

1. Initailze git repo or fork this Repo with all these files
2. Configure Aws Cli and login into aws account on linux
3. Create a Public AWS ECR Repo on Aws account (Dashboard)
4. Copy the image Push commands and add into pipeline jenkins push stage (from ecr repo dashboard)
5. setup env variables accoording your aws account and ecr repo(pipeline stage 1)
6. Add the jenkins user into docker group on linux (" sudo usermod -aG docker jenkins ")
7. On K8S side add the add the .kubeconfig file to the jenkins users directory, in my case im using minikube (" sudo cp -r /home/riteshh/.minikube/* /var/lib/jenkins/.minikube/") 
8. make jenkins user owner of that file (" sudo chown -R jenkins:jenkins /var/lib/jenkins/.minikube")
9. if you are using minikube then create the clusterrolebinding and svc accouny for jenkins user to give admin access (jenkins-rbac.yaml) (only for minikube ) (optional)
10. Or multinode k8s cluster then create credential in jenkins give name kubeconfig ,copy the .kubeconfig file data and paste in field , then type = secret file and id =kube-config and save(recommended)
11. Build pipeline if succesfully build ,
12. minikube kubectl get pods (check pods running)
13. minikube kubectl get svc and see node port
14. Access using the minikube ip and nodeport
15. If other k8s cluster Access using the local machines ip and node port

