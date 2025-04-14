
# SonarQube Testing of Java, JS, and Python Code (Two-Server Setup)

## Goal  
Set up **two separate servers**:  
-  **Build Server**: for compiling code and running SonarQube analysis  
-  **SonarQube Server**: for hosting SonarQube and viewing results  

Test static analysis using SonarQube for Java, JavaScript, and Python projects.

---

##  1. SonarQube Server Setup

### OS Info  
```bash
uname -a
# Linux ip-172-31-20-239 ... x86_64 GNU/Linux
```

###  Install Prerequisites  
```bash
sudo apt update -y
sudo apt install -y unzip
sudo apt install openjdk-17-jre-headless -y
```

###  Download and Extract SonarQube  
```bash
wget https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-25.4.0.105899.zip
unzip sonarqube-25.4.0.105899.zip
```

###  Start SonarQube  
```bash
cd sonarqube-25.4.0.105899/bin/linux-x86-64
./sonar.sh start
./sonar.sh status
```

 Expected Output:
```
Starting SonarQube...
Started SonarQube.
SonarQube is running (PID)
```

###  Open SonarQube in Browser  
```bash
curl ifconfig.me
# Output: 18.232.140.167
```

Access SonarQube at:  
**http://44.202.193.53:9000**  
Login: `admin / admin`
Reset admin Password

---

##  2. Configure SonarQube Projects (Web UI)

For each language (Java / JS / Python):

Go to **Projects** => **Create Project** 


choose **Local project** 



give **Required details** 



Select **Use global settings**, then choose **Locally**

Choose **Locally** 
 Enter:
   - Project key (e.g., `java`, `js`, or `python`)
   - Project name


Generate a token:
   - Click **Generate** => **Continue**



In 2. choose **the package/code** and **copy the code**


---

##  3. Build Server Setup

###  Install General Prerequisites  
```bash
apt update -y
apt install git -y
apt install openjdk-17-jre-headless -y
apt install maven -y
apt install npm -y
```

---

##  Java Project Analysis (Maven)

###  Clone and Build Java Project  
```bash
git clone https://github.com/<your-java-repo>.git
cd Java-Blog
mvn package
```
```bash
root@ip-172-31-26-213:~/Documentation/java_code# ll
total 64
drwxr-xr-x 4 root root  4096 Apr 13 09:33 ./
drwxr-xr-x 6 root root  4096 Apr 13 09:33 ../
-rw-r--r-- 1 root root 15759 Apr 13 09:33 MySQLDesign.mwb
-rw-r--r-- 1 root root 15550 Apr 13 09:33 MySQLDesign.mwb.bak
-rw-r--r-- 1 root root  4494 Apr 13 09:33 pom.xml
-rw-r--r-- 1 root root  7262 Apr 13 09:33 readme.md
drwxr-xr-x 3 root root  4096 Apr 13 09:33 src/
drwxr-xr-x 7 root root  4096 Apr 13 09:33 target/

```


###  Run SonarQube Analysis  
```bash
mvn clean verify sonar:sonar \
  -Dsonar.projectKey=java \
  -Dsonar.projectName='java' \
  -Dsonar.host.url=http://18.232.140.167:9000 \
  -Dsonar.token=sqp_XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
```

 Output will show analysis and upload to SonarQube.

---

##  JavaScript Project Analysis (NPM + SonarScanner)

###  Clone JS Project and Setup  
```bash
git clone https://github.com/<your-js-repo>.git
cd js_code/
npm install
npm install -g @sonar/scan
```
```bash
root@ip-172-31-26-213:~/Documentation/js_code# ll
total 208
drwxr-xr-x 4 root root   4096 Apr 13 09:33 ./
drwxr-xr-x 6 root root   4096 Apr 13 09:33 ../
drwxr-xr-x 3 root root   4096 Apr 13 09:33 dist/
-rw-r--r-- 1 root root 172684 Apr 13 09:33 package-lock.json
-rw-r--r-- 1 root root   1086 Apr 13 09:33 package.json
-rw-r--r-- 1 root root   6041 Apr 13 09:33 readme.md
drwxr-xr-x 4 root root   4096 Apr 13 09:33 src/
-rw-r--r-- 1 root root    322 Apr 13 09:33 tsconfig.json
-rw-r--r-- 1 root root   1317 Apr 13 09:33 webpack.config.js
```


###  Create `sonar-project.properties`
```bash
vi sonar-project.properties
```
![](images/Screenshot%20(37).png)

Paste the following:
```properties
sonar.projectKey=js
sonar.projectName=js
sonar.projectVersion=1.0
sonar.sources=.
sonar.host.url=http://18.232.140.167:9000
sonar.login=sqp_ff83de42e0c3931f39f99357a83f4c0fa98efbc6
```

###  Run SonarQube Scanner  
```bash
sonar-scanner
```

 Results will be uploaded to the SonarQube server.

---

##  Python Project Analysis

###  Prepare and Analyze Python Project  
```bash
cd python_code/
```

```bash

root@ip-172-31-26-213:~/Documentation/python_code# ll
total 20
drwxr-xr-x 3 root root 4096 Apr 13 09:33 ./
drwxr-xr-x 6 root root 4096 Apr 13 09:33 ../
drwxr-xr-x 4 root root 4096 Apr 13 09:33 app/
-rw-r--r-- 1 root root 6514 Apr 13 09:33 readme.md

```


###  Run SonarQube Scanner  
```bash
sonar-scanner \
  -Dsonar.projectKey=python \
  -Dsonar.sources=. \
  -Dsonar.host.url=http://44.202.193.53:9000 \
  -Dsonar.token=sqp_e36b7c7b5af01a866631fd7a9dff62cf5b0c3c59
```

```bash
[INFO]  ScannerEngine: Analysis report compressed in 1475ms, zip size=11.1 MB
[INFO]  ScannerEngine: Analysis report uploaded in 160ms
[INFO]  ScannerEngine: ANALYSIS SUCCESSFUL, you can find the results at: http://18.232.140.167:9000/dashboard?id=java-1
[INFO]  ScannerEngine: Note that you will be able to access the updated dashboard once the server has processed the submitted analysis report
[INFO]  ScannerEngine: More about the report processing at http://18.232.140.167:9000/api/ce/task?id=d0ce00cc-7031-4b90-bcfb-c43b5f087224
[INFO]  ScannerEngine: Analysis total time: 1:19.266 s
[INFO]  ScannerEngine: SonarScanner Engine completed successfully
```



 Python code analysis will appear in the SonarQube dashboard.

![](images/Screenshot%20(38).png)

---

##  4. View Results in SonarQube

Open browser => http://18.232.140.167:9000 => Select your **project dashboard**

 Review for each project:
- Code smells
- Bugs
- Vulnerabilities
- Test coverage (if applicable)

---

##  Summary

| Language | Build Tool      | Analysis Command                                      |
|----------|------------------|------------------------------------------------------|
| Java     | Maven            | `mvn clean verify sonar:sonar ...`                  |
| JS       | NPM + Scanner    | `sonar-scanner` with `sonar-project.properties`     |
| Python   | Sonar Scanner    | `sonar-scanner -Dsonar.projectKey=...`              |

 All results are published to: **http://44.202.193.53:9000**

![](images/Screenshot%20(39).png)