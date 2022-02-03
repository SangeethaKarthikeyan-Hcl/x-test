- [ITP/RI/Telehealth/01: Telehealth RI Installation and Verification](#itpritelehealth01-Telehealth-RI-Installation-and-Verification)
  - [Test Summary](#test-summary)
  - [Prerequisites](#prerequisites)
  - [Hardware Requirements](#hardware-requirements)
  - [Test steps](#test-steps)

- [ITP/RI/Telehealth/02: Perform input parameter validation for single node host ip address during Telehealth installation](#itpritelehealth02-Perform-input-parameter-validation-for-single-node-host-ip-address-during-Telehealth-installation)
  - [Test Summary](#test-summary-1)
  - [Prerequisites](#prerequisites-1)
  - [Test steps](#test-steps-1) 

- [ITP/RI/Telehealth/03: Verify accessibility to web RTC UI with invalid port for Telehealth RI](#itpritelehealth03-Verify-accessibility-to-web-RTC-UI-with-invalid-port-for-Telehealth-RI)
  - [Test Summary](#test-summary-2)
  - [Prerequisites](#prerequisites-2)
  - [Teist steps](#test-steps-2)

- [ITP/RI/Telehealth/04: Verify recovery of single node cluster with Telehealth RI from forced reboot](#itpritelehealth04-Verify-recovery-of-single-node-cluster-with-Telehealth-RI-from-forced-reboot)
  - [Test Summary](#test-summary-3)
  - [Prerequisites](#prerequisites-3)
  - [Test steps](#test-steps-3)

- [ITP/RI/Telehealth/05: Delete Telehealth application pod and verify its existence on web visualizer portal via web browser](#itpritelehealth05-Delete-Telehealth-application-pod-and-verify-its-existence-on-web-visualizer-portal-via-web-browser)
  - [Test Summary](#test-summary-4)
  - [Prerequisites](#prerequisites-4)
  - [Test steps](#test-steps-4)

- [ITP/RI/Telehealth/06: Verify Product key integrity during Telehealth installation](#itpritelehealth06-Verify-Product-key-integrity-during-telehealth-installation)
  - [Test Summary](#test-summary-5)
  - [Prerequisites](#prerequisites-5)
  - [Test steps](#test-steps-5)
 
- [ITP/RI/Telehealth/07: Verify reinstallation of Telehealth RI](#itpritelehealth07-Verify-reinstallation-of-Telehealth-RI)
  - [Test Summary](#test-summary-6)
  - [Prerequisites](#prerequisites-6)
  - [Test steps](#test-steps-6)

- [ITP/RI/Telehealth/08: Verify Telehealth RI installation with product key of another RI instance](#itpritelehealth08-Verify-Telehealth-RI-installation-with-product-key-of-another-RI-instance)
  - [Test Summary](#test-summary-7)
  - [Prerequisites](#prerequisites-7)
  - [Test steps](#test-steps-7) 

- [ITP/RI/Telehealth/09: Verify uninstall and install of Telehealth RI with an appropriate product key](#itpritelehealth09-Verify-uninstall-and-install-of-Telehealth-RI-with-an-appropriate-product-key)
  - [Test Summary](#test-summary-8)
  - [Prerequisites](#prerequisites-8)
  - [Test steps](#test-steps-8)

- [ITP/RI/Telehealth/10: Verify uninstall and  install of Telehealth RI with an invalid product key](#itpritelehealth10-Verify-uninstall-and-install-of-Telehealth-RI-with-an-invalid-product-key)
  - [Test Summary](#test-summary-9)
  - [Prerequisites](#prerequisites-9)
  - [Test steps](#test-steps-9)


## ITP/RI/Telehealth/01: Telehealth RI Installation and Verification

### Test Summary

Telehealth Remote Monitoring is designed to connect WebRTC-based video and data sessions between parties to facilitate optimized modern telehealth application design

This test suite covers the following features

- The containerized WebRTC server facilitates sessions between users that could include clinicians, patients, and caregivers

- A web application interface provides a way for users to connect, view, and communicate from a range of possible end point devices including cameras, phones, and laptops

- Audio capabilities for remote monitoring are not included in this release, communication is provided through video, chat, and screen sharing

- Use Intel® Collaboration Suite for WebRTC (Intel® CS for WebRTC) and Open Visual Cloud pipeline building blocks for real-time video analytics-based deep learning for telepresence and video conference capabilities.

The application is delivered in a container format and installed via helm chart

### Prerequisites

- Downloaded RI with Product Key

- Refer following link for ESP-ISO ESP w/ Ubuntu OS image creation
  
  https://github.com/intel-innersource/applications.services.smart-edge-open.test-validation/blob/main/test_plans/se-o/esp/singlenode/ts01-esp-platform-setup-singlenode.md#itpseoespsingle0102-deploy-esp-on-host-machine-with-ubuntu-provision-target-machine-with-ubuntu-profile-using-usb-boot-method
  

### Hardware Requirements

**Controller and Node System**

Smart Edge Cluster Node

- One of the following processors:  
  - Intel® Xeon® scalable processor
  - Intel® Xeon® processor D 
- At least 32 GB RAM. 
- At least 512 GB hard drive. 
- An Internet connection. 
- Ubuntu 20.04.2 LTS

### Test Steps

**Step 1 :Install the Reference Implementation**

1.Download and configure the Reference Implementation from the following link :

https://eshqawebui.intel.com/iot/edgesoftwarehub/download/home/ri/telehealth_remote_monitoring

2. Select and download required version to be tested as shown below:


![image](https://github.com/intel-sandbox/applications.services.smart-edge-open.test-validation/assets/77773989/43ce026f-e4d5-435e-bc02-cf1b52c9937b)



3. Once your download is complete, open a target host as a non root user "smartedge-open" and copy the zip file to your target system /home/smartedge-open directory

```shell
$ cd /home/smartedge-open
$ mv <path-of-downloaded-directory>/telehealth_remote_monitoring.zip

3. Unzip the RI package and change permission of 'edgesoftware' executable file

```shell
$ unzip telehealth_remote_monitoring.zip
$ cd telehealth_remote_monitoring.zip
$ chmod 777 edgesoftware 
```
4.Run the command below to install the Reference Implementation:

```shell
$ ./edgesoftware install
``` 
5.During the installation, you will be prompted for the Product Key. the Product Key is generated while you are downloading the package 

**Note:** Installation logs are available at path: /var/log/esb-cli/telehealth_remote_monitoring_<version>/<Component_name>/install.log

```shell

smartedge-open@ubuntu-0f98a4df7d:~/telehealth_remote_monitoring$ ./edgesoftware install

Please enter the Product Key. The Product Key is contained in the email you received from Intel confirming your download:


Starting the setup...
ESB CLI version: 2021.3
Target OS: Ubuntu 20.04
Python version: 3.8.10
Checking Internet connection
Connected to the Internet
Validating product key
Successfully validated Product Key
Checking for prerequisites
creating build
creating build/bdist.linux-x86_64
creating build/bdist.linux-x86_64/egg
creating build/bdist.linux-x86_64/egg/EGG-INFO
copying esb_common.egg-info/PKG-INFO -> build/bdist.linux-x86_64/egg/EGG-INFO
copying esb_common.egg-info/SOURCES.txt -> build/bdist.linux-x86_64/egg/EGG-INFO
copying esb_common.egg-info/dependency_links.txt -> build/bdist.linux-x86_64/egg/EGG-INFO
copying esb_common.egg-info/top_level.txt -> build/bdist.linux-x86_64/egg/EGG-INFO
zip_safe flag not set; analyzing archive contents...
creating dist
creating 'dist/esb_common-0.1-py3.8.egg' and adding 'build/bdist.linux-x86_64/egg' to it
removing 'build/bdist.linux-x86_64/egg' (and everything under it)
Processing esb_common-0.1-py3.8.egg
Copying esb_common-0.1-py3.8.egg to /usr/local/lib/python3.8/dist-packages
Adding esb-common 0.1 to easy-install.pth file
```

6. During the installation, user will be prompted to configure Host system IP address. 
   Refer below output and When the installation is complete, user will see the message **Installation 
   of package complete** with the installation status of module
	

Refer the below output to configure


```shell
Installed /usr/local/lib/python3.8/dist-packages/esb_common-0.1-py3.8.egg
Processing dependencies for esb-common==0.1
Finished processing dependencies for esb-common==0.1
Successfully installed shared module 'esb_common'.
Unzipping the module telehealth_remote_monitoring...
Modules to be installed by package are ['telehealth_remote_monitoring']
<frozen importlib._bootstrap>:219: RuntimeWarning: compiletime version 3.6 of module 'esb_install' does not match runtime version 3.8
Installing telehealth_remote_monitoring
Installing Telehealth Dependencies on SE Node      [..................................................] 100%
Enter correct IP address of SE Node [must be host system IP] (Example:: 123.123.123.123): *<target host ip>*
Verifying SE Node (<target host ip>)                 [..................................................] 100%
```
```shell
Telehealth Remote Monitoring started. This might take upto 10 mins
Installing Telehealth Remote Monitoring            [.........................                         ]  100%
Successfully installed: Telehealth Remote Monitoring Application
Successfully installed telehealth_remote_monitoring took 2 minutes 59.22 seconds
Installation of package complete
***Recommended to reboot system after installation***
+--------------------------+------------------------------+---------+
|            Id            |            Module            |  Status |
+--------------------------+------------------------------+---------+
| 611a2d4abd4d13002b6ba4f4 | telehealth remote monitoring | SUCCESS |
+--------------------------+------------------------------+---------+
```



**Note:** Installation logs are available at path: /var/log/esb-cli/telehealth_remote_monitoring/<Component_name>/install.log



7. If Intel® Smart Edge Open Developer Experience Kit is installed, running the following command should show output like the  below. 

All the pods should be either in running or completed stage. 

```shell
smartedge-open@ubuntu-0f98a4df7d:~/telehealth_remote_monitoring$ Kubectl get pods -A
NAMESPACE                NAME                                                         READY   STATUS    RESTARTS   AGE
harbor                   harbor-app-chartmuseum-68c888db5b-hprtf                      1/1     Running   4          35h
harbor                   harbor-app-core-7dcc974965-wcggd                             1/1     Running   4          35h
harbor                   harbor-app-database-0                                        1/1     Running   4          35h
harbor                   harbor-app-jobservice-7fdd6bb875-hqfnc                       1/1     Running   7          35h
harbor                   harbor-app-nginx-686dfb5c85-vz99v                            1/1     Running   9          35h
harbor                   harbor-app-notary-server-76bbc4cc58-zkxmb                    1/1     Running   5          35h
harbor                   harbor-app-notary-signer-6bb5df9db6-z6wqj                    1/1     Running   4          35h
harbor                   harbor-app-portal-88895f59f-rzt8k                            1/1     Running   4          35h
harbor                   harbor-app-redis-0                                           1/1     Running   4          35h
harbor                   harbor-app-registry-69cc9c94fd-xkjlx                         2/2     Running   8          35h
harbor                   harbor-app-trivy-0                                           1/1     Running   4          35h
kube-system              calico-kube-controllers-6b59cd85f8-ss6jq                     1/1     Running   4          35h
kube-system              calico-node-7qsxj                                            1/1     Running   4          35h
kube-system              coredns-558bd4d5db-jwwq2                                     1/1     Running   4          35h
kube-system              coredns-558bd4d5db-r6gwd                                     1/1     Running   4          35h
kube-system              etcd-ubuntu-429c278fc4                                       1/1     Running   4          35h
kube-system              kube-apiserver-ubuntu-429c278fc4                             1/1     Running   4          35h
kube-system              kube-controller-manager-ubuntu-429c278fc4                    1/1     Running   4          35h
kube-system              kube-multus-ds-amd64-lhn56                                   1/1     Running   4          34h
kube-system              kube-proxy-qmqtl                                             1/1     Running   4          35h
kube-system              kube-scheduler-ubuntu-429c278fc4                             1/1     Running   4          35h
smartedge-apps           telehealth-64bb799f96-nkpx8                                  2/2     Running   0          10m
smartedge-system         nfd-release-node-feature-discovery-master-5976cddddb-hcgk8   1/1     Running   4          34h
smartedge-system         nfd-release-node-feature-discovery-worker-f49gp              1/1     Running   4          34h
sriov-network-operator   network-resources-injector-jlh5t                             1/1     Running   4          34h
sriov-network-operator   operator-webhook-qmznt                                       1/1     Running   4          34h
sriov-network-operator   sriov-network-config-daemon-2bhsp                            3/3     Running   12         34h
sriov-network-operator   sriov-network-operator-69c9cf7df6-2x4d5                      1/1     Running   4          34h
telemetry                cadvisor-f5hgs                                               2/2     Running   8          34h
telemetry                collectd-9vrjs                                               3/3     Running   12         34h
telemetry                grafana-7755bf46-qtkrh                                       3/3     Running   12         34h
telemetry                prometheus-node-exporter-kkkhp                               1/1     Running   6          34h
telemetry                prometheus-server-55f574ff56-fmggr                           3/3     Running   12         34h
telemetry                statsd-exporter-c6b896b94-rz8zm                              2/2     Running   8          34h
telemetry                telemetry-node-certs-fj2qz                                   1/1     Running   4          34h
```

8. If Telehealth monitoring is installed, running the following command should show output like below:

```shell
smartedge-open@ubuntu-0f98a4df7d:~/telehealth_remote_monitoring$ kubectl get pods -n smartedge-apps

NAME                          READY   STATUS    RESTARTS   AGE
telehealth-64bb799f96-nkpx8   2/2     Running   0          12m

smartedge-open@ubuntu-0f98a4df7d:~/telehealth_remote_monitoring$ kubectl get pods -A | grep telehealth
smartedge-apps           telehealth-64bb799f96-nkpx8                                2/2     Running   0          12m
```

9.Run the below command to make sure the network policy is created

**NOTE**: In Intel® Smart Edge Open Developer Experience Kit, the default network policy blocks ingress traffic to all pods, hence the telehealth pod network policy is created to allow the ingress traffic.

```shell
smartedge-open@ubuntu-0f98a4df7d:~/telehealth_remote_monitoring$ kubectl get networkpolicies.
NAME                  POD-SELECTOR     AGE
allow-gst-analytics   app=telehealth   15m
block-all-ingress     <none>           2d14h
```

10. Run the command below to check the docker images and their details
	
	
```shell
smartedge-open@ubuntu-0f98a4df7d:~/docker images | grep owt
openvisualcloud/xeon-ubuntu1804-service-vcs-owt latest a1639d7ccb7c 6 weeks ago 214MB 
openvisualcloud/xeon-ubuntu1804-service-owt   latest 073486dba08   5 weeks ago     1.63GB
```	

**STEP 2:** Visualize the Output

1. To Visualize the results, launch an Internet browser and navigate to: https://<Smartedge-Node-IP-Address>:30443

![image](https://github.com/intel-sandbox/applications.services.smart-edge-open.test-validation/assets/77773989/ecc5bd5f-4e38-4f52-a44c-e76f00655b66)



2. Click on the Advanced option and it will be redirected to the page to proceed with unsafe mode:

![image](https://github.com/intel-sandbox/applications.services.smart-edge-open.test-validation/assets/77773989/15e72341-f9fa-4930-ab69-ffc899f19a22)



3.Enter the username to join the web conference, up to three participants can join a conference using chrome browser using Smart edge node IP and perform video conference, 

chats and screen sharing by selecting the options in webRTC web UI. 

In the Settings choose “Video and Audio” option. As audio is not supported in this release.


![image](https://github.com/intel-sandbox/applications.services.smart-edge-open.test-validation/assets/77773989/4f412268-2fc4-4f0a-90bf-dd28f480d730)




4. The security certificate of the following URL is not trusted by your computer's operating system. 
	
	
![image](https://github.com/intel-sandbox/applications.services.smart-edge-open.test-validation/assets/77773989/5df965ab-5853-43a1-bd1a-d83ca7cd4a33)
	
	

If you confirm to continue, click the URL and proceed to the unsafe host, then come back to this page and refresh. 
	

https://<smartedge-node-ip-address>:<socker io node port>/socket.io/




5. Participants will see similar types of options like video conferences, chats and screen sharing on webRTC web UI
	
	
![image](https://github.com/intel-sandbox/applications.services.smart-edge-open.test-validation/assets/77773989/41215877-1df4-4781-aeef-cdeae1bd4f06)	



6. You will see results like the image below when you join the web conference. In the image below two participants are joined
	
![image](https://github.com/intel-sandbox/applications.services.smart-edge-open.test-validation/assets/77773989/96004485-c684-4137-9d00-b4b45b255036)
	

	
**Step 3 : Uninstall the Telehealth Application**

To uninstall the deployment of this Reference Implementation, run the following commands. 

**Note:** This will remove all the running pods and the data and configuration stored in the device.

1. To list the package modules , running the below command will display the module status 

```shell
smartedge-open@ubuntu-0f98a4df7d:~/telehealth_remote_monitoring$ ./edgesofware list

+--------------------------+------------------------------+---------+
|            Id            |            Module            |  Status |
+--------------------------+------------------------------+---------+
| 611a2d4abd4d13002b6ba4f4 | telehealth remote monitoring | SUCCESS |
+--------------------------+------------------------------+---------+
```

2. During uninstall of telehealth application, telehealth pod and network policy will be removed from the smartedge-apps
  which are specific to telehealth RI.

```shell
smartedge-open@ubuntu-0f98a4df7d:~/telehealth_remote_monitoring$ ./edgesoftware uninstall 611a2d4abd4d13002b6ba4f4
Components to be uninstalled are :['telehealth_remote_monitoring']
<frozen importlib._bootstrap>:219: RuntimeWarning: compiletime version 3.6 of module 'esb_install' does not match runtime version 3.8
Uninstalling telehealth_remote_monitoring
Uninstalling Telehealth Remote Monitoring. This might take upto 10 minutes.
Cleaning up Telehealth Remote Monitoring Application [                                                  ]   0%
Successfully removed Telehealth Remote Monitoring Application from machine
Cleaning up Telehealth Remote Monitoring Application [..................................................] 100%
Successfully uninstalled telehealth_remote_monitoring took 1.12 seconds
Uninstall Finished
+--------------------------+------------------------------+---------+
|            Id            |            Module            |  Status |
+--------------------------+------------------------------+---------+
| 611a2d4abd4d13002b6ba4f4 | telehealth remote monitoring | SUCCESS |
+--------------------------+------------------------------+---------+
```

3. Run the command below to make sure the network policy is deleted after uninstall

```shell
smartedge-open@ubuntu-0f98a4df7d:~/telehealth_remote_monitoring$ kubectl get networkpolicies
NAME                POD-SELECTOR   AGE
block-all-ingress   <none>         2d15h
```

## ITP/RI/Telehealth/02: Perform input parameter validation for single node host ip address during Telehealth installation
  
### Test Summary

 Telehealth RI Installation should fail for an input of invalid single node ip address 
  
### Prerequisites
  
Start RI Installation (./edgesoftware install) as per [ITP/RI/Telehealth/01: Telehealth RI Installation and Verification](#itpritelehealth01-Telehealth-RI-Installation-and-Verification)

### Test Steps

1. During the installation, 'edgesoftware' script shall prompt user for input options before
   When prompted to enter IP address of SE Node, enter 'invalid' single node ip address as shown below: 

```shell
Installed /usr/local/lib/python3.8/dist-packages/esb_common-0.1-py3.8.egg	
Processing dependencies for esb-common==0.1	
Finished processing dependencies for esb-common==0.1	
Successfully installed shared module 'esb_common'.	
Modules to be installed by package are ['telehealth_remote_monitoring']	
<frozen importlib._bootstrap>:219: RuntimeWarning: compiletime version 3.6 of module 'esb_install' does not match runtime version 3.8	
Verifying Telehealth POD status in SE Node ()      [..................................................] 100%	
telehealth_remote_monitoring is already installed. Type YES to reinstall or NO to skip installation.	
yes	
Installing telehealth_remote_monitoring	
Installing Telehealth Dependencies on SE Node      [..................................................] 100%	
Enter correct IP address of SE Node [must be host system IP] (Example:: 123.123.123.123): 10.11.11.1	
Machine is unreachable. Cannot ping IP address 10.11.11.1	
Incorrect input entered: 10.11.11.1	
```

The Installation script should fail as the keyed in single node ip address is incorrect & not reachable.


## ITP/RI/Telehealth/03: Verify accessibility to web RTC UI with invalid port for Telehealth RI

### Test Summary
  
Verify the access to Telehealth grafana dashboard shall fail with invalid or out of bound port id (Local port ID)
  
## Prerequisites
  
Bringup the RI as per [ITP/RI/Telehealth/01: Telehealth RI Installation and Verification](#itpritelehealth01-Telehealth-RI-Installation-and-Verification)

### Test Steps
  
Visualizing the output on web browser
  
1.  Navigate to http://<Controller_IP_address>:<grafana_local_port_num> on your browser.
  
  Expected Sample Output :
  
 Preferred port range closest to the valid port are (30801, 30301, 8080,30004 etc.)

 once the invalid port is given , browser is not reachable as shown in below image.
	
![image](https://github.com/intel-sandbox/applications.services.smart-edge-open.test-validation/assets/77773989/bfe6d566-2e8f-4477-b437-0fd3210122b3)	

	


## ITP/RI/Telehealth/04: Verify recovery of single node cluster with Telehealth RI from forced reboot
  
###  Test Summary
  
Verify recovery of Telehealth RI by rebooting the Controller. Verify all pods are up and running and also verify output on web visualize


### Prerequisites
  
Bringup the RI as per [ITP/RI/Telehealth/01: Telehealth RI Installation and Verification](#itpritelehealth01-Telehealth-RI-Installation-and-Verification)
  
### Test Steps
  
1. Force reboot the single node host where the Telehealth RI has been installed.
   Pods status should prompt either as Running or Completed,

Expected o/p:

```shell
smartedge-open@ubuntu-0f98a4df7d:~/telehealth_remote_monitoring$ sudo reboot

Remote side unexpectedly closed network connection

```
after reboot , once machine reachable check pod status 

```shell
smartedge-open@ubuntu-0f98a4df7d:~/telehealth_remote_monitoring$ kubectl get pods -n smartedge-apps
NAME                          READY   STATUS    RESTARTS   AGE
telehealth-64bb799f96-jnvms   2/2     Running   0          81s
```

2. Navigate to http://<Controller_IP_address>:30040 on your browser

3. Login with user name and Verify functionality of telehealth RI through browsser


Expected o/p:
	
![image](https://github.com/intel-sandbox/applications.services.smart-edge-open.test-validation/assets/77773989/c2bb618d-cdc7-4fe5-967b-313b83fd8cee)
	
	

## ITP/RI/Telehealth/05: Delete Telehealth application pod and verify its existence on web visualizer portal via web browser

###  Test Summary
  
Delete Telehealth application pod and access the web visualizer on your browser
  
  
### Prerequisites
  
Bringup the RI as per [ITP/RI/Telehealth/01: Telehealth RI Installation and Verification](#itpritelehealth01-Telehealth-RI-Installation-and-Verification)
  
### Test Steps
  
1. Delete the Telehealth application pod

```shell

smartedge-open@ubuntu-0f98a4df7d:~/telehealth_remote_monitoring$ kubectl get deployments -n smartedge-apps	
NAME         READY   UP-TO-DATE   AVAILABLE   AGE	
telehealth   1/1     1            1           57m	
	
smartedge-open@ubuntu-0f98a4df7d:~/telehealth_remote_monitoring$ kubectl delete deployments.apps telehealth -n smartedge-apps	
deployment.apps "telehealth" deleted	
	
smartedge-open@ubuntu-0f98a4df7d:~/telehealth_remote_monitoring$ kubectl get pods -n smartedge-apps	
No resources found in smartedge-apps namespace.	
```

2. Verify the output on browser , Navigate to http://<Controller_IP_address>:30040 on your browser.

	
![image](https://github.com/intel-sandbox/applications.services.smart-edge-open.test-validation/assets/77773989/a26648b5-1f62-484d-8844-323343664cc6)

	
	
## ITP/RI/Telehealth/06: Verify Product key integrity during Telehealth installation

###  Test Summary
  
Telehealth RI Installation should fail for an input of invalid Product key

### Prerequisites
  
Bringup the RI as per [ITP/RI/Telehealth/01: Telehealth RI Installation and Verification](#itpritelehealth01-Telehealth-RI-Installation-and-Verification)

### Test Steps

During the installation, you will be prompted for the Product Key. Enter invalid Product key and verify whether installation failed

o/p:

```shell

smartedge-open@ubuntu-0f98a4df7d:~/telehealth_remote_monitoring$ ./edgesoftware install	
Please enter the Product Key. The Product Key is contained in the email you received from Intel confirming your download:  097fb656-b18d-4648-b5b1-f249a4a3b	
Starting the setup...	
ESB CLI version: 2021.3	
Target OS: Ubuntu 20.04	
Python version: 3.8.10	
Checking Internet connection	
Connected to the Internet	
Validating product key	
Invalid Product Key. Exiting installation	
```

## ITP/RI/Telehealth/07: Verify reinstallation of Telehealth RI

###  Test Summary
  
Re-install Telehealth RI in an already installed node

### Prerequisites
  
Bringup the RI as per [ITP/RI/Telehealth/01: Telehealth RI Installation and Verification](#itpritelehealth01-Telehealth-RI-Installation-and-Verification)
	
### Test Steps

Re-install Telehealth RI again in the same node, after the previous installation is success


```shell
nstalled /usr/local/lib/python3.8/dist-packages/esb_common-0.1-py3.8.egg	
Processing dependencies for esb-common==0.1	
Finished processing dependencies for esb-common==0.1	
Successfully installed shared module 'esb_common'.	
Unzipping the module telehealth_remote_monitoring...	
Modules to be installed by package are ['telehealth_remote_monitoring']	
<frozen importlib._bootstrap>:219: RuntimeWarning: compiletime version 3.6 of module 'esb_install' does not match runtime version 3.8	
Installing telehealth_remote_monitoring	
Installing Telehealth Dependencies on SE Node      [..................................................] 100%	
Enter correct IP address of SE Node [must be host system IP] (Example:: 123.123.123.123): < Host IP address > 	
Verifying SE Node < Host IP address > 	                [..................................................] 100%	
	
	
Telehealth Remote Monitoring started. This might take upto 10 mins	
Installing Telehealth Remote Monitoring            [.........................                         ]  50%	
Successfully installed: Telehealth Remote Monitoring Application	
Successfully installed telehealth_remote_monitoring took 2 minutes 59.22 seconds	
Installation of package complete	
***Recommended to reboot system after installation***	
+--------------------------+------------------------------+---------+	
|            Id            |            Module            |  Status |	
+--------------------------+------------------------------+---------+	
| 611a2d4abd4d13002b6ba4f4 | telehealth remote monitoring | SUCCESS |	
+--------------------------+------------------------------+---------+	
	
Modules to be installed by package are ['telehealth_remote_monitoring']	
<frozen importlib._bootstrap>:219: RuntimeWarning: compiletime version 3.6 of module 'esb_install' does not match runtime version 3.8	
Verifying Telehealth POD status in SE Node ()      [..................................................] 100%	
telehealth_remote_monitoring is already installed. Type YES to reinstall or NO to skip installation.	
*yes*	
Installing telehealth_remote_monitoring	
Installing Telehealth Dependencies on SE Node      [..................................................] 100%	
Enter correct IP address of SE Node [must be host system IP] (Example:: 123.123.123.123): < Host IP address > 		
Verifying SE Node < Host IP address > 	                 [..................................................] 100%	
	
	
Telehealth Remote Monitoring started. This might take upto 10 mins	
Installing Telehealth Remote Monitoring            [.........................                         ]  50%	
Successfully re-installed: Telehealth Remote Monitoring	
	
Verifying Telehealth POD status in SE Node < Host IP address > 	 [..................................................] 100%	
Successfully installed telehealth_remote_monitoring took 51.25 seconds	
Installation of package complete	
***Recommended to reboot system after installation***	
+--------------------------+------------------------------+---------+	
|            Id            |            Module            |  Status |	
+--------------------------+------------------------------+---------+	
| 611a2d4abd4d13002b6ba4f4 | telehealth remote monitoring | SUCCESS |	
+--------------------------+------------------------------+---------+	
```
## ITP/RI/Telehealth/08: Verify Telehealth RI installation with product key of another RI instance

###  Test Summary
  
Install the Telehealth RI and verify with product key of another RI

### Prerequisites
  
Bringup the RI as per [ITP/RI/Telehealth/01: Telehealth RI Installation and Verification](#itpritelehealth01-Telehealth-RI-Installation-and-Verification)
	
### Test Steps
	
Expected sample output:
	
```shell
To Be Updated - Pending JIRA closure
```	
	
## ITP/RI/Telehealth/09: Verify uninstall and install of Telehealth RI with an appropriate product key


###  Test Summary
  
uninstall the Telehealth RI and install again with correct product key and check the installation status

### Prerequisites
  
Bringup the RI as per [ITP/RI/Telehealth/01: Telehealth RI Installation and Verification](#itpritelehealth01-Telehealth-RI-Installation-and-Verification)

### Test Steps

Expected sample output

```shell
smartedge-open@ubuntu-0f98a4df7d:~/telehealth_remote_monitoring$ ./edgesoftware uninstall 611a2d4abd4d13002b6ba4f4
Components to be uninstalled are :['telehealth_remote_monitoring']
<frozen importlib._bootstrap>:219: RuntimeWarning: compiletime version 3.6 of module 'esb_install' does not match runtime version 3.8
Uninstalling telehealth_remote_monitoring
Uninstalling Telehealth Remote Monitoring. This might take upto 10 minutes.
Cleaning up Telehealth Remote Monitoring Application [                                                  ]   0%
Successfully removed Telehealth Remote Monitoring Application from machine
Cleaning up Telehealth Remote Monitoring Application [..................................................] 100%
Successfully uninstalled telehealth_remote_monitoring took 1.12 seconds
Uninstall Finished
+--------------------------+------------------------------+---------+
|            Id            |            Module            |  Status |
+--------------------------+------------------------------+---------+
| 611a2d4abd4d13002b6ba4f4 | telehealth remote monitoring | SUCCESS |
+--------------------------+------------------------------+---------+
```
```shell
smartedge-open@ubuntu-0f98a4df7d:~/telehealth_remote_monitoring$ ./edgesoftware install

Please enter the Product Key. The Product Key is contained in the email you received from Intel confirming your download:

Starting the setup...
ESB CLI version: 2021.3
Target OS: Ubuntu 20.04
Python version: 3.8.10
Checking Internet connection
Connected to the Internet
Validating product key
Successfully validated Product Key
Checking for prerequisites
Finished processing dependencies for esb-common==0.1	
Successfully installed shared module 'esb_common'.	
Unzipping the module telehealth_remote_monitoring...	
Modules to be installed by package are ['telehealth_remote_monitoring']	
<frozen importlib._bootstrap>:219: RuntimeWarning: compiletime version 3.6 of module 'esb_install' does not match runtime version 3.8	
Installing telehealth_remote_monitoring	< Host IP address> 
Installing Telehealth Dependencies on SE Node      [..................................................] 100%	
Enter correct IP address of SE Node [must be host system IP] (Example:: 123.123.123.123): 	
Verifying SE Node < Host IP address>                  [..................................................] 100%	
Telehealth Remote Monitoring started. This might take upto 10 mins	
Installing Telehealth Remote Monitoring            [.........................                         ]  50%	
Successfully installed: Telehealth Remote Monitoring Application	
Successfully installed telehealth_remote_monitoring took 2 minutes 59.22 seconds	
Installation of package complete	
***Recommended to reboot system after installation***	
+--------------------------+------------------------------+---------+	
|            Id            |            Module            |  Status |	
+--------------------------+------------------------------+---------+	
| 611a2d4abd4d13002b6ba4f4 | telehealth remote monitoring | SUCCESS |	
+--------------------------+------------------------------+---------+	
```
```shell
smartedge-open@ubuntu-0f98a4df7d:~/telehealth_remote_monitoring$ kubectl get pods -n smartedge-apps
NAME                          READY   STATUS    RESTARTS   AGE
telehealth-64bb799f96-nkpx8   2/2     Running   0          12m

smartedge-open@ubuntu-0f98a4df7d:~/telehealth_remote_monitoring$ kubectl get pods -A | grep telehealth
smartedge-apps           telehealth-64bb799f96-nkpx8                                2/2     Running   0          12m
```
	
## ITP/RI/Telehealth/10: Verify uninstall and install of Telehealth RI with an invalid product key
	
###  Test Summary
  
Uninstall the Telehealth RI application and try to install again with another RI product key instance

### Prerequisites
  
Bringup the RI as per [ITP/RI/Telehealth/01: Telehealth RI Installation and Verification](#itpritelehealth01-Telehealth-RI-Installation-and-Verification)

### Test Steps
	
Expected sample output:
	
```shell
To Be Updated - Pending JIRA closure
```	
	
	
	

