# Containerizing Baby Tools Shop with Docker

This guide walks you through the steps required to containerize a **Baby Tools Shop** application using Docker. It covers setting up the Docker environment, writing the Dockerfile, building the image, and running the container.

---

## Table of Contents

1. [Requirements](#requirements)  
2. [Quickstart](#quickstart)   
3. [Dockerfile Setup](#dockerfile-setup)   
4. [Building the Docker Image](#building-the-docker-image)  
5. [Accessing the Application](#accessing-the-application)  

---

## 1. Requirements

Ensure the following are installed on your **Ubuntu** system:

### Software

- **Docker**  
  ```bash
  sudo apt update
  sudo apt install -y docker
    ```
  
- **Python**  
  ```bash
  sudo apt install -y python3.10
    ```
## 2. Quickstart 

Follow these steps to quickly run the Baby Tools Shop app using Docker:

```bash
# Step 1: Clone the project (or copy files into a folder)
git clone https://github.com/e1pmiS/baby-tools-shop.git
cd baby-tools-shop/babyshop_app

# Step 2: Build the Docker image
docker build -t baby_shop:latest .

# Step 3: Run the container
docker run -it -p 8025:8000 baby_shop:latest
```

After Step 3, the Baby Tools Shop Django application will be running inside a Docker container, and it will be accessible externally on port 8025 of your host machine.

## 3. Dockerfile Setup

This section explains how to create a Dockerfile to containerize the **Baby Tools Shop** Django application.

---

### Step-by-Step Dockerfile

1. **Create a file named `Dockerfile` in the directory babyshop_app/

2. **Add the following contents into the Dockerfile:**

Choose a base image that provides Python 3.10 to ensure the right runtime environment:

```Dockerfile
FROM python:3.10
```

Set environment variables to prevent Python from creating .pyc files and to ensure output logs show immediately:

```Dockerfile
ENV PYTHONDONTWRITEBYTECODE=1
ENV PYTHONUNBUFFERED=1
```

Define a working directory inside the container where all commands will be run and files copied:

```Dockerfile
WORKDIR /babyshop_app
```

Update system packages and install any needed OS dependencies:

```Dockerfile
RUN apt-get update && apt-get install -y
```

Copy your Python dependencies file so you can install them inside the container:

```Dockerfile
COPY requirements.txt /babyshop_app/
```

Upgrade pip and install the Python packages listed in requirements.txt:

```Dockerfile
RUN pip install --upgrade pip && pip install -r requirements.txt
```

Copy all your project files into the container’s working directory:

```Dockerfile
COPY . $WORKDIR
```

Expose port 8000 so the container can listen for incoming traffic on Django’s default port:

```Dockerfile
EXPOSE 8000
```

Set the default command to run the Django development server, making it accessible from any IP:

```Dockerfile
CMD ["python3", "manage.py", "runserver", "0.0.0.0:8000"]
```

## 4. Building the image

After creating the Dockerfile, you need to build the Docker image that packages your Baby Tools Shop app by exuting the following command:

```Bash
docker build -t baby_shop:latest .
```