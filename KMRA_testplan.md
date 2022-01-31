### Test Summary

Key Management Reference Application (KMRA) is a proof-of-concept software created to demonstrate the integration of Intel® Software Guard Extensions (Intel® SGX) asymmetric key
capability with a hardware security model (HSM) on a centralized key server. 
This reference application sets up a NGINX workload to access the private key in an Intel® SGX enclave on a 3rd Generation Intel® Xeon® Scalable processor, 
using the Public-Key Cryptography Standard (PKCS) #11 interface and OpenSSL.


### Hardware Requirements

*Controller and Node System*

Smart Edge node & Verification controller(AWS instance) 

 Smart Edge Cluster Node
- ICX Xeon processor with secure boot and TPM enabled 
- At least 64 GB RAM
- At least 512 GB hard drive
- Kernel Version> 5.13
- An Internet connection
- Ubuntu 20.04

ITP/SGX/KMRA/01: Deploy Verification Controller on AWS and verify the pods 

1. The Key serve running on AWS cloud that the edge node’s enclave is running with Intel SGX protections on a trusted
Intel SGX-enabled platform, with a valid TCB. 

2. The Intel SGX quote of the edge node is validated by the quote verification library (QVL) on the AWS cloud.
If quote is correct, the hash of enode_pub is verified with sgx_quote_t, enode_pub is imported into the HSM as a session object,
and a symmetric wrapping key (aes_swk) is generated as a session object. 

3. AppHSM creates wrapped_priv_key by wrapping RSA private key (rsa_priv) with SWK (aes_swk) using
CKM_AES_KEY_WRAP_PAD. AppHSM also creates wrapped_swk by wrapping SWK (aes_swk) with the imported edge node public key (enode_pub) using RSA OAEP.

4. As a part of successfull the verification, Wrapped keys (wrapped_priv_key and wrapped_swk) are released by the SoftHSM key server and sent back to
the edge node for provisioning into the enclave.



ITP/SGX/KMRA/02: Deployment of KMRA and verify Integration with DEK

1.KMRA Crypto Engine is running on Smart Edge Node with an Intel SGX-enabled platform.
 
2. The Edge node sends a request to AWS cloud containing an Intel® SGX quote, a public key from Crypto API Toolkit for Intel SGX,
and a unique ID to identify the key pair to extract. 

3. This requests is constructed using json-c and requests are sent using libcurl.


4. For constructing a request an public/private key pair (enode_pub/enode__priv) is generated as a session object on the edge node. An
attestation quote sgx_quote_t is generated using Crypto API Toolkit for Intel SGX and attests the hash of ss_pub and the enclave.

5. Edge node sends REST API request containing a (enode_pub) to AppHSM to trigger the Intel SGX quote verification library.


ITP/SGX/KMRA/03: Deploying and verifying NGINX pods  on DEK 

1. once NGNIX pods are  created need to verify the web application

2. For NGINX Application to access the secured private key provisioned by the key server,
a libp11 engine is configured with OpenSSL. The libp11 engine is an interface for NGINX Application to access keys secured by Crypto API Toolkit for Intel SGX.




ITP/SGX/KMRA/04: Verifying the symmetric application keys for two ngnix  pods inside the encalve

1. create two ngnix pods on DEK
2. Verify if it is accessing the pods with the same or different keys 


ITP/SGX/KMRA/05:















