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
        
        # Kill old PM2 app
       sudo pm2 delete nextjs-app 2>/dev/null || true
        sleep 2
        
        # Start app with flags passed to PM2
        cd ${appDir}
        sudo pm2 start npm --name "nextjs-app" -- start -- -H 0.0.0.0 -p 3000
        pm2 save
        
        # Verify
        sleep 3
        pm2 list
       


        """
    }
}