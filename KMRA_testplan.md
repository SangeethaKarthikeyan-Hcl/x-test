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
sgx_enabled: false

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
ubuntu@ip-10-0-0-159:~$ kubectl get pods -A
NAMESPACE      NAME                                       READY   STATUS    RESTARTS   AGE
cert-manager   cert-manager-cainjector-7db56c5fd6-knbzn   1/1     Running   0          4h32m
cert-manager   cert-manager-f5c794b65-wl52s               1/1     Running   0          4h32m
cert-manager   cert-manager-webhook-6c86d79f7c-gkhxc      1/1     Running   0          4h32m
kube-system    calico-kube-controllers-6c85b56fcb-9dxpg   1/1     Running   0          4h32m
kube-system    calico-node-fn9d4                          1/1     Running   0          4h32m
kube-system    coredns-78fcd69978-fhzgh                   1/1     Running   0          4h33m
kube-system    coredns-78fcd69978-hxrlz                   1/1     Running   0          4h33m
kube-system    etcd-ip-10-0-0-159                         1/1     Running   3          4h33m
kube-system    kube-apiserver-ip-10-0-0-159               1/1     Running   0          4h33m
kube-system    kube-controller-manager-ip-10-0-0-159      1/1     Running   0          4h33m
kube-system    kube-proxy-jskjv                           1/1     Running   0          4h33m
kube-system    kube-scheduler-ip-10-0-0-159               1/1     Running   0          4h33m
pccs           pccs-7dfb57dfc6-m4dmw                      1/1     Running   0          4h26m
```




## ITP/DEK/SEC/KMRA/02: Verify SGX and KMRA provisioning on DEK with ESP

### Test Summary

#### Prerequesites

[Follow Suite Prerequisites](#prerequisites)

#### Test steps

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




