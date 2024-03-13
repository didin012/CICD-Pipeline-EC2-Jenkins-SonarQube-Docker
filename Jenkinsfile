node {
    stage('SCM') {
        git branch: 'main', 
        url: 'https://github.com/didin012/Jenkins-EC2-CICD-SonarQube-Docker'
    }
        
    stage('SonarQube Analysis') {
        def scannerHome = tool 'SonarScanner';
        withSonarQubeEnv() {
            sh "${scannerHome}/bin/sonar-scanner"
        }
    }
    
    stage('Copying Files to Docker instance') {
        sh """
        ssh ubuntu@54.152.93.9 rm -r ./website/* &&\\
        scp -r ./* ubuntu@54.152.93.9:~/website/
        """
    }
    stage('Running Docker on Remote SSH'){
        def remote = [:]
        remote.user = 'ubuntu'
        remote.host = '54.152.93.9'
        remote.name = 'ubuntu'
        remote.password = 'admin123'
        remote.allowAnyHosts = 'true'
        
        sshCommand(remote: remote, command: 'pwd')
        sshCommand(remote: remote, command: 'docker stop $(docker ps -a -q)')
        sshCommand(remote: remote, command: 'docker rm $(docker ps -a -q)')
        sshCommand(remote: remote, command: 'docker build -t staticwebsite /home/ubuntu/website')
        sshCommand(remote: remote, command: 'docker run -d -p 8085:80 --name=My-Website staticwebsite')
    }
}
