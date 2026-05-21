# dockerize-nodejs-application-on-ec2-with-health-endpoint


    mkdir node-health-app
cd node-health-app
nnpm init -y
nnpm install express
vim index.js
----
const express = require('express');
const app = express();

app.get('/', (req, res) => {
  res.send('App is running');
});

app.get('/health', (req, res) => {
  res.status(200).json({ status: 'OK' });
});

app.listen(3000, () => console.log('Server running on port 3000'));

-------
vim package.json
------edit script tag with your node and index.js file------
"scripts": {
  "start": "node index.js"
}
-------------------------------------------------------
vim Dockerfile
------------
FROM node:18

WORKDIR /app
COPY package*.json ./
RUN npm install

COPY . .

EXPOSE 3000
CMD ["npm", "start"]
-----------------------------------
docker build -t node-health-app .\ndocker run -p 3000:3000 node-health-app
docker ps
----check your app with localhost and port number-----
http://localhost:3000/health
------------------------------------------------------


create Ec2 and install docker and node in that

sudo yum update -y
sudo yum install docker -y
sudo service docker start
sudo usermod -aG docker ec2-user
exit


copy file from you local to ec2
scp -i your-key.pem -r node-health-app ec2-user@YOUR_PUBLIC_IP:~

now loginnto ec2 and build app----------
cd node-health-app
docker build -t node-health-app .
docker run -d -p 3000:3000 node-health-app

check with your instane ip
http://YOUR_PUBLIC_IP:3000/health
