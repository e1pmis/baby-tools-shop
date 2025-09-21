# Baby Tools Shop - Docker setup

This guide walks you through the steps required to containerize a Django application using Docker. It covers setting up the Docker environment, writing Dockerfile, building the image, and running the container.

---

## Table of Contents

- [Baby Tools Shop - Docker setup](#baby-tools-shop---docker-setup)
  - [Table of Contents](#table-of-contents)
  - [Prerequisites](#prerequisites)
  - [Quickstart](#quickstart)
  - [Usage](#usage)
    - [Creating a Django Superuser](#creating-a-django-superuser)
    - [Logging into Django Admin and Managing Products](#logging-into-django-admin-and-managing-products)
    - [External Deployment](#external-deployment)
  - [Explanation](#explanation)
    - [Dockerfile Setup](#dockerfile-setup)

---

## Prerequisites

- **Docker**  

---

## Quickstart 

Follow these steps to quickly run the Baby Tools Shop app using Docker:

1. Clone the project (or copy files into a folder)
```bash
git clone https://github.com/e1pmiS/baby-tools-shop.git
cd baby-tools-shop/babyshop_app
```

2. Build the Docker image

```bash
docker build -t baby_shop:latest .
```

3. Run the container
   
```bash
docker run -it -p 8025:8000 baby_shop:latest
```

After Step 3, the Baby Tools Shop Django application will be running inside a Docker container, and it will be accessible externally on port 8025 of your host machine.

## Usage

### Creating a Django Superuser

To manage products and categories on the admin interface, you need a Django superuser.

Follow these steps:

1. **List running containers to find your Django container ID**:

```bash
docker container list -a
```

2. **Access the running container's shell**:

```bash
docker exec -it <container_id> bash
```
3. **Run the Django management command**:

```bash
python3 manage.py createsuperuser
```
Follow the prompt and once complete, a superuser account will be created in the database.

### Logging into Django Admin and Managing Products

After creating a superuser, you can log in and start managing your shop.

1. Open your browser and go to:

```bash
http://localhost:8025/admin
```

2. Log in using the superuser credentials you just created.

3. In the Django admin dashboard, you can:

* Add Categories (e.g., Strollers, Toys, Clothing)

* Add Products under specific categories

* Edit or delete existing items

### External Deployment

To publish the server externally, you need to configure Django to allow requests from your external IP address or domain.

Open your [settings.py](settings.py) file and find the ALLOWED_HOSTS setting. Add your server's IP address or domain name:

```python
ALLOWED_HOSTS = ['your.server.ip.address', 'localhost']
```

## Explanation

This section explains how to create a Dockerfile to containerize a Django application.

---

### Dockerfile Setup

Create a file named `Dockerfile` in the root directory.

Choose a base image that provides Python 3.10-slim to ensure the right runtime environment with minimal installations:

```Dockerfile
FROM python:3.10-slim
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

Copy your Python dependencies file so you can install them inside the container:

```Dockerfile
COPY requirements.txt .
```

Upgrade pip and install the Python packages listed in requirements.txt:

```Dockerfile
RUN pip install --upgrade pip && pip install -r requirements.txt
```

Copy all your project files into the container’s working directory:

```Dockerfile
COPY . $WORKDIR
```

Expose port 8000 to allow external access to the Django development server running inside the container:

```Dockerfile
EXPOSE 8000
```

Set the default command to run database migrations and then start the Django development server, making it accessible from any IP:

```Dockerfile
CMD python3 manage.py migrate && python3 manage.py runserver 0.0.0.0:8000
```
---
