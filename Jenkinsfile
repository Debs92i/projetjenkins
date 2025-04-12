pipeline {
    agent any

    environment {
        ANSIBLE_HOST_KEY_CHECKING = 'False'
        VENV_DIR = 'venv'  // Dossier de l'environnement virtuel
    }

    stages {
        stage('Cloner le dépôt') {
            steps {
                git 'https://github.com/Debs92i/projetjenkins.git'
            }
        }

        stage('Créer un environnement virtuel') {
            steps {
                sh 'python3 -m venv $VENV_DIR'
            }
        }

        stage('Installer les dépendances') {
            steps {
                sh './$VENV_DIR/bin/pip install --upgrade pip'
                sh './$VENV_DIR/bin/pip install -r requirements.txt'
            }
        }

        stage('Tests unitaires') {
            steps {
                sh './$VENV_DIR/bin/pytest test_app.py'
            }
        }

        stage('Déploiement avec Ansible') {
            steps {
                sh './$VENV_DIR/bin/ansible-playbook -i inventory deploy.yml --vault-password-file ~/.vault_pass.txt'
            }
        }
    }

    post {
        success {
            echo "✅ Déploiement réussi !"
        }
        failure {
            echo "🚨 Le pipeline a échoué ! Vérifie les logs."
        }
    }
}

