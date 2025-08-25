
Here are the core commands for managing the entire lifecycle of images and containers.

### **Managing Images**

- `docker pull [image]` - Downloads an image from a registry (like Docker Hub).
    
- `docker images` - Lists all the images stored locally.
    
- `docker rmi [image]` - Removes a specific local image.
    
- `docker build -t [tag_name] .` - Builds an image from a Dockerfile in the current directory.
    

### **Running Containers**

- `docker run [image]` - Creates and starts a new container from an image.
    
- `docker ps` - Lists all **running** containers.
    
- `docker ps -a` - Lists **all** containers, including stopped ones.
    
- `docker stop [container_id]` - Gracefully stops a running container.
    
- `docker start [container_id]` - Restarts a stopped container.
    
- `docker rm [container_id]` - Removes a stopped container.
    

### **Inspecting Containers**

- `docker logs [container_id]` - Fetches the logs from inside a container.
    
- `docker exec -it [container_id] /bin/sh` - Opens an interactive shell inside a running container (great for debugging!).
    

Think of these 12 commands as your foundation. Mastering them will allow you to handle most common Docker tasks with confidence.
