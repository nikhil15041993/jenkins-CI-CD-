### 1. Simply download one of the binaries for your system:

```
# Linux x86-64
sudo curl -L --output /usr/local/bin/gitlab-runner "https://gitlab-runner-downloads.s3.amazonaws.com/latest/binaries/gitlab-runner-linux-amd64"
```


### 2. Give it permissions to execute:

```
sudo chmod +x /usr/local/bin/gitlab-runner
```


### 3. Create a GitLab CI user:

```
sudo useradd --comment 'GitLab Runner' --create-home gitlab-runner --shell /bin/bash
```


### 4. Install and run as service:

```
sudo gitlab-runner install --user=gitlab-runner --working-directory=/home/gitlab-runner
sudo gitlab-runner start
```



### 5. To register a runner under Linux:

Run the following command:

```
sudo gitlab-runner register
```



if you are facing error " ERROR: error during connect: Get http://docker:2375/v1.40/info: dial tcp: lookup docker on 192.168.65.1:53: no such host"

update /etc/gitlab-runner/config.toml


```
[[runners]]
  ...
  [runners.docker]
    privileged = true
    volumes = ["/cache","/var/run/docker.sock:/var/run/docker.sock"]
    
```    
    
then run the code.
    
