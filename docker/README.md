# LINUX Install instruction:
# Download the binary for your system
sudo curl -L --output /usr/local/bin/gitlab-runner https://gitlab-runner-downloads.s3.amazonaws.com/latest/binaries/gitlab-runner-linux-amd64

# Give it permission to execute
sudo chmod +x /usr/local/bin/gitlab-runner

# Create a GitLab Runner user
sudo useradd --comment 'GitLab Runner' --create-home gitlab-runner --shell /bin/bash

# Install and run as a service
sudo gitlab-runner install --user=gitlab-runner --working-directory=/home/gitlab-runner
sudo gitlab-runner start

# After above step, you will have an almost empty /etc/gitlab-runner/config.toml file

# Set proxy if gitlab runner talks to gitlab via proxy
```
export NO_PROXY="127.0.0.0/8,.otxlab.net,10.0.0.0/8"
export HTTP_PROXY=http://bp2-prox01-l001.otxlab.net:3128
export HTTPS_PROXY=http://bp2-prox01-l001.otxlab.net:3128
```

# Registration
sudo gitlab-runner register --url https://gitlab.otxlab.net/ --registration-token $REGISTRATION_TOKEN
this REGISTRATION_TOKEN comes from project or project group

# After above step, /etc/gitlab-runner/config.toml will be populated and ready to go unless you want tweak something like docker pull policy




