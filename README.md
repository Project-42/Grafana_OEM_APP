# Grafana_OEM_APP (WIP)
Docker-Compose playbook for Grafana Installation Example for Oracle Enterprise Manager App

More on this application:
Oracle Post from [Murtaza Husain](https://blogs.oracle.com/author/ac14de17-0dbc-4105-b88d-905b719d4d7b) - [Introducing the brand new Oracle Enterprise Manager App for Grafana](https://blogs.oracle.com/oem/introducing-the-brand-new-grafana-plug-in-for-oracle-enterprise-manager)

I have created this github repository for a docker-compose installation type, but the installation using just docker or any other method is quiet simple as you can see in the [Oracle Enterprise Manager App for Grafana Documentation](https://docs.oracle.com/en/enterprise-manager/cloud-control/enterprise-manager-cloud-control/13.4/emgrf/oracle-enterprise-manager-data-source-grafana.html)


If you want to use my example, just clone the [Grafana_OEM_APP repository](https://github.com/Project-42/Grafana_OEM_APP)
```bash
|=| oem13 in ~/ ○ → git clone https://github.com/Project-42/Grafana_OEM_APP.git
Cloning into 'Grafana_OEM_APP'...
remote: Enumerating objects: 33, done.
remote: Counting objects: 100% (33/33), done.
remote: Compressing objects: 100% (24/24), done.
Unpacking objects: 100% (33/33), done.
remote: Total 33 (delta 8), reused 27 (delta 4), pack-reused 0
```

You should end up with the following files/directories:
```
|=| oem13 in ~/Grafana_OEM_APP ± |master ✓| → tree
.
├── docker-compose.yml
├── plugins
│   └── LEAVE_Oracle_Enterprise_Manager_App_HERE.txt
└── README.md
```

The docker compose playbook is quite simple, just enough to make grafana to run as root since is the user I use for running the container (I should change that in the future, I know :) ), set the admin password and make docker to install Oracle Enterprise Manager App for Grafana plugin directly (but you will need to enable it after)
```bash
version: '3.4'
services:
  grafana:
    image: grafana/grafana:latest
    container_name: grafana_oem_app
    volumes:
      - ./plugins:/var/lib/grafana/plugins
    user: "0"
    environment:
      - GF_SECURITY_ADMIN_USER=admin
      - GF_SECURITY_ADMIN_PASSWORD=Welcome1
      - GF_INSTALL_PLUGINS=/var/lib/grafana/plugins/oracle-emcc-app-2.0.0.zip;oracle-emcc-app
    restart: unless-stopped
    ports:
      - 3000:3000
```

Download [Oracle Enterprise Manager App for Grafana](https://www.oracle.com/enterprise-manager/downloads/grafana-downloads.html)
Since Im using Grafana 7.x and OEM 13.4.0.9, I'm using oracle-emcc-app-2.0.0 for this example.

Move the zip file to the *plugins* folder to get somehting just like this:
```bash
|=| oem13 in ~/Grafana_OEM_APP ± |master ✓| →  tree
.
├── docker-compose.yml
├── plugins
│   ├── LEAVE_Oracle_Enterprise_Manager_App_HERE.txt
│   └── oracle-emcc-app-2.0.0.zip   <<<<<<<<<<<
└── README.md
```

Since we have our playbook and plugin ready, we can start Grafana using docker-compose as follow
```bash
|=| oem13 in ~/Grafana_OEM_APP ± |master ✓| → docker-compose up -d
Building with native build. Learn about native build in Compose here: https://docs.docker.com/go/compose-native-build/
Starting grafana_oem_app ... done

|=| oem13 in ~/Grafana_OEM_APP ± |master ✓| → 
```

Now, login into Grafana you should be able to see the plugin in your grafana plugin list and enable it


**Full post will be soon published for this in [Project42 Website](https://project42.site/)**


