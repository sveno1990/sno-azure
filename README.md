## Steps:

- Create Service Principal
- Create Virtual Machine (to be used as bastion) with eurolinux or RHEL (little bit more expensive) as a spot instance
- Assign the Service Principal the Owner role on the resource group (of the vm)
- Create a second resource group
- Assign the Service Principal the Owner role on the second resource group (of the vm)
- create a second subnet in the bastion vm virtual network with the name: `controlPlane`
- Login in the VM and do the following:
  - `sudo dnf install wget tar git -y `
  - `wget https://mirror.openshift.com/pub/openshift-v4/x86_64/clients/ocp/4.16.9/openshift-install-linux.tar.gz`
  - `tar -xvf openshift-install-linux.tar.gz`
  - `mkdir install`
  - `curl -o install/install-config.yaml https://raw.githubusercontent.com/sveno1990/sno-azure/refs/heads/main/bootstrap/1-install-config.yaml`
  - edit the install/install-config.yaml
  - Run the openshift installer `./openshift-install --dir ./install/`
