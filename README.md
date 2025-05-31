# Ansible EC2 User Management Project

This project demonstrates automated user creation on AWS EC2 instances using Ansible.

## Project Structure
-Create a VPC in AWS
*Name it Project VPN and give it a 10.0.0.0/16 CIDR Range a
-Create 2 Subnets
*Name them Project 1 and 2 Subnets and give 1 a 10.0.1.0/24 CIDR Range and the other a 10.0.2.0/24 CIDR Range.  Set one for AZ 1A and the other for AZ 1B of your Region
*Click on Project 1 Subnet and then the Actions tab.  Go to Edit subnet settings and check the Enable auto-assign public IPV4 address and save.  Do the same for Project Subnet 2
-Create a Internet Gatway
*Name it Project IGW and attach the Project VPN to it
-Create 1 Route Table
*Name it Project RT.  Go to the Route Table and click on the Subnet association tab and click Edit Subnet Associations
*Associate Project 1 and Project 2 Subnets and save
*Go to the Routes tab and edit routes. Create a new route as 0.0.0.0/0 and set the target as the Internet Gateway created (Project IGW)
-Create 2 EC2 Instances
*Name it Ansible-Sever-1 and choose the free tier Ubuntu Server and free tier and free tier t2.micro instance type
*Create a keypair, name and save it
*Click on Edit for Network settings and choose the Project VPC and Project 1 Subnet under AZ 1A.  Create a new Security Group and set it as SSH with the source type being My IP and create the Instance
*Create another instance and do the same steps for Project 2's Subnet but name the server Ansible-Server-2 using Project 2 AZ 1B under network settings and create another keypair and name it and create the instance

## Adding Ansible
While the instances are spinning up, if you're on a Mac, download Homebrew using this command:
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

Once downloaded you'll need to run these two commands to set Homebrew's path
(echo; echo 'eval "$(/opt/homebrew/bin/brew shellenv)"') >> ~/.zprofile
    eval "$(/opt/homebrew/bin/brew shellenv)"

Now you'll want to install Python with this command
brew install python
Now install Anisble
brew update; brew install ansible;

Now that Ansible is installed, check it's version with 
ansible --version

## Visual Code Studio Setup
-First, create a new GitHub Repository and name it.  You can make it public if you wish, but you don't have to.
*Take the repository clone information and in your local terminal, run the code to clone the repository to your local machine
git clone https://github.com/user/repository.git

-Now open Visual Code Studio
*Open the folder of your GitHub Repository
*Create your first file named "create_user.yml" See the code in the file
*Use the code given but change the details to your specifics
*Create your second your second file named "hosts" and place in your specific EC2 Instance information as well as paths to your keypairs. See the code in the file

-Open 2 Terminals on your mac
*In the first one, run the commands to SSH into your EC2 Instance making sure to chmod your pem files for connection
*In the second one, run the commands to SSH into your other EC2 Instance

-Once connected head back over to Visual Studio Code
Run this command to ping each Ansible Server
ansible -i hosts all -m ping

You should see this response if correct:
Ansi1 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
Ans2 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}

Then do this command to push the Ansible Playbook
ansible-playbook -i hosts create_user.yml

To verify that the Playbook worked, check it with this code
ansible -i hosts all -a "id Nightcrawler"

If the response came back showing Tasks and Play Recap, you've successfully ran an Ansible Playbook using AWS, Ansible and Visual Code Studio

