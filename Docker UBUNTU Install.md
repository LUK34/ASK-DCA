### Docker installation in UBUNTU AMI (By Ashom Sir)
- **Note:** The commands is specific to UBUNTU. If its ubuntu the commands to install docker is different.
- **Step-1 :** Create UBUNTU (Amazon linux ami) 
- **Step-2 :** Connect with that vm using ssh client (git bash)
- **Step-3 :** Execute below commands
- **Step 4:**1️⃣ Update your packages
```
sudo apt update
sudo apt upgrade -y
```
- **Step 5:** Install prerequisites
```
sudo apt install apt-transport-https ca-certificates curl software-properties-common -y
```
- **Step 6:** Add Docker’s official GPG key
```
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker.gpg
```
- **Step 7:** Add Docker’s official repository
```
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```
- **Step 8:** Install Docker Engine
```
sudo apt update
sudo apt install docker-ce docker-ce-cli containerd.io -y
```
- **Step 9:** Verify installation
```
docker --version
sudo docker run hello-world
```