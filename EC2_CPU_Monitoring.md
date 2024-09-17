### Monitor CPU Utilization of EC2 instance using CloudWatch and Create an Alarm

1. Create Linux EC2 instance

2. Create and Metric for CPU Utilization of current EC2 instance

3. Create an Alarm. Set threshold limit as needed.

4. To increase the stress level, use following commands
```bash
sudo yum update && sudo yum upgrade
sudo yum install stress
sudo -c 1 -t 3600
```

"-c" - It is number of CPUs and "-t" - It is timeout in seconds
