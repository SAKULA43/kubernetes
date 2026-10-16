# kubernetes
kubernetes Training
started node with shortcut node1-connect from desktop

systemctl status docker


docker -v
docker run hello-world

 
Run Hello World Container
 
docker run hello-world
docker ps
docker ps -a
 
Get the container ID and manage the container:
 
docker start <container-id>
docker ps -a
docker logs <container-id>
docker rm <container-id>
docker images
docker image rm hello-world
docker images

 
Working with Ubuntu Containers
1. Pull a Base Image
Choose a base image from Docker Hub.
 
docker pull ubuntu
2. Run a Container
Start a basic container without specifying a command.
 
docker run -itd --name ubuntu-container ubuntu
docker ps -a
docker exec -it ubuntu-container bash
3. Explore the Container
Once inside the container, you can explore its filesystem or execute commands.
 
ls
cat /etc/os-release
4. Exit the Container
To exit the container, type exit.
 
exit
5. View and Manage Containers
Check the list of all containers, including the exited ones.
 
docker ps
docker top ubuntu-container
docker stop <container-id>
docker rm <container-id>



Pull the CentOS image.
docker pull centos:7
 
Create and start a CentOS container.
docker run --name app1 -itd centos:7
 
Access the container's shell.
docker exec -it app1 bash
 
Create a directory named '/data'.
mkdir /data
Create a file named 'file1.txt' inside '/data'.
touch /data/app.txt
ls /data
 
Exit the container.
exit
 
Remove the container.
docker stop app1
docker rm app1
 
Remove the image.
docker image rm centos:7
docker images
 
Hosting a Website with Apache HTTP Server
Objective:
To provide hands-on experience with Docker, hosting a website using the Apache HTTP Server container, modifying the website content inside the container, and utilizing port forwarding.
 
Practice Steps:
Step 1: Pull Apache HTTP Server Container
docker pull httpd:latest
 
Step 2: Run Apache HTTP Server Container
docker run -d -p 8080:80 --name my-apache-container httpd:latest
This command runs the Apache HTTP Server container in detached mode (-d), maps host port 8080 to container port 80 (-p 8080:80), and assigns a name to the container (--name my-apache-container).
Note: check the used port number “ss -tulpen”
Step 3: Verify the Running Container
docker ps
Ensure the Apache HTTP Server container is running.
docker inspect my-apache-container
get the ip
curl container-ip
Step 4: Access the Website
check ip adddress of node 1: ip ad | grep 192.168
Open a web browser and navigate to http://192.168.61.130:8080 to see the default Apache welcome page.
 
Step 5: Get Inside the Container
docker exec -it my-apache-container /bin/bash
Access the container's shell interactively (-it).
 
Step 6: Change Index.html Content
echo "Welcome to My Dockerized Website" > /usr/local/apache2/htdocs/index.html
Modify the content of the index.html file with a unique message.
 
Step 7: Exit the Container
exit
 
Step 8: Refresh the Website
Visit http://192.168.137.128:8080 in your web browser again to see the updated content.
 
 
check used port no on linux system
ss -tulen | grep 8090
<change 8090 with any port>

 

Host a container with image docker.io/pengbai/docker-supermario on
docker run -d --name mario -p 8090:8080 docker.io/pengbai/docker-supermario
docker ps
 
 
open on browser on nodeip:8090 and verify the game is running.
http://192.168.61.130:8090

Task: MySQL Container Configuration
1.	Pull the MySQL image:
docker pull mysql
2.	Run the MySQL container with environment variables:
docker run --name db1 -d -e MYSQL_ROOT_PASSWORD=password123 -e MYSQL_DATABASE=blankdb -e MYSQL_USER=<your-name> -e MYSQL_PASSWORD=<your-password> mysql
Verification
3.	Check if the container is running:
docker ps -a
4.	Verify container logs:
docker logs db1
Connection Options
Option 1: Connect via container bash
docker exec -it db1 bash
mysql -u root -ppassword123
show databases;
exit
exit
Option 2: Connect directly to MySQL
docker exec -it db1 mysql -u root -ppassword123
docker exec -it db1 mysql -u <username> -p<password>

#exit from db
exit




Environment Variable File Configuration
5.	Create an environment file (db.env):


nano db.env

MYSQL_ROOT_PASSWORD=root123
MYSQL_DATABASE=shubhamdb
MYSQL_USER=shubham
MYSQL_PASSWORD=shubham123

Cntr + o 
enter
Cntr + x

6.	Run the container using the env-file:
docker run --name db2 -d --env-file=db.env mysql
docker rm -f db1 db2



Wordpress configuration
frontend : wordpress
backend: mysql
create mysql engine
configure users cred and blank db for wordpress
configure wordpress
connect wordpress with mysql using env varible: https://hub.docker.com/_/wordpress
-e WORDPRESS_DB_HOST=...
-e WORDPRESS_DB_USER=...
-e WORDPRESS_DB_PASSWORD=...
-e WORDPRESS_DB_NAME=...
port forwarding and open the wordpess on browser to verify
setup wordpress and verify on browser

Host frontend and backend.
1.	Host mysql continer.
docker run --name db1 -d -e MYSQL_ROOT_PASSWORD=password123 -e MYSQL_DATABASE=blankdb -e MYSQL_USER=shubham -e MYSQL_PASSWORD=shubham123 mysql
2.	Grab mysql details
1.	username : shubham
2.	password : shubham123
3.	ip addrees: 172.17.0.7 (get the ip from docker inspect command)
4.	database name: blankdb  (match with your databse name used while creation of mysql container MYSQL_DATABASE=””)
 
3.	host wordpress container and Provide mysql connetions parameters
docker run --name wp1 -d -e WORDPRESS_DB_HOST=<container-ip> -e WORDPRESS_DB_USER=shubham -e WORDPRESS_DB_PASSWORD=shubham123  -e WORDPRESS_DB_NAME=blankdb -p 8095:80 wordpress
 
 
4.	Open the hosted wordpress on browser and verify configurations.
open browser: node1ip:8095

Practice Scenario: Docker Bind Mount with Index.html
 
In this updated practice, students will work with a Docker bind mount to share an index.html webpage between the host and the container. Sample website data will be used for demonstration.
Practice Overview:
Bind Mount Practice with Index.html:
Scenario: Students will create an Nginx container, bind-mounting a local directory containing an index.html file with sample website data.
 
Step-by-Step Guide:
1. Bind Mount Practice with Index.html:
Create Web Content Directory:
mkdir /web-content
 
echo “some content” > /web-content/index.html
 
Run Nginx Container with Bind Mount:
docker run -d --name nginx-container -v /web-content:/usr/share/nginx/html -p 8080:80 nginx:latest
 
Verify Sample Website:
Open http://nodeip:8080 in a web browser.
 
Update Content Locally:
Make changes to /web-content/index.html locally.
echo “version 2” >> /web-content/index.html
 
 
Verify Updated Content in Container:
Refresh the web browser at http://nodeip:8080 to see the changes reflected.
 
Verify Container Data Locally:
Check the modified /web-content/index.html file on the local machine to ensure changes persist.
 
 
 
 
 
 
MySQL Database Persistence with Docker Volumes
To ensure MySQL database persistence using Docker volumes, follow this revised scenario. It includes creating a MySQL container with a volume, connecting to it, creating a sample database, exiting the container, launching another MySQL container with the same volume, and verifying the presence of the database in the newly launched container.
 
1.   Scenario Overview:
2.   Create MySQL Container with Volume: Create a Docker container with MySQL, utilizing a Docker volume for data persistence.
3.   Connect and Access MySQL Database: Connect to the MySQL container, access the MySQL database inside, and create a sample database.
4.   Exit Container: Exit the MySQL container to simulate a shutdown or container termination.
5.   Launch Another MySQL Container: Launch a new MySQL container with the same volume.
6.   Verify Database Persistence:  Verify that the sample database created earlier is present in the newly launched MySQL container.
 
 
 
 
Step-by-Step Guide:
1. Create MySQL Container with Volume:
docker run -d --name mysql-container -v mysql-data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=password mysql:latest
 
2. Connect and Access MySQL Database:
docker exec -it mysql-container mysql -uroot -p
 
Inside MySQL:
CREATE DATABASE sample_db;
show databases;
exit;
 
3. Exit Container:
exit
docker stop mysql-container
4. Launch Another MySQL Container:
docker run -d --name mysql-container-2 -v mysql-data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=password mysql:latest
 
5. Login to second container database and list database.
docker exec -it mysql-container-2  mysql -uroot -p
Inside MySQL:
show databases;
 
6. Clean up. Stop and remove both containers.
docker stop mysql-container-2  mysql-container
docker rm mysql-container-2  mysql-container
docker volume rm mysql-data

Practice Guide: Docker Commit and Push to Docker Hub
Objective:
Demonstrate the use of the docker commit command to create a new Docker image and push it to Docker Hub.
Practice Steps:
Sign Up for Docker Hub
Visit Docker Hub and sign up for a new account.
https://hub.docker.com/
Create a Public Repository
Log in to Docker Hub.
In the top navigation, click on "Repositories" and then "Create Repository."
Provide a name for the repository (e.g., my-public-repo) and set it to public. Click "Create."
Get User Access Keys
In the Docker Hub dashboard, go to "Account Settings" > "personal access tokens" > "New Access Token."
Generate a new access token, providing the necessary permissions (read and write).
Connect to Docker Hub
In the terminal, log in to Docker Hub using the Docker CLI:

docker login -u YOUR_DOCKER_HUB_USERNAME -p YOUR_ACCESS_TOKEN
Create a Container
Launch a new container from a base image:

docker run -itd --name my-container ubuntu:latest
Connect to the Container

docker exec -it my-container /bin/bash
Make Changes in the Container
Create a sample directory and file:

mkdir sample-dir
echo "Enter some content and save the file" > sample.txt
ls
cat sample.txt
Commit Changes to a New Image
Exit the container and create a new image using docker commit:

exit
docker commit my-container YOUR_DOCKER_HUB_USERNAME/my-custom-image:latest
Push the Image to Docker Hub
Push the newly created image to Docker Hub:

docker push YOUR_DOCKER_HUB_USERNAME/my-custom-image:latest
Verify on Docker Hub
Stop and remove the currently running container.
Get the new image path from your uploaded repository.
Create a container with the image.
Connect to the container and verify the files.
Task: Host Repository and Convert to Image
Host the following repository in a Docker container and convert it to an image: https://github.com/shubhamrd/electro-sample.git
Create container with nginx / httpd.
Get inside container.
Install git:

apt update
apt upgrade -y
apt install git -y
Clone the git repository:

git clone https://github.com/shubhamrd/electro-sample.git
Copy cloned data to web hosting directory (e.g. for nginx: /usr/share/nginx/html).
Check locally.
Convert container to an image.
Create a public repo named "electro" on Docker Hub.
Push image to the repository.
Working with Dockerfiles
Reference documentation: https://docs.docker.com/reference/dockerfile/
This guide walks you through creating a directory, setting up a Dockerfile, building images, and managing container versions.
Setup Workspace
Create a directory to keep your project files organized and move into it:

mkdir data && cd data
Create the Dockerfile
Open nano to create the Dockerfile:

nano Dockerfile
Paste the following content into the file:

FROM nginx
WORKDIR /usr/share/nginx/html
RUN apt update
RUN apt install git -y
RUN git clone https://github.com/shubhamrd/electro-sample.git
RUN cp -rf electro-sample/* .
COPY ca.cert .
ADD https://dockerlabs.collabnix.com/beginners/docker/what-is-docker.html .
Save and exit nano (Ctrl+O, Enter, Ctrl+X), then verify:

cat Dockerfile
Create Initial Content
Create a file that will be copied into your image:

echo "file created using dockerfile" > ca.cert
ls
Build and Run Version 1
Build your first Docker image tagged as v1 and run it in detached mode:

docker build -t app2:v1 .
docker images
docker run --name app4 -d -p 8090:80 app2:v1
docker ps
Inspect Container Internals
Open a web browser at nodeip:8090 to check the website.
Create Version 2
Modify the file and build a new image tagged as v2:

echo "version2" >> ca.cert
docker build -t app2:v2 .
docker images
Run Version 2
Start a new container using the updated v2 image under a different name (app5):

docker run --name app5 -d -p 8092:80 app2:v2
Verify in browser at nodeip:8090/ca.cert.
Ubuntu Shell Script Application
This section covers creating a custom shell script, using a specific Dockerfile name, and checking container logs.
Create the Shell Script
Create a script that prints a message repeatedly:

nano script1.sh
Add the following script content:

while true
do
  echo "the script is running"
  sleep 5
done
Save and exit (Ctrl+O, Enter, Ctrl+X), then check for the file:

ls
Create a Custom Dockerfile
Create a Dockerfile with a specific name (dockerfile1) instead of the default:

nano dockerfile1
Paste the following content:

FROM ubuntu
WORKDIR /mnt
COPY *.sh .
CMD ["sh", "script1.sh"]
Save and exit (Ctrl+O, Enter, Ctrl+X), then verify:

cat dockerfile1
Build with Custom File
Build the image using the -f flag to point to your custom Dockerfile:

docker build -t bashapp:v1 -f dockerfile1 .
docker images
Run and Monitor
Run the container in detached mode, check status, and inspect logs:

docker run --name bashapp -d bashapp:v1
docker ps -a
docker logs bashapp

Pull the repo. 
https://github.com/shubhamrd/flappy_bird.git

git clone https://github.com/shubhamrd/flappy_bird.git
cd flappy_bird
ls
cat Dockerfile #understand docker file
 
Run the Dockerfile and build the image. 
docker build -t flappy:v1 .
Create a container with the port forwarding. 

	docker images
  	docker run --name flappy-con -d -p 8090:8080 flappy:v1


Check on the web whether the Flappy Bird game is running. 
open browser with nodeip:8090 

Create a public repo on Docker Hub. 
5.1 create repo on  docker hub, copy name.
5.2 login to cli
docker login -u username -p passwrod/PAT

Push this same image to Docker Hub. 
docker tag flappy:v1 your_username/reponame
 docker push your_username/reponame

Share the link on a chat. 
https://hub.docker.com/r/shubhamrdalvi/flappy_bird


minikube setup

You are already connected to node 1. 
Run the command `init 0`. 
Close the putty. 
Open the GitBash from the desktop shortcut. 
Run the command `minikube start`. 
And wait to get it started. 
Check the status with `minikube status`. 



Kubernetes setup guide with kubeadm: https://github.com/shubhamrd/Kubernetes/tree/main/kubernetes-setup


minikube setup:https://minikube.sigs.k8s.io/docs/start/?arch=%2Fwindows%2Fx86-64%2Fstable%2F.exe+download

Minikube on a browser with code space : https://codespaces.new/kubernetes/minikube?quickstart=1


Practical Guide: Basic Pod Management (CLI)
This guide focuses on managing Pods directly via the command line (CLI). Using the CLI is essential for quick troubleshooting, labeling, and administrative tasks in a containerized environment.
🛠 Basic Concepts
● 	Pod: The smallest deployable unit in Kubernetes/container orchestration, representing a single instance of a running process.
● 	Labels: Key-value pairs attached to Pods used for organizing and filtering.
● 	CLI Management: Performing actions directly via commands rather than configuration files (YAML).
🏗 Scenario: Non-Prod Application Management
You are tasked with managing several application pods in a testing environment (nonprod). You will create, label, and filter these pods to simulate a real-world workflow.
Step 1: Create a Pod
We will create a simple Nginx pod to represent a web server.
kubectl get nodes
Command: kubectl run nonprod-webapp --image=nginx
● 	Explanation:
○ 	run: Creates a new Pod.
○ 	nonprod-webapp: The name of the Pod.
○ 	--image=nginx: Specifies the container image to use.
Step 2: List Pods
To see if your Pod was created successfully.
Command: kubectl get pods
● 	Explanation: Lists all Pods in the current namespace and shows their status (e.g., Running, Pending).
Step 3: Describe a Pod (Detailed Info)
If a Pod is stuck in "Pending" or "CrashLoopBackOff," you need to see the detailed status and events.
Command: kubectl describe pod nonprod-webapp
● 	Explanation:
○ 	describe: Shows detailed information about the resource, including its IP, labels, container status, and a list of recent Events (e.g., "Successfully pulled image").
Step 4: View Pod Logs
Checking logs is the first step in debugging an application.
Command: kubectl logs nonprod-webapp
● 	Explanation: Displays the standard output (STDOUT) of the container running inside the Pod.
Step 5: Execute Commands Inside a Pod
Sometimes you need to enter the Pod to check files or configurations.
Command: kubectl exec -it nonprod-webapp -- //bin/bash
● 	Explanation:
○ 	-it: Interactive terminal mode.
○ 	-- /bin/bash: The command to run inside the container to give you a shell prompt.
○ 	run command cat /etc/os-release
○ 	To Exit: Type exit and press Enter.
Step 6: Labeling and Overriding
Labels allow you to categorize Pods (e.g., by environment or tier).
Adding a Label:
Command: kubectl label pods nonprod-webapp env=nonprod tier=frontend
kubectl get pods –-show-labels
● 	Explanation: Adds two labels (env and tier) to the nonprod-webapp pod.
Overriding an Existing Label:
Command: kubectl label pods nonprod-webapp tier=web-server --overwrite
kubectl get pods –-show-labels
● 	Explanation:
○ 	--overwrite: Required if the label key (tier) already exists and you want to change its value.
Step 7: Filtering Pods Based on Labels
When you have hundreds of pods, you use filters (selectors) to find specific ones.
Command: kubectl get pods -l env=nonprod
 
● 	Explanation:
○ 	-l: Stands for "selector" or "label". This command only shows pods that have the env=nonprod label.
Step 8: Removing a Label (De-labeling)
If a label is no longer needed, you can remove it.
Command: kubectl label pods nonprod-webapp tier-
● 	Explanation: Adding a dash (-) immediately after the label key name removes that label from the Pod.
🧹 Clean Up Steps
Always remove your practice resources to keep the environment clean.
1. Delete the Pod
Command: kubectl delete pod nonprod-webapp
● 	Explanation: Stops the container and removes the Pod definition from the system.
💡 Best Practices
1.	Descriptive Names: Always include the environment prefix (e.g., nonprod-) in the Pod name.
2.	Standard Labels: Establish a standard set of labels (e.g., app, env, owner) for all Pods.
3.	Describe First for Infrastructure Errors: If the pod won't start, use describe. If the pod starts but the app crashes, use logs.
 

Practical Guide: Declarative Pod Management (YAML & Inspection)
This guide covers how to manage Kubernetes resources using YAML files (declarative approach) and how to use inspection tools to understand resource structures.
🛠 Basic Concepts
● 	Declarative Management: Using files (YAML) to define the "desired state" of your cluster.
● 	Dry Run: Simulating a command to see the output or generate a template without actually creating a resource.
● 	API Exploration: Using tools to see what fields are available in a Kubernetes resource.
🏗 Scenario: Non-Prod Resource Exploration and Deployment
You are moving beyond simple CLI commands to managing resources using configuration files, which is the standard for production environments.
Step 1: Explore API Resources
Before creating a YAML file, you need to know what fields are available and what resources the cluster supports.
Command: kubectl api-resources
● 	Explanation: Lists all available resource types (Pods, Services, etc.) supported by your cluster.
Command: kubectl explain pods
● 	Explanation: Provides documentation for the Pod resource. It shows the top-level fields like apiVersion, kind, metadata, and spec.
Command: kubectl explain pods --recursive
● 	Explanation: Shows the entire tree of fields available within a Pod, helping you find deeply nested configuration options.
Step 2: Generate a YAML Template
Instead of writing YAML from scratch, use a dry-run to generate a template.
Command: kubectl run firstpod1 --image=nginx --dry-run=client -o yaml > newpod.yml
● 	Explanation:
○ 	--dry-run=client: Tells Kubernetes NOT to create the pod, just simulate the request.
○ 	-o yaml: Outputs the result in YAML format.
○ 	> newpod.yml: Redirects the output into a new file named newpod.yml.
Step 3: Edit the YAML File
Open the file to modify it for your specific needs.
1.	Open the file: nano newpod.yml / create file in vscode
2.	Modify the content to match this configuration:
apiVersion: v1
kind: Pod
metadata:
  labels:
    run: firstpod1
    env: test
  name: firstpod2
spec:
  containers:
    - image: httpd
      name: firstpod1
 
3.	To Save and Exit: Press Ctrl + O, then Enter, then Ctrl + X.
Step 4: Validate and Create the Pod
Always validate your YAML before applying it to the cluster.
Command: kubectl create -f newpod.yml --dry-run=client
● 	Explanation: Validates that the YAML syntax is correct without creating the resource.
Command: kubectl create -f newpod.yml
● 	Explanation: Creates the Pod defined in the file.
Step 5: Verify and Inspect
Command: kubectl get pods
Command: kubectl describe pod firstpod2
● 	Explanation: Use describe to check if the Pod started correctly or to see if there are any errors.
Step 6: The "Apply" Workflow (Updates)
In professional environments, we use apply to update resources.
1.	Edit the file to add a new label: nano newpod.yml
2.	Under labels:, add team: dev. Save and exit.
Command: kubectl diff -f newpod.yml
● 	Explanation: Shows the difference between your local file and the live resource in the cluster (like a "git diff").
Command: kubectl apply -f newpod.yml
● 	Explanation: Updates the live Pod with the changes made in your YAML file.
Command: kubectl get pod firstpod2 --show-labels
Step 7: Direct Live Editing
Sometimes you need to fix a live resource without changing the local file.
Command: kubectl edit pod firstpod2
● 	Explanation: Opens the live configuration in your default editor. After you save and exit, Kubernetes applies the changes immediately.
Command: kubectl get pod firstpod2 --show-labels
🧹 Clean Up Steps
Command: kubectl delete -f newpod.yml
● 	Explanation: Deletes the resource defined in the file.
Command: rm newpod.yml
💡 Best Practices
1.	Dry-Run Often: Use --dry-run=client -o yaml to generate perfect templates every time.
2.	Apply over Create: Use kubectl apply for most tasks as it handles updates gracefully.
Check Labels: Use --show-labels frequently to ensure your organization logic is correct.
 



create a pod for with mysql image
connect to database and check tables

mysql.yaml
apiVersion: v1
kind: Pod
metadata:
  labels:
    managed: shubham
    type: pod
    app: webapp
    version: v2
  name: mysql-pod
spec:
  containers:
  - image: mysql
    name: con1
    env:
    - name: MYSQL_ROOT_PASSWORD
      value: "root123"





kubectl apply -f mysql-pod.yaml 



   kubectl get pods -w
  kubectl exec -it mysql-pod -- bash
mysql -u root -proot123
show databases;
exit;
exit


Multi container
 
apiVersion: v1
kind: Pod
metadata:
  labels:
	app: webapp
	owner: team1
	cc: NIIT
  name: multi-pod
spec:
  containers:
  - image: httpd
	name: con1
  - image: mysql
	name: con2
	env:
  	- name: MYSQL_ROOT_PASSWORD
    	value: example
 
kubectl delete -f multi.yaml
kubectl apply -f multi.yaml
kubectl get pods -w 
Pod management
 kubectl logs multi-pod -c con1
 kubectl logs multi-pod -c con2
 
kubectl exec -it multi-pod -c con1 --  bash
exit
kubectl exec -it multi-pod -c con2 --  bash
exit 
 
init container
 
apiVersion: v1
kind: Pod
metadata:
  labels:
	app: webapp
	owner: team1
	cc: NIIT
  name: init-pod
spec:
  containers:
  - image: httpd
	name: main
 
  initContainers:
  - image: busybox
	name: init-container
	command: ['sh', '-c', 'echo The app is running! && sleep 60']
 
apply file
describe pod
check events
Init container will start first. It will stop and then the main container will start.




Working with ReplicaSet
Create a ReplicaSet (RS)
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: nginx-rs

spec:
  replicas: 3

  selector:
    matchLabels:
      app: nginx

  template:
    metadata:
      labels:
        app: nginx

    spec:
      containers:
        - name: nginx
          image: nginx

 
 
 
kubectl apply -f nginx-rs.yaml
 
Verify the ReplicaSet
 
Command: kubectl get rs
Expectation: nginx-rs should be running with 3 replicas.
 
Scale the ReplicaSet
Scale Up: kubectl scale rs nginx-rs --replicas=5
Verify: kubectl get rs
Scale Down: kubectl scale rs nginx-rs --replicas=2
Verify again.
 
edit the label of any pod under rs and check the reaction.
restore the label to original and again check
kubectl get pods --show-labels
select any pod
kubectl label pod pod-name app=webapp --overwrite
kubectl get pods --show-labels
kubectl label pod pod-name app=nginx 
pods already has labels app=nginx , new labels environment=production
 
kubectl label pods -l app=nginx environment=production
 
kubectl label pod pod-alpha pod-beta pod-gamma tier=backend
 
kubectl label pods --all env=staging

 

Services
kubectl run webapp --image=nginx --port=80
kubectl expose pod webapp --target-port=80 --port=8090
 
kubectl get pods
kubectl get svc
copy the IP of services
minikube ssh
 curl <service-ip>:8090
exit
 
NodeIP servoie
 
kubectl delete svc webapp
kubectl expose pod webapp --target-port=80 --port=8090 --type=NodePort
kubectl get svc
 
Check external port no.
 
minikube ip
Open browser with minikube ip and port no
 




 
 
 
Labels and selectors with service
Step 1: Create a Pod with Labels using YAML
Create a YAML file named pod-with-labels.yaml with the following content:
 
apiVersion: v1
kind: Pod
metadata:
  name: my-nginx
  labels:
  	app: my-nginx
  	env: production
spec:
  containers:
  - name: nginx-container
  	image: nginx
 
 
 
 
 
Apply the YAML file:
 
kubectl apply -f pod-with-labels.yaml
 
Step 2: List Labels for the Pod
List labels for the created pod:
kubectl get pod my-nginx --show-labels
 
 
Step 3: Create a Service with the Same Selectors using YAML
Create service-with-selectors.yaml with the following content:
 
apiVersion: v1
kind: Service
metadata:
  name: my-nginx-svc
spec:
  selector:
	app: my-nginx
	env: production
  type: NodePort
  ports:
	- protocol: TCP
  	port: 80
  	targetPort: 80
  	nodePort: 30007  # Optional: defines the specific port on the Node
 
 
 
 
 
Apply the YAML file:
kubectl apply -f service-with-selectors.yaml
 
Step 4: Test the Service
 
kubectl get svc my-nginx-svc
open broser with minikube ip and port
 
Extend the demos of service:
Step 1: Remove Selectors from Existing Pod
 
kubectl label pod podname --overwrite app=webapp
kubectl label pod podname --overwrite env=test
 
 
Step 2: Check Connectivity to the Service (Expect Failure)
# Access the service internally after removing selectors (Expect failure)
check on browser
deploy app version1 with RS
shubhamrdalvi/application:1.0
expose the app with nodeport service and verify on browser
take a screenshot and paste on chat
deploy the app with version2 in new RS
shubhamrdalvi/application:2.0
route the traffic from v1 app to v2 app RS from service
make version 1 replica=0
take a screenshot and paste on chat
rollback
make version 1 RS=3
route back your traffic from v2 to v1 and make version2 RS=0.
verify on browser


 
Deployment Practice
Step 1: Create a Deployment
Create a YAML file for the Deployment: Name it nginx-deployment.yaml. This will create a Deployment running an Nginx server.
 
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 5
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: shubhamrdalvi/application:1.0




 
 
 
 
Create the Deployment:
kubectl apply -f nginx-deployment.yaml
 
 kubectl annotate deployment nginx-deployment kubernetes.io/change-cause="version change1" --overwrite=true
 
expose the deployment and check on browser.
 
 
Step 2: Update the Deployment
Update the Deployment to Use a New Version of Nginx:
 
Edit the nginx-deployment.yaml file, changing the image from shubhamrdalvi/application:1.0 to shubhamrdalvi/application:2.0
 
 Apply the yaml file
 
Check deployment
Kubectl get pods -w
 kubectl annotate deployment nginx-deployment kubernetes.io/change-cause="upgraded to stable-perl" --overwrite=true
 
verify changes on browser with service
 
Do the same for image  shubhamrdalvi/application:3.0
 
 
Step 3: Monitor the Update
Monitor the Status of the Deployment:
kubectl rollout status deployment/nginx-deployment
 
 
 
Step 5: Rollback if Necessary
Rollback to the Previous Version:
If you need to rollback the update for any reason:
 kubectl rollout history deployment/nginx-deployment
 kubectl get rs -o wide
 
Rollback deployment
kubectl rollout undo deployment/nginx-deployment
 
or
kubectl rollout history deployment/nginx-deployment
kubectl rollout undo –-to-revision=2 deployment/nginx-deployment # last version



DaemonSet
create a daemon set to host an nginx app.
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: nginx-daemonset
  labels:
  	app: nginx-daemonset
spec:
  selector:
  	matchLabels:
  	app: nginx-pod
  template:
  	metadata:
  	labels:
	  app: nginx-pod
  	spec:
  	tolerations:
  	- key: "type"
	  operator: "Equal"
	  value: "master"
	  effect: "NoSchedule"
  	containers:
  	- name: nginx
	  image: nginx:latest
	  ports:
	  - containerPort: 80
 
 
check the pods are running on all nodes
apply file and verify

 
 
 delete daemon set

Using Volumes in Pods
Using an emptyDir Volume
 
Create a Pod with an emptyDir volume to share data between containers:
apiVersion: v1
kind: Pod
metadata:
  name: mypod
spec:
  containers:
  - name: myfrontend
    image: nginx
    volumeMounts:
    - mountPath: /var/www/html
      name: myvolume
  volumes:
  - name: myvolume
    emptyDir: {}
 




 
Verify :
kubectl get pod
kubectl describe pod mypod
check for volume configuration
 
 
 
 
Using a hostPath Volume
minikube ssh -n minikube-m02
sudo su -
> mkdir /mnt/data
> touch /mnt/data/config.txt
> echo “some text” > /mnt/data/config.txt
>exit
> exit
 
kubectl label node minikube-m02 nodetype=worker1
Useful for accessing host filesystem, typically for advanced use cases:
 
apiVersion: v1
kind: Pod
metadata:
  name: app2
spec:
  containers:
  - image: nginx
    name: con1
    volumeMounts:
    - name: testvolume
      mountPath: /mnt/data
  affinity:
    nodeAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
        - matchExpressions:
          - key: nodetype
            operator: In
            values:
            - worker1
  volumes:
  - name: testvolume
    hostPath:
      path: /mnt/data
      type: Directory



 
 
 
 
 
 
 
Apply the file
kubectl exec -it app2 -- bash
cd /mnt/data
ls
cat config.txt
exit

 
Practice Guide: StatefulSets and HostPath Volumes
Objective
In this modified practice, you will learn how to constrain a StatefulSet to a specific worker node using nodeSelector. Instead of giving each pod its own unique Persistent Volume, you will use a hostPath volume with DirectoryOrCreate to force all pod replicas to share the exact same physical directory on the minikube-m02 node.
Prerequisites
• 	Environment: A multi-node Minikube cluster (must have a node named minikube-m02). (If you need to start one, use: minikube start --nodes 2)
• 	Tools: kubectl installed and configured.
Step-by-Step Instructions
Step 1: Create a Headless Service
StatefulSets still require a "Headless Service" to manage the network identity of the pods, even when sharing a volume.
1.	Create the YAML file:
vi headless-svc.yaml
Paste the following configuration:
 
apiVersion: v1
kind: Service
metadata:
  name: nginx
  labels:
    app: nginx
spec:
  ports:
  - port: 80
    name: web
  clusterIP: None
  selector:
    app: nginx



 
kubectl apply -f headless-svc.yaml
Step 2: Deploy the StatefulSet with nodeSelector and hostPath
Now we deploy the StatefulSet. We are removing the volumeClaimTemplates and adding a nodeSelector (to target minikube-m02) and a hostPath volume.
 
Create the YAML file:
 
Bash
vi web-statefulset.yaml
Paste the following configuration:
 
YAML
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: web
spec:
  serviceName: "nginx"
  replicas: 2
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      # Force all pods to schedule ONLY on the minikube-m02 node
      nodeSelector:
        kubernetes.io/hostname: minikube-m02
      containers:
      - name: nginx
        image: nginx:latest
        ports:
        - containerPort: 80
          name: web
        volumeMounts:
        - name: shared-host-dir
          mountPath: /usr/share/nginx/html
      volumes:
      - name: shared-host-dir
        hostPath:
          # This path will be created on the minikube-m02 node
          path: /data/statefulset-shared
          # This ensures the directory is created if it doesn't already exist
          type: DirectoryOrCreate



Apply the configuration and watch the pods initialize:
 
Bash
kubectl apply -f web-statefulset.yaml
kubectl get pods -w -o wide
(Expected Output: You will see web-0 created first, followed by web-1. Look at the NODE column—both of them MUST say minikube-m02. Press Ctrl+C to stop watching once both are running).
 
Step 3: Test Shared Storage Across Pods
Unlike a standard StatefulSet where each pod gets unique storage, these pods are now sharing the exact same directory on minikube-m02. Let's prove they are sharing data.
 
Write custom data using the web-0 pod:
 
Bash
kubectl exec -it web-0 -- sh -c 'echo "This is shared storage on minikube-m02!" > /usr/share/nginx/html/index.html'
Read the data from the web-1 pod:
 
Bash
kubectl exec -it web-1 -- cat /usr/share/nginx/html/index.html
(Expected Output: "This is shared storage on minikube-m02!". Even though you wrote it from pod 0, pod 1 can see it perfectly because they are mounted to the same host directory).
 
Step 4: Test Crash Resilience
Because the data is tied to the physical node's file system (hostPath), it will survive a pod crash as long as the pod is rescheduled back to minikube-m02.
 
Simulate a crash by deleting web-0:
 
Bash
kubectl delete pod web-0
Wait for Kubernetes to recreate it:
 
Bash
kubectl get pods -w -o wide
(Wait until web-0 is Running again on minikube-m02)
 
Verify the data is still there:
 
Bash
kubectl exec -it web-0 -- cat /usr/share/nginx/html/index.html
(Expected Output: The text will still be there because the /data/statefulset-shared folder on minikube-m02 was untouched by the pod crash).
 
Step 5: Cleanup
When using hostPath, the data is permanently stored on the node until you manually SSH into the node and delete it.
 
Delete the StatefulSet and Service:
 
Bash
kubectl delete -f web-statefulset.yaml
kubectl delete -f headless-svc.yaml
(Optional) Clean up the node's hard drive:
To fully remove the data, you would need to access the node directly.

1.Create the Helm Chart:
Generate the initial scaffolding for your new chart.
 
helm create nginx-chart
 
2.Configure values.yaml:
Open the values.yaml file inside the nginx-chart directory and update it with the following configuration. Save and exit the file when finished.
 
replicaCount: 4
 
image:
  repository: nginx
  # This sets the pull policy for images.
  pullPolicy: IfNotPresent
  # Overrides the image tag whose default is the chart appVersion.
  tag: "stable-perl"
 
service:
  # This sets the service type more information can be found here: https://kubernetes.io/docs/concepts/services-networking/service/#publishing-services-service-types
  type: NodePort
  # This sets the ports more information can be found here: https://kubernetes.io/docs/concepts/services-networking/service/#field-spec-ports
  port: 80
 
resources:
  # We usually recommend not to specify default resources and to leave this as a conscious
  # choice for the user. This also increases chances charts run on environments with little
  # resources, such as Minikube. If you do want to specify resources, uncomment the following
  # lines, adjust them as necessary, and remove the curly braces after 'resources:'.
  limits:
	cpu: 200m
	memory: 300Mi
  requests:
	cpu: 100m
	memory: 128Mi
 
autoscaling:
  enabled: true
  minReplicas: 4
  maxReplicas: 8
  targetCPUUtilizationPercentage: 80
 
3.Install and Verify the Release:
Deploy the chart to your cluster and verify that the resources are running.
 
helm install nginx nginx-chart
kubectl get all
 
4.Prepare an Upgrade:Edit configurations.
Modify your configuration files to prepare for a release upgrade.
1.	Edit values.yaml: Change the image tag from stable-perl to mainline-perl.
2.	Edit Chart.yaml: Change the appVersion (or version) from 1.16.0 to 1.18.0.
5.Apply Upgrade and Verify Changes:
Apply the updated chart, check the revision history, and confirm the new image is running inside the pods.
 
helm upgrade nginx nginx-chart
helm history nginx
kubectl get pods
kubectl describe pod <any-pod-id>
 
Note: Look under the "Containers" section in the output of the describe command to verify the updated image tag.
6.Test Rollback Functionality:
Revert the release back to its first revision (before the upgrade) and verify that the original image is restored.
 
helm rollback nginx 1
kubectl describe pod <any-pod-id>
helm history nginx
 
7.Clean Up Resources:
Remove the deployment and clean up your cluster once practice is complete.
 
helm uninstall nginx
 
Resource Link: To find and explore other community Helm charts, visit the Artifact Hub. https://artifacthub.io/
 

Practice Guide: Kubernetes Logging, Monitoring, and Troubleshooting
Topic
Observability and Debugging in a Kubernetes Cluster
Objective
Equip yourself with essential skills to diagnose and resolve common Kubernetes issues. You will learn how to enable resource monitoring, extract logs from running applications, read cluster events to diagnose deployment failures, open interactive shells inside running pods, and finally, test your skills in a comprehensive troubleshooting challenge.
Prerequisites
Environment: A running Kubernetes cluster (e.g., Minikube).
Tools: kubectl installed and configured.

Step 1: Enable Resource Monitoring (Metrics Server)
By default, Kubernetes does not constantly stream CPU and memory usage to your CLI. We need to enable the Metrics Server to collect this data so we can use the kubectl top command (similar to the Linux top command).
Enable the Metrics Server (Minikube):
minikube addons enable metrics-server
(Note: It may take 1-2 minutes for the metrics server to start collecting data).
2.                  Verify Node Resource Usage:
kubectl top nodes
(Expected Output: You will see your Minikube node's CPU and Memory usage in millicores (m) and Megabytes (Mi)).
3.                  Verify Pod Resource Usage:
kubectl top pods -A
(Expected Output: A list of all running pods across all namespaces and their current resource consumption).
4.   start and open Kubernetes dashboard
minikube dashboard
(Expected Output: Opens a webpage with Kubernetes dashboard).
Explore dashboard nodes, pods, services, metrics and events
5. back to cli and use Cntr+ c to stop dashboard
 

Step 2: Extracting Application Logs
When an application misbehaves or is receiving traffic, the first place to look is its standard output (logs). Let's deploy a web server and generate some real traffic.
Deploy an NGINX web server:
kubectl run nginx-logger --image=nginx --port=80
2.                  Expose the pod so we can access it:
kubectl expose pod nginx-logger --type=NodePort --port=80
3.                  Get the URL for the application:
minikube service nginx-logger --url
(Expected Action: Copy the generated URL, open it in your web browser, and refresh the page a few times to generate traffic).
4.                  View the logs (Static Snapshot):
kubectl logs nginx-logger
(Expected Output: You will see standard NGINX access logs detailing your browser's HTTP GET requests).
5.                  Tail the live logs stream (Follow mode):
kubectl logs -f nginx-logger
(Expected Action: Keep this terminal open and refresh your browser again. You will see new log entries appear instantly in the terminal. Press Ctrl+C to exit the stream).

Step 3: Troubleshooting Pod Failures (describe and --previous)
If a pod crashes immediately, kubectl logs might be empty. You need to inspect the pod's underlying Events. Let's intentionally create two broken pods.
Scenario A: The Image Typo (ImagePullBackOff)
Deploy a pod with a non-existent image:
kubectl run typo-pod --image=nginxxxx:latest
2.                  Check the pod status:
kubectl get pods
(Expected Output: typo-pod will show a status of ErrImagePull or ImagePullBackOff).
3.                  Diagnose the root cause:
kubectl describe pod typo-pod
(Scroll down to the Events section at the very bottom. You will see a clear error message: Failed to pull image "nginxxxx:latest": rpc error... not found. This tells you exactly why it failed).
Scenario B: The Crashing Application (CrashLoopBackOff)
Deploy a pod that crashes immediately:
kubectl run crash-pod --image=alpine -- sh -c "echo 'CRITICAL FATAL ERROR' && exit 1"
2.                  Wait a few seconds and check the status:
kubectl get pods
(Expected Output: crash-pod will show CrashLoopBackOff, meaning Kubernetes keeps trying to restart it, but it keeps dying).
3.                  Check the logs of the previous crashed instance: Normally, kubectl logs only shows the current running container. If it's dead, you must use the --previous flag.
kubectl logs crash-pod --previous
(Expected Output: CRITICAL FATAL ERROR. You have successfully retrieved the dying words of a crashed pod!).

Step 4: Interactive Debugging (kubectl exec)
Sometimes logs aren't enough, and you need to jump inside a running container to test network connectivity or view configuration files.
Deploy a simple NGINX web server:
kubectl run web-server --image=nginx
2.                  Wait for it to be ready:
kubectl get pods -w
(Press Ctrl+C when web-server is Running).
3.                  Open an interactive shell inside the pod:
kubectl exec -it web-server -- bash
Your terminal prompt will change. You are now inside the NGINX container!
4.                  Test the internal network (simulate a curl request):
curl localhost
(Expected Output: The raw HTML of the NGINX welcome page. This proves the web server is running properly on the inside).
5.                  Exit the container:
exit

Step 5: The Ultimate Troubleshooting Challenge
It is time to put everything you've learned to the test. Below is a YAML file containing a Deployment and a Service. It has three distinct errors intentionally written into it.
Your goal is to apply this broken file, use your debugging commands to find the problems, edit the running resources to fix them, and access the website in your browser.
Create the broken YAML file:
Create file challenge.yaml
2.                  Paste the following configuration:
apiVersion: v1
kind: ConfigMap
metadata:
  name: challenge-content
data:
  index.html: |
    <h1>Congrats challenge completed</h1>
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: challenge-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      app: challenge-web
  template:
    metadata:
      labels:
        app: challenge-web
    spec:
      containers:
      - name: nginx
        image: nginx:1.999.9-alpine
        ports:
        - containerPort: 80
        volumeMounts:
        - name: html-volume
          mountPath: /usr/share/nginx/html
      volumes:
      - name: html-volume
        configMap:
          name: challenge-content
---
apiVersion: v1
kind: Service
metadata:
  name: challenge-service
spec:
  type: NodePort
  selector:
    app: challenge-nginx
  ports:
  - port: 80
    targetPort: 8080
```
3.                  Apply the broken configuration:
kubectl apply -f challenge.yaml
4.                  Your Mission: The website is supposed to be accessible, but right now it is completely broken.
Hint 1: Start by checking if the pod is actually running (kubectl get pods). If it isn't, use kubectl describe pod to figure out why, and edit the deployment to fix the image.
 
Hint 2: Once the pod is running, check if the Service is properly linking to the pod by checking its endpoints (kubectl get endpoints challenge-service). If it says <none>, edit the service and fix the selector.
 
Hint 3: The Service is sending traffic to the wrong internal container port. Edit the service one last time to fix the targetPort.
5.                  Verify Your Success: Once you believe you have fixed all three errors, get the URL for the service:
minikube service challenge-service --url
Open that URL in your web browser. If you succeeded, the page will display:
Congrats change completed

Step 6: Cleanup
Let's clean up our workspace by deleting all the troubleshooting pods, services, and the challenge resources we created.
kubectl delete pod nginx-logger typo-pod crash-pod web-server
kubectl delete svc nginx-logger
kubectl delete -f challenge.yaml
 

 
Practice Guide: End-to-End Docker Build to Kubernetes Deployment
Topic
Containerizing a Web App and Deploying to Kubernetes
Objective
Experience the complete software delivery lifecycle. You will connect to a remote build node, clone a website from GitHub, package it into a custom NGINX Docker image, push it to Docker Hub, and then switch to your local Minikube cluster to deploy and expose the application to the internet.
Prerequisites
Docker Hub Account: You will need your Docker Hub username and password.
Environments:
Your build machine ("Node 1") with Docker installed.
Your base machine with Minikube and kubectl installed.

PART 1: Build and Push (On Node 1)
Step 1: Connect to Node 1
First, we need to access your build environment.
Go to your desktop.
Double-click the shortcut named node1-connect to establish an SSH session to Node 1.
Step 2: Clone the Repo and Create the Dockerfile
Now that you are on Node 1, we will download the website source code and write a Dockerfile to serve it using NGINX.
Clone the GitHub repository:
git clone https://github.com/shubhamrd/electro-sample.git
cd electro-sample
2.                  Create the Dockerfile:
vi Dockerfile
3.                  Paste the following configuration: (This tells Docker to pull a lightweight NGINX image and copy your website files into NGINX's default hosting directory).
```dockerfile
FROM nginx:alpine
COPY . /usr/share/nginx/html
EXPOSE 80
```
4.                  Build the Docker Image: (Note: Replace <your-dockerhub-username> with your actual Docker Hub ID. Don't forget the . at the end, which tells Docker to build from the current directory).
docker build -t <your-dockerhub-username>/electro-sample:v1 .
Step 3: Test the Image Locally on Node 1
Before pushing it to the internet, let's make sure the image actually works.
Run the container locally:
docker run -d --name electro-test -p 8080:80 <your-dockerhub-username>/electro-sample:v1
2.                  Verify it is running:
docker ps
3.                  Test the webpage (via CLI):
curl http://localhost:8080
(Expected Output: You should see the raw HTML of the electro-sample website).
4.                  Clean up the test container:
docker rm -f electro-test
Step 4: Push the Image to Docker Hub
Now we must upload this image to a public registry so your Kubernetes cluster can download it later.
Log in to Docker Hub:
docker login
(Enter your Docker Hub username and password when prompted).
2.                  Push the image:
docker push <your-dockerhub-username>/electro-sample:v1
(Wait for the upload to complete. Your image is now live on the internet!)

PART 2: Deploy and Expose (On Base Machine)
Step 5: Switch to Your Base Machine
IMPORTANT: Minimize or close your node1-connect terminal. Open a standard terminal on your base machine where Minikube is installed and running.
Ensure Minikube is running:
minikube status
 
Step 6: Deploy the Image to Kubernetes
We will create a Kubernetes Deployment that pulls your custom image from Docker Hub.
Create a Deployment YAML file:
vi electro-deploy.yaml
2.                  Paste the following configuration: (Remember to replace <your-dockerhub-username> with your actual username!)
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: electro-deployment
spec:
  replicas: 2
  selector:
    matchLabels:
      app: electro-web
  template:
    metadata:
      labels:
        app: electro-web
    spec:
      containers:
      - name: web
        image: <your-dockerhub-username>/electro-sample:v1
        ports:
        - containerPort: 80
```
3.                  Apply the Deployment:
kubectl apply -f electro-deploy.yaml
4.                  Verify the pods are running:
kubectl get pods -w
(Wait for them to show Running. Press Ctrl+C to exit).
Step 7: Expose the Application with a Service
To access the website from outside the cluster, we need a NodePort Service.
Create a Service YAML file:
vi electro-svc.yaml
2.                  Paste the following configuration:
```yaml
apiVersion: v1
kind: Service
metadata:
  name: electro-service
spec:
  type: NodePort
  selector:
    app: electro-web
  ports:
  - port: 80
    targetPort: 80
```
3.                  Apply the Service:
kubectl apply -f electro-svc.yaml
Step 8: Verify in the Browser
Everything is deployed! Let's get the URL and view your website.
Ask Minikube for the accessible URL:
minikube service electro-service --url
2.                  Test it: Copy the URL output by the command above (e.g., http://192.168.49.2:31234), open your web browser on your base machine, and paste it into the address bar.
(Expected Result: You should see the fully rendered "electro-sample" website hosted by your Kubernetes cluster!)
 

