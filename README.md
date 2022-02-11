#CICD with Jenkins and AWS
![My project CICD](https://user-images.githubusercontent.com/98215575/153620770-fe2eecfe-bc06-40f6-8b89-54b6885fa91a.png)

# CICD Jenskins AWS
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
Ensure you are in the ./ssh dr
Enter: `ssh-add ~/.ssh/103a`
cd into the repo created

![git block](https://user-images.githubusercontent.com/98215575/153618194-2e656c35-9a27-4778-9409-ba507505fc68.png)

## Starting with Jenkins
Open Jenkins and enter login details
- Select New Build then enter item name
- Build two items, a test build to see the date and time, anotherr build is the post build, for the first test.

Build 1:
Description: "Testing Jenkins on AWS"
Strategy: Log ratation
Max builds: 3
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
