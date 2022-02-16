
# CICD Jenskins and AWS


![My project CICD](https://user-images.githubusercontent.com/98215575/153620770-fe2eecfe-bc06-40f6-8b89-54b6885fa91a.png)

## Linking localhost and Github

- On the terminal
- cd into `~/.ssh`
- Enter the command `ssh-keygen -t rsa -b 4096 -C "email@email.com"`
- The follwing instructions will appear:
   - Enter file in which to save the key : `103a`
   - Enter again: `<empty>`
   - Enter passphrase again: `<empty>`
   - The key fingerprint is: `<private>`
   - The key's randomart image is `<image>`
  
On github:
- Go to your profile > settings > SSH and GPG keys
- Go to `SSH Keys / Add new`
- Save key 

Image:
Creating a new repo for CICD on Github
- Select SSH after creating it and follow the commands below
Image:  
- On the terminal create a new directory adn add a README.md



![SSH github new keys](https://user-images.githubusercontent.com/98215575/153615499-8041b59d-ae31-4d0a-8f6d-3dfd38d1e782.png)

Creating a new repo for CICD on Github
- Select and copy SSH after creating it
- 
![SSH for cicd jenkins aws - edited](https://user-images.githubusercontent.com/98215575/153615927-e67a8357-ee39-4963-92f7-ebc731e8e616.png)

- Return to the terminal create a new directory, cd into the made directory and add a README.md (cat README.MD)
- Add relevant text and then save
Follow the commmands below:

![commands for terminal](https://user-images.githubusercontent.com/98215575/153617093-9e0bd71c-1c1f-4cad-b577-08c3057c5604.png)

After entering the commands you may get an error,stopping you from pusing the README.
Ensure you are in the`./ssh dr`
Enter: `eval "$(ssh-agent -s)"`
Enter: `ssh-add ~/.ssh/103a`
cd into the repo created

![git block](https://user-images.githubusercontent.com/98215575/153618194-2e656c35-9a27-4778-9409-ba507505fc68.png)



## Starting with Jenkins
Open Jenkins and enter login details
- Select New Build then enter item name
- Build two items, a test build to see the date and time, anotherr build is the post build, for the first test.

Build 1:
- Description: "Testing Jenkins on AWS"
- Strategy: Log ratation
- Max builds: 3
Scroll to the bottom, until build is seen.
Execute shell
enter commands:
`uname -a`
`date`

Build test 2
- Description: "This job will be triggered automatically if the last job was successful"
- Scroll to build 
Execute shell
- Enter commmands: `ps aux`
- Check the build time and date
- Select `console output`
- A separate page saying the build was a success should appear
- Select test 2
- Select console output again
- `ps aux` command would have successfully executed.

## Jenkins
Building CI job with GitHub
- Enter local host and generate a new key using `ssh-keygen -t ed25519 -C "yourgitemail@yourgitemail.com"` (follwoing the steps above)
- Name the key appropriately to avoid confusion with other keys.
- A public and private key will be generated.

Adding key to Github

- On your terminal `cat KEYNAME.pub`
- Copy key exactly without missing any content.
- Create a new repor or in an exsisting one complete the following:
- Click on Settings > Deploy keys > Add deploy key > Use the same name as key created and paste the public key.

Connect to Jenkins

- Log in
- Create a `new item`
- Enter a the name follwing the previous naming convetion 
- Select `Freestyle Project`
- Click `Discard old builds` and set the Max number of builds as 3
- Select `GitHub project` and enter the repos HTTPS. (Make sure the link is HTTP)
- Under `Office 365 Connector` select Restrict where this project can be run and type sparta-ubuntu-node
- Scroll to `Source Code Management` and select git
- In `Repository URL` paste your repos SSH link
- In the`Credentials` section: 
- `add` > `jenkins` > `SSH Username with private key` > `Username: KEYNAME` > `Private Key`
- Select `Enter directly` > `Add` and paste the PRIVATE key created on the terminal.
- `Branches build` enter `main`
- `Build Triggers` select `GitHub hook trigger for GITScm polling`

Creating a Webhook

- Go to your repo
- Select `settings` > `Webhooks` > `Add webhook`
- Next to `Payload URL` enter your IP from Jenkins but ending with `/github-webhook/`
- Under `Content type` drop down mendu select `application/json`
- `Which events would you like to trigger this webhook?` Select `Send me everything`
Blocker: Webhook did not create itself on first attempt. Spelling was correct but the webhook did not launch. Try multiple times and it should work.
Return to Jenkins and edit our job:

- Select task > `Configure`
- Change Branches to build from main to dev
- Under `Build Environment` select `Provide Node & npm bin/folder to PATH`

A second job must be created to merge branches.
- Create a new item
- Name it appropriately 
- Click `Discard old builds` and set the Max number of builds as 3
- Follow the steps used to create the first task but with the following exceptions:
- `Branches to build */dev` not main
- Under `Build` select `Execute shell`
- Enter:

```
git checkout main
git pull origin main
git merge origin/dev
git push origin main

```
- Apply and save
- Return to the first task made > select `Configure`
- Scroll to `Post-build Actions` and select the second task made
- Select `Trigger only if build is stable`

Creating EC2 instance using Jenkins

- Create a third task
- Follow the steps used to create the first task but with the following exceptions:
- Enter under build `scp -o "StrictHostKeyChecking=no" -r app ubuntu@<AWSIP>:~`

In AWS

- Create an EC2 instance
- Use the subnet and VPC you have created before.
- Use the same secuirty group for our app but add another rule
- Add `Type`: `ssh`
- `Port`: `22`
- `IP` : `custom`
- Enter Jenkins IP
- Follwing the steps before Launch the instance.

Installing dependencies

- ssh into local host
- run the commands in our provision file

```
sudo apt-get update -y
sudo apt-get upgrade -y
sudo apt-get install nginx -y
sudo apt-get install nodejs -y
sudo apt install python-software-properties -y
curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash -
sudo apt-get install nodejs -y
sudo apt-get install npm
sudo npm install pm2 -g
```
- cd app
- `npm install`
- `npm start`
- App should be viewable on browser

Blocker:
App did not appear on first attempt. Reboot instance and the app should appear on browser.

## Creating Server and Launching Master and Agent nodes

Step 1) Spin up an EC2 Instance, that allows all to ssh in
Step 2) Install Java onto EC2 instance
Step 3) Install Jenkins
Step 4) Start Jenkins and install plugins
Step 5) Connect github to jenkins using priv key
Step 6) Set webhook to new jenkins ip
Step 7) Return to Jenkins and ensure `node.js version 13.30`is used
Step 8) Spin up another ec2 instance called agent and one using your app AMi.
Step 9) Create a job that obtains new code on the main  branch, one that merges code from main to dev, one that deploys new changes to aws



