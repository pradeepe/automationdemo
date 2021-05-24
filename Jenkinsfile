node(){
    stage('Cloning Git') {
        checkout scm
    }    
    stage('Install dependencies') {
        nodejs('nodejs') {
            sh 'npm install'
            echo "Modules installed"
        }
    }
    stage('Build') {
        nodejs('nodejs') {
            sh 'npm run build'
            echo "NPM Build is completed..."
        }
    }
    stage('Code Quality Check via SonarQube') {
        script {
           def scannerHome = tool name: 'SonarQube';
            withSonarQubeEnv("SonarQube") {
             sh "${tool("SonarQube")}/bin/sonar-scanner \
              -Dsonar.sources=. \
              -Dsonar.css.node=. \
              -Dsonar.host.url=http://52.53.178.237:9000 \
              -Dsonar.login=7e1575ed01bb6a3bed80600080b93fe58870d424 \
              -Dsonar.projectKey=NPM-CICD-automation" 
            }
        }
    } 
    stage('Package') {
        
        sh "tar -zcvf bundle.tar.gz dist/automationdemo/"
        echo "Packaging the build is completed..."
    }

    stage('Artifacts Creation') {
        fingerprint 'bundle.tar.gz'
        archiveArtifacts 'bundle.tar.gz'
        echo "Artifacts created"
    }
    stage('Code Coverage') {
            jacoco()
    } 
    stage('Stash changes') {
        stash allowEmpty: true, includes: 'bundle.tar.gz', name: 'buildArtifacts'
    }
}
