# TourPlaces Vagrant Cluster Deployment

## Overview

This project sets up a Vagrant-managed cluster for the React application "Tour Places" using:

- 1 Load Balancer (NGINX)
- 4 Web Servers (web1, web2, web3, web4)
- Automatic building and serving of the React app

GitHub Source: https://github.com/cw-barry/Tour-Places-App.git

---

## Part 1: Cluster Setup

### Steps:
1. Clone the React app:

```bash
git clone https://github.com/cw-barry/Tour-Places-App.git frontend

2.	Build the app:

cd frontend
npm install
npm run build
cd ..

	3.	Create Vagrantfile with:

	•	lb (IP: 192.168.70.100)
	•	web1 (IP: 192.168.70.101)
	•	web2 (IP: 192.168.70.102)
	•	web3 (IP: 192.168.70.103)
	•	web4 (IP: 192.168.70.104)

## Part 2: Load Balancer and Web Server Configuration

	•	Load Balancer installs nginx and configures upstream servers.
	•	Web servers host the built React app at /var/www/react-app.

Configuration script used: lb-setup.sh

Load Balancer config path: /etc/nginx/conf.d/tour-places.conf

curl.exe -I http://192.168.70.100

✅ Website opened successfully.

## Part 3: Failure Testing

	•	Stopped web2 server:

vagrant halt web2

	•	Verified with:

vagrant status

	•	Load Balancer continued forwarding requests only to web1 and web3.
	•	Used loop to test multiple requests:

for ($i=1; $i -le 10; $i++) { curl.exe http://192.168.70.100; echo "`n" }

✅ No errors found.

## Part 4: Scaling Challenge

	•	Added web4 to Vagrantfile.
	•	Started web4:

vagrant up web4

	•	SSH into Load Balancer:

vagrant ssh loadbalancer

	•	Updated nginx config:

sudo sed -i '/upstream react_servers {/a     server 192.168.70.104;' /etc/nginx/conf.d/tour-places.conf
sudo nginx -t && sudo systemctl reload nginx

	•	Verified all 4 servers load-balanced successfully.

Files Structure

	•	Vagrantfile
	•	lb-setup.sh
	•	frontend/
	•	README.md
	•	Screenshots for each part

Final Notes

✅ Full cluster deployed successfully.
✅ Load Balancer forwarding properly.
✅ Scaling up with a new server verified