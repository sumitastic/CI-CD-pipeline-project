mkdir html-project
cd html-project
echo "<html><head><title>CI/CD Example</title></head><body><h1>Hello, CI/CD!</h1></body></html>" > index.html
#Python Script to Check for New Commits
import requests
import subprocess

def check_commits():
    repo_url = "<your-github-repo-url>"
    response = requests.get(f"{repo_url}/commits")
    commits = response.json()

    latest_commit_sha = commits[0]['sha']

    with open('.latest_commit', 'r') as f:
        stored_commit_sha = f.read().strip()

    if latest_commit_sha != stored_commit_sha:
        subprocess.run(['bash', 'deploy.sh'])
        with open('.latest_commit', 'w') as f:
            f.write(latest_commit_sha)

if __name__ == "__main__":
    check_commits()
# Bash Script to Deploy the Code
#!/bin/bash

# Clone the latest code from GitHub
git pull origin master

# Restart Nginx (replace with your Nginx restart command)
sudo service nginx restart
chmod +x deploy.sh
#Set Up a Cron Job to Run the Python Script
#Edit the crontab
crontab -e
#Add the following line to run the script every 10 minutes (adjust as needed)
*/10 * * * * /path/to/python3 /path/to/check_commits.py
#Test the Setup
echo "<p>New commit content</p>" >> index.html
git add .
git commit -m "Test commit"
git push origin master
#cron logs to ensure the Python script is executed
grep CRON /var/log/syslog


