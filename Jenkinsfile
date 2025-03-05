pipeline {
    agent any
    parameters {
        string(name: 'tipo', defaultValue: '', description: 'Ingresa un tipo de respuesta ')
        string(name: 'response', defaultValue: '', description: 'Ingresa el response code')
    }
    environment {
        // CONTADOR;
    }
    stages {
        stage('Input') {
            steps {
                script {
                    // Los parametros pueden ser accedidos mediante params.parameterName
                    sh curl -O https://raw.githubusercontent.com/elastic/examples/master/Common%20Data%20Formats/apache_logs/apache_logs
                    sh cat apache_logs | grep - i logstash    
                }
            }
        }
    }
}
