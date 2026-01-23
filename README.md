# devops_qualification
Jenkins, Terraform, Ansible, Yandex Cloud\
OS: Ubuntu 20.04
```
apt update
apt install unzip
```

## Jenkins:
```
apt install openjdk-17-jdk -y
java -version
wget -O /etc/apt/keyrings/jenkins-keyring.asc   https://pkg.jenkins.io/debian-stable/jenkins.io-2026.key
echo "deb [signed-by=/etc/apt/keyrings/jenkins-keyring.asc]"   https://pkg.jenkins.io/debian-stable binary/ | sudo tee   /etc/apt/sources.list.d/jenkins.list > /dev/null
apt update
apt install jenkins
systemctl enable jenkins
systemctl start jenkins
systemctl status jenkins
cat /var/lib/jenkins/secrets/initialAdminPassword
```
>If there is an error when executing wget, then you need to create a directory:
>```
>mkdir -p /etc/apt/keyrings
>```
**In Credentials, you need to add:**

|             Type              |        ID         |      Discription       |
|:-----------------------------:|:-----------------:|:----------------------:|
|          Secret text          |     YC_TOKEN      |   Yandex Cloud token   |
|          Secret text          |   YC_FOLDER_ID    | Yandex Cloud folder id |
|          Secret text          |    id_rsa_pub     |     SSH id_rsa.pub     |
| SSH Username with private key |      id_rsa       |       SSH id_rsa       |




## Terraform:
```
unzip terraform_1.13.4_linux_amd64.zip
cp terraform /bin
terraform --version
nano ~/.terraformrc

>>>
provider_installation {
  network_mirror {
    url = "https://terraform-mirror.yandexcloud.net/"
    include = ["registry.terraform.io/*/*"]
  }
  direct {
    exclude = ["registry.terraform.io/*/*"]
  }
}
```

## Ansible:
```
apt install ansible
```

## Yandex Cloud:
```
curl -sSL https://storage.yandexcloud.net/yandexcloud-yc/install.sh | bash
yc init

yc iam service-account list
export YC_TOKEN=$(yc iam create-token --impersonate-service-account-id <идентификатор_сервисного_аккаунта>)
export YC_CLOUD_ID=$(yc config get cloud-id)
export YC_FOLDER_ID=$(yc config get folder-id)
```