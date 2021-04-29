pipeline {
    agent any

    
    stages {
        
        stage('Build') {
            steps {
                withGradle {
                    sh 'chmod +x ./gradlew'
                    sh './gradlew assemble'

                }
            }

            post {
                success {
                    archiveArtifacts 'build/libs/*.jar'
                }
            }

        }
        
        stage('Test') {
            steps {
                withGradle {
                    sh './gradlew test'
                }
            }

            post {
                always {
                    junit 'build/test-results/test/TEST-*.xml'
                    jacoco(execPattern: 'build/jacoco/*.exec')
                    
                }
            }
        }

        stage('pitest') {
            steps {
                withGradle {
                    sh './gradlew clean pitest'
                }
            }

            post {
                always {
                    pitmutation mutationStatsFile:  'build/reports/pitest/**/mutations.xml'
                }
            }
        }



        stage('OWASP') {
            steps {
                withGradle {
                    sh './gradlew dependencyCheckUpdate'
                    sh './gradlew dependencyCheckAnalyze'
                }
            }

            post {
                always {
                   dependencyCheckPublisher pattern: 'build/reports/dependency-check-report.xml' 
                }
            }

        }
    }
}
