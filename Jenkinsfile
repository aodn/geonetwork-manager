pipeline {
    agent {
        dockerfile {
            args '-v ${HOME}/.m2:/home/builder/.m2 -v ${HOME}/bin:${HOME}/bin'
            additionalBuildArgs '--build-arg BUILDER_UID=$(id -u)'
        }
    }
    stages {
        stage('package') {
            steps {
                sh 'mvn -B clean package -DskipTests'
            }
        }
        stage('deploy') {
            when { branch "imos-master" }
            steps {
                sh 'mvn -B deploy'
            }
        }
    }
    post {
        success {
            dir('target/') {
                archiveArtifacts artifacts: 'target/geonetwork-manager-*-SNAPSHOT.jar', fingerprint: true, onlyIfSuccessful: true
            }
        }
    }
}
