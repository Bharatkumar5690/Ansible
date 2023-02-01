## Install Dotnet on Ubuntu & Centos

### Ubuntu
* Dotnet manual steps for ubuntu [refer here](https://learn.microsoft.com/en-us/dotnet/core/install/linux-ubuntu#install-the-sdk)
```
sudo apt-get update
sudo apt-get install -y dotnet-sdk-6.0
sudo apt-get install -y aspnetcore-runtime-6.0
sudo apt-get install -y dotnet-runtime-6.0
```
### Centos
* Dotnet manual steps for centos [refer here](https://learn.microsoft.com/en-us/dotnet/core/install/linux-centos#centos-7)
```
sudo rpm -Uvh https://packages.microsoft.com/config/centos/7/packages-microsoft-prod.rpm
sudo yum install dotnet-sdk-7.0
sudo yum install aspnetcore-runtime-7.0
sudo yum install dotnet-runtime-7.0
```
