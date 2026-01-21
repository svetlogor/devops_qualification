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
				  terraform plan -var='ssh_key=${SSH_PUB_KEY}' \
				  	-var='folder_id=${YC_FOLDER_ID}' \
				  	-detailed-exitcode || true
			  '''
		  }
	  }

	  stage('Terraform apply') {
		  steps {
			  sh '''
				  set -e
				  terraform apply -auto-approve \
				  	-var='ssh_key=${SSH_PUB_KEY}' \
				  	-var='folder_id=${YC_FOLDER_ID}'
			  '''
		  }
	  }

	  stage('Terraform destroy') {
		  steps {
			  input message: 'Destroy Terraform?'
			  sh '''
				  set -e
				  terraform destroy -auto-approve
			  '''
		  }
	  }
  }
}