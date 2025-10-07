Create EC2 Instance for Build Server
Launch a new EC2 instance for building the project.

Use your preferred AMI (Ubuntu 24.04 LTS recommended).

Configure key pair (use .pem file).

Image
Connect to Build Server via SSH
ssh -i "name.pem" ubuntu@<BUILD_SERVER_IP>

Image
Ensure you can access the server terminal.
Image
Check Java and Maven Installation
Check if Java is installed:
java -version
Check Maven:
mvn -version
If not installed, install:
sudo apt update
sudo apt install openjdk-17-jdk -y
sudo apt install maven -y

Clone Code from GitHub
Image Image
Validate Maven Build and Package
mvn validate
mvn package
If build fails due to Java version, edit pom.xml to use Java 17:

17 17 Image
Re-run:
mvn package

Build Success
