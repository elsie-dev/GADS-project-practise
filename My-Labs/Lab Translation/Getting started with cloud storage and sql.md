#LAB:GETTING STARTED WITH CLOUD STORAGE AND CLOUD SQL

Overview;
In this lab, you create a Cloud Storage bucket and place an image in it.You'll also configure an application running in Compute Engine to use a database managed by Cloud SQL.
For this lab, you will configure a web server with PHP, a web development environment that is the basis for popular blogging software.

Objectives

Create a Cloud Storage bucket and place an image into it.

Create a Cloud SQL instance and configure it.

Connect to the Cloud SQL instance from a web server.

Use the image in the Cloud Storage bucket on a web page.


#STEPS
1.DEPLOYING A WEB-SERVER VM INSTANCE.
(Specify the region)
gcloud config set region us-central1

(Create a vm instance,with firewall allow http and a startup script)
gcloud compute instances create bloghost --tags http\
--metadata startup-script ='#!/bin/bash
# installing apache
apt-get update;
apt-get install apache2 php php-mysql-y
service apache2 restart
gcloud compute firewall-rules create MYRULE --action ALLOW --destination INGRESS --rules http:80 --target-tags http-server

(Once the instance-bloghost is running,copy bloghost internal and external ip fot later use)


2.CREAT A BUCKET USING COMMAND LINE.
(Using the below command,a bucket called bopm3 is created in us)
gcloud mb -l us gs:// bopm3

(Retrieve a banner image from the public that will later be used using...)
gsutil cp gs://cloud-training/gcpfci/my-excellent-blog.png my-excellent-blog.png

(Copy the banner image to our bucket-bopm3)
gsutil cp my-excellent-blog.png gs://bopm3/my-excellent-blog.png

(Then modify the bucket using acl to be read by everyone)
gsutil acl ch -u allUsers:R gs://bopm3/my-excellent-blog.png


3. CREAT THE CLOUD SQL INSTANCE.
(Creating an instance ld of type blog-db and any password)
gcloud sql instances create blog-db --region=us-central1 --root-password=password 234

(adding user account in the instance,with name as blogdbuser and any password)
gcloud sql users create blogdbuser\
--host=% --instances=blog-db --password=password 123client

(adding network with name web front end)
gcloud sql instances patch blog-db --authorized-networks=35.192.208.2/32
35.192.208.2/32- is the external ip of bloghost we copied from step1


4.CONFIGURE AN APPLICATION IN COMPUTE ENGINE TO USE SQL INSTANCE.
(Using the ssh command on bloghost we change the working directory to document root of web-server)
gcloud compute ssh --project PROJECT_ID --zone ZONE VM_NAME
{
PROJECT_ID - your project's id
ZONE - your default zone
VM_NAME - bloghost
}

(changing directory using...)
cd /var/www/html

(To use the nano text editor we run...)
sudo nano index.php

(Then paste the file...)
curl http://htmlfile/


(Later restart the web-server...)
sudo service apache2 restart

(Then on a new browser we paste bloghost external ip...)
35.192.208.2/index.php

The connection fails because we haven't configured with our sql instance

(Return to ssh on bloghost and using text editor replace CLOUDSQLIP with sql instance public ip address)
(Also replacing DBPASSWORD with cloud sql password we created earlier)
sudo nano index.php
(Then save,exit the nano using cltr+O and cltr+X and finally restart our web-server)
sudo service apache2 restart


5.CONFIGURE AN APPLICATION IN COMPUTE ENGINE INSTANCE TO USE A CLOUD STORAGE OBJECT.
(First list the bucket present)
gsutil ls -r gs://bopm3

(Open ssh bloghost Vm instance,and setting the web directory)
gcloud compute ssh --project PROJECT_ID --zone ZONE VM_NAME
cd /var/www/html

(Open nano text editor,save using ctrl+O and ctrl+X)
<img src=''>

(Then restart the webserver)
sudo service apache2 restart

Expecting "database connection successful" because the database instance has been successfully configured.