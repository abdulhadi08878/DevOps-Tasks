# Deploying frontend on S3, backend on EC2 and configuring database of RDS
# RDS

1- create RDS for mysql in free tier

2- sql configuration and creating database
```http
mysql -h database-1.cl4suwy60mbo.us-east-1.rds.amazonaws.com -u admin -p
```
3- clone sql file in db-sql folder of backend
```http
mysql -h database-1.cl4suwy60mbo.us-east-1.rds.amazonaws.com -u admin -p devops < users.sql
```
# EC2
1- Create Instance and connect with it, make sure security groups are configured correctly for 9001, 5713

2- clone repo
```http
git clone https://github.com/abdulhadi08878/React-NodeAPI-MySQL.git
```
3- Install NodeJs and npm as well as nginx

```http
sudo apt-get update
sudo apt-get install -y ca-certificates curl gnupg
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://deb.nodesource.com/gpgkey/nodesource-repo.gpg.key | sudo gpg --dearmor -o /etc/apt/keyrings/nodesource.gpg
NODE_MAJOR=20
echo "deb [signed-by=/etc/apt/keyrings/nodesource.gpg] https://deb.nodesource.com/node_$NODE_MAJOR.x nodistro main" | sudo tee /etc/apt/sources.list.d/nodesource.list
sudo apt-get update
sudo apt-get install nodejs -y
sudo apt-get install nginx
```
4- set env, Build and upload dist to s3
```http
nano .env
http://server-ip:9001
npm run build
aws s3 sync ./dist s3://blog-ammar --region us-east-1
```
4- update sql connection in index.js file of backend
![Capture20](https://github.com/user-attachments/assets/d11cffc8-5fd6-4b8b-9c6f-0753cb05dda8)

# S3
1- Create S3 bucket and configure permission
```http
{
  "Id": "Policy1725433843681",
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "Stmt1725433841167",
      "Action": "s3:*",
      "Effect": "Allow",
      "Resource": "arn:aws:s3:::blog-ammar/*",
      "Principal": "*"
    }
  ]
}
```
2- Open terminal and configure
```http
sudo apt install unzip curl -y
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
aws configure
```

3- Build frontend and create dist folder
4- copy folder to s3 bucket

```http
 aws s3 sync ./dist s3://abdul-hadi88 --region us-east-1
```
5- Enable Static Website Hosting

 *To serve your website from S3, you need to enable static website hosting:

 *Go to the Amazon S3 console.

 *Select the bucket where you uploaded your frontend build.

 *Go to Properties.

 *Enable Static website hosting and set:

 *Index document: index.html

 *Error document (optional): index.html (for single-page apps)
![Capture25](https://github.com/user-attachments/assets/efbdb369-aeff-403b-b761-1cc3a938a53b)
6- Frontend Deployed on S3
![Capture21](https://github.com/user-attachments/assets/35b1221d-eccb-477b-bb70-48ded0ceabd0)
# Finally Running
![Capture24](https://github.com/user-attachments/assets/9152f75e-0ae8-4d7a-abec-606f30d0e6b8)
