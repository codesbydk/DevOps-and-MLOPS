[[Docker cheatsheet]]

**Day 1: Docker Fundamentals**. 🚀

Our goal today is to understand the "why" behind Docker before we get to the "how."

---
## The Core Problem: "It works on my machine!"

Imagine you're a developer. You build a fantastic new web application on your laptop. It uses a specific version of Python, a particular database, and a bunch of other libraries. It works perfectly.

Now, you hand it over to your colleague to test, or you try to deploy it to a server. Suddenly, nothing works. This is a classic problem. Why? Because their machine or the server has:

- A different operating system.
- A different version of Python.
- Missing libraries.

This creates a messy, unpredictable environment. Docker was created to solve this very problem.

### The Analogy: Shipping Containers 📦

Think about how international shipping works. Before shipping containers, loading a ship was chaotic. You had barrels of wine next to sacks of coffee next to crates of electronics. It was slow, inefficient, and things often broke.

Then, the standardized shipping container was invented. Now, it doesn't matter what's _inside_ the container. A crane can pick up a container of electronics just as easily as a container of coffee because they all have the same standard shape and fittings.

**Docker is exactly like a shipping container, but for software.**

A Docker container wraps up your application code, along with all its dependencies (libraries, settings, etc.), into a single, standardized unit. This "container" can then be run on any machine that has Docker installed, and it will work identically everywhere.

Now that we understand the "why," let's compare Docker containers to a more traditional technology you might have heard of: **Virtual Machines (VMs)**. This will clarify why Docker is so efficient.

---

### Containers vs. Virtual Machines

Imagine you want to rent out a property.

A **Virtual Machine** is like building a complete, separate house for each tenant. Each house has its own foundation, plumbing, electrical system, and roof (the **Guest Operating System**), in addition to the furniture (your **app and its dependencies**). This is very isolated and secure, but it's also slow to build and uses a lot of resources (land, materials).

A **Docker Container**, on the other hand, is like renting out apartments in a large apartment building. The building already has the main foundation, plumbing, and structure (the **Host Operating System**). Each apartment (container) only needs to contain the furniture (your **app and its dependencies**). They share the underlying infrastructure, making them much faster to set up and far more lightweight.

Here's the key takeaway:

- **VMs** virtualize the **hardware**, so each VM needs its own full operating system.
    
- **Containers** virtualize the **operating system**, so they share the host's OS kernel.
    

This makes containers much smaller, faster to start, and allows you to run many more of them on a single machine compared to VMs.

---

### The Core Docker Components

Now, let's learn the three most important terms in the Docker world:

1. **Docker Engine**: This is the underlying Docker software that runs on your machine. It's the "crane" and the "shipping yard" from our analogy. It's responsible for building, running, and managing containers. You interact with it using Docker commands.

2. **Image**: This is a **blueprint** or a **template**. It’s a read-only file that contains everything needed to run your application: the code, a runtime (like Python), libraries, and environment variables. You don't run an image directly. Instead, you use it to create a container.

3. **Container**: This is a **running instance** of an image. If the image is the blueprint for a house, the container is the actual house you built from that blueprint. You can create, start, stop, and delete containers. You can have many running containers all created from the same single image.


So, the workflow is: You use the **Docker Engine** to create a running **Container** based on a read-only **Image**.

this distinction between an Image (the blueprint) and a Container (the running instance) hope it makes sense.


Docker uses a **client-server architecture**. It consists of three main components: the Docker **Client**, the Docker **Daemon** (or server), and a Docker **Registry**.

---

## Docker's Three Main Components

Think of it like ordering food at a restaurant:

1. **The Docker Client (`docker`):** This is **you**, the customer. It's the command-line tool that you interact with by typing commands like `docker run` or `docker build`. Your command is the "order" you place.

2. **The Docker Daemon (`dockerd`):** This is the **kitchen**. It's a background process that listens for commands from the Docker Client. The Daemon does all the heavy lifting: it builds the images, runs the containers, and manages everything (networks, storage, etc.). It's the brain of the operation.

3. **The Docker Registry:** This is the restaurant's giant **storeroom or pantry**. It's where Docker images are stored. The default public registry is **Docker Hub**, but you can also have private registries. When you ask for an image that isn't on your local machine, the Daemon pulls it from a registry.
    

---

## How They Work Together: A Practical Example

Let's see how they interact when you run a simple command like `docker run hello-world`:

1. **You type the command:** You enter `docker run hello-world` into your terminal. This command is sent to the **Docker Client**.

2. **Client talks to Daemon:** The Docker Client tells the **Docker Daemon** that you want to run a container from the `hello-world` image.

3. **Daemon checks locally:** The Daemon looks on your local machine to see if it already has the `hello-world` image.

4. **Daemon pulls from Registry:** If it can't find the image locally, the Daemon connects to the **Docker Registry** (Docker Hub by default) and pulls the `hello-world` image down to your machine.

5. **Daemon creates the Container:** Once the image is available, the Daemon uses it as a blueprint to create and start a new **Container**.

6. **Daemon sends output back:** The Daemon streams the output from the running container back to the **Docker Client**, which then displays the "Hello from Docker!" message on your screen.


So, while it looks like you're doing everything from your terminal, it's actually the Daemon doing all the real work in the background.

This separation is powerful because your Docker Client could even be on a different machine from the Docker Daemon, allowing you to control a remote Docker Engine from your own laptop.

**Why it is called a client server architecture?**

It's called a client-server architecture because it is split into two main, independent parts that have distinct roles: a **client** that makes requests and a **server** that fulfills those requests. They are designed to work together but are not a single, monolithic program.

### The Two Key Roles

1. **The Client (The "Asker")** 🗣️ The Docker **client** is the `docker` command you type into your terminal. Its only job is to take your instructions (like `docker run` or `docker build`) and send them as a request to the server. The client doesn't know how to build an image or run a container; it only knows how to ask the server to do it.
    
2. **The Server (The "Doer")** 🛠️ The Docker **daemon** (`dockerd`) is the **server**. It runs in the background, constantly listening for requests from the client. When it receives a request, it performs the actual work: it pulls images, creates containers, manages networks, and handles storage.
    

---

### Why This Separation Matters

This separation is the defining feature. Think about how you use the internet:

Your web browser (like Chrome or Safari) is a **client**. When you type in a website address, your browser sends a request to a remote web **server**. That server finds the webpage and sends it back to your browser to display.

The same principle applies to Docker. The client and server are separate processes. While they usually run on the same machine for a developer, they don't have to. You could have the Docker client on your laptop and command a powerful Docker daemon (server) running in the cloud.

In short, it's a client-server model because the **client gives the orders** and the **server does the work**.

**So what is the daemon and how does it work?**

The Docker daemon, technically named `dockerd`, is the persistent background service that acts as the brain and engine of Docker. 🧠 It's the "server" in the client-server architecture we just discussed.

While you interact with the Docker client in your terminal, the daemon is the component that quietly does all the actual work behind the scenes.

---

## The Engine Room of Docker

Think of the Docker daemon as the engine room of a ship. The captain on the bridge (the client) gives an order like "full speed ahead," but it's the engineers in the engine room (the daemon) who actually manipulate the engines, manage fuel flow, and make the ship move. You don't see them, but nothing happens without them.

---

## How It Works

The daemon's job is to listen for instructions and manage all the Docker objects on your machine.

Here’s a breakdown of its core functions:

- **Listens for API Requests:** The daemon exposes a secure Application Programming Interface (API). The Docker client communicates with the daemon by sending it API requests. This is how your `docker run` command is translated into an action.
    
- **Manages Images:** It handles everything related to images. When you ask to run a container, the daemon first checks for the image locally. If it's not there, the daemon connects to a Docker registry (like Docker Hub) and pulls the image for you. It also builds new images from a `Dockerfile`.
    
- **Manages Containers:** This is its most visible job. The daemon creates, starts, stops, and deletes containers. It interacts directly with the host operating system's kernel to provide the process isolation and resource management that containers need.
    
- **Manages Networking and Storage:** The daemon sets up the virtual networks that allow containers to communicate with each other and the outside world. It also manages **volumes**, which are used to persist data even after a container is deleted.
    

So, when you type `docker ps` to list running containers, the client simply asks the daemon, "What containers are running?" The daemon checks its list and sends the information back to the client to display on your screen.

Essentially, the daemon is the central management hub for your entire Docker environment.


**why in docker we call it like layer by layer architecture for the images?**
Docker images use a layered architecture primarily for **efficiency**. Each instruction in a `Dockerfile` (the blueprint for an image) creates a new, read-only layer stacked on top of the previous one.

---

### The Analogy: Transparent Sheets

Imagine you're creating a drawing using a stack of transparent sheets.

1. **Base Layer:** Your first sheet is the background, perhaps a map of a country. This is your `FROM` instruction (e.g., `FROM ubuntu`).
    
2. **Second Layer:** You place a new sheet on top and draw the major cities. This could be an instruction like `RUN apt-get install python3`.
    
3. **Third Layer:** You add another sheet on top and draw the roads connecting the cities. This could be your `COPY my_app /app` instruction.
    

When you look through the stack, you see the complete picture: a map with cities and roads. Each sheet is a **layer**. It only contains the changes you made on that specific step. The final image is a combination of all these layers.

---

### How It Works in Practice

Consider a simple `Dockerfile`:

Dockerfile
```
# Layer 1: Base Image
FROM python:3.9-slim

# Layer 2: Set working directory
WORKDIR /app

# Layer 3: Copy requirements file
COPY requirements.txt .

# Layer 4: Install dependencies
RUN pip install -r requirements.txt

# Layer 5: Copy application code
COPY . .

# Layer 6: Set the command to run
CMD ["python", "app.py"]
```

Each of these commands creates a new layer. The `python:3.9-slim` image itself is made of its own set of layers. Your new image simply adds more layers on top.

---

### The Three Big Advantages

This layered approach provides three huge benefits:

- **Storage Efficiency:** If you have 10 different images that all use `FROM python:3.9-slim` as their base, Docker is smart enough to store the Python base layers **only once** on your machine. The 10 images just add their own small, unique layers on top, saving a significant amount of disk space.
    
- **Faster Builds (Caching):** When you rebuild an image, Docker checks each layer. If an instruction in the `Dockerfile` hasn't changed, Docker uses the existing layer from its cache instead of rebuilding it. This makes subsequent builds incredibly fast, as only the changed layers are recreated.
    
- **Quicker Transfers:** When you `pull` or `push` an image, Docker only transfers the layers that don't already exist at the destination. If you update your application code (Layer 5) and push a new image, you only send that one small layer, not the entire base image and dependencies every time.
    

In short, the layered architecture is a clever design that makes building, storing, and sharing images remarkably fast and lightweight.

### **Step 1: Pull the `alpine` Image**

First, let's download the `alpine` image from Docker Hub to your local machine. It's a tiny but complete Linux operating system, perfect for testing.

Go to your terminal and run this command:

Bash

```
docker pull alpine
```

You should see output showing that Docker is downloading the image, often displaying the progress for each layer.

### **Step 2: List Your Local Images**

Next, let's verify that the image was successfully downloaded. To see all the images on your computer, run this command:

Bash

```
docker images
```

The output will be a table that should now include an entry for `alpine`, showing its tag, ID, and small size.

pulled an image, which is the first major step. Now you have a blueprint ready to go.

The next logical step is to use that blueprint (**image**) to create an actual running instance (a **container**).

---

## From Image to Container: `docker run`

The `docker run` command is the most important one in Docker. It creates a new container from a specified image and then starts it.

Let's run a container from our `alpine` image. We'll also tell the container to execute a simple command for us: `ls -l`, which lists the files in the root directory _inside_ the container.

In your terminal, run this:

Bash

```
docker run alpine ls -l
```

You'll see a list of directories like `bin`, `etc`, `home`, and `usr`. This is the filesystem _inside_ the Alpine Linux container, not on your own machine.

Here's what just happened:

1. Docker created a new container from the `alpine` image.
    
2. It ran the command `ls -l` inside that container.
    
3. After the command finished, the container stopped because it had nothing else to do.
    

---

## Checking on Your Containers: `docker ps`

So, where did the container go? The `docker ps` command shows you all the containers that are currently **running**.

Try it now:

Bash

```
docker ps
```

You'll likely see an empty list. This is because our container stopped as soon as its `ls -l` job was done.

To see **all** containers, including stopped ones, you need to add the `-a` flag.

Bash

```
docker ps -a
```

Now you'll see your container listed, along with a status like "Exited (0) ... ago". This confirms you created and ran a container.

- `docker ps` = Shows only the **currently running** containers.
    
- `docker ps -a` = Shows **all** containers, both running and exited.
    

---

## Running a Container in the Background

Our last container stopped immediately because its job was done. But what if you want to run a container that keeps running in the background, like a web server or a database?

For this, we use the **detached mode** flag: `-d`.

Let's try running a container that continuously "pings" an address. This process won't finish, so the container will stay alive.

Run this command:

Bash

```
docker run -d alpine ping 8.8.8.8
```

This time, Docker will print a very long string of characters. This is the unique **Container ID**. The container is now running in the background.

Now, check your running containers again:

Bash

```
docker ps
```

You should now see your `alpine` container running, with the command `ping 8.8.8.8`.

---

## Cleaning Up: Stopping and Removing a Container

To stop the container from running, you use the `docker stop` command with the first few unique characters of its Container ID.

1. **Stop the container:**
    ```
    # Replace '123abcde' with the first few characters of YOUR container ID
    docker stop 123abcde 
    ```
    
2. **Remove the container:** A stopped container still exists. To permanently remove it, use `docker rm`.
```
 docker rm 123abcde
```

we've just mastered the fundamental lifecycle of a Docker container: pulling an image, running it, checking its status, stopping it, and finally, removing it. That's a huge milestone.

You've covered all the core concepts of **Docker Fundamentals**:

- Why we need Docker (the "it works on my machine" problem).
    
- The difference between an **Image** (a blueprint) and a **Container** (a running instance).
    
- Docker's efficient layered architecture.
    
- The essential commands like `run`, `ps`, `stop`, and `rm`.
    

This is a fantastic foundation to build upon.

---

Our next topic is **Containerizing Applications with Docker**. This is where things get really creative. We'll move from using pre-built images like `alpine` to building your very own custom image for an application using a `Dockerfile`.

## What is a Dockerfile?

A **`Dockerfile`** is simply a text file that contains a list of instructions, like a recipe. Each instruction tells Docker how to assemble your application into a custom image, step-by-step. Docker reads this file from top to bottom and executes each command to create the final image.

---

## Our First Project: A Simple Web App

We're going to containerize a very simple Python web application. Don't worry if you don't know Python; the concepts apply to any programming language.

Our goal is to create an image that runs a web server displaying "Hello from your first Docker App!".

First, create a new folder on your computer for this project. Call it something like `docker-hello`. Inside this folder, you'll need to create two files:

1. **`app.py`** (Our application code)
```
    from flask import Flask
    
    app = Flask(__name__)
    
    @app.route('/')
    def hello():
        return "<h1>Hello from your first Docker App!</h1>"
    
    if __name__ == '__main__':
        app.run(host='0.0.0.0', port=5000)
```
    
2. **`requirements.txt`** (Our app's dependencies)
    
    ```
    Flask==2.0.1
    ```
    

---

## Writing the Dockerfile

Now, inside that same `docker-hello` folder, create the most important file: `Dockerfile` (no extension, just that name).

This file will tell Docker how to build our image. Add the following content to it:

Dockerfile

```
# 1. Start from an official Python base image
FROM python:3.9-slim

# 2. Set the working directory inside the container
WORKDIR /app

# 3. Copy the requirements file and install dependencies
COPY requirements.txt .
RUN pip install -r requirements.txt

# 4. Copy the rest of the application code
COPY . .

# 5. Expose the port the app runs on
EXPOSE 5000

# 6. Define the command to run the application
CMD ["python", "app.py"]
```

Go ahead and create your `docker-hello` folder with these three files inside: `app.py`, `requirements.txt`, and `Dockerfile`.


The `.` (dot) in the `COPY requirements.txt .` command means **"copy it to the current working directory inside the container."**

---
### The Role of `WORKDIR`

The key is the instruction that comes right before it in our `Dockerfile`:

Dockerfile

```
WORKDIR /app
```

This command does two things:

1. If the `/app` directory doesn't exist inside the container, it creates it.
    
2. It sets this directory as the current working directory. Think of it as running the `cd /app` command inside your container for all subsequent instructions.
    

---

### Putting It Together

So, when Docker processes your file, here's the sequence of events:

1. **`WORKDIR /app`**: Docker "steps into" the `/app` directory inside the container.
    
2. **`COPY requirements.txt .`**: Docker now copies the `requirements.txt` file from your project folder on your computer (the build context) directly into the `/app` directory inside the container (because that's where it's "currently standing").
    

So, after that line runs, the file path inside the container will be `/app/requirements.txt`.

Now for the exciting part: bringing your application to life. We'll do this in two steps: building the image from your `Dockerfile`, and then running a container from that new image.

---

## Step 1: Build Your Custom Image

Navigate your terminal into the `docker-hello` folder you created. This is crucial because the `.` at the end of the next command tells Docker to look for the `Dockerfile` in your current directory.

Run the following `docker build` command:

Bash

```
docker build -t my-first-app .
```

Let's break that down:

- `docker build`: The command to build an image from a `Dockerfile`.
    
- `-t my-first-app`: The `-t` flag stands for "tag." It gives your new image a memorable name, in this case, `my-first-app`.
    
- `.`: This tells Docker that the build context (your `Dockerfile` and application files) is in the current directory.
    

You will see Docker go through each step in your `Dockerfile`, creating a layer for each instruction.

---

## Step 2: Run Your Application

Once the build is complete, you have a new image ready to go. Now, run it using the `docker run` command, with one important new flag: `-p`.

Bash

```
docker run -p 5000:5000 my-first-app
```

Here’s the breakdown:

- `docker run`: The command to start a container.
    
- `-p 5000:5000`: This is **port mapping**. It connects your computer's port (the first `5000`) to the container's internal port (the second `5000`). This is what lets you access the web app running inside the container from your browser.
    
- `my-first-app`: The name of the image you just built.
    

---

## Step 3: See It in Action!

After running the command, your terminal will look like it's "stuck," but that's a good sign! It means your web server is running.

Now, open your web browser and go to this address:

**`http://localhost:5000`**

You should see your message: "Hello from your first Docker App!".


But we might have faced a error saying "ImportError: cannot import name 'url_quote' from 'werkzeug.urls'"

This is a classic dependency version mismatch error, and it's a perfect example of why Docker is so useful for preventing these kinds of problems! Don't worry, this is a very common issue and easy to fix.

The error `ImportError: cannot import name 'url_quote' from 'werkzeug.urls'` happens because the version of **Flask** (`2.0.1`) you specified is trying to use a function that no longer exists in a newer version of its dependency, **Werkzeug**.

---

## The Solution

We just need to update the version of Flask in your `requirements.txt` file to a more recent one that is compatible with the latest version of Werkzeug.

1. **Stop the running container.** Go to the terminal where the container is running and press `Ctrl + C`.
    
2. **Update `requirements.txt`**. Change the content of your `requirements.txt` file to a newer version of Flask. Let's use version `2.3.2`, which is known to be stable.
    
    ```
    Flask==2.3.2
    ```
    
3. **Rebuild your image.** Since you've changed a file, you need to rebuild the image so your changes are included. Docker is smart and will use the cache for the unchanged layers, so it will be very fast.
    
    Bash
    
    ```
    docker build -t my-first-app .
    ```
    
4. **Run the container again.** Now run the container using the newly built image.
    
    Bash
    
    ```
    docker run -p 5000:5000 my-first-app
    ```
    

Now, try accessing `http://localhost:5000` in your browser again. It should work perfectly.

This experience highlights a key reason for specifying exact versions in a `requirements.txt` file: it creates a predictable, repeatable environment, which is the core goal of using Docker.

Congratulations on building your first custom Docker image and running a containerized web application. 🥳

You've not only successfully written a `Dockerfile` but also diagnosed and fixed a real-world dependency issue. That's a huge step.

To recap, in this section you've learned:

- How to write a `Dockerfile` as a recipe for your application.
    
- What key instructions like `FROM`, `WORKDIR`, `COPY`, and `RUN` do.
    
- How to build a custom image using `docker build -t`.
    
- How to run your application and expose it to the world with `docker run -p`.
    

Before we move on, go to the terminal where your app is running and press **`Ctrl + C`** to stop the container.

