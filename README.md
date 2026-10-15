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


