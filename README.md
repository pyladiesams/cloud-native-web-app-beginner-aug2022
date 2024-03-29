
# An introduction to cloud-native web applications

### Level: Beginner 
### Presentation: [Introduction to Cloud-native Web Applications](workshop/intro-to-cloud-native-web-apps.pdf)

## Workshop description
During the workshop you will learn "how a website works" in detail, how a simple and modern web application works with the help of cloud computing.

In this workshop, we will go through these topics:

* A brief introduction to cloud computing
* What is a cloud-native web application (and where Python plays a role)
* Front-end
* Back-end
* Cloud Storage
* Message queue
* Microservices architecture

## Requirements
In general, this workshop will guide you through the concepts, elements and components in the field of cloud-native web applications. No prior experience with web development is totally fine.

At the end of the workshop, we will give a demo to deploy a Django App to Google Cloud Run, a managed compute platform that lets you run containers directly on Google's infrastructure. If you want to try it yourself, you can register your Google Cloud account at: https://console.cloud.google.com/.

Dependencies:
* [Docker 4.10.1](https://www.docker.com/products/docker-desktop/)
* [gcloud CLI](https://cloud.google.com/sdk/docs/install)

## Usage
* Download [`Dockerfile`](./workshop/django-cloud-run/Dockerfile) and [`requirements.txt`](./workshop/django-cloud-run/requirements.txt) to the same directory.
* Use `docker build -t app .` to build the context in the current diretory with the name `app`.
* Deploy the Docker image to Google Cloud. This will be given in the demo session in the presentation.

## Video record
Re-watch YouTube stream [here](https://youtu.be/x1wzl0prjBc)

## Credits
This workshop was set up by [@pyladiesams](https://github.com/pyladiesams) and [@xli12](https://github.com/xli12).
