## Setting Up the Netflix Clone Application

Follow these steps to set up and run the Netflix clone application on your local machine:

### 1. Update Package List

Run the following commands to update the package list on your system:

```bash
sudo apt update
```

### 2. Install Required Packages

Install the necessary packages and dependencies for the application using the following command:

```bash
sudo apt install -y curl dirmngr apt-transport-https lsb-release ca-certificates
```

### 3. Install Node.js

Install Node.js, a JavaScript runtime, with the following commands:

```bash
curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash -
sudo apt install -y nodejs
```

### 4. Install Project Dependencies

Navigate to the project directory and install the project-specific dependencies:

```bash
npm install
```

### 5. Start the Application: Works

To start the Netflix clone application, use the following command:

```bash
npm start &
```

### 6. Optional & May Not Work: Serve the Built Application

If you want to serve the built version of the application, you can use the `serve` package. First, install it globally:

```bash
npm install -g serve
```

Then, serve the built application on port 4000:

```bash
serve -s build -l 4000
```

Now you should be able to access the Netflix clone application by opening your web browser and navigating to `http://localhost:4000`.

Enjoy exploring the Netflix clone!


### If you get any error while building code, execute below command

```bash
export NODE_OPTIONS=--openssl-legacy-provider
npm run build
```
### 7. Deploy application in apache2
```bash
sudo apt install apache2
```
```bash
ubuntu@ip-172-31-30-177:/var/www/html$ ll ~/buildtasks/js_code/build
total 100
drwxrwxr-x 5 ubuntu ubuntu  4096 Apr 15 09:21 ./
drwxrwxr-x 7 ubuntu ubuntu  4096 Apr 15 09:18 ../
-rw-rw-r-- 1 ubuntu ubuntu  1808 Apr 15 09:21 404.html
-rw-rw-r-- 1 ubuntu ubuntu  1139 Apr 15 09:21 asset-manifest.json
-rw-rw-r-- 1 ubuntu ubuntu 40743 Apr 15 09:21 favicon.ico
drwxrwxr-x 7 ubuntu ubuntu  4096 Apr 15 09:21 images/
-rw-rw-r-- 1 ubuntu ubuntu  2062 Apr 15 09:21 index.html
-rw-rw-r-- 1 ubuntu ubuntu  5347 Apr 15 09:21 logo192.png
-rw-rw-r-- 1 ubuntu ubuntu  9664 Apr 15 09:21 logo512.png
-rw-rw-r-- 1 ubuntu ubuntu   492 Apr 15 09:21 manifest.json
-rw-rw-r-- 1 ubuntu ubuntu    67 Apr 15 09:21 robots.txt
drwxrwxr-x 5 ubuntu ubuntu  4096 Apr 15 09:21 static/
drwxrwxr-x 2 ubuntu ubuntu  4096 Apr 15 09:21 videos/
```

### Copy all files to /var/www/html and restart apache2 service

```bash
cd /var/www/html/
rm -rf index.html
sudo rm -rf index.html
sudo cp -r ~/buildtasks/js_code/build/* .
sudo systemctl restart apcahe2
```

```bash
ubuntu@ip-172-31-30-177:/var/www/html$ netstat -ntpl
(No info could be read for "-p": geteuid()=1000 but you should be root.)
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name
tcp        0      0 127.0.0.54:53           0.0.0.0:*               LISTEN      -
tcp        0      0 127.0.0.53:53           0.0.0.0:*               LISTEN      -
tcp6       0      0 :::22                   :::*                    LISTEN      -
tcp6       0      0 :::80                   :::*                    LISTEN      -
ubuntu@ip-172-31-30-177:/var/www/html$ curl ifconfig.me
13.218.244.174
```


### access the application

http://<public ip>:80

http://13.218.244.174:80

![Image](https://github.com/user-attachments/assets/51fc9ede-de8e-4d56-a6db-c2d7b929a26b)



### commands used
```bash
  1  nodejs
  
    2  sudo apt install nodejs
    3  apt update
    4  sudo apt update
    5  sudo apt install nodejs
    6  npm
    7  sudo apt install npm
    8  ls
    9  git clone https://github.com/saikrishna8763/buildtasks.git
   10  cd buildtasks/
   11  ls
   12  cd js_code/
   13  ls
   14  npm install
   15  npm audit fix
   16  npm run build
   17  export NODE_OPTIONS=--openssl-legacy-provider
   18  npm run build
   19  ls
   20  sudo apt install apache2
   21  ll
   22  cd build/
   23  ll
   24  cd /var/www/html/
   25  ll
   26  rm -rf index.html
   27  sudo rm -rf index.html
   28  sudo cp -r ~/buildtasks/js_code/build/* .
   29  systemctl restart apcahe2
   30  sudo systemctl restart apache2
   31  netstat -ntpl
   32  sudo apt install net-tools
   33  netstat -ntpl
   34  curl ifconfig.me
```
