# SpringBoot_Counter_Application
 
 
 ### 1.	Create a spring boot app that prints “hello-world-$counter” ($counter should increment on every refresh)
 
 
 
 ### 2. Create an Uber jar for the spring boot app and execute your jar from the command line.
 
 
 
 
 ### 3.	Create Docker image for the spring boot app and run it as Docker container exporting the URL port (9090).
 
 
        docker build -t springbootapp .                                           #Build Docker Image 
  
        docker run -td --name springbootapp -p 9090:9090 springbootapp:latest    
        
        
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
             image: springbootapp:latest              # Docker Image we pushed to dockerHub now using here the same..
             imagePullPolicy: Always
             ports:
             - containerPort: 80
             
             
 
