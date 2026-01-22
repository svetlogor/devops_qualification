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
Если при выполненеи wget ошибка, тогда нужно создать деррикторию:
```
mkdir -p /etc/apt/keyrings
```


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