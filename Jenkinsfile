node {
    def buildNumber = BUILD_NUMBER
    stage("Git clone"){
        git url:'https://github.com/vishnupriyan3595/java-web-app-docker.git',branch:'master'
    }
    stage("maven clean package"){
        def mavenHome= tool name: "maven",type: "maven"
        sh "${mavenHome}/bin/mvn clean package"
    }
    stage ("Build Docker Image"){
        sh "docker build -t vishnu3595/java-web-app-docker:${buildNumber}  ."
    }
    stage ("Docker Login and Push"){
        withCredentials([string(credentialsId: 'vishnu3595', variable: 'vishnu3595')]) {
        sh "docker login -u vishnu3595 -p ${vishnu3595}"
        }
        sh "docker push vishnu3595/java-web-app-docker:${buildNumber}"
    }
    stage ("Deploy Application"){
        sshagent(['ubuntu']) {
    sh "ssh -o StrictHostKeyChecking=no ubuntu@172.31.44.45 docker rm -f javawebappcontainer || true"
    sh "ssh -o StrictHostKeyChecking=no ubuntu@172.31.44.45 docker run -d -p 8080:8080 --name javawebappcontainer vishnu3595/java-web-app-docker:${buildNumber}"
     }
    }
}
