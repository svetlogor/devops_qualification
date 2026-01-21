pipeline {
  agent any

  stages {
	  stage('Creating two instances in Yandex Cloud') {
		  environment {
			SSH_PUB_KEY = credentials('id_rsa_pub')
			YC_TOKEN = credentials('YC_TOKEN')
		  }
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
				  export YC_TOKEN=$(yc iam create-token --impersonate-service-account-id ${YC_TOKEN})
				  export YC_CLOUD_ID=$(yc config get cloud-id)
				  export YC_FOLDER_ID=$(yc config get folder-id)
				  terraform init
				  terraform plan -var='ssh_key=${SSH_PUB_KEY}'
				  terraform apply -auto-approve
			  '''
		  }
	  }
  }
}