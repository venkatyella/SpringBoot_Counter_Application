# SpringBoot_Counter_Application
 
 
 ### 1.	Create a spring boot app that prints “hello-world-$counter” ($counter should increment on every refresh)
 
          1. SpringBoot_Counter_Application generates jar named Uber.jar, same jar has been used in Dockerfile to build & deploy images. 
          2. applications.properties contains the new port :9090 

 
 ### 2. Create an Uber jar for the spring boot app and execute your jar from the command line.
 
         1. mvn install              # Maven takes my springboot application and generated aftifacts(jar file inside 'target/Uber.jar' ), 
         2. java - jar Uber.jar      # Executing the jar file
 
 
 ### 3.	Create Docker image for the spring boot app and run it as Docker container exporting the URL port (9090).
 
 
        docker build -t springbootapp .                                           #Build Docker Image 
        
### STEP 1 >> Building Docker Image from Docker file
    
    docker image build -t sringbootapp .
    docker image tag sringbootapp vikashashoke/sringbootapp:1.0             # Version 1.0 (Optionl for version maintaining i did & pushed to dockerHUb for Kubernetes)
    docker image tag sringbootapp vikashashoke/sringbootapp:latest          # Version 1.0 Latest version
    
    
### STEP 2 >> Pushing Docker Images to DockerHub

    docker image push vikashashoke/sringbootapp:1.0          # Docker login needed before pushing the image
    docker image push vikashashoke/sringbootapp:latest   



### Running docker container with exposing port 9090

     docker run -td --name springbootapp -p 9090:9090 springbootapp:latest    #Detached mode
        
      
        
### 4.	Create a deployment YAML file for Kubernetes and deploy it on Minikube.


### Deployment.yml
   
      kind: Deployment
      apiVersion: apps/v1
      metadata:
         name: mydeployment
      spec:
         replicas: 2
         selector:     
            matchLabels:
               app: myspringbootapp
         template:
             metadata:
               labels:
                 app: myspringbootapp
             spec:
              containers:
               - name: myspringbootapp
                 image: vikashashoke/springbootapp:latest              # Docker Image we pushed to dockerHub now using here the same..
                 imagePullPolicy: Always
                 ports:
                 - containerPort: 9090
                 
### Step 1 >> Running deployment.yml on minikube
   
      kubectl apply -f deployment.yml
      
      kubectl get deploy  mydeployment      
      
### service.yml  (Optional Just for testing not mentioned in test)

     apiVersion: v1
     kind: Service
     metadata:
        name: my-nodeport-service
    spec:
      selector:
        app: myspringbootapp
      type: NodePort
      ports:
       - name: http
         port: 9090
         targetPort: 9090
         nodePort: 30036
         protocol: TCP

### Step 2 >> Exposing pods using service.yml (nodeport)


      kubectl apply -f service.yml
     
             
             
 
