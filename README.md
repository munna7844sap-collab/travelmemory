# travelmemory
# Travel Memory

`.env` file to work with the backend after creating a database in mongodb: 

```
MONGO_URI='https://cloud.mongodb.com/v2/6a4a8d192d2ab80d09f3cd65#/overview'
PORT=3000
Step 1 Create AWS Infrastructure

# Create

# VPC
# Public Subnet
# Internet Gateway
# Route Table
# Security Group
# EC2 Amazon Linux 2023
Security Group

# Port	Purpose
# 22	SSH
# 80	HTTP
# 443	HTTPS
# 3000	Node Backend (optional)
# 5000	React (optional)

Launch two EC2 instances.

Example

travel-backend-1

travel-backend-2

<img width="1099" height="201" alt="image" src="https://github.com/user-attachments/assets/cdac86b1-a6b9-421d-8e5e-0e4eb463094b" />









<img width="1084" height="371" alt="image" src="https://github.com/user-attachments/assets/fffeccd2-aa0d-4a69-80d7-e74e11d45fbf" />








<img width="1069" height="283" alt="image" src="https://github.com/user-attachments/assets/6d543a94-0a6a-4b02-b576-7cadc187cfcb" />


Step 2 Connect EC2
ssh -i travel.pem ec2-user@PUBLIC-IP

<img width="779" height="344" alt="image" src="https://github.com/user-attachments/assets/2fa1b816-6d79-4cdc-af88-3c6439b432d1" />


Update packages

sudo dnf update -y


<img width="560" height="110" alt="image" src="https://github.com/user-attachments/assets/9329836f-2123-4863-ad01-1c88862a9246" />



# Step 3 Install Git
  sudo dnf install git -y

    Verify

    git --version

    <img width="1363" height="259" alt="image" src="https://github.com/user-attachments/assets/80d3b21e-c07c-4148-8eba-476e2eebfc03" />


    <img width="501" height="70" alt="image" src="https://github.com/user-attachments/assets/6e2e9d26-eca4-4c91-a869-0dabf031599d" />


    Step 4 Install NodeJS
      sudo dnf install nodejs -y

      verify

    node -v

     npm -v


     <img width="1357" height="415" alt="image" src="https://github.com/user-attachments/assets/87ed675e-e915-4769-97ab-39fd16f2b5dd" />

     <img width="1342" height="411" alt="image" src="https://github.com/user-attachments/assets/fdd605da-ead4-4215-ba89-c88cd4c1e863" />


     <img width="401" height="100" alt="image" src="https://github.com/user-attachments/assets/9fb7a0bb-058c-4086-ae89-2f1532a86c8c" />

     #Step 5 Install Nginx
      sudo dnf install nginx -y

      sudo systemctl enable nginx

     sudo systemctl start nginx

        check

       systemctl status nginx

       <img width="1355" height="357" alt="image" src="https://github.com/user-attachments/assets/20dd0e0d-7822-4791-b81d-185383907d86" />

       <img width="1346" height="464" alt="image" src="https://github.com/user-attachments/assets/fbbba8d6-090d-4ea0-854a-130a979150cd" />

       <img width="1069" height="61" alt="image" src="https://github.com/user-attachments/assets/7f1525d4-fb92-41c0-8f72-ccf9bde6fc8d" />



       Step 6 Install PM2
       sudo npm install pm2 -g

       Verify

       pm2 -v

       <img width="661" height="214" alt="image" src="https://github.com/user-attachments/assets/4dede38a-9a0d-46d8-abbe-9c2ecc2b4963" />



       <img width="707" height="311" alt="image" src="https://github.com/user-attachments/assets/90dd6ce7-f689-4321-a473-7481d58f65df" />


       <img width="652" height="479" alt="image" src="https://github.com/user-attachments/assets/cd177ad2-24a6-42f4-916e-8926eb139326" />


       # Step 7 Clone Project
git clone https://github.com/UnpredictablePrashant/TravelMemory.git

Go inside

cd TravelMemory

<img width="918" height="168" alt="image" src="https://github.com/user-attachments/assets/e7a9ebd1-ad0d-43f3-93db-1506430cf469" />


<img width="741" height="142" alt="image" src="https://github.com/user-attachments/assets/881b5c82-9b65-4234-aaab-093de643a535" />


Step 8 Configure Backend

Go backend

cd backend

Install

npm install

<img width="713" height="401" alt="image" src="https://github.com/user-attachments/assets/82e73fe8-6f4e-49a1-9d1b-6512d60755e8" />



Create .env
nano .env

Example

PORT=3000

MONGO_URI=mongodb+srv://username:password@cluster.mongodb.net/travelmemory

JWT_SECRET=mysecretkey

NODE_ENV=production

Save

CTRL+O

ENTER

CTRL+X

<img width="593" height="111" alt="image" src="https://github.com/user-attachments/assets/1fe8d9be-e0ff-4c19-897a-902e3a3070a1" />



Start Backend
pm2 start index.js --name backend

Check

pm2 list

Save PM2

pm2 save

<img width="1274" height="185" alt="image" src="https://github.com/user-attachments/assets/cb5a840c-13f7-4aff-b3a2-9b2a4b6e1a26" />


# Step 9 Configure Frontend

Go

cd ../frontend

Install

npm install

Open

src/utils/urls.js

Original

export const API="http://localhost:3000";

Update

export const API="https://travel.example.com/api";


<img width="1346" height="311" alt="image" src="https://github.com/user-attachments/assets/dea74c8d-ad00-46d2-be84-09fb5a9e4072" />



Step 10 Configure Nginx

Open

sudo nano /etc/nginx/nginx.conf

or

sudo nano /etc/nginx/conf.d/travel.conf

Configuration

server {

listen 80;

server_name travel.example.com;

location / {

root /home/ec2-user/TravelMemory/frontend/build;

index index.html;

try_files $uri /index.html;

}

location /api/ {

proxy_pass http://localhost:3000/;

proxy_http_version 1.1;

proxy_set_header Upgrade $http_upgrade;

proxy_set_header Connection 'upgrade';

proxy_set_header Host $host;

proxy_cache_bypass $http_upgrade;

}

}

Test

sudo nginx -t

Restart

sudo systemctl restart nginx


Step 11 Test

Browser

http://PUBLIC-IP

React page should open.

Backend

http://PUBLIC-IP/api


Step 12 Create Multiple Instances

Repeat same deployment

EC2-1

Frontend

Backend

EC2-2

Frontend

Backend


# Step 13 Create Target Group

AWS Console

Target Groups

Create Target Group

Type

Instances

Protocol

HTTP

Port

80

Register

EC2-1

EC2-2

Health Check

/


Step 14 Create Load Balancer

Create

Application Load Balancer

Listener

80

Target Group

TravelMemoryTG

After creation

You'll receive

travel-alb-123456.ap-south-1.elb.amazonaws.com
Step 15 Configure Cloudflare

Domain

travel.example.com

DNS

Create

CNAME
Name

@

Target

travel-alb-123456.ap-south-1.elb.amazonaws.com

Proxy

Enabled

Create A Record

Name

frontend

IP

EC2 Public IP

Result

travel.example.com

↓

Cloudflare

↓

AWS ALB

↓

EC2 1

EC2 2

↓

MongoDB Atlas
Step 16 Verify

Open

https://travel.example.com

Refresh multiple times.

Requests should rotate between servers.

Step 17 PM2 Commands

Start

pm2 start index.js

Restart

pm2 restart backend

Logs

pm2 logs

Delete

pm2 delete backend

List

pm2 list
Step 18 Nginx Commands

Start

sudo systemctl start nginx

Restart

sudo systemctl restart nginx

Status

sudo systemctl status nginx
Step 19 Useful Linux Commands
pwd

ls

cd

mkdir

cp

mv

rm

cat

nano

top

ps

netstat -tulnp

ss -tulnp
Step 20 Deployment Screenshots (Recommended)

Include screenshots for:

AWS EC2 Dashboard
Security Group
Git Clone
npm install
Backend running
PM2 list
Nginx configuration
nginx -t
React Build
MongoDB Atlas
Target Group
Load Balancer
Cloudflare DNS
Browser showing application
GitHub Repository
Vlearn Submission


















    










