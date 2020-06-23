
# Osquery x Fleet

### What is this about?
[Osquery](http://osquery.io/) is an open-source instrumentation framework that exposes an operating system as a relational database.

[Kolide Fleet](https://www.kolide.com/fleet/) is an open-source management server for a fleet of hosts machines running Osquery.

This repo guides you through setting up and running a local Fleet server and enrolling hosts into the fleet.

### Requirements

You'll need the following:
- [Docker](https://docs.docker.com/engine/install/)
- [docker-compose](https://docs.docker.com/compose/install/)
- [VirtualBox](https://www.virtualbox.org/)
- [Vagrant](https://www.vagrantup.com/)

### Run Kolide Fleet locally
1. **Generate a certificate for the Fleet server:** run the following command and make sure to set a value for the certificate common name when prompted (the value has to match the hostname or IP address of the server that will run Fleet)
    ```bash
   sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout ./cert/kolide.key -out ./cert/kolide.cert
    ```

1. **Setup the environment:** create the following environment variables and set values of your choosing
    1. MYSQL_ROOT_PASSWORD
    1. MYSQL_PASSWORD
    1. KOLIDE_AUTH_JWT_KEY

1. **Initiate the services:** run the following command to create the MySQL, Redis and Fleet services. Note that Fleet will prepare its database then exit, this is the expected behavior.
    ```bash
   docker-compose -f docker-compose-init.yml up -d
    ```

1. **Start Fleet and its dependencies:** once Fleet database is initiated, run the following command to bring up MySQL, Redis and the Fleet Server.
    ```bash
   docker-compose stop && docker-compose up -d
    ```

1. **Configure Fleet:** open `https://[fleet-server-ip]:8080/setup` in a browser and follow the instructions to create an admin user.

1. **Grab the Fleet enroll secret:** from the Fleet UI, navigate to `Hosts > Add New Host` then copy the enroll secret from the `Retrieve Osquery Enroll Secret` section and paste its value into the `enroll_secret` file that's alongside this README document.

### Enroll a host into Fleet
1. **Make the host aware of the Fleet server:** edit the `osquery.flags` file that's alongside this README document and replace the `CHANGEME` placeholder on line 3 by the hostname or IP address of the Fleet server (value has to match the certificate common name set earlier).

1. **Create a host:** run the following command to create a virtual machine that will act as a host. Osquery daemon will be automatically installed and configured following the virtual machine creation.
    ```bash
    vagrant up
    ```

1. **Check that the host has enrolled successfully:** in the Fleet UI, navigate to the `Hosts` section and the host should be right there.

### Experiment and have fun
From now on, you can use Fleet to run ad-hoc queries on the fly in the host, to create queries and packs, etc.

It's also possible to use Fleet command-line tool `fleetctl` to manage the fleet of hosts.
