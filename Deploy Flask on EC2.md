# Deploy Flask App on EC2 Instance

Create an EC2 instance of Ubuntu AMI. Add key-pair. Add ports "80 http" and "443 https". Let the Storage volume be default and Click on Launch Instance.
It will take couple of minutes to instance get ready. You will see 2/2 checks passed below in Status Check. Now instance is ready to Connect

## Commands to deploy flask

```bash
-> sudo apt-get update
```

Install new python virtual env
```bash
-> sudo apt-get install python3-venv
```

Activate the new virtual env in new directory
```bash
-> mkdir College
-> cd College
```

Create new virtual env
```bash
-> python3 -m venv venv
```

Activate virtual env
```bash
-> source venv/bin/activate
```

Install flask
```bash
-> pip install flask
```

Create a simple file and Add this code in file
```bash
-> sudo vi firstProg.py

Add from below line
from flask import Flask

app = Flask(__name__)

@app.route('/')
def hello_world():
	return 'Hello World!'

if __name__ == "__main__":
	app.run()
```
Verify it
```bash
-> python fisrtProg.py
```

Install Gunicorn
```bash
-> pip install gunicorn
```

### NOTE : While using below command make sure to use your own "filename:app"
Run Gunicorn
```bash
-> gunicorn -b 0.0.0.0:8000 firstProg:app
```

### NOTE : While using below command make sure to use your own "directoryname.service"
Open file 
-> sudo vi /etc/system/system/College.service
```
Add this content and make the changes in the path as per your filename and directory name.
```bash
```bash
[Unit]
Description=Gunicorn instance for a simple hello world app
After=network.target
[Service]
User=ubuntu
Group=www-data
WorkingDirectory=/home/ubuntu/College
ExecStart=/home/ubuntu/College/venv/bin/gunicorn -b localhost:8000 firstProg:app
Restart=always
[Install]
WantedBy=multi-user.target
```
Save and exit
Start service
```bash
Start service
-> sudo systemctl daemon-reload
-> sudo systemctl start College
-> sudo systemctl enable College
```
Install nginx
```bash
-> sudo apt-get install nginx
-> sudo systemctl start nginx
-> sudo systemctl enable nginx
```

Edit the default nginx file 
```bash
-> sudo vi /etc//nginx/sites-available/default
```

Make the changes at the start of the code above the server
```bash
-> upstream flaskhelloworld {
	server 127.0.0.1:8000;
}
```
![y](https://github.com/user-attachments/assets/18616c2d-9e2b-4065-b516-a90a90b678da.jpg)


And a single line at procy-pass to flaskhelloworld location
```bash
-> proxy_pass http://flaskhelloworld;
```
![z](https://github.com/user-attachments/assets/2a3e89aa-2566-4a1c-a8ce-09f184e45f2a.jpg)


Restart nginx
```bash
-> sudo systemctl restart nginx
```

Open any browser and paste your public ip address of EC2 instance and hit enter. You will able to see the output.

