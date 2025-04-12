pipeline {
    agent any  // Jenkins peut exécuter ce job sur n'importe quel agent

    environment {
        ANSIBLE_HOST_KEY_CHECKING = 'False'  // pour éviter les erreurs SSH avec Ansible
    }

    stages {
        stage('Cloner le dépôt') {
            steps {
                git 'https://github.com/Debs92i/projetjenkins.git'
            }
        }

        stage('Installer les dépendances') {
            steps {
                sh 'pip install -r requirements.txt || true'  // en cas de requirements, sinon on ignore
            }
        }

        stage('Tests unitaires') {
            steps {
                sh 'pytest test_app.py'
            }
        }

        stage('Déploiement avec Ansible') {
            steps {
                // Adapte le chemin de ton playbook
		sh 'ansible-playbook -i ./ansible/hosts deploy.yml --vault-password-file ~/.vault_pass.txt'
               
            }
        }
    }

    post {
        failure {
            echo "🚨 Le pipeline a échoué ! Vérifie les logs."
        }
        success {
            echo "✅ Déploiement réussi !"
        }
    }
}
