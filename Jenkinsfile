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
    stage('Source Code Analysis') {
        nodejs('nodejs') {
             sh 'npm install -D sonarqube-scanner'
            echo "Sonar - Source Code Analysis is completed..."
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
