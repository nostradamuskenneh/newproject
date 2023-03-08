pipeline {
    agent any
      environment {
	DOCKERHUB_CREDENTIALS=credentials('dockerhub-cred-ID')
	}

    stages {
        stage('Clone') {
            steps {
                sh '''
		rm -rf Industry-Grade-Project-Java-Project
			    git clone https://github.com/nostradamuskenneh/Industry-Grade-Project-Java-Project.git
			    cd Industry-Grade-Project-Java-Project
                ls 
                pwd
                '''
            }
        }
        stage('Compile') {
            steps {
                sh '''
                cd Industry-Grade-Project-Java-Project
			    mvn compile
				ls 
                pwd
                '''
            }
        }
        stage('Test') {
            steps {
                sh '''
			    cd Industry-Grade-Project-Java-Project
			    mvn test
                ls 
                pwd
                '''
            }
        }
        stage('Package') {
            steps {
                sh '''
			   rm -rf /$WORKSPACEABCtechnologies-1.0.war
			   cd Industry-Grade-Project-Java-Project
			   mvn package
			   cd target
			   cp -R ABCtechnologies-1.0.war /$WORKSPACE 
			   ls 
			   pwd
                '''
            }
        }
        stage('Build-image') {
            steps {
                sh '''
		                pwd
				ls
				cd /$WORKSPACE
			     # "ansible-playbook create_image_edureka.yml -i /etc/ansible/create_image_edureka.yml  --user ec2-user "
				 docker build -t edu-project .
                '''
            }
        }
        stage('Push-to-dockerhub') {
            steps {
                sh '''
				cd /$WORKSPACE
			        echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u oumarkenneh --password-stdin
				docker tag edu-project oumarkenneh/edu-project-reka
				docker push oumarkenneh/edu-project-reka


                ls 
                pwd
                '''
            }
        }
        stage('Ansible Deploy-to-k8s-cluster') {
             
            steps {
                 
                sh "ansible-playbook /etc/ansible/kube_deploy.yml"
}
}
    }
}
