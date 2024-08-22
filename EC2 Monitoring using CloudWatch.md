# Monitor Memory Utilization Of EC2 using CloudWatch

## 1. Create a IAM Role for EC2 with CloudWatch Full Access and SSM(AWS System Manager) Full Access

Click on IAM Role -> Create Role -> Select entrusted type as AWS Service.

![Screenshot 2024-08-22 151450](https://github.com/user-attachments/assets/b81c1c42-1a8a-4f94-a9ea-25263cf5cc4e)

Select EC2 -> Click Next

![Screenshot 2024-08-22 151611](https://github.com/user-attachments/assets/21eada9e-ad12-4a42-bfd8-19047957bb58)

Add Permission "CloudWatchFullAccess" -> Next

![Screenshot 2024-08-22 151651](https://github.com/user-attachments/assets/44c36afe-4c0d-4d30-9145-60ecf24f47d3)

Give a understandable name to Role -> Next

![Screenshot 2024-08-22 151930](https://github.com/user-attachments/assets/672170cb-b75e-4543-922e-730202a9cdec)

Click Create Role

![Screenshot 2024-08-22 151954](https://github.com/user-attachments/assets/94f40747-f853-4567-9240-793c7e725e41)

Go to Permission -> RHS Add Permission -> Attach Policy

![Screenshot (120)](https://github.com/user-attachments/assets/8c1f8de0-8895-495d-b183-90f617367824)

Select "AmazonSSMFullAcess" -> Add Permission

![Screenshot 2024-08-22 152209](https://github.com/user-attachments/assets/3f46c011-ffa1-402b-881a-532630b9d5fc)

Now the IAM Role for EC2 instance with full access of CLoudWatch and SSM is created

## Create an SSM(AWS System Manager) Parameter

Go to SSM -> Select Parameter Store -> Create Parameter -> Enter Name -> Let everything as default

![Screenshot (121)](https://github.com/user-attachments/assets/0035a889-7970-4fe5-8eab-80d74a4cd131)

In Value add this data -> CLick Create Parameter
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
Explanation : CloudWatch Metric will monitor the EC2 by using instance id which is unique for each instance and collected metric will be measured in percent at mentioned time interval in seconds.

![Screenshot (122)](https://github.com/user-attachments/assets/4ae15ce4-698b-4384-b167-729715fb1030)





