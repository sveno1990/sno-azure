## Steps:

- Create Service Principal
- Create Virtual Machine (to be used as bastion) with eurolinux or RHEL (little bit more expensive) as a spot instance.
  SKU: `Standard_D4s_v3`
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
  - edit the `install/install-config.yaml`
  - Run the openshift installer `./openshift-install create cluster --dir ./install/`
  - You will be prompted for the properties of your Service Principal
  - You can follow the installation in the `~/install/.openshift_install.log`
  - When the installation is done you can find the kubeadmin config and password in `~/install/auth/`
- To use the OpenShift Console from your workstation you need to add the following ssh config
  ```
  Host oc-local
  Hostname <insert public ip of bastion>
  User azureuser
  pubkeyauthentication yes
  IdentityFile ~/.ssh/id_rsa
  DynamicForward 9998
  ServerAliveInterval 60
  ServerAliveInterval 60
  ```
- Now use your favourite browser proxy configuration to direct the xforce.local domain to the socks proxy. For example foxy proxy:
![image](https://github.com/user-attachments/assets/64f8c8bb-66c0-47f4-95fa-ca545067d98f)

