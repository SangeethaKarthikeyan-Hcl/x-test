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


- [ITP/DEK/SEC/KMRA/05: Verify deployment of verification controller with PCCS KMRA and Platform attestation services in single AWS instance](#itpDEKSECKMRA05-Verify-deployment-of-verification-controller-with-PCCS-KMRA-and-Platform-attestation-services-in-single- AWS instance)
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

## ITP/DEK/SEC/KMRA/01: Verify deployment of verification controller on AWS for PCCS and KMRA control plane services

### Test Summary

#### Prerequesites

#### Test steps


## ITP/DEK/SEC/KMRA/02: Verify SGX and KMRA provisioning on DEK with ESP

### Test Summary

#### Prerequesites

#### Test steps

## ITP/DEK/SEC/KMRA/03: Verify onboard of single instance KMRA NGINX application within secure enclave on DEK node.

### Test Summary

#### Prerequesites

#### Test steps


## ITP/DEK/SEC/KMRA/04: Verify onboard of 3 instances of KMRA NGINX application having their own secure enclave on DEK node

### Test Summary

#### Prerequesites

#### Test steps

## ITP/DEK/SEC/KMRA/05: Verify deployment of verification controller with PCCS, KMRA-C and Platform attestation services in single AWS instance

### Test Summary

#### Prerequesites

#### Test steps


## ITP/DEK/SEC/KMRA/06: Verify simultaneous bring up of 10 KMRA NGINX applications having their own secure enclave on DEK node

### Test Summary

#### Prerequesites

#### Test steps


## TP/DEK/SEC/KMRA/07: Verify onboard of KMRA NGINX application in multinode DEK referring same PCCS and KMRA cloud services

### Test Summary

#### Prerequesites

#### Test steps


## ITP/DEK/SEC/KMRA/08: Verify the cryptographic performance of SKM against non KMRA SGX enclave

### Test Summary

#### Prerequesites

#### Test steps

## ITP/DEK/SEC/KMRA/09: Verify onboard of two NGINX application with and without secure enclave on DEK node.

### Test Summary

#### Prerequesites

#### Test steps


## ITP/DEK/SEC/KMRA/10: Verify deployment of KMRA NGINX application post recovery of AWS instance from forced reboot

### Test Summary

#### Prerequesites

#### Test steps


## ITP/DEK/SEC/KMRA/11: Verify deployment of NGINX application with KMRA control plane services in AWS instance not reachable

### Test Summary

#### Prerequesites

#### Test steps


## ITP/DEK/SEC/KMRA/12: Verify the enclave memory exhaustion handling with multiple instances of NGINX application

### Test Summary

#### Prerequesites

#### Test steps




















