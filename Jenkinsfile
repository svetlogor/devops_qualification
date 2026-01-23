pipeline {
  agent any

  environment {
	  SSH_PUB_KEY = credentials('id_rsa_pub')
	  YC_TOKEN = credentials('YC_TOKEN')
	  YC_FOLDER_ID = credentials('YC_FOLDER_ID')
	  TF_IN_AUTOMATION = "true"
      TF_INPUT = "false"
  }

  stages {
	  stage('Terraform init') {
		  steps {
			  sh '''
			  cat <<EOF > .terraformrc
provider_installation {
  network_mirror {
    url = "https://terraform-mirror.yandexcloud.net"
    include = ["registry.terraform.io/*/*"]
  }
  direct {
    exclude = ["registry.terraform.io/*/*"]
  }
}
EOF
            export TF_CLI_CONFIG_FILE=\$(pwd)/.terraformrc
				  set -e
				  export YC_TOKEN=${YC_TOKEN}
				  terraform init
			  '''
		  }
	  }

	  stage('Terraform plan') {
		  steps {
			  sh '''
				  set -e
				  terraform plan -var="ssh_key=${SSH_PUB_KEY}" \
				  	-var="folder_id=${YC_FOLDER_ID}" \
				  	-detailed-exitcode || true
			  '''
		  }
	  }

	  stage('Terraform apply') {
		  steps {
			  sh '''
				  set -e
				  terraform apply -auto-approve \
				  	-var="ssh_key=${SSH_PUB_KEY}" \
				  	-var="folder_id=${YC_FOLDER_ID}"
				  sleep 30
			  '''
		  }
	  }

	  stage('Generate Ansible inventory') {
		  steps {
			  sh '''
				  set -e
				  BUILD_IP=$(terraform output -raw external_ip_address_build)
				  PROD_IP=$(terraform output -raw external_ip_address_prod)

      cat > inventory.ini <<EOF
[build]
${BUILD_IP}

[prod]
${PROD_IP}

[all:vars]
ansible_user=ubuntu
ansible_ssh_private_key_file=~/.ssh/id_rsa
EOF
'''
		  }
	  }

	  stage('Ansible deploy') {
		  steps {
			  sh '''
			 	  cat inventory.ini
    		  '''
		  }
	  }

	  //ansible -i inventory.ini all -m ping
	  //ansible-playbook -i inventory.ini playbook.yml

	  stage('Terraform destroy') {
		  steps {
			  input message: 'Destroy Terraform?'
			  sh '''
				  set -e
				  terraform destroy -auto-approve \
				  	-var="ssh_key=${SSH_PUB_KEY}" \
				  	-var="folder_id=${YC_FOLDER_ID}"
			  '''
		  }
	  }
  }
}