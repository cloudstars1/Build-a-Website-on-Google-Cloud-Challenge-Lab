# Build a Website on Google Cloud: Challenge Lab || GSP319

This lab demonstrates how to deploy a monolithic application and convert it into microservices on Google Kubernetes Engine (GKE). Below are the steps to clone the application, build the necessary images, and deploy them to your Kubernetes cluster.


## Setup

### Export Environment Variables

Make sure to set the following environment variables:

```
export ZONE=
export MONOLITH_IDENTIFIER=
export CLUSTER_NAME=
export ORDERS_IDENTIFIER=
export PRODUCTS_IDENTIFIER=
export FRONTEND_IDENTIFIER=
```

### Run the following Commands in the Cloud shell

```
gcloud auth list

git clone https://github.com/googlecodelabs/monolith-to-microservices.git
cd ~/monolith-to-microservices
./setup.sh

cd ~/monolith-to-microservices/monolith
timeout 30 npm start

gcloud services enable cloudbuild.googleapis.com
gcloud builds submit --tag gcr.io/${GOOGLE_CLOUD_PROJECT}/$MONOLITH_IDENTIFIER:1.0.0 .

gcloud config set compute/zone $ZONE
gcloud services enable container.googleapis.com
gcloud container clusters create $CLUSTER_NAME --num-nodes 3

kubectl create deployment $MONOLITH_IDENTIFIER --image=gcr.io/${GOOGLE_CLOUD_PROJECT}/$MONOLITH_IDENTIFIER:1.0.0
kubectl expose deployment $MONOLITH_IDENTIFIER --type=LoadBalancer --port 80 --target-port 8080

cd ~/monolith-to-microservices/microservices/src/orders
gcloud builds submit --tag gcr.io/${GOOGLE_CLOUD_PROJECT}/$ORDERS_IDENTIFIER:1.0.0 .

cd ~/monolith-to-microservices/microservices/src/products
gcloud builds submit --tag gcr.io/${GOOGLE_CLOUD_PROJECT}/$PRODUCTS_IDENTIFIER:1.0.0 .

kubectl create deployment $ORDERS_IDENTIFIER --image=gcr.io/${GOOGLE_CLOUD_PROJECT}/$ORDERS_IDENTIFIER:1.0.0
kubectl expose deployment $ORDERS_IDENTIFIER --type=LoadBalancer --port 80 --target-port 8081

kubectl create deployment $PRODUCTS_IDENTIFIER --image=gcr.io/${GOOGLE_CLOUD_PROJECT}/$PRODUCTS_IDENTIFIER:1.0.0
kubectl expose deployment $PRODUCTS_IDENTIFIER --type=LoadBalancer --port 80 --target-port 8082

cd ~/monolith-to-microservices/react-app

cd ~/monolith-to-microservices/microservices/src/frontend
gcloud builds submit --tag gcr.io/${GOOGLE_CLOUD_PROJECT}/$FRONTEND_IDENTIFIER:1.0.0 .

kubectl create deployment $FRONTEND_IDENTIFIER --image=gcr.io/${GOOGLE_CLOUD_PROJECT}/$FRONTEND_IDENTIFIER:1.0.0
kubectl expose deployment $FRONTEND_IDENTIFIER --type=LoadBalancer --port 80 --target-port 8080
```

### Congratulations 🎉 for completing the Lab !

##### You Have Successfully Demonstrated Your Skills And Determination.

#### *Well done!*

#### Don't Forget to Join the [Telegram Channel](https://t.me/cloudstars24)

### Subscribe to our YouTube Channel [CLOUD STARS](https://www.youtube.com/@cloud-stars)
