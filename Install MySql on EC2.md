# Hosting MySQL on EC2

Create an EC2 instance of Ubuntu AMI. Add key-pair. Add ports "80 http" and "443 https". Let the Storage volume be default and Click on Launch Instance. 
It will take couple of minutes to instance get ready. You will see 2/2 checks passed below in Status Check. Now instance is ready to Connect

```bash
-> sudo apt-get update && sudo apt-get upgrade
```

Install MySQL 
```bash
-> sudo apt install mysql-server 
-> sudo systemctl status mysql
```

Login to MySQL as root
```bash
-> sudo mysql
```

Update the password for root user
```bash
-> ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'your-password';

-> FLUSH PRIVILEGES;
```

Verify the root user 
```bash
-> exit 
-> sudo -u root -p 
Enter the newly created password
```

### Create Database, Tables and start working with queries

Create Database company;
```bash
use company;
Create table Employee(id int, Name varchar(50), JOb_Role varchar(50));
Insert into Employee values (16, 'Swaraj', 'Developer'),(15, 'Vedant', 'Architect');
Select * From Employee;
```
