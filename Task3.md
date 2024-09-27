# Creating CI / CD using Github Actions
1- Installing Nodejs, npm packages

    sudo apt-get update
    sudo apt-get install -y ca-certificates curl gnupg
    sudo mkdir -p /etc/apt/keyrings
    curl -fsSL https://deb.nodesource.com/gpgkey/nodesource-repo.gpg.key | sudo gpg --dearmor -o /etc/apt/keyrings/nodesource.gpg
    NODE_MAJOR=20
    echo "deb [signed-by=/etc/apt/keyrings/nodesource.gpg] https://deb.nodesource.com/node_$NODE_MAJOR.x nodistro main" | sudo tee /etc/apt/sources.list.d/nodesource.list
    sudo apt-get update
    sudo apt-get install nodejs -y
    
# configuring Github Actions
1- Go to setting -> actions -> runners -> new self-hosted-runner

![Capture31](https://github.com/user-attachments/assets/411e796a-154b-4457-8cb2-59417af80bdd)

2- Run provided commands on server

![Capture32](https://github.com/user-attachments/assets/ad67218a-5493-4140-a687-36ae5e9aa7f5)

    sudo ./svc.sh install
    sudo ./svc.sh start

3- GitHub Repository Settings:

  * You define these secrets in your GitHub repository settings under Settings > Secrets and variables > Actions.

  * In that section, you can add secrets by providing a name and value. For example:

  * Secret Name: AWS_ACCESS_KEY_ID

  * Secret Value: The actual AWS Access Key ID.

4- Create file and paste .github>workflows>main.yaml


```http
name: s3-depl
```
     push:
       branches: 
         - main
         
```htto         
   jobs:
     build:
       runs-on: self-hosted
       steps:
        - uses: actions/checkout@v3
```
      
        - name: Configure AWS Credentials
          uses: aws-actions/configure-aws-credentials@v2
          with:
            aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
            aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
            aws-region: us-east-1
          
        - name: Backend package install and restart
          run: |
            cd backend
            pwd
            npm install
            pm2 restart index
            cd ..
          
        - name: Build React App
          run: |
            cd frontend
            npm install
            npm run build
            ls
            pwd
        
        - name: Check Directory and Path
          run: |
            pwd
            ls
        
        - name: Navigate to Frontend
          run: |
            cd frontend
            ls
            pwd
        
        - name: Deploy app build to S3 bucket
          run: |
            aws s3 sync /home/ubuntu/actions-runner/_work/React-NodeAPI-MySQL/React-NodeAPI-MySQL/frontend/dist s3://myweb.com.cm --delete


4- Create bucket and make sure that you put the same name in last command of .yaml file

5- Commit in repo and push

6- you may face error for backend or pm2 error, its solution is you will see _work folder in actions-runner folder

     cd _work/React-NodeAPI-MySQL/React-NodeAPI-MySQL/backend
     npm i
     pm2 start index.js

7- After connecting with database
     
![Capture21](https://github.com/user-attachments/assets/734801ac-6e41-4abd-9cd2-6e23df033f46)
     
