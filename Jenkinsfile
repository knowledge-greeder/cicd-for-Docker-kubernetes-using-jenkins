pipeline {
    agent any

    environment {
        registry = "kubeimran/vproappdock"
        registryCredential = 'dockerhub'
        mavenTool = 'maven3' // Define Maven tool name
        mavenSettingsConfig = 'your-maven-settings' // Specify Maven settings config file
    }

    stages {
        stage('BUILD') {
            steps {
                script {
                    // Execute Maven clean install skipping tests
                    withMaven(
                        maven: mavenTool,
                        mavenSettingsConfig: mavenSettingsConfig
                    ) {
                        sh 'mvn clean install -DskipTests'
                    }
                }
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }

        stage('UNIT TEST') {
            steps {
                script {
                    // Run Maven tests
                    withMaven(
                        maven: mavenTool,
                        mavenSettingsConfig: mavenSettingsConfig
                    ) {
                        sh 'mvn test'
                    }
                }
            }
        }

        stage('INTEGRATION TEST') {
            steps {
                script {
                    // Run Maven verify (integration tests)
                    withMaven(
                        maven: mavenTool,
                        mavenSettingsConfig: mavenSettingsConfig
                    ) {
                        sh 'mvn verify -DskipUnitTests'
                    }
                }
            }
        }

        stage('CODE ANALYSIS WITH CHECKSTYLE') {
            steps {
                script {
                    // Execute Maven Checkstyle plugin
                    withMaven(
                        maven: mavenTool,
                        mavenSettingsConfig: mavenSettingsConfig
                    ) {
                        sh 'mvn checkstyle:checkstyle'
                    }
                }
            }
            post {
                success {
                    echo 'Generated Analysis Result'
                }
            }
        }
    }
}
