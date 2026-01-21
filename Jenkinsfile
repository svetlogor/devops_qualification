pipeline {
  agent any

  stages {
	  stage('Creating two instances in Yandex Cloud') {
		  environment {
			SSH_PUB_KEY = credentials('id_rsa_pub')
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
				  terraform init
				  terraform plan -var='ssh_key=${SSH_PUB_KEY}'
				  terraform apply -auto-approve
			  '''
		  }
	  }

    //stage('Copy source with configs') {
    //  steps {
    //    git(url: 'https://github.com/svetlogor/hello-world-war.git', branch: 'backend1-staging', poll: true)
    //  }
    //}
	//
    //stage('Build war') {
    //  steps {
    //    sh 'cd ./hello-world-war && mvn package'
    //  }
    //}

//     stage('Make docker image') {
//       steps {
//         sh 'cp -R gateway-api/build/libs/* docker-setup/shop/gateway-api && cd docker-setup/shop/gateway-api && docker build --tag=gateway-api .'
//         sh '''docker tag gateway-api devcvs-srv01:5000/shop2-backend/gateway-api:2-staging && docker push devcvs-srv01:5000/shop2-backend/gateway-api:2-staging'''
//
//       }
//     }

//     stage('Run docker on devbe-srv01') {
//       steps {
//         sh 'ssh-keyscan -H devbe-srv01 >> ~/.ssh/known_hosts'
//         sh '''ssh jenkins@devbe-srv01 << EOF
// 	sudo docker pull devcvs-srv01:5000/shop2-backend/gateway-api:2-staging
// 	cd /etc/shop/docker
// 	sudo docker-compose up -d
// EOF'''
//       }
//     }
  }
//   triggers {
//     pollSCM('*/1 H * * *')
//   }
}