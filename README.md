**1.Create EC2 Instance for Build Server**
Launch a new EC2 instance for building the project.
Use your preferred AMI (Ubuntu 24.04 LTS recommended).
When creating EC2 instance, choose t2.micro.
Configure key pair (use .pem file).

<img width="1896" height="899" alt="Image" src="https://github.com/user-attachments/assets/a9d8568d-f495-4e7c-b29f-9051c36b1725" />

<img width="1876" height="835" alt="Image" src="https://github.com/user-attachments/assets/659c9775-1b50-4ffa-8171-cf94b1d0fab0" />

**2.Connect to Build Server via SSH**
Use the following command to connect:
ssh -i "name.pem" ubuntu@<BUILD_SERVER_IP>

<img width="1888" height="838" alt="Image" src="https://github.com/user-attachments/assets/47995d70-fd5c-45c6-bbc4-101bfdf2a04f" />

**3.Ensure that you can access the server terminal successfully.**

<img width="1466" height="747" alt="Image" src="https://github.com/user-attachments/assets/10f3229a-dad0-4e2a-aa80-256c738d4d2c" />

**4.Check Java and Maven Installation**
 Check if Java is installed:
 java --version
 Check Maven is installed:
 mvn --version
 If not installed, run:
 sudo apt update
 sudo apt install openjdk-17-jdk -y
 sudo apt install maven -y

**5.Clone Code from GitHub**

<img width="1875" height="898" alt="Image" src="https://github.com/user-attachments/assets/9a9c6179-db89-4a2b-aa13-dab6d7c53655" />

Using a command: git clone "URL"

<img width="1465" height="762" alt="Image" src="https://github.com/user-attachments/assets/35f3d49f-c4b8-409d-a093-76378bf65acb" />

**6.Validate Maven Build and Package**
mvn validate
mvn package

If build fails due to Java version, edit pom.xml to use Java 17:
<properties>
    <maven.compiler.source>17</maven.compiler.source>
    <maven.compiler.target>17</maven.compiler.target>
</properties>


<img width="1470" height="945" alt="Image" src="https://github.com/user-attachments/assets/da48f062-6f25-4cee-8e61-4b9f3d00f9e8" />

Re-run:
mvn validate

<img width="1461" height="950" alt="Image" src="https://github.com/user-attachments/assets/6a92f82f-8c71-4ebd-8e92-49c7206fe253" />

mvn package

<img width="1473" height="948" alt="Image" src="https://github.com/user-attachments/assets/745f5031-d7dd-4b76-8576-26c394ab5206" />

**7.On success, .war file will be generated in target/ folder.**

<img width="1455" height="940" alt="Image" src="https://github.com/user-attachments/assets/31476160-7394-44f8-a353-6ada9b939f39" />

**8.Create Deploy Server**
Launch another EC2 instance for deployment.
Use same key pair (.pem) to access.

<img width="1898" height="972" alt="Image" src="https://github.com/user-attachments/assets/54125cbe-cec0-49af-9b2f-58c1709c0cc2" />

**9.Install Java on Deploy Server**
sudo apt update
sudo apt install openjdk-17-jdk -y

**10.Install and Setup Tomcat**
Download Tomcat from Apache Tomcat website
Copy link address

<img width="1897" height="965" alt="Image" src="https://github.com/user-attachments/assets/e1ab419f-9ae7-47db-b125-2e54de2c7384" />

Paste it on the terminal you connected 
By using a command: wget "Link address"

<img width="1425" height="966" alt="Image" src="https://github.com/user-attachments/assets/88b820f1-116e-4ac7-8df7-9085036b51f2" />

**11.Extract apache-tomcat**
By using a command: tar -xvzf apache-tomcat-9.0.110.tar.gz
Check whether it is extracted or not by using a command: ls

<img width="1482" height="1017" alt="Image" src="https://github.com/user-attachments/assets/12734f3f-ff73-441c-93b0-950fefd31f10" />

**12.Check Tomcat started using startup.sh**
startup.sh: shell script used to start the server. It is found inside the bin/ folder.
By using a command: cd apache-tomcat-9.0.110/bin
Start Tomcat: ./startup.sh
It starts the application server by calling internal Java commands.
Tomcat should start successfully.

**13. Open Tomcat in Browser**
Try visiting: http://<DEPLOY_SERVER_IP>:8080

<img width="1850" height="835" alt="Image" src="https://github.com/user-attachments/assets/b12da198-a378-465e-9299-58039c986342" />

The page doesn’t load.

<img width="1901" height="981" alt="Image" src="https://github.com/user-attachments/assets/f5e7e502-9a9e-4bc6-a2d1-845bcb769a35" />


If the page doesn’t load, open port 8080 in EC2 Security Group inbound rules.

<img width="1918" height="912" alt="Image" src="https://github.com/user-attachments/assets/6905cdb3-e56d-46f6-b7d5-50338b19c8d4" />
<img width="1915" height="962" alt="Image" src="https://github.com/user-attachments/assets/90213f82-5172-40c1-b278-e30535507370" />

Now reload the page,

<img width="1898" height="968" alt="Image" src="https://github.com/user-attachments/assets/e1a13866-d6e8-4212-ac90-338d8169521e" />

**14.Configure Tomcat Manager Access**
Navigate to Manager Apps in Tomcat.
We get an error-403 Access Denied

<img width="1232" height="588" alt="Image" src="https://github.com/user-attachments/assets/61d0c316-0a10-4d18-ad31-e4f443c8a400" />

**15.In that we need to edit Two files:**

<img width="1458" height="1002" alt="Image" src="https://github.com/user-attachments/assets/cc23740a-ffbf-4779-bb49-071ffff33f2e" />

 **1.context.xml**

<img width="1453" height="956" alt="Image" src="https://github.com/user-attachments/assets/b50dfa23-a5c9-4552-a402-061e82d45966" />

Comment out or adjust RemoteAddrValve to allow your IP:

<!-- <Valve className="org.apache.catalina.valves.RemoteAddrValve"
       allow="127\.\d+\.\d+\.\d+|::1"/> -->

<img width="1457" height="966" alt="Image" src="https://github.com/user-attachments/assets/4ffe3e5c-62ca-45ee-ab50-7240182b1e89" />

    **2.tomcat-users.xml**

<img width="1453" height="987" alt="Image" src="https://github.com/user-attachments/assets/b4831e16-ff5b-4829-86d0-90f4977bb63b" />

<role rolename="manager-gui"/>
<user username="admin" password="admin123" roles="manager-gui"/>

<img width="1445" height="958" alt="Image" src="https://github.com/user-attachments/assets/3e807037-d167-4e13-95e6-4fc2db78eeb7" />

Save files and restart Tomcat
./shutdown.sh
./startup.sh
You get a sign in page, Fill the details and sign in.

<img width="1908" height="1013" alt="Image" src="https://github.com/user-attachments/assets/0a9e6ecc-516f-4c3d-b662-766d7fc1c3c8" />

**16.You can see Tomcat Web Application Manager**

<img width="1910" height="1012" alt="Image" src="https://github.com/user-attachments/assets/445e634e-46c6-4169-a9a0-4c9eae6aad43" />

**17. Share PEM Key Between Servers**
Copy .pem file from local machine to build server
Using a command: scp -i "javaweb.pem" javaweb.pem ubuntu@<BUILD_SERVER_IP>:~/

<img width="1470" height="708" alt="Image" src="https://github.com/user-attachments/assets/a4447e13-24a7-4b71-9e0c-2ac98517812f" />

Check .pem file is copied in your build server

<img width="1472" height="997" alt="Image" src="https://github.com/user-attachments/assets/deadfca8-ebea-490c-ab15-3acc5b83a66d" />

**17.Set permissions:**
chmod 400 javaweb.pem

<img width="1469" height="998" alt="Image" src="https://github.com/user-attachments/assets/088d51e0-e938-43df-aa69-b55cef5b1a5e" />

**18. Deploy .war from Build to Deploy Server**
From build server, copy .war to deploy server using scp:

scp -i javaweb.pem /home/ubuntu/JavaWebCalculator/target/webapp-0.1.3.war ubuntu@<DEPLOY_SERVER_IP>:/home/ubuntu/apache-tomcat-9.0.110/webapps/

<img width="1469" height="511" alt="Image" src="https://github.com/user-attachments/assets/b2c643bf-3a3f-4378-a373-75eb7e4fa190" />

**19.Refresh Tomcat Manager → web app should appear.**

<img width="1903" height="928" alt="Image" src="https://github.com/user-attachments/assets/40bd8c5c-34c8-4843-9cac-6f55c722db0c" />

**20. Access Java Calculator Application**
Open in browser:
http://<DEPLOY_SERVER_IP>:8080/webapp
Verify the calculator works.

<img width="1885" height="1021" alt="Image" src="https://github.com/user-attachments/assets/524ac0bf-28c4-4f54-9c77-499f3ed662cf" />































 




