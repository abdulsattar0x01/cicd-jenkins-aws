node{

def appDir = '/var/www/nextjs-app'



stage('Clean Workspace') {
echo 'Cleaning workspace...'
deleteDir()
} 

stage('Clone Repository') {
    echo 'Cloning repository...'
    git(
        branch: 'main',
        url: 'https://github.com/abdulsattar0x01/cicd-jenkins-aws.git',
        
    )
}

    stage('Deploy to Ec2') {
        echo 'Deploying to EC2...'
        sh """
        sudo mkdir -p ${appDir}

        sudo chown -R jenkins:jenkins ${appDir}

        rsync -av --delete --exclude='.git' --exclude='node_modules' ./ ${appDir}/

        cd ${appDir}

        npm install 
        npm run build 
    

   # Start or restart app via PM2
        pm2 restart nextjs-app || pm2 start npm --name "nextjs-app" -- run start -- -H 0.0.0.0 -p 3000
    pm2 save
        """
        
    }
}
