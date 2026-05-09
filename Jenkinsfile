pipeline {
    agent any // Use any available agent

    environment {
        // Fixes potential encoding errors (UTF-8 vs ISO)
        LANG = 'en_US.UTF-8'
        LC_ALL = 'en_US.UTF-8'
    }

    tools {
        // This must match the name you gave Maven in 'Global Tool Configuration'
        maven 'Maven'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'master', 
                    url: 'https://github.com/shravan-hegde/MavenAnsibleWebApp1.git'
            }
        }

        stage('Build') {
            steps {
                // Compiles code and creates the .war file
                sh 'mvn clean package'
            }
        }

        stage('Archive') {
            steps {
                // Saves the .war file in Jenkins for record-keeping
                archiveArtifacts artifacts: 'target/*.war', fingerprint: true
            }
        }

        stage('Deploy') {
            steps {
                /* 
                   Note: You are running 'mvn clean package' again here. 
                   Usually, once in the Build stage is enough! 
                */
                sh 'mvn clean package'
                
                // Triggers the Ansible playbook using the inventory file you wrote
                sh 'ansible-playbook ansible/playbook.yml -i ansible/hosts.ini'
            }
        }
    }
}

