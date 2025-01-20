[Docker For Open Source Contributors - Part 2 - YouTube](https://www.youtube.com/watch?v=xPT8mXa-sJg&t=366s)
### Part 1

1. Problem Statement
    
2. Installation of Docker CLI and Desktop
    
3. Understanding Images v/s Containers
    
4. Running Ubuntu Image in Container
    
5. Multiple Containers
    
6. Port Mappings - 
   Just  put `-p 8000:9000` flag, right one is the container internal port, left one is the host machine port.
    
7. Environment Variables
   Put `-e key=value` flag in the command for passing environment vars.
    
8. Dockerization of Node.js Application
    
    1. Dockerfile
        
    2. Caching Layers
        
    3. Publishing to Hub
        
9. Docker Compose
   Gives the ability to spin up multiple containers by just executing one single file.
   we can specify the config for each container and then spin them up no problem from one single file.
    
    1. Services
        
    2. Port Mapping
        
    3. Env Variables
        

### Part 2

1. Docker Networking
    
    1. Bridge
        
    2. Host
        
2. Volume Mounting
    
3. Efficient Caching in Layers
    
4. Docker Multi-Stage Builds