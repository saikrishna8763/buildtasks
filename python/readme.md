# real-time Python web application.

# Running this project
# To get this project up and running you should start by having Python installed on your computer. It's advised you create a virtual environment to store your projects dependencies separately. You can install virtualenv with python E-commerce

## This is a very simple e-commerce website built with python.

### Project Summaryhe website displays products. Users can add and remove products to/from their cart while also specifying the quantity of each item. They can then enter their address and choose Stripe to handle the payment processing.


## A detailed guide for creating a real-time Python web application using Flask,and deploying it on an AWS EC2 instance launched from a custom Amazon Machine Image (AMI).

### Project conspectus: Real-Time Python Web Application Using AWS AMI

we will set up a simple Flask application that displays the current server time.

We will create a custom AMI after the setup so that the application can be easily replicated

**we have to do sum Creaction and Configure an EC2 Instance**

2. **Install Required Packages**
3. **Develop the Flask Application**
4. **Run the Application**
5. **Create a Custom AMI**
6. **Cleanup**

## Step-by-Step Task Instructions.

### now we have Configure an EC2 Instance.

**Log in to the AWS Management Console** and navigate to the **EC2** dashboard.
**Launch an Instance:**  - Click on "Launch Instance." - Choose the **Amazon Linux 2 AMI**. - Select an instance type `t2.micro` for the free tier.
![](pyimages/pyt-1.png)
- Click "Next: Configure Instance Details."    **Configure Instance Details:**   - Leave default settings or customize as needed.   - Click "Next: Add Storage."
**Configure Storage:**   - Default settings are usually fine (8 GiB).   - Click "Next: Add Tags."
![](pyimages/pyt-1.2.png)
**Add Tags (Optional):**
- Add any tags for organization, Key: `pyt`, Value: `pytInstance`.   - Click "Next: Configure Security Group."   **Configure Security Group:**  - Create a new security group.   - Add the following rules: 
![](pyimages/pyt-1.1.png)

http-80 (web)  
ssh-22  (security),
Custom TCP-5000 (app)
![](images/pyt-1.2.png)
Click "Review and Launch."

Review your configurations and click "Launch." - Choose or create a key pair for **SSH access**, and download it. ![](pyimages/3.png)

**Connect via SSH:** ssh -i your-key.pem ec2-user@your-public-insteance

Step 3: Install Required Packages

1. **Update the Package Repository:**   sudo yum update -y
   
2. **Install Python 3 and pip:**
  sudo yum install -y python3

3. **Install Flask:**
pip3 install Flask
![](pyimages/pyt-3-cmd.png)

Develop the Flask Application

1. **Create a New Directory for YourApplication:** mkdir flask-app  cd flask-app   2. **Create the Flask App:**   - Create a file named `app.py`: (exp) `vi app.py` (or) `vim app.py`(or) `nano app.py`
![](pyimages/pyt-4-cmd.png)

## Run the Application 

1. **Start the Flask Application:**  `python3 app.py`
**Access Your Application:** - Open a web browser and navigate to `http://your-instance-public-dns:5000`.
- You should see the current server time displayed.
![](pyimages/pyt-13-amicmd2.png)

# Create a Custom AMI

**Stop the EC2 Instance:** - Go back to the EC2 console and select your instance.
- Click on "Instance State" and then *Stop*
![](pyimages/pyt-6-stp.png)

**Create an AMI:**
- Right-click on your stopped instance, select "Image," and then "Create Image."
- Name it ( `FlaskAppAMI`), and fill in any details.
- Click "Create Image."
![](pyimages/pyt-7-ami.png)
![](pyimages/pyt-ami-crt-img.png)

**Launch New Instances from Your AMI:**
You can now launch new EC2 instances using this custom AMI, which includes your pre-configured Flask application ![](pyimages/pyt-9-amiint.png)          ![](pyimages/pyt-10-amiint2.png)
![](pyimages/pyt-11-amiint3.png)
launch an ami insteance

Review your configurations and click "Launch." - Choose or create a key pair for **SSH access** of ami insteance.
![](pyimages/pyt-12-amissh.png)

next do this steps
- ls
- cd flask.app
- python3 app.py
![](pyimages/pyt-13-amicmd2.png)

copy ami public ip adderess ec2-user@your-public-insteance & past in url (exp) 23.22.29.136:5000
![](pyimages/py-output2.png)
![](pyimages/py-output.png)

* Create and edit the systemd service file for your Flask app
sudo nano /etc/systemd/system/flask-app.service

* Reload systemd to recognize the new service file
sudo systemctl daemon-reload

* Enable the service (set to start automatically at boot)
sudo systemctl enable flask-app

* Start the service
sudo systemctl start flask-app

* Check the status of your service
sudo systemctl status flask-app

* Follow the logs for the Flask app service
sudo journalctl -u flask-app -f

* Allow incoming traffic on port 8000 (if using ufw or another firewall)
sudo ufw allow 8000
* Install pip and Create a Virtual Environment
Install pip for Python 3: sudo apt install python3-pip -y

Verify pip Installation:
pip3 --version

Create a Virtual Environment:
python3 -m venv venv

Activate the Virtual Environment:
source venv/bin/activate

* Setting Up Your Flask Application
Navigate to Your Flask Application Directory: cd ~/Flask-App

(Optional) Re-Create the Virtual Environment in Your Project Folder: python3 -m venv venv source venv/bin/activate

Install Flask: Inside your virtual environment, install Flask: pip3 install flask

Run the Application in Development Mode:

Start your Flask app using: python3 app.py

* Testing with Gunicorn
pip install gunicorn

Run Gunicorn to Serve the Application: Test Gunicorn by running:

gunicorn --bind 0.0.0.0:8000 app:app

* Creating the systemd Service File
[Unit] Description=Gunicorn instance to serve Flask-App After=network.target

[Service] User=ubuntu Group=ubuntu WorkingDirectory=/home/ubuntu/Flask-App ExecStart=/home/ubuntu/Flask-App/venv/bin/gunicorn --workers 3 --bind 0.0.0.0:8000 app:app


**Terminate the EC2 Instance:**  After verifying everything, make sure to terminate your instance to avoid charges.

Conclusion

**You've successfully created a real-time Python web application using Flask on an EC2 instance,and you learned how to create a custom AMI to replicate this setup easily.**