# CloudComputing-assignment3


This is the repo imported from TU Berlin GitLab with the code for the assignment 3 in Cloud Computing course. 

We got **maximum number of points**, so enjoy our work.    
If you find it useful, you can give us a star.

## Tasks:

* Prepare 3 VMs (in public cloud or locally)
* Deploy Kubernetes cluster using Kubespray (Ansible playbook)
* Prepare simple Docker containers for dummy web service
* Roll out web service in Kubernetes cluster using an own Ansible playbook
* Evaluate the deployment with a provided test script

[The exercise sheet is here.](/Assignment3-TUB.pdf)

## Commands

```shell
# Install ansible 2.6
sudo pip install ansible==2.6

# install python-netaddr
sudo apt install python-netaddr


# Clone Kubespray repository
git clone https://github.com/kubernetes-sigs/kubespray.git

# Checkout to v2.8.1 commit
cd kubespray
git checkout v2.8.1

# Install dependencies from ``requirements.txt``
sudo pip install -r requirements.txt

# Copy ``inventory/sample`` as ``inventory/ec2cluster``
cp -rfp inventory/sample inventory/ec2cluster

# remove sample hosts.ini
rm inventory/ec2cluster/hosts.ini

# copy our hosts.ini
cp ../hosts.ini inventory/ec2cluster/hosts.ini

# To run ansible playbook
ansible-playbook -i inventory/ec2cluster/hosts.ini --become --become-user=root cluster.yml

# To become root on the VM
sudo su -

# To check if all nodes are up and part of the cluster
root@node1:/home/ubuntu# kubectl get nodes

# To build image from Dockerfile
docker build -f "frontend.Dockerfile" -t "frontend" .

# To tag the image before pushing it to the docker hub
docker tag [local_image_tag] [docker_hub_repository_name]:[docker_hub_image_tag]

# To push the image to the docker hub using the same values as for tagging
docker push [docker_hub_repository_name]:[docker_hub_image_tag]

# To run ansible playbook which deploys the application
ansible-playbook -i hosts.ini --become --become-user=root cc-webapp.yml

# To determine the node port
kubectl get svc -n assignment-3

# To scale the deployments
kubectl -n assignment-3 scale deployments/backend --replicas=6
kubectl -n assignment-3 scale deployments/frontend --replicas=4

# To start the test script
python3 test-deployment.py 3.87.56.100:30711 3.88.34.232:30711 3.85.91.153:30711
```
