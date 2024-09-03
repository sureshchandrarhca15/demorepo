pipeline {
    agent {
        kubernetes {
            label 'k8s-agent'
            yaml """
            apiVersion: v1
            kind: Pod
            spec:
              containers:
              - name: node
                image: sureshchandrarhca15/devops-toolkit:nodejs18-gsdk-v2
                tty: true
                command:
                - bash
              nodeSelector:
                app: "jenkins"
                node_pool: "jenkins"
              tolerations:
              - key: "app"
                operator: "Equal"
                value: "jenkins"
                effect: "NoSchedule"
              - key: "node_pool"
                operator: "Equal"
                value: "jenkins"
                effect: "NoSchedule"
            """
        }
    }
    
    parameters {
        // Active Choices Parameter equivalent
        choice(name: 'ACTION_TYPE', choices: ['none', 'create', 'update'], description: 'What would you like to do?')
        
        // Active Choices Reactive Reference Parameter equivalent for ENV_NAME
        string(name: 'ENV_NAME', defaultValue: '', description: 'Enter the environment name if creating a new one, or select an existing environment to update')
        
        // Active Choices Reactive Reference Parameter equivalent for VAULT_SECRET
        choice(name: 'VAULT_SECRET', choices: ['None'], description: 'Would you like to configure Vault secrets?')
    }

    stages {
        stage('Set Parameters Dynamically') {
            steps {
                script {
                    // Dynamically set VAULT_SECRET options based on ACTION_TYPE
                    if (params.ACTION_TYPE == 'create') {
                        currentBuild.description = 'Setting VAULT_SECRET to Yes/No'
                        env.VAULT_SECRET_OPTIONS = ['Yes', 'No']
                    } else if (params.ACTION_TYPE == 'update') {
                        currentBuild.description = 'Setting VAULT_SECRET to No'
                        env.VAULT_SECRET_OPTIONS = ['No']
                    } else {
                        currentBuild.description = 'No action selected, setting VAULT_SECRET to None'
                        env.VAULT_SECRET_OPTIONS = ['None']
                    }
                }
            }
        }

        stage('Check User Action') {
            steps {
                script {
                    echo "INFO=> Checking user action, create|update..."
                    echo "You want to ${params.ACTION_TYPE} the dev environment, Running below script to do so..."
                }
            }
        }

        stage('Check Env Name') {
            steps {
                script {
                    echo "INFO=> Checking env name..."
                    if (params.ENV_NAME?.trim()) {
                        echo "Your provided dev env name is: ${params.ENV_NAME}"
                    } else {
                        error("dev env name not provided, exiting...")
                    }
                }
            }
        }

        stage('Check Vault Secret') {
            steps {
                script {
                    echo "INFO=> Checking Vault secrets action, Yes|No..."
                    if (params.VAULT_SECRET == "Yes") {
                        echo "Vault Secrets will be configured for this dev environment, Running below script to do so..."
                    } else {
                        echo "Skipping Vault Secrets configuration..."
                    }
                }
            }
        }
    }
}
