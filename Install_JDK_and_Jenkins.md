# Installing JAVA JDK and Jenkins in EC2 Instance

Create an EC2 instance of Ubuntu AMI. Add key-pair. Add ports "80 http" and "443 https". Let the Storage volume be default and Click on Launch Instance.
It will take couple of minutes to instance get ready. You will see 2/2 checks passed below in Status Check. Now instance is ready to Connect

## Commands to Install JDK
```bash
wget https://download.oracle.com/java/21/latest/jdk-21_linux-x64_bin.deb
ls
```

Copy the file name
```bash
sudo dpkg -i "file name"
java --version
```
## Commands to Install Jenkins
```bash
sudo wget -O /usr/share/keyrings/jenkins-keyring.asc \
    https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
 
  echo "deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc]" \
    https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
    /etc/apt/sources.list.d/jenkins.list > /dev/null
  
sudo apt-get install fontconfig openjdk-21-jre
sudo apt-get install Jenkins
sudo systemctl start Jenkins
sudo systemctl enable Jenkins
sudo systemctl status Jenkins
cat /var/lib/jenkins/secrets/initialAdminPassword
```
You will get an password enter in Jenkins official page
copy the public ip address of instance and paste in browser along with default Jenkins port number
13.201.30.33:8080

In Instance -> Go to security -> Edit the inbound rules and add port number 8080 and save.
