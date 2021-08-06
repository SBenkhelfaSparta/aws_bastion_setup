# Creating a Bastion Host for your App
### All the Steps:
- Create a VPC. In my case I chose the IP 10.208.0.0/16
- Name the route table that was created
- Create an internet gateway and attach it to the VPC
- Name the NACL created and create 3 NACLs for the private, public and bastion instances with custom inbound and outbound rules
- Go to the Subnets page and create a public, private and bastion subnet with mask /24. Link them with the NACLs
- Go back to the route table and add the internet gateway to the routes and link the public and bastion subnets to explicit associations
- Create security groups for all 3:
   - app will have ssh, http and open port 3000
   - db will have ssh and port 27017 open. The inbound rule source should point to the security gates of the public and bastion ones instead of manually typing in the IPs that can change
   - bastion will have ssh
- Make instances for all 3. When making the db make sure to disable the public ip

- Use this line to copy the key to the bastion server:
`scp -i eng89_devops.pem eng89_devops.pem ubuntu@3.250.2.82:~/`
- Don't forget to give the key the required permissions by running `chmod 400 eng89_devops.pem`

- ssh into the bastion then into the db and run "sudo systemctl status mongod" to make sure MongoDB is running
- Exit twice and ssh into the app. Make sure the ip of `MONGODB` in the environment variables is the correct ip
- Head into the app folder and run "pm2 start app.js"
