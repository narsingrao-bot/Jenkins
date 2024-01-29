
## Installation on GCP Instance

Install Jenkins, configure Docker as agent, set up cicd, deploy applications to GKE Cluster and much more.

## GCP Compute Engine Instance

## In this Project I have use the Google Cloud Compute Engine Instance

- Go to GCP Console
- Instances(running)
- Launch instances


<img width="994" alt="Screenshot 2023-02-01 at 12 37 45 PM" src="https://github.com/narsingrao-bot/Jenkins/assets/59975179/d942b545-e084-4ee9-b6a8-fd74fa7bd859">

### Install Jenkins.

Pre-Requisites:
 - Java (JDK)

### Run the below commands to install Java and Jenkins

Install Java

```
sudo apt update
sudo apt install openjdk-17-jre
```

Verify Java is Installed

```
java -version
```

Now, you can proceed with installing Jenkins

```
curl -fsSL https://pkg.jenkins.io/debian/jenkins.io-2023.key | sudo tee \
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update
sudo apt-get install jenkins
```

**Note: ** By default, Jenkins will not be accessible to the external world due to the inbound firewall traffic restriction by GCP. Open port 8080 in the inbound traffic rules as show below.

- GCP > Instances > Click on <Drop-Down> then Click on <VPC Network>
- In the DropDown tabs -> Click on Firewall
- Create Firewall Rule
- Add Firewall traffic rules as shown in the image (you can just allow TCP 8080 as well, in my case, I allowed `All traffic`).

  https://github.com/narsingrao-bot/Jenkins/assets/59975179/98b6c4e0-5f89-4c95-8469-80bbf3a8dac2


<img width="1187" alt="Screenshot 2023-02-01 at 12 42 01 PM" src="https://github.com/narsingrao-bot/Jenkins/assets/59975179/98b6c4e0-5f89-4c95-8469-80bbf3a8dac2">


### Login to Jenkins using the below URL:

http://<gcp-instance-public-ip-address>:8080    [You can get the GCP-instance-public-ip-address from your Google Cloud==console page]

Note: If you are not interested in allowing `All Traffic` to your gcp compute-engine instance
      1. Delete the Firewall traffic rule for your instance
      2. Edit the Firewall traffic rule to only allow custom TCP port `8080`
  
After you login to Jenkins, 
      - Run the command to copy the Jenkins Admin Password - `sudo cat /var/lib/jenkins/secrets/initialAdminPassword`
      - Enter the Administrator password
      
<img width="1291" alt="Screenshot 2023-02-01 at 10 56 25 AM" src="https://user-images.githubusercontent.com/43399466/215959008-3ebca431-1f14-4d81-9f12-6bb232bfbee3.png">

### Click on Install suggested plugins

<img width="1291" alt="Screenshot 2023-02-01 at 10 58 40 AM" src="https://user-images.githubusercontent.com/43399466/215959294-047eadef-7e64-4795-bd3b-b1efb0375988.png">

Wait for the Jenkins to Install suggested plugins

<img width="1291" alt="Screenshot 2023-02-01 at 10 59 31 AM" src="https://user-images.githubusercontent.com/43399466/215959398-344b5721-28ec-47a5-8908-b698e435608d.png">

Create First Admin User or Skip the step [If you want to use this Jenkins instance for future use-cases as well, better to create admin user]

<img width="990" alt="Screenshot 2023-02-01 at 11 02 09 AM" src="https://user-images.githubusercontent.com/43399466/215959757-403246c8-e739-4103-9265-6bdab418013e.png">

Jenkins Installation is Successful. You can now starting using the Jenkins 

<img width="990" alt="Screenshot 2023-02-01 at 11 14 13 AM" src="https://user-images.githubusercontent.com/43399466/215961440-3f13f82b-61a2-4117-88bc-0da265a67fa7.png">

## Install the Docker Pipeline plugin in Jenkins:

   - Log in to Jenkins.
   - Go to Manage Jenkins > Manage Plugins.
   - In the Available tab, search for "Docker Pipeline".
   - Select the plugin and click the Install button.
   - Restart Jenkins after the plugin is installed.
   
<img width="1392" alt="Screenshot 2023-02-01 at 12 17 02 PM" src="https://user-images.githubusercontent.com/43399466/215973898-7c366525-15db-4876-bd71-49522ecb267d.png">

Wait for the Jenkins to be restarted.


## Docker Slave Configuration

Run the below command to Install Docker

```
sudo apt update
sudo apt install docker.io
```
 
### Grant Jenkins user and Ubuntu user permission to docker deamon.

```
sudo usermod -aG docker jenkins
sudo usermod -aG docker ubuntu
sudo systemctl restart docker
```

Once you are done with the above steps, it is better to restart Jenkins.

```
http://<gcp-instance-public-ip>:8080/restart
```

The docker agent configuration is now successful.




