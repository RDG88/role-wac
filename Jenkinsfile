/*
 * Standardized Jenkinsfile for Ansible Role
 */

pipeline {
    agent {
        docker {
            image 'quay.io/ansible/molecule:3.0.4'
            args '--network host -u root:root -v $HOME/.cache:/root/.cache'
        }
    }

    environment {
        VCENTER = credentials('infra-vcenter-zollo-vca01')
        ANSIBLE_HOST_KEY_CHECKING = "False"
    }

    stages {

        stage ('Configure Molecule Drivers') {
            steps {
                checkout(
                    [
                        $class: 'GitSCM',
                        branches: [[name: '*/master']],
                        doGenerateSubmoduleConfigurations: false,
                        extensions: [[$class: 'RelativeTargetDirectory', relativeTargetDir: 'ansible-ci-win']],
                        submoduleCfg: [],
                        userRemoteConfigs: [[credentialsId: 'github-ssh-joezollo',
                            url: 'git@github.com:joezollo/ansible-ci-windows.git']
                        ]
                    ]
                )

            }
        }

        stage ('Display Debug Info') {
            steps {
                sh '''
                    ansible --version
                    molecule --version
                '''
            }
        }

        stage ('Install Prerequisites') {
            steps {
                sh "apk add --update --no-cache --repository http://dl-3.alpinelinux.org/alpine/edge/testing/ sshpass"
                sh "pip install -r requirements.txt"
            }
        }

        stage ('Molecule Test') {
            steps {
                sh "rm -rf drivers/"
                sh "mv -fv ansible-ci-win/drivers/ ./"
                sh "rm -rf ansible-ci-win/"
                sh "molecule --debug test --all"
            }
        }
    }
}