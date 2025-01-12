- Signup on AWS Management Console
- Launch an EC2 instance
- Create a secret key 
- Locate the folder where the key is and open cmd
  - chmod 400 secret.pem [.pem is a digital signature for accesing the system]
  - ssh -i "secret.pem" ubuntu@ec2-43-204-96-49.ap-south-1.compute.amazonaws.com
- Install nvm [check nodejs website] then install node [version should be acc to the code]
- Clone the repo using [git clone <link>]

- Frontend installation process 
    - npm install
    - npm run build
    - sudo apt update [sudo is used to get root level access]
    - sudo apt install nginx
    - sudo systemctl start nginx
    - sudo systemctl enable nginx
    - sudo scp -r dist/* /var/www/html/ [this copies the files from dist to var/www/html, -r is recurssive which is it copies every file, scp is copy cmd]
    - Enable port :80 of your instance (AWS console)

- Backend installation process
    - Allow EC2 instance public IP on mongodb server
    - npm intsall pm2 -g [pm is a process manager to keep the server running in the background so its mandatory]
    - pm2 start npm --name "canBeAnything" -- start
    - pm2 logs [console logs and error, normal terminal]
    - pm2 list, pm2 stop <name>, pm2 delete <name>
    - config nginx - /etc/nginx/sites-available/default [give port details, checkc ngnix config below]
    - restart nginx - sudo systemctl restart nginx
    - Modify the BASEURL in frontend project to "/api" [in .env]

- Ngxinx config:
    ```
    server_name 43.204.96.49;

    location /api/ {
        proxy_pass http://localhost:7777/;  # Pass the request to the Node.js app
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
    ```

- Addding a custom Domain name
    - purchase domain name
    - signup on cloudflare & add a new domain name
    - change the nameservers on godaddy and point it to cloudflare
    - wait for sometime till your nameservers are updated ~15 minutes
    - DNS record: A devtinder.in 43.204.96.49
    - Enable SSL for website 

- Sending Emails via SES
    - Create a IAM user
    - Give Access to AmazonSESFullAccess
    - Amazon SES: Create an Identity
    - Verify your domain name
    - Verify an email address identity
    - Install AWS SDK - v3 
    - Code Example https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/ses#code-examples
    - Setup SesClient
    - Access Credentials should be created in IAm under SecurityCredentials Tab
    - Add the credentials to the env file
    - Write code for SESClient
    - Write code for Sending email address
    - Make the email dynamic by passing more params to the run function
