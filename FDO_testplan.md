### Test Summary

The FIDO protocols use standard public key cryptography techniques to provide stronger authentication. During registration with an online service, the user's client device creates a new key pair. It retains the private key and registers the public key with the online service.

Provisioning with FDOs

1.	The manufacturer creates an Ownership Voucher based on the credentials in the Device Initialize Protocol (DI). The Ownership Voucher is a digital document that provides the Owner with the credentials to take ownership of the Device. It is extended with each owner while the device is offline (i.e., boxed or shipped) between Manufacturer and Owner.
2.	Create ownership proxy: Load Ownership Voucher to FDO owner service at procurement.


### Hardware Requirements

*Controller and Node System*
Controller node:
Clean AWS instance or VM with Ubuntu 20.04

- At least 16 GB RAM
- At least 2 CPU 

Smartedge node:

- Processor: Intel® Xeon® processor.
-	At least 32 GB RAM.
-	At least 512 GB hard drive.
-	An Internet connection.
-	Ubuntu 20.04.2 LTS Server



ITP/DEK/SEC/FDO/01: Verify deployment of verification controller on AWS for manufacturer, owner , Rvs services for FDO( checking the health api’s for the three pods)
Test summary :
To verify the deployment of fdo on AWS and verifying the pod services 
ITP/DEK/SEC/FDO/02 Verify the deployment of isecl and fdo on DEK without ESP 
ITP/DEK/SEC/FDO/03 Verify the manual copy of the device serial number after DI
ITP/DEK/SEC/FDO/04 Verify the creation and download  of ownership voucher on the manufacturer service 
ITP/DEK/SEC/FDO/05 verify the TO0 protocol to map device GUID to the rv service and also to discover the owner
ITP/DEK/SEC/FDO/06 Verify the TO2 protocol with owner service and obtain credentials & scripts 
ITP/DEK/SEC/FDO/07 Verify the TA demonstration on node 
ITP/DEK/SEC/FDO/08 Verify the  API’s
ITP/DEK/SEC/FDO/09  lables applied on the node or not 
Negative :  switching off the DI , to1 and to2 
Combination of other security features(sgx,kmra)
Disabling the fdo and validating other security features(sgx, pa,kmra) it should work fine 



 









