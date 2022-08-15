# Deploy a Django App to Google Cloud Run

In this demo, we will deploy a Django App to Google Cloud Run.

## Prerequisites

- **A Google Cloud account.** You may visit https://console.cloud.google.com/ to register your Google Cloud account. Note that if you register the account for the first time, Google will offer you $300 free credit, and you need a credit card in order to finish the registration.
- **Install Docker Desktop.** You may download Docker Desktop from:
    - macOS (Intel): https://docs.docker.com/desktop/install/mac-install/
    - macOS (Apple silicon): https://docs.docker.com/desktop/mac/apple-silicon/
    - Windows: https://docs.docker.com/desktop/install/windows-install/
    - Linux: https://docs.docker.com/desktop/install/linux-install/
- **Install the Google Cloud CLI.** You may download and install the Google Cloud CLI via: https://cloud.google.com/sdk/docs/install-sdk#installing_the_latest_version.
- **Install Python 3.10.** You may find more information about installing Python at: https://www.python.org/downloads/.

## What is Docker?

Please refer to https://docs.docker.com/get-started/overview/ for more information about Docker.

## Dockerfile explained

We'll prepare a Dockerfile in order to build a Docker image.

```Dockerfile
FROM python:3.10-alpine
ENV PYTHONUNBUFFERED 1

WORKDIR /app
COPY requirements.txt .
RUN pip install -r requirements.txt
COPY . .
CMD python manage.py runserver 0.0.0.0:8080
```

- `FROM python:3.10-alpine`: This will install Python 3.10 Alpine. This image is based on the popular Alpine Linux project, available in the alpine official image. Alpine Linux is much smaller than most distribution base images (~5MB), and thus leads to much slimmer images in general.
- `ENV PYTHONUNBUFFERED 1`: Add an environment variable `PYTHONUNBUFFERED` and set the value to `1`. This allows us to see the logs in the Python App.
- `WORKDIR /app`: Create a working directory called `app`.
- `COPY requirements.txt .`: This is to copy the Python dependency specification file called `requirements.txt` to the working directory (note the `.` at the end).
- `RUN pip install -r requirements.txt`: Run the command `pip install -r requirements.txt` in order to install the dependency (Django) specified in the file `requirements.txt`.
- `COPY . .`: Copy all the files to the working directory after the installation finishes.
- `CMD python manage.py runserver 0.0.0.0:8080`: Execute the command `python manage.py runserver 0.0.0.0:8080` to start the Django server.

## `Requirements.txt`: Dependency specification file for Python

We only have one line in this file:

```
Django==4.0.0
```

We need Django to be installed.

## Install Django and create our first app

> Note: In the demo we'll use the virtual environment (https://docs.python.org/3/library/venv.html). The commands to activate the virtual environment may differ in different opearting systems. You may find [this article](https://tutorial.djangogirls.org/en/installation/#virtualenv) for more information about the virtual environment in Python.

We first upgrade pip (optional):

```
python -m pip install --upgrade pip
```

Then we install Django:

```
pip install -r requirements.txt
```

We finally create our first Django App which is called `app`:

```
django-admin startproject app .
```

## Change settings file for Django

In the `settings.py` file, you need to configure the `ALLOWED_HOSTS` parameter to specify the host/domain names the Django site can serve. In our case we'll change it to:

```Python
ALLOWED_HOSTS = ['127.0.0.1', '.run.app']
```

This allows Django to run in localhost and the domain `.run.app`, which is used for Cloud Run applications.

> Note: We deploy the app to Cloud Run only for the demo purpose. PLEASE NEVER commit the source file with `SECRET_KEY` written in the code to a production environment!! Use, for example, environment variable instead. For more information about this, please see: https://docs.djangoproject.com/en/4.1/howto/deployment/checklist/#secret-key.


## Build the Docker image

In order to build the image, we need to execute this command:

```
docker build -t app . --platform linux/amd64
```

This will name the image with `app`, and select what's in the current directory (`.` at the end) as the context to build.

## Push the Docker image to the Cloud (Google Container Registry)

To push the Docker image we just built to Google Container Registry (make sure you have the gcloud CLI installed):

```
docker tag app gcr.io/<google-project-id>/app
docker push gcr.io/<google-project-id>/app
```

If you encounter the error below:

```
unauthorized: You don't have the needed permissions to perform this operation, and you may have invalid credentials. To authenticate your request, follow the steps in: https://cloud.google.com/container-registry/docs/advanced-authentication
```

You may need to execute the command below in order to push the image to Google Container Registry:

```
gcloud auth configure-docker
```
