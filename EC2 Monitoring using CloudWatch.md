# Monitor Memory Utilization Of EC2 using CloudWatch

## 1. Create a IAM Role for EC2 with CloudWatch Full Access and SSM(AWS System Manager) Full Access

### Click on IAM Role -> Create Role -> Select entrusted type as AWS Service.

![Screenshot 2024-08-22 151450](https://github.com/user-attachments/assets/b81c1c42-1a8a-4f94-a9ea-25263cf5cc4e)

### Select EC2 -> Click Next

![Screenshot 2024-08-22 151611](https://github.com/user-attachments/assets/21eada9e-ad12-4a42-bfd8-19047957bb58)

### Add Permission "CloudWatchFullAccess" -> Next

![Screenshot 2024-08-22 151651](https://github.com/user-attachments/assets/44c36afe-4c0d-4d30-9145-60ecf24f47d3)

### Give a understandable name to Role -> Next

![Screenshot 2024-08-22 151930](https://github.com/user-attachments/assets/672170cb-b75e-4543-922e-730202a9cdec)

### Click Create Role

![Screenshot 2024-08-22 151954](https://github.com/user-attachments/assets/94f40747-f853-4567-9240-793c7e725e41)

### Go to Permission -> RHS Add Permission -> Attach Policy

![Screenshot (120)](https://github.com/user-attachments/assets/8c1f8de0-8895-495d-b183-90f617367824)

### Select "AmazonSSMFullAcess" -> Add Permission

![Screenshot 2024-08-22 152209](https://github.com/user-attachments/assets/3f46c011-ffa1-402b-881a-532630b9d5fc)

### Now the IAM Role for EC2 instance with full access of CLoudWatch and SSM is created

## 2. Create an SSM(AWS System Manager) Parameter

### Go to SSM -> Select Parameter Store -> Create Parameter -> Enter Name -> Let everything as default

![Screenshot (121)](https://github.com/user-attachments/assets/0035a889-7970-4fe5-8eab-80d74a4cd131)

### In Value add this data -> Click Create Parameter
### Explanation : CloudWatch Metric will monitor the EC2 by using instance id which is unique for each instance and collected metric will be measured in percent at mentioned time interval in seconds.
```bash
{
	"metrics": {
		"append_dimensions": {
			"InstanceId": "${aws:InstanceId}"
		},
		"metrics_collected": {
			"mem": {
				"measurement": [
					"mem_used_percent"
				],
				"metrics_collection_interval": 60
			}
		}
	}
}
```

![Screenshot (122)](https://github.com/user-attachments/assets/4ae15ce4-698b-4384-b167-729715fb1030)

## 3. Create an EC2 Instance and Attach the role 

### Create an EC2 instance of Linux AMI. Add key-pair. Add ports "80 http" and "443 https". Let the Storage volume be default. In Advanced details select IAM role and in Userdata enter below mentioned data and Click on Launch Instance. It will take couple of minutes to instance get ready. You will see 2/2 checks passed below in Status Check. Now instance is ready to Connect.

### Explanation : It is the link that points to a specific Amazon S3 bucket location where the latest version of the Amazon CloudWatch Agent for Linux is stored. As it is a zip file unzip it and install the file present inside it and then fetch the CloudWatch Agent created in SSM.
```bash
#!/bin/bash
wget https://s3.amazonaws.com/amazoncloudwatch-agent/linux/amd64/latest/AmazonCloudWatchAgent.zip
unzip AmazonCloudWatchAgent.zip
sudo ./install.sh
sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl -a fetch-config -m ec2 -c ssm:CWAgent -s
```


![Screenshot 2024-08-22 153011](https://github.com/user-attachments/assets/68f8fea4-b330-4ca1-b811-aea9d62c92fc)

### Verify whether CW Agent is installed or not
```bash
sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl -m ec2 -a status
```

![Screenshot 2024-08-22 155838](https://github.com/user-attachments/assets/2353f0bd-c84b-4667-a59a-2e132bc01e53)

## 4. Create an Metric and Alarm 

### Go the CW Metric -> Click CW Agent 

![Screenshot 2024-08-22 234238](https://github.com/user-attachments/assets/ddb3731e-1457-4678-9c96-0379e2dc6707)

### Search by Instance_ID -> Select and Click on Create Alarm

![Screenshot 2024-08-22 234355](https://github.com/user-attachments/assets/849be72a-4240-49af-a904-705eb9a45bd5)

### Create Alarm and enter minimum threshold value and Click on Next

![Screenshot 2024-08-22 155937](https://github.com/user-attachments/assets/ac043249-2d72-40ad-b45f-3a9f393e5d74)

![Screenshot 2024-08-22 155959](https://github.com/user-attachments/assets/3adcece1-f92c-471e-91b3-e964d304bba6)

### When the Limit will get exceeded the alarm will be raised and then you can take actions.







