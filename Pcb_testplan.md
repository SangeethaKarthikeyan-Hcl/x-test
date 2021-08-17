- [ITP/RI/PCB/01: PCB Defect Detection RI Installation and Verification](#itpripcb01-PCB-Defect-Detection-RI-Installation-and-Verification)
  - [Test Summary](#test-summary)
  - [Prerequisites](#prerequisites)
  - [Hardware Requirements](#hardware-requirements)
  - [Test steps](#test-steps)

## ITP/RI/PCB/01: PCB Defect Detection RI Installation and Verification

### Test Summary

The PCB demo sample application when deployed on OpenNESS creates an impactful edge computing use case that utilizes the capability of OpenNESS

1. Verify PCB detection for deployment flavor minimal with on single node cluster (Controller & Edge Node in same host) using edge sofware

2. Verify all pods/services applicable (openness/esi pods)

### Prerequisites

- Fresh centOS installed Server (Controller and Node system)
- Sync the Date and time settings on the server.
- IP address conflict (Make sure that the Edge Controller IP is not conflicting with OpenNESS reserved IPs)
- Downloaded RI with Product Key 

### Hardware Requirements
**Controller and Node System**
- One of the following processors: 
  - Intel® Xeon® scalable processor. 
  - Intel® Xeon® processor D. 
- At least 64 GB RAM. 
- At least 256 GB hard drive. 
- CentOS* 7.9 
- An Internet connection.
- IP camera or pre-recorded video(s)

### Test Steps

**Note:** If you are behind a proxy network, please ensure that proxy addresses are configured in the system.
```shell
export http_proxy=http://proxy-mu.intel.com:911
export https_proxy=http://proxy-mu.intel.com:912
```

**Step 1: Install the Reference Implementation**

1. Install the python dependent libraries

```shell
Sudo yum install python-devel
Sudo yum install python3-devel
```
2. Download the Reference Implementation from the following link with required specifications:
   https://software.intel.com/iot/edgesoftwarehub/download/home/ri/wireless-network-ready-pcb-defect-detection
   
  **Note:** For single-device mode, only one machine is needed. (Both Controller and edge node will be on same device.)

3. Open a new terminal as a root user: 
```shell
sudo passwd
su
```
4. Move the downloaded zip package to the /root folder: 

```shell
mv <path-of-downloaded-directory>/wireless-network-ready-pcb-defect-detection.zip /root 
```
5. Go to the /root directory using the following command and unzip the RI: 

```shell
cd /root 
unzip wireless-network-ready-pcb-defect-detection.zip 
```
6. Go to wireless-network-ready-pcb-defect-detection/ directory:

```shell
cd wireless-network-ready-pcb-defect-detection
```
7. Change permission of the executable edgesoftware file: 

```shell
chmod 755 edgesoftware 
```
8. Run the command below to install the Reference Implementation:
```shell
./edgesoftware install 
```
9. During the installation, you will be prompted for the Product Key. 
 the Product Key is contained in the email you received from Intel confirming your download. 
 
 ![image](https://user-images.githubusercontent.com/66106244/124550735-bd51c480-de4e-11eb-804f-629b831d6edd.png)
 
 10. During the installation, you will be prompted to configure few things before installing OpenNESS. Refer the below output 
 ```shell
 Installing OpenNESS
 Target system(s) may get rebooted. Restart setup process after reboot.
 If you have any unsaved documents/work, then press CTRL+C. Save your documents/work.
 installation will continue in (seconds) : 01
 Select the type of installation:
 1. Single Device
 2. Multi Device
 Enter your choice [1/2]: 1
 Enter correct IP address of (Single Device) Controller target[must be host system IP] (Example: 123.123.123.123): ****************
 Verifying Controller (10.190.204.164)              [.........................                         ]  50%  00:0
 verifying Controller (10.190.204.164)              [.................................                 ]  66%  00:0
 Do you wish to configure NTP server? [yes/no]: no
 ```
 11. When the installation is complete, you see the message Installation of package complete and the installation status for each module.
 
 ```shell
 Enter proxy for yum with port number [Eg: http://example.org:1000 ]: , Press enter to leave blank: http://proxy-mu.intel.com:911
 You have entered http://proxy-mu.intel.com:911 as the proxy for yum.
 Confirm y/n: y
 http://proxy-mu.intel.com:911
 Configuring Proxy Values in localhost.yaml file    [..................................................] 100%

 Updating proxy values                             [..................................................] 100%
 Configuring localhost.yaml file                    [..................................................] 100%

 Installing PCB Dependencies . . .                 [..................................................] 100%
 PCB Defect Detection installation started. This might take upto 45 mins
 Installing PCB Defect Detection                    [.................................                 ]  66%  00:13:05
 Successfully installed: EIS PCB Defect Detection Application
 Installing PCB Defect Detection                    [..................................................] 100%
 cleanup
 Verifying EIS PCB installation in (10.190.213.102) [..................................................] 100%
 Successfully installed Wireless_NetworkReady_PCB_defect_detection took 27 minutes 34.45 seconds
 Installation of package complete
***Recommended to reboot system after installation***
+--------------------------+--------------------------------------------+---------+
|            Id            |                   Module                   |  Status |
+--------------------------+--------------------------------------------+---------+
| 60377554a2fba5002cc8e0cc |                  OpenNESS                  | SUCCESS |
| 603e38647ef9c6002aff06e5 | Wireless NetworkReady PCB defect detection | SUCCESS |
+--------------------------+--------------------------------------------+---------+
```
12. If OpenNESS is installed, running the following command should show output similar to the below. All the pods should be either in the running or completed stage. 
    
```shell
[root@esi47 ~]# kubectl get pod -A
NAMESPACE              NAME                                                         READY   STATUS             RESTARTS   AGE
eis                    ia-camera-stream-66b6d44455-nrjx5                            1/1     Running            0          10h
eis                    ia-etcd-977747d59-xkgpr                                      1/1     Running            0          10h
eis                    ia-video-analytics-54ddf5f6b4-n74fq                          1/1     Running            3          10h
eis                    ia-video-ingestion-7b7d4bb6cf-2g5kv                          1/1     Running            0          10h
eis                    ia-visualizer-77f6fb5b8-lk7gq                                0/1     CrashLoopBackOff   132        10h
eis                    ia-web-visualizer-75df65887d-pgg6r                           1/1     Running            0          10h
harbor                 harbor-app-harbor-chartmuseum-5f865f4bf4-t7jjv               1/1     Running            1          12h
harbor                 harbor-app-harbor-clair-779df4555b-8nffw                     2/2     Running            3          12h
harbor                 harbor-app-harbor-core-fdb4d44f8-wkzx7                       1/1     Running            2          12h
harbor                 harbor-app-harbor-database-0                                 1/1     Running            1          12h
harbor                 harbor-app-harbor-jobservice-67df65fdb8-v9z27                1/1     Running            1          12h
harbor                 harbor-app-harbor-nginx-6fbd86c598-48dqh                     1/1     Running            1          12h
harbor                 harbor-app-harbor-notary-server-7cbdd5796b-pscwf             1/1     Running            2          12h
harbor                 harbor-app-harbor-notary-signer-b4f58c858-wfghh              1/1     Running            2          12h
harbor                 harbor-app-harbor-portal-fd5ff4bc9-rbg5s                     1/1     Running            1          12h
harbor                 harbor-app-harbor-redis-0                                    1/1     Running            1          12h
harbor                 harbor-app-harbor-registry-6f7d795ff8-h8mxk                  2/2     Running            2          12h
harbor                 harbor-app-harbor-trivy-0                                    1/1     Running            3          12h
kafka                  cluster-entity-operator-55894648cb-ltkw7                     3/3     Running            5          12h
kafka                  cluster-kafka-0                                              2/2     Running            3          12h
kafka                  cluster-zookeeper-0                                          1/1     Running            1          12h
kafka                  strimzi-cluster-operator-68b6d59f74-nlznm                    1/1     Running            1          12h
kube-system            coredns-f9fd979d6-bqdvk                                      1/1     Running            1          12h
kube-system            coredns-f9fd979d6-v9tvx                                      1/1     Running            1          12h
kube-system            descheduler-cronjob-1623908040-9fqx7                         0/1     Completed          0          5m
kube-system            descheduler-cronjob-1623908160-zh4wl                         0/1     Completed          0          3m
kube-system            descheduler-cronjob-1623908280-pcl9f                         0/1     Completed          0          59s
kube-system            etcd-esi33                                                   1/1     Running            1          12h
kube-system            kube-apiserver-esi33                                         1/1     Running            1          12h
kube-system            kube-controller-manager-esi33                                1/1     Running            2          12h
kube-system            kube-ovn-cni-855hd                                           1/1     Running            1          12h
kube-system            kube-ovn-controller-76d6bd7c8d-lsvrn                         1/1     Running            2          12h
kube-system            kube-ovn-pinger-np6ls                                        1/1     Running            1          12h
kube-system            kube-proxy-96ljb                                             1/1     Running            1          12h
kube-system            kube-scheduler-esi33                                         1/1     Running            4          12h
kube-system            ovn-central-6475fbf654-hd6tb                                 1/1     Running            1          12h
kube-system            ovs-ovn-wwczm                                                1/1     Running            1          12h
kubernetes-dashboard   dashboard-metrics-scraper-7b9b99d599-b747s                   1/1     Running            1          12h
kubernetes-dashboard   kubernetes-dashboard-6d4799d74-b5cq5                         1/1     Running            1          12h
openness               certsigner-6cb79468b5-df7v5                                  1/1     Running            1          12h
openness               eaa-69c7bb7b5d-26726                                         1/1     Running            1          12h
openness               edgedns-l68x4                                                1/1     Running            1          12h
openness               interfaceservice-rlp66                                       1/1     Running            1          12h
openness               nfd-release-node-feature-discovery-master-6d74f4b57b-7cfqz   1/1     Running            1          12h
openness               nfd-release-node-feature-discovery-worker-gk7tt              1/1     Running            2          12h
telemetry              collectd-cbz8t                                               2/2     Running            2          12h
telemetry              custom-metrics-apiserver-55bdf684ff-4t57n                    1/1     Running            1          12h
telemetry              grafana-6c4c5555f6-b2kh4                                     2/2     Running            2          12h
telemetry              otel-collector-f9b9d494-t8f2p                                2/2     Running            2          12h
telemetry              prometheus-node-exporter-tmcl6                               1/1     Running            3          12h
telemetry              prometheus-server-8656f6bf98-dmkxg                           3/3     Running            3          12h
telemetry              telemetry-aware-scheduling-6b97598f66-d2tzr                  2/2     Running            2          12h
telemetry              telemetry-collector-certs-rnsz9                              0/1     Completed          0          12h
telemetry              telemetry-node-certs-lrkth                                   1/1     Running            1          12h
```
**Note:** The status of the Visualizer pod might be CrashLoopBackOff. This is an expected behavior and can be ignored. 

**Step 2 : Visualizing Results**

1. To visualize the results, open a browser and navigate to https://:5050
  to access the visualizer, log in with the username = admin and password = admin@123
  
  ![image](https://user-images.githubusercontent.com/66106244/124591220-affdff80-de79-11eb-9599-9f15889fb367.png)

2. To View the Harbor User Interface (optional)
   To access the Harbor user interface, open a browser and navigate to: https://:30003
   Log in with the username = admin and password = Harbor12345
   
   ![image](https://user-images.githubusercontent.com/66106244/124553797-cf356680-de52-11eb-9a72-19728c7f3536.png)
