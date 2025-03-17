pipeline {
    agent any
    parameters {
        string(name: 'LOGIN', defaultValue: '', description: 'Identificador del usuario')
        string(name: 'NOMBRE', defaultValue: '', description: 'Nombre del usuario')
        string(name: 'APELLIDO', defaultValue: '', description: 'Apellido del usuario')
        choice(name: 'DEPARTAMENTO', choices: ['Contabilidad', 'Finanzas', 'Tecnologia'], description: 'Grupo al cual se asigna el usuario')
    }
    stages {
        stage('Validar Grupos') {
            steps {
                script {
                    def grupos = ['Contabilidad', 'Finanzas', 'Tecnologia']
                    grupos.each { group ->
                        def grupoExiste = sh(script: "getent group | grep -q \"^${group}:\"", returnStatus: true)
                        if (grupoExiste != 0) {
                            echo "Creando grupo '${group}'..."
                            sh "sudo groupadd ${group}"
                        } else {
                            echo "Grupo '${group}' ya existe."
                        }
                    }
                }
            }
        }

        stage('Validar Usuario') {
            steps {
                script {
                    def usuarioExiste = sh(script: "id ${params.LOGIN}", returnStatus: true)
                    if (usuarioExiste == 0) {
                        error "Error: El usuario '${params.LOGIN}' ya existe. Saliendo..."
                    }
                }
            }
        }

        stage('Validar Grupo del Usuario') {
            steps {
                script {
                    def grupoUsuarioExiste = sh(script: "getent group | grep -q \"^${params.DEPARTAMENTO}:\"", returnStatus: true)
                    if (grupoUsuarioExiste != 0) {
                        error "Error: El grupo '${params.DEPARTAMENTO}' no existe. Saliendo..."
                    }
                }
            }
        }

        stage('Crear Usuario') {
            steps {
                script {
                    echo "Creando usuario '${params.LOGIN}'..."
                    sh "sudo useradd -m -s /bin/bash -g ${params.DEPARTAMENTO} -c \"${params.NOMBRE} ${params.APELLIDO}\" ${params.LOGIN}"
                    
                    def verificacionUsuario = sh(script: "id ${params.LOGIN}", returnStatus: true)
                    if (verificacionUsuario != 0) {
                        error "Error al crear el usuario '${params.LOGIN}'. Saliendo..."
                    }
                    echo "Usuario '${params.LOGIN}' creado con éxito."
                }
            }
        }

        stage('Generar Contraseña y Asignar') {
            steps {
                script {
                    def PASSWORD = sh(script: "openssl rand -base64 12", returnStdout: true).trim()
                    sh "echo '${params.LOGIN}:${PASSWORD}' | sudo chpasswd"
                    sh "sudo passwd -e ${params.LOGIN}"
                    echo "Contraseña temporal para '${params.LOGIN}': ${PASSWORD}"
                }
            }
        }
    }
}

