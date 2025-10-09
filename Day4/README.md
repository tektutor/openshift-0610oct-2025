# Day 4

## Info - Persistent Volume(PV) Overview
<pre>
- any application that needs to store application logs, data, etc., must be used an external storage
- though Pod supports storing data inside Pod storage, as Pods are considered temporary resources, storing data inside Pod storage is considered a bad practice
- Persistent Volume can be provisioned one of the below ways
  1. NFS Shared Path
  2. AWS S3 buck
  3. AWS EBS
  4. Any other Custom Storage Solution
- It can be provisioned manually or dynamically
- System Administrators can manually provision Disks of different size and access for variaour teams as per their project requirement
- Alternatelly, Openshift Administrators can create a StorageClass for NFS/AWS S3 Bucket, etc. which will provision the required Persistent Volume dynamically on demand
- When application teams create a Persistent Volume Claim, in case your openshift cluster supports a corresponding storage class it will automatically provision such a disk (PV) and let the application claim and use it
</pre>

## Info - Persistent Volume Claim(PVC) Overview
<pre>
- Any application that needs external storage, must request for such external storage by creating a Persistent Volume Claim
- Typically, PVC will express the below
  1. Size of the disk
  2. Access Mode
     - 
  3. StorageClass(Optional)
  4. Label Selector (Optional)
- When an application requests for PV by way of PVC, the storage controller in Openshift will scan the openshift cluster looking for a matching PV, if it finds a matching PV, it will let your application claim and use it
- In case there is no macthin PV for your PVC, then your respestive Pod will be in Pending until it gets a matching PV.
</pre>

## Lab - Deploying multipod Wordpress and Mysql database that uses external storage
In case you haven't clone this training, you can do now
```
cd ~
git clone https://github.com/tektutor/openshift-0610oct-2025.git
```

After cloning the repository
```
cd ~/openshift-0610oct-2025
git pull
cd Day4/wordpress
ls -l
# Make sure you have updated the mysql-pv.yml, mysql-pvc.yml, mysql-deploy.yml
# Make sure you have updated the wordpress-pv.yml, wordpress-pvc.yml and wordpress-deploy.yml
./deploy.sh
```
<img width="1920" height="1168" alt="image" src="https://github.com/user-attachments/assets/ffebe2a9-0837-43f9-aeee-a494ed1eb09d" />
<img width="1920" height="1168" alt="image" src="https://github.com/user-attachments/assets/9cac90a9-c6ba-4be0-9714-0ed596735819" />
<img width="1920" height="1168" alt="image" src="https://github.com/user-attachments/assets/844336a1-9235-49ed-84f8-52fdb5704455" />
