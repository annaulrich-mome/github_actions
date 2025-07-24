# Basic GitHub Actions template repository
- Builds Docker images in STAGING environment automatically upon pushing to main branch
- Tests some (or all) workflows in the new Docker containers
- Stops and deletes old Docker containers if workflows pass

## Prerequisites

### Add secrets and other variables to the GitHub repo

#### Add SSH keys
##### SSH keys for GitHub Authentication
Open VM Terminal
Generate key-pair:
~~~
ssh-keygen -t ed25519 -C "your_email@example.com"
# Add key to SSH agent
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
~~~
Add the public key to the GitHub user's profile:
~~~
cat ~/.ssh/id_ed25519.pub
~~~
On GitHub, go to your user's "Settings" > "SSH and GPG keys" > "New SSH key" > "Add SSH key"

Test your VM's connection to GitHub:
~~~
ssh -T git@github.com
~~~
What you should see:
~~~
Hi <git_user>! You've successfully authenticated, but GitHub does not provide shell access.
~~~

Helpful resources:
https://docs.github.com/en/authentication/connecting-to-github-with-ssh
https://www.youtube.com/watch?v=aHcflUMfCp8

##### SSH keys for the GitHub runner - VM connection
Name the key differently (-f) from the GitHub Authentication key
~~~
# Add this line at the end (replace youruser with the actual username):
ssh-keygen -f ~/.ssh/github_runner_key -t ed25519
~~~
Add public key as a 'Deploy Key' to GitHub repo triggering the action and private key as a secret to GitHub repo triggering the action.
See: https://github.com/webfactory/ssh-agent?tab=readme-ov-file#usage
#### Add other variables
On GitHub, go to the repo's "Settings" > "Secrets and variables" > "Actions" > "New repository secret" and add:
- VM_HOST: IP address of VM
- VM_USER: username on the VM


### Set up runner server
Since the IP of the VM is private, we need to set up a self-hosted runner.
Follow instructions: https://docs.github.com/en/actions/how-tos/hosting-your-own-runners/managing-self-hosted-runners/adding-self-hosted-runners
#### Configure the runner application as a service
This is needed to keep the runner application running on the VM.
Follow instructions: https://docs.github.com/en/actions/how-tos/hosting-your-own-runners/managing-self-hosted-runners/configuring-the-self-hosted-runner-application-as-a-service

## Basic file structure
- .github/workflows/deploy-docker.yml: contains the instructions for GitHub Actions
- docker-compose.stage.yml: contains the instructions for the Docker service(s)
- init.sql: contains SQL instructions for initializing the database for the mysql service 

## Notes for debugging
- If sudo has to be used to run commands on the VM, the following must be added to the VM's sudoers file to allow execution without entering the password:
    ~~~
    sudo visudo
    ~~~
    ~~~
    # Add this line at the end (replace youruser with the actual username):
    youruser ALL=(ALL) NOPASSWD:ALL
    ~~~