##LAB: GETTING STARTED WITH VIRTUAL MACHINES 


OVERVIEW;Creating two vm instances,one using the console the other using a command line and connecting them.

Objectives;
Create a Compute Engine virtual machine using the Google Cloud Platform (GCP) Console.

Create a Compute Engine virtual machine using the gcloud command-line interface.

Connect between the two instances.

# STEPS
0. SETTING UP THE LAB.
(Run this command,replacing project ID with the one allocated)
gcloud config set project project-ID

(Run this commnad to set the zone to us-central1-a)
gcloud config set compute/zone us-central1-a 


1. CREATE VM IN THE CONSOLE.
cloud compute instangces create my-vm-1 --machine-type "n1-standard-2" --image-project "debian-cloud" --subnet default --tags:http
(allowing firewall rule)
gcloud compute firewall-rules create MYRULE --action ALLOW --destination INGRESS --rules http:80 --target-tags:http


2. CREATING VM USING COMMAND LINE.
(This command lists available zones and sets our one to us-central1)
gcloud compute zones list | grep us-central1

(This command creats our second vm)
gcloud compute instangces create my-vm-2 --machine-type "n1-standard-2" --image-project "debian-cloud" --subnet default --tags:http

3. CONNECTING BETWEEN VM INSTANCES.
(To connect between instances we use ssh and ping to confirm instance can be reached from vm1 to vm2)
gcloud compute ssh my-vm-2
ping -c 4 my-vm-1

(use shh command to open command on my-vm-1)
ssh my-vm-1

(installing the nginx web server to vm 1)
sudo apt-get install nginx-light -y

(Use a nano text editor to view and edit the file)
sudo nano /var/www/html/index.nginx-debian.html
This command opens up text editor and we add a custom message to the webserver
Hi from elsie
 then exit nano using:ctrl+X before saving using ctrl+o 

(To confirm that the webserver is serving our newpage we ran...)
curl http://localhost/

(To exit vm1 to go to vm2 exit using...)
exit

(Once more confirm the vm2 can reach the webserver using...)
curl http://my-vm-1/

Now get the external ip and list vm instances in our zone
gcloud compute instances list --zone us-central1-a
 
 With that the lab becomes succesfully deployed.



