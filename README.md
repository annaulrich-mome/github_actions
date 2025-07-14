# Basic GitHub Actions template repository
- Builds Docker image(s) automatically upon pushing to main branch
- Tests some (or all) workflows in the new Docker container
- Stops and deletes old Docker image(s) if workflows pass

## Prerequisites
### Add SSH keys
#### SSH keys for GitHub Authentication
github ssh: https://codewithsusan.com/notes/ssh-keys-and-github

#### SSH keys for deployment
~~~
ssh-keygen -t ed25519 -C "https://github.com/gituser/repo_name.git"
~~~
Add public key as a 'Deploy Key' to GitHub repo triggering the action and private key as a secret to GitHub repo triggering the action.
See: https://github.com/webfactory/ssh-agent?tab=readme-ov-file#usage

### Set up runner server
Since the IP of the VM is private, we need to set up a self-hosted runner for GitHub Actions.
See: https://docs.github.com/en/actions/how-tos/hosting-your-own-runners/managing-self-hosted-runners/adding-self-hosted-runners

## Basic file structure
- .github/workflows/deploy-mysql.yml: contains the instructions for GitHub Actions
- docker-compose.yml: contains the instructions for the Docker service(s), in this case only the mySQL image
- init.sql: contains the SQL instructions for initializing the database. 

