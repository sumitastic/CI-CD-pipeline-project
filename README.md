# CI-CD-pipeline-project
# In your local machine
mkdir SimpleHTMLProject
cd SimpleHTMLProject
echo "<html><body><h1>Hello, CI/CD!</h1></body></html>" > index.html
git init
git add .
git commit -m "Initial commit"
# Create a GitHub repository and push your code
# Follow the commands provided after creating the repository on GitHub
# On your AWS EC2 instance or local Linux machine
sudo apt-get update
sudo apt-get install nginx

# check_commits.py
import requests

def check_commits(username, repo):
    url = f'https://api.github.com/repos/{username}/{repo}/commits'
    response = requests.get(url)
    return len(response.json())

if __name__ == "__main__":
    username = "your-github-username"
    repo = "your-repo-name"
    print(f"Number of commits: {check_commits(username, repo)}")
# deploy.sh
#!/bin/bash

# Replace with your GitHub repo URL
REPO_URL="https://github.com/your-github-username/your-repo-name.git"
WEB_DIR="/var/www/html"

# Clone the latest code
git clone $REPO_URL $WEB_DIR

# Restart Nginx
sudo systemctl restart nginx
*/30 * * * * /usr/bin/python3 /path/to/check_commits.py
