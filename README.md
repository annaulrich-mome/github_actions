# Basic GitHub Actions template repository
- Builds Docker image(s) automatically upon pushing to main branch
- Tests some (or all) workflows in the new Docker container
- Stops and deletes old Docker image(s) if workflows pass

## Prerequisites

### Add SSH keys

#### SSH keys for GitHub Authentication
https://docs.github.com/en/authentication/connecting-to-github-with-ssh
Helpful video: https://www.youtube.com/watch?v=aHcflUMfCp8
#### SSH keys for deployment
~~~
ssh-keygen -t ed25519 -C "https://github.com/gituser/repo_name.git"
~~~
Add public key as a 'Deploy Key' to GitHub repo triggering the action and private key as a secret to GitHub repo triggering the action.
See: https://github.com/webfactory/ssh-agent?tab=readme-ov-file#usage

### Set up runner server
Since the IP of the VM is private, we need to set up a self-hosted runner for GitHub Actions.
Follow the steps in: https://docs.github.com/en/actions/how-tos/hosting-your-own-runners/managing-self-hosted-runners/adding-self-hosted-runners
#### Configure the runner application as a service
This is needed to automatically start the runner application when the machine starts.

## Basic file structure
- .github/workflows/deploy-mysql.yml: contains the instructions for GitHub Actions
- docker-compose.yml: contains the instructions for the Docker service(s), in this case only the mySQL image
- init.sql: contains the SQL instructions for initializing the database. 

## Notes for debugging
- If sudo has to be used to run commands on the VM, the following like must be added in the VM's sudoers file to allow execution without entering the password:
    ~~~
    sudo visudo
    ~~~
    ~~~
    # Add this line at the end (replace youruser with the actual username):
    youruser ALL=(ALL) NOPASSWD:ALL
    ~~~