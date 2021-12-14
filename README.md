## Spring Boot, PostgreSQL, JPA, Hibernate REST API Demo

Pre-requisites:
--------
    - Install Git
    - Install Maven
    - Install Docker
    - EKS Cluster with eksctl commands
    - Install ALB Ingress Controller
    - Create Cerificate for our domain in Certificate manager
Clone code from github:
-------
    git clone https://github.com/Naresh240/external-dns-springbootWithDatabase.git
    cd external-dns-springbootWithDatabase
    
Build Maven Artifact:
-------
    mvn clean install -DskipTests=true
 
Build Docker image for Springboot Application
--------------
    docker build -t naresh240/springbootpostgresrestapidemo .
  
Docker login
-------------
    docker login
    
Push docker image to dockerhub
-----------
    docker push naresh240/springbootpostgresrestapidemo

Encode USERNAME and PASSWORD of Postgres using following commands:
--------
    echo -n "admin" | base64

Deploying Postgres with kubectl apply:
-----------
    cd postgres
    kubectl apply -f .
  
Deploy Spring Application:
--------
    cd springboot
    kubectl apply -f .

# Check secrets, configmaps, pv, pvc, deployments, pods and services:
    kubectl get secrets
    kubectl get configmaps
    kubectl get pv
    kubectl get pvc
    kubectl get deploy
    kubectl get pods
    kubectl get svc
    
Now Goto Loadbalancer and check whether service comes Inservice or not, If it comes Inservice run ingress file

# Run Ingress file using below command
    kubectl apply -f ingress.yml
    
# Check ingress attached to ALB ingress controller or not:
    kubectl get ingress
![image](https://user-images.githubusercontent.com/58024415/94955690-986ba200-0508-11eb-9eac-8c6257bb7035.png)

# Add "A" Record with in Route53
![image](https://user-images.githubusercontent.com/58024415/94956726-49267100-050a-11eb-9611-f4a512edc381.png)

Use POST Method:
--------
    http://springboot.cloudtechmasters.ml/questions
    select raw and Json format with in the Body section
        {
	          "title": "Which country you belongs too?",
	          "description": "I am from India..."
        }
Use GET Method:
-------
    http://springboot.cloudtechmasters.ml/questions
    
Use POST Method:
--------
http://springboot.cloudtechmasters.ml/questions/{questionId}/answers
    
    http://springboot.cloudtechmasters.ml/questions/1000/answers
    select raw and Json format with in the Body section
        {
	          "text": "I am an Indian"
        }
  
Use GET Method:
-------
    http://springboot.cloudtechmasters.ml/questions/1000/answers

You can Test other API also.........

# Cluean UP process:
    kubectl delete -f .
