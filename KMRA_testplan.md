```text
SPDX-License-Identifier: Apache-2.0
Copyright © 2021 Intel Corporation
```


- [ITP/DEK/SEC/KMRA/01: Verify deployment of verification controller on AWS for PCCS and KMRA control plane services](#itpDEKSECKMRA01-Verify-deployment-of-verification-controller-on-AWS-for-PCCS-and-KMRA-control-plane-services)
  - [Test Summary](#test-summary)
  - [Prerequisites](#prerequisites)
  - [Hardware Requirements](#hardware-requirements)
  - [Test steps](#test-steps)


- [ITP/DEK/SEC/KMRA/02: Verify SGX and KMRA provisioning on DEK with ESP](#itpDEKSECKMRA02-Verify-SGX-and-KMRA-provisioning-on-DEK-with-ESP)
  - [Test Summary](#test-summary-1)
  - [Prerequisites](#prerequisites-1)
  - [Test steps](#test-steps-1)

- [ITP/DEK/SEC/KMRA/03: Verify onboard of single instance KMRA NGINX application within secure enclave on DEK node](#itpDEKSECKMRA03-Verify-onboard-of-single-instance-KMRA-NGINX-application-within-secure-enclave-on-DEK-node)
  - [Test Summary](#test-summary-2)
  - [Prerequisites](#prerequisites-2)
  - [Test steps](#test-steps-2)

- [ITP/DEK/SEC/KMRA/04: Verify onboard of 3 instances of KMRA NGINX application having their own secure enclave on DEK node](#itpDEKSECKMRA04-Verify-onboard-of-3-instances-of-KMRA-NGINX-application-having-their-own-secure-enclave-on-DEK-node)
  - [Test Summary](#test-summary-3)
  - [Prerequisites](#prerequisites-3)
  - [Test steps](#test-steps-3)  


- [ITP/DEK/SEC/KMRA/05: Verify deployment of verification controller with PCCS KMRA and Platform attestation services in single AWS instance](#itpDEKSECKMRA05-Verify-deployment-of-verification-controller-with-PCCS-KMRA-and-Platform-attestation-services-in-single-AWS-instance)
  - [Test Summary](#test-summary-4)
  - [Prerequisites](#prerequisites-4)
  - [Test steps](#test-steps-4)

- [ITP/DEK/SEC/KMRA/06: Verify simultaneous bring up of 10 KMRA NGINX applications having their own secure enclave on DEK node](#itpDEKSECKMRA06-Verify-simultaneous-bring-up-of-10-KMRA-NGINX-applications-having-their-own-secure-enclave-on-DEK-node)
  - [Test Summary](#test-summary-5)
  - [Prerequisites](#prerequisites-5)
  - [Test steps](#test-steps-5)  

- [ITP/DEK/SEC/KMRA/07: Verify onboard of KMRA NGINX application in multinode DEK referring same PCCS and KMRA cloud services](#itpDEKSECKMRA07-Verify-onboard-of-KMRA-NGINX-application-in-multinode-DEK-referring-same-PCCS-and-KMRA-cloud-services)
  - [Test Summary](#test-summary-6)
  - [Prerequisites](#prerequisites-6)
  - [Test steps](#test-steps-6)  

- [ITP/DEK/SEC/KMRA/08: Verify the cryptographic performance of SKM against non KMRA SGX enclave](#itpDEKSECKMRA08-Verify-the-cryptographic-performance-of-SKM-against-non-KMRA-SGX-enclave)
  - [Test Summary](#test-summary-7)
  - [Prerequisites](#prerequisites-7)
  - [Test steps](#test-steps-7)

- [ITP/DEK/SEC/KMRA/09: Verify onboard of two NGINX application with and without secure enclave on DEK node](#itpDEKSECKMRA09-Verify-onboard-of-two-NGINX-application-with-and-without-secure-enclave-on-DEK-node)
  - [Test Summary](#test-summary-8)
  - [Prerequisites](#prerequisites-8)
  - [Test steps](#test-steps-8)

- [ITP/DEK/SEC/KMRA/10: Verify deployment of KMRA NGINX application post recovery of AWS instance from forced reboot](#itpDEKSECKMRA10-Verify-deployment-of-KMRA-NGINX-application-post-recovery-of-AWS-instance-from-forced-reboot)
  - [Test Summary](#test-summary-9)
  - [Prerequisites](#prerequisites-9)
  - [Test steps](#test-steps-9)

- [ITP/DEK/SEC/KMRA/11: Verify deployment of NGINX application with KMRA control plane services in AWS instance not reachable](#itpDEKSECKMRA11-Verify-deployment-of-NGINX-application-with-KMRA-control-plane-services-in-AWS-instance-not-reachable)
  - [Test Summary](#test-summary-10)
  - [Prerequisites](#prerequisites-10)
  - [Test steps](#test-steps-10) 

- [ITP/DEK/SEC/KMRA/12: Verify the enclave memory exhaustion handling with multiple instances of NGINX application](#itpDEKSECKMRA12-Verify-the-enclave-memory-exhaustion-handling-with-multiple-instances-of-NGINX-application)
  - [Test Summary](#test-summary-11)
  - [Prerequisites](#prerequisites-11)
  - [Test steps](#test-steps-11)


## ITP/DEK/SEC/KMRA/01: Verify deployment of verification controller on AWS for PCCS and KMRA control plane services

### Test Summary

Key Management Reference Application (KMRA) is a proof-of-concept software created to demonstrate the integration of Intel® Software Guard Extensions (Intel® SGX) asymmetric key
capability with a hardware security model (HSM) on a centralized key server. 
This reference application sets up a NGINX workload to access the private key in an Intel® SGX enclave on a 3rd Generation Intel® Xeon® Scalable processor, 
using the Public-Key Cryptography Standard (PKCS) #11 interface and OpenSSL.


### Hardware Requirements

*Controller and Node System*

Smart Edge node & Verification controller(AWS instance) 

- Smart Edge Cluster Node
- ICX Xeon processor with Flexible launch control(FLC) enabled(requirement for DCAP)
- At least 64 GB RAM
- At least 512 GB hard drive
- Kernel Version> 5.11
- An Internet connection
- Ubuntu 20.04

### Prerequisites

1. Enable Intel SGX in BIOS (below steps according to https://wiki.ith.intel.com/display/ESSE/Enable+Intel+SGX+in+BIOS)

  Perform all below steps from internal BIOS not from IDRAC

    ```shell
    - Verify memory settings in System Setup of the BIOS, keys to look out for:
      - Memory Operating Mode : Optimizer Mode
      - Node Interleaving : Disabled
    - Check if Memory Encryption is enabled/disabled in System Security. Save and Reboot after selection keys:
      - Memory Encryption : Single Key
      - Note here that if you do not see the Disabled button selected when landing on this section, you first need to disable Memory Encryption and then save changes and reboot
      - Only then, go ahead and choose Single Key here
    - Enable SGX and Factory reset together. On selection, Save and Reboot:
      - Intel(R) SGX : On
      - SGX Factory Reset : On
    ```

2. Settting Up DCAP - only if there is no existing DCAP server (smartedge node):

  ```
    - Subscribe to the Intel PCS (https://www.intel.com/content/www/us/en/developer/articles/guide/intel-software-guard-extensions-data-center-attestation-primitives-quick-install-guide.html#inpage-nav-2-undefined)

    - Set up the Intel PCCS (https://www.intel.com/content/www/us/en/developer/articles/guide/intel-software-guard-extensions-data-center-attestation-primitives-quick-install-guide.html#inpage-nav-2-1)
  ```
 
3. refer following link for ESP-ISO ESP w/ Ubuntu OS image creation
https://github.com/intel-innersource/applications.services.smart-edge-open.test-validation/blob/main/test_plans/se-o/esp/singlenode/ts01-esp-platform-setup-singlenode.md#itpseoespsingle0102-deploy-esp-on-host-machine-with-ubuntu-provision-target-machine-with-ubuntu-profile-using-usb-boot-method
  
### Test steps

1. open terminal as a non root User and Fetch latest DEK repo and clone it on ansible host

   Note: Refer following link for Non-root user creation - https://github.com/otcshare/specs/blob/master/doc/getting-started/non-root-user.md

2. Update inventory.yaml with AWS IP and Username

 ```shell
all:
  vars:
    cluster_name: pccs_controller        # NOTE: Use `_` instead of spaces.
    deployment: Verification_controller               # NOTE: Available deployment type: Developer expirience kits (dek).
    single_node_deployment: true  # Request single node deployment (true/false).
    limit:                        # Limit ansible deployment to certain inventory group or hosts
controller_group:
  hosts:
    controller:
      ansible_host: <AWS instance IP> 
      ansible_user: ubuntu
edgenode_group:
  hosts:
    node01:
      ansible_host: <AWS instance IP>
      ansible_user: ubuntu
 ```     
 
3. Update host name/list of host names for TA and set true for platform attestation controller in `deployments/verification_controller/all.yml` file.

  Refer below template for the changes:
  
  
 ```shell
 # Disable SGX
 sgx_enabled: false

# Enable isecl controller instalation
platform_attestation_controller: false

# Disable installing isecl attestation components for nodes
platform_attestation_node: false

# List of possible host names for TA. Coma separated list with possible wildcards: *.example.com,192.168.1.*
isecl_ta_san_list: ""

# Enable PCCS deployment
pccs_enable: true

```
4. Modify inventory/default/group_vars/all/10-default.yml as shown below 
 
  ```shell
  #http_proxy: ""
  #https_proxy: ""
  #ftp_proxy: ""
  # No proxy setting contains addresses and networks that should not be accessed using proxy (e.g. local network, Kubernetes CNI networks)
  no_proxy: "localhost,127.0.0.1,10.244.0.0/24,10.96.0.0/12,192.168.0.1/24,10.190.212.0/23,10.243.22.0/23,10.237.213.0/24,10.190.204.134"
  # all proxy need to use socks5 proxy to connect nats based servers(case with platform attestation components)
  #all_proxy: ""
  
 # Proxy for be used by GIT HTTP - required if GIT HTTP via proxy
 #git_http_proxy: "{{ proxy_env['http_proxy'] | default('') }}"
 git_http_proxy: " "
```
User should disable the sgx and PA node as shown below

```shell
# SGX requires kernel 5.11+, SGX enabled in BIOS and access to PCC service
sgx_enabled: "{{ false if ansible_product_name == 'MOROCITY' else true }}"
sgx_enabled: true

## Install HWE Kernel for SGX
install_hwe_kernel_enable: true

# PCCS server IP address
sgx_pccs_ip: "<AWS instance IP>"

# PCCS server port address
sgx_pccs_port: "32666"

# To accept insecure HTTPS cert, set this option to FALSE
sgx_use_secure_cert: false

# Provide a token to access PCCS
pccs_user_token: "smartegdesgx"

#platform_attestation_node: "{{ false if ansible_product_name == 'MOROCITY' else true }}"
platform_attestation_node: false

# CMS hash from Intel-secl controller. Use following command:
# kubectl exec -n isecl --stdin "$(kubectl get pod -n isecl -l app=cms -o jsonpath="{.items[0].metadata.name}")" -- cms tlscertsha384
isecl_cms_tls_hash: " "

# Host on which NFS server is setup. If left empty, NFS server will be installed on kubernetes controller
isecl_nfs_server: ""

# List of nfs clients allowed to server. Only used when NFS server installed on kubernetes controller
isecl_nfs_server_clients: []

# Host IP of node hosting isecl controlplane(core) services. This could be hosted on cloud as well.
isecl_control_plane_ip: ""

###############
## PCCS for SGX

# ApiKey - The PCCS uses this API key to request collaterals from Intel's Provisioning Certificate Service
pccs_api_key: "<pccs_primary_key>"

# PCCS client
pccs_user_password: "smartedgesgx"

# PCCS administrator
pccs_admin_password: "smartedgesgx"

# NodePort to access PCCS from outside of cluster
# pccs_access_port: 32666

```

5. Secure password-less connection between AWS EC2 instance and Ansible host by executing the below commands.

  Copy public key from Ansible host 'vi .ssh/id_rsa.pub' and pass the keys to 'vi .ssh/authorized_keys' on AWS instance

6. Create a config file and configure the below details in Ansible host 
  
  ```shell       
   [smartedge-open@XXX .ssh]# cat config
    Host * !*.intel.com !10.*.*.*
    ProxyCommand connect-proxy -S proxy-iind.intel.com:1080 %h %p
  ``` 
 
7. Verify the connection between the ansible node and AWS instance 

```shell
smartedge-open@XXXXXX:~$ ssh ubuntu@< AWS instance ip >
Last login: Wed Dec 22 10:16:07 2021 from 192.55.79.171
ubuntu@ip-10-0-0-159:~$
```

8. After the  connection successful, run the deployment using `./deploy.sh`

9. Once deployment successful run `kubectl get pods -A` on AWS instance to ensure all pods are in running state

 ```shell
ubuntu@ip-10-0-0-85:~$ kubectl get pods -A
NAMESPACE      NAME                                       READY   STATUS       RESTARTS       AGE
apphsm         apphsm-657845978b-wfxcb                    2/2     Running      2 (19h ago)    2d5h
cert-manager   cert-manager-779bc6bf59-f6fmz              1/1     Running      2 (19h ago)    7d4h
cert-manager   cert-manager-cainjector-55765d687b-2655k   1/1     Running      2 (19h ago)    7d4h
cert-manager   cert-manager-webhook-85d85f46c7-cxlgn      1/1     Running      2 (19h ago)    7d4h
isecl          aas-687c4cc568-rv7n7                       1/1     Running      2 (19h ago)    7d4h
isecl          aasdb-74b748ccdd-4whmz                     1/1     Running      2 (19h ago)    7d4h
isecl          cms-84d5bdf9db-m2hnt                       1/1     Running      2 (19h ago)    7d4h
isecl          cms-init-nlr52                             0/1     Completed    0              7d4h
isecl          hvs-6d6648f968-mm52b                       1/1     Running      2 (19h ago)    7d4h
isecl          hvsdb-85c595564-9n8p4                      1/1     Running      2 (19h ago)    7d4h
isecl          nats-0                                     1/1     Running      2 (19h ago)    7d4h
isecl          nats-1                                     1/1     Running      2 (19h ago)    7d4h
isecl          nats-init-bvkg7                            0/1     Init:Error   0              7d4h
isecl          nats-init-gzb5s                            0/1     Completed    0              7d4h
kube-system    calico-kube-controllers-85b5b5888d-tgdgf   1/1     Running      2 (19h ago)    7d4h
kube-system    calico-node-czn5s                          1/1     Running      2 (19h ago)    7d4h
kube-system    coredns-64897985d-8wbhd                    1/1     Running      2 (19h ago)    7d5h
kube-system    coredns-64897985d-sqw9f                    1/1     Running      2 (19h ago)    7d5h
kube-system    etcd-ip-10-0-0-85                          1/1     Running      14 (19h ago)   7d5h
kube-system    kube-apiserver-ip-10-0-0-85                1/1     Running      14 (19h ago)   7d5h
kube-system    kube-controller-manager-ip-10-0-0-85       1/1     Running      14 (19h ago)   7d5h
kube-system    kube-proxy-h8977                           1/1     Running      2 (19h ago)    7d5h
kube-system    kube-scheduler-ip-10-0-0-85                1/1     Running      14 (19h ago)   7d5h
pccs           pccs-7567f6dd4b-5qbt7                      1/1     Running      2 (19h ago)    7d4h
```

## ITP/DEK/SEC/KMRA/02: Verify SGX and KMRA provisioning on DEK with ESP

### Test Summary

Test walks through singlenode deployment procedure and asserts that
- docker is installed and configured
- proxy for client is configured
- proxy for daemon is configured
- Edge Service Provisioner repository is checked out
- config.yml file is set up

### Prerequisites

- Clean machine or VM with Ubuntu 20.04 with proxy configured
- Should be executed as root user

### Test Steps

1. Get update on Ubuntu machnie `apt-get update`
   
2. Install certificates `apt-get -y install apt-transport-https ca-certificates curl gnupg lsb-release`

3. Download package from ubuntu site `curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg` and make changes in files `echo \
  "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null`

4. Get update on Ubuntu machine `apt-get update` again

5. Install docker on Ubuntu machine using command `apt-get -y install docker-ce docker-ce-cli containerd.io`

6. Prepare docker compose to enable using multi-container applications: 
   `curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose && chmod +x /usr/local/bin/docker-compose`

7. Add environment variables `HOST_IP=$(ip route get 8.8.8.8 | awk '{print $7}')` and `LOCAL_NETWORK="192.168.122.0/24,10.102.227.128/25"`
   
8. Create docker folder for client on `home` catalog: `mkdir ~/.docker`
   
9. Configure proxy config file for client in `~/.docker/config.json` file:
  ```
  cat << EOF > ~/.docker/config.json
  {
   "proxies":
   {
    "default":
    {
      "httpProxy": "http://proxy-mu.intel.com:911",
      "httpsProxy": "http://proxy-mu.intel.com:912",
      "noProxy": "localhost,127.0.0.0/8,${HOST_IP},${LOCAL_NETWORK}"
    }
   }
  }
  EOF
  ```
10. Create docker folder for daemon proxy configuration: `mkdir -p /etc/systemd/system/docker.service.d`
   
11. Configure proxy configuration for docker daemon in `/etc/systemd/system/docker.service.d/http-proxy.conf` file: 
  ```
  cat << EOF > /etc/systemd/system/docker.service.d/http-proxy.conf
  [Service]
  Environment="HTTP_PROXY=http://proxy-mu.intel.com:911"
  Environment="HTTPS_PROXY=http://proxy-mu.intel.com:912"
  Environment="NO_PROXY=localhost,127.0.0.1,${HOST_IP},${LOCAL_NETWORK}"
  EOF
  ```

12. Install git on machine using `apt install git`

13. Add to your machine environment variables as below: 
    `GH_USER="github_user_name"`
    `GH_TOKEN="github_token"`
    `REGISTRY_MIRROR=http://example.local:5000`
  
**NOTE**:
To generate token on `https://github.com/` go to Settings -> Developer settings -> Personal access tokens.
Don't forget to authorize this token to access intel-collab, intel-innersource and intel-sandbox.

14. Configure and add docker registry mirror using `mkdir -p /etc/docker` and then add 
    ```
    cat << EOF > /etc/docker/daemon.json
    {
      "registry-mirrors": ["http://example.local:5000"]
    }
    EOF
    ```

15. Reload docker settings `systemctl daemon-reload`
   
16. Restart docker `systemctl restart docker`
    
17. Clone Developer Experience Kits repo on machine `git clone https://${GH_TOKEN}@github.com/intel-innersource/applications.services.smart-edge-open.developer-experience-kits-open.git --branch github_branch_name ./dek`

18. Entered to pulled folder `cd dek`
    
19. Create config file using `./dek_provision.py --init-config > provision.yml` and modify fields 
    ```
    github:
      user: 'github_user'
      token: 'gihtub_token'
    ...
    registry_mirrors: [registry_mirror_address]
    ...
    ntp_server: 'ntp_server'
    ...
        # Secure boot and trusted media platform options.
    bios:
      secure_boot: true
      tpm: true
    bmc:
     address: 10.190.211.59
     user: root
     password: root@123  
    ...
    group_vars:
      groups:
        all:
          sgx_enabled: true
          sgx_pccs_ip: "Aws_controller_ip"
          sgx_pccs_port: "32666"
          sgx_use_secure_cert: false
          pccs_user_token: "smartedgesgx"
          pccs_api_key: "xxxxxxxxxxxx"
          pccs_user_password: "smartedgesgx"
          pccs_admin_password: "smartedgesgx"
          platform_attestation_node: true
          isecl_control_plane_ip: "AWS_Controller_Ip"
          isecl_cms_tls_hash: "xxxxxxxxxxxxxxxxxxxxxx"
        proxy_env:
          all_proxy: "socks5://proxy-iind.intel.com:1080"
          ftp_proxy: http://proxy-mu.intel.com:911
          http_proxy: http://proxy-mu.intel.com:911
          https_proxy: http://proxy-mu.intel.com:912
          no_proxy: "localhost,127.0.0.1,10.244.0.0/24,10.96.0.0/12,192.168.0.1/24,10.190.212.0/23,10.243.22.0/23,10.237.213.0/24,10.190.204.134"      
        controller_group:
        edgenode_group:      
    ```
    
20. Run ESP script using command `./dek_provision.py --config provision.yml`

21. Run server using command `./dek_provision.py --config provision.yml --run-esp-for-usb-boot`

22. On provisioned machine mount UOS image. Log in to machine using BMC and then open console. Mount `efi` image using `Map Removable Disk` and after aproving that change `Boot Option` from `Normal Boot` to `Virtual Floppy`. Reboot machine. 

23.  After reboot wait until Smart Edge Open deployment status has changed from `in progress` to `deployed` (you can also login with default credentials and check if file `/opt/seo/.deployed` exists). Ansible deployment should ended with pass status - in file `/opt/seo/logs/` are ansible logs.

24. once  deployment successful , login with default credentials and run `kubectl get pods -A` on target host to ensure all pods are in running state

  

## ITP/DEK/SEC/KMRA/03: Verify onboard of single instance KMRA NGINX application within secure enclave on DEK node.

### Test Summary

#### Prerequesites

Follow [ITP/DEK/SEC/KMRA/01: Verify deployment of verification controller on AWS for PCCS and KMRA control plane services](#itpDEKSECKMRA01-Verify-deployment-of-verification-controller-on-AWS-for-PCCS-and-KMRA-control-plane-services)


#### Test steps


## ITP/DEK/SEC/KMRA/04: Verify onboard of 3 instances of KMRA NGINX application having their own secure enclave on DEK node

### Test Summary

#### Prerequesites

Follow [ITP/DEK/SEC/KMRA/01: Verify deployment of verification controller on AWS for PCCS and KMRA control plane services](#itpDEKSECKMRA01-Verify-deployment-of-verification-controller-on-AWS-for-PCCS-and-KMRA-control-plane-services)

#### Test steps

## ITP/DEK/SEC/KMRA/05: Verify deployment of verification controller with PCCS KMRA and Platform attestation services in single AWS instance

### Test Summary

#### Prerequesites
Follow [ITP/DEK/SEC/KMRA/01: Verify deployment of verification controller on AWS for PCCS and KMRA control plane services](#itpDEKSECKMRA01-Verify-deployment-of-verification-controller-on-AWS-for-PCCS-and-KMRA-control-plane-services)

#### Test steps


## ITP/DEK/SEC/KMRA/06: Verify simultaneous bring up of 10 KMRA NGINX applications having their own secure enclave on DEK node

### Test Summary

#### Prerequesites

Follow [ITP/DEK/SEC/KMRA/01: Verify deployment of verification controller on AWS for PCCS and KMRA control plane services](#itpDEKSECKMRA01-Verify-deployment-of-verification-controller-on-AWS-for-PCCS-and-KMRA-control-plane-services)

#### Test steps


## TP/DEK/SEC/KMRA/07: Verify onboard of KMRA NGINX application in multinode DEK referring same PCCS and KMRA cloud services

### Test Summary

#### Prerequesites

Follow [ITP/DEK/SEC/KMRA/01: Verify deployment of verification controller on AWS for PCCS and KMRA control plane services](#itpDEKSECKMRA01-Verify-deployment-of-verification-controller-on-AWS-for-PCCS-and-KMRA-control-plane-services)

#### Test steps


## ITP/DEK/SEC/KMRA/08: Verify the cryptographic performance of SKM against non KMRA SGX enclave

### Test Summary

#### Prerequesites

Follow [ITP/DEK/SEC/KMRA/01: Verify deployment of verification controller on AWS for PCCS and KMRA control plane services](#itpDEKSECKMRA01-Verify-deployment-of-verification-controller-on-AWS-for-PCCS-and-KMRA-control-plane-services)

#### Test steps

## ITP/DEK/SEC/KMRA/09: Verify onboard of two NGINX application with and without secure enclave on DEK node.

### Test Summary

#### Prerequesites

Follow [ITP/DEK/SEC/KMRA/01: Verify deployment of verification controller on AWS for PCCS and KMRA control plane services](#itpDEKSECKMRA01-Verify-deployment-of-verification-controller-on-AWS-for-PCCS-and-KMRA-control-plane-services)

#### Test steps


## ITP/DEK/SEC/KMRA/10: Verify deployment of KMRA NGINX application post recovery of AWS instance from forced reboot

### Test Summary

#### Prerequesites

Follow [ITP/DEK/SEC/KMRA/01: Verify deployment of verification controller on AWS for PCCS and KMRA control plane services](#itpDEKSECKMRA01-Verify-deployment-of-verification-controller-on-AWS-for-PCCS-and-KMRA-control-plane-services)

#### Test steps


## ITP/DEK/SEC/KMRA/11: Verify deployment of NGINX application with KMRA control plane services in AWS instance not reachable

### Test Summary

#### Prerequesites

Follow [ITP/DEK/SEC/KMRA/01: Verify deployment of verification controller on AWS for PCCS and KMRA control plane services](#itpDEKSECKMRA01-Verify-deployment-of-verification-controller-on-AWS-for-PCCS-and-KMRA-control-plane-services)

#### Test steps


## ITP/DEK/SEC/KMRA/12: Verify the enclave memory exhaustion handling with multiple instances of NGINX application

### Test Summary

#### Prerequesites

Follow [ITP/DEK/SEC/KMRA/01: Verify deployment of verification controller on AWS for PCCS and KMRA control plane services](#itpDEKSECKMRA01-Verify-deployment-of-verification-controller-on-AWS-for-PCCS-and-KMRA-control-plane-services)

#### Test steps




