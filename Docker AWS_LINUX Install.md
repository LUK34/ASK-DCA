### Docker installation in Amazon Linux AMI (By Ashom Sir)
- **Note:** The commands is specific to Amazon Linux. If its ubuntu the commands to install docker is different.
- **Step-1 :** Create EC2 VM (Amazon linux ami) 
- **Step-2 :** Connect with that vm using ssh client (git bash)
- **Step-3 :** Execute below commands
- **Installation of Docker**
```
sudo yum update -y
sudo yum install docker -y
sudo service docker start
```
- ** Add ec2-user user to docker group**
```
sudo usermod -aG docker ec2-user
```
- **Exit from terminal and Connect again**
```
exit
```
- **Verify Docker installation**
```
docker -v
```