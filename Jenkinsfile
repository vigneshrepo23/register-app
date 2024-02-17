pipeline {
    agent {
        label "slave"
    }
    tools {
        jdk "java17"
        maven "maven3"
    }
    stages {
        stage ("clean workspace") {
            steps {
                cleanWs()
            }
        }
        
        stage {"git checkout"} {
            steps {
                git branch: "main", crendentialsID: "gitcred", url: "https://github.com/vigneshrepo23/register-app"
            }
        }
    }
}
