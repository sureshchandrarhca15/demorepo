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
        activeChoiceParam('COUNTRY') {
            description('Select a country')
            filterable(true)
            choiceType('SINGLE_SELECT')
            groovyScript {
                return ['USA', 'Canada', 'Germany', 'France', 'Australia']
            }
        }

        activeChoiceReactiveParam('CITY') {
            description('Select a city based on the selected country')
            filterable(true)
            choiceType('SINGLE_SELECT')
            groovyScript {
                def country = COUNTRY
                if (country == 'USA') {
                    return ['New York', 'Los Angeles', 'Chicago']
                } else if (country == 'Canada') {
                    return ['Toronto', 'Vancouver', 'Montreal']
                } else if (country == 'Germany') {
                    return ['Berlin', 'Munich', 'Frankfurt']
                } else if (country == 'France') {
                    return ['Paris', 'Lyon', 'Marseille']
                } else if (country == 'Australia') {
                    return ['Sydney', 'Melbourne', 'Brisbane']
                } else {
                    return ['No cities available']
                }
            }
            referencedParameters('COUNTRY')
        }
    }

    stages {
        stage('Build') {
            steps {
                script {
                    echo "Selected Country: ${params.COUNTRY}"
                    echo "Selected City: ${params.CITY}"
                }
            }
        }
    }
}
