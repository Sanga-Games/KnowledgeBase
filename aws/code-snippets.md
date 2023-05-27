# Code Snippets

#### Upload Files to EC2 Instance

{% code overflow="wrap" %}
```sh
scp -i C:/AWS_Keypairs/Keypair.pem C:/GameBuilds/LinuxArm64Server.zip ec2-user@ec2-3-100-60-120.ap-south-1.compute.amazonaws.com:/home/ec2-user/
```
{% endcode %}



#### EC2 File Execution Permissions

{% code overflow="wrap" %}
```

sudo chown root:root /home
sudo chown 755 /home
sudo chown ec2-user:ec2-user /home/ec2-user -R
sudo chmod 700 /home/ec2-user /home/ec2-user/.ssh
sudo chmod 600 /home/ec2-user/.ssh/authorized_keys
sudo chmod u+x filepath
```
{% endcode %}



#### EC2 User script to Start game server on instance start

```
Content-Type: multipart/mixed; boundary="//"
MIME-Version: 1.0

--//
Content-Type: text/cloud-config; charset="us-ascii"
MIME-Version: 1.0
Content-Transfer-Encoding: 7bit
Content-Disposition: attachment; filename="cloud-config.txt"

#cloud-config
cloud_final_modules:
- [scripts-user, always]

--//
Content-Type: text/x-shellscript; charset="us-ascii"
MIME-Version: 1.0
Content-Transfer-Encoding: 7bit
Content-Disposition: attachment; filename="userdata.txt"

#!/bin/bash
cd /home/ec2-user/LinuxArm64Server
nohup sh ./LocomotionServer-Arm64.sh
--//
```





