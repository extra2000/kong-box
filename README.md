# kong-box

| License | Versioning | Build |
| ------- | ---------- | ----- |
| [![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT) | [![semantic-release](https://img.shields.io/badge/%20%20%F0%9F%93%A6%F0%9F%9A%80-semantic--release-e10079.svg)](https://github.com/semantic-release/semantic-release) | [![Build status](https://ci.appveyor.com/api/projects/status/n2s2jbymxdo7c29m/branch/master?svg=true)](https://ci.appveyor.com/project/nikAizuddin/kong-box/branch/master) |

Developer box for [Kong API Gateway](https://github.com/Kong/kong).


## Getting started

Clone this repository recursively and then `cd` into the repository:
```
$ git clone --recursive https://github.com/extra2000/kong-box.git
$ cd kong-box
```


## Creating Vagrant Box

Create Salt pillar file for Kong based on the example. You may modify the value:
```
$ cp -v salt/roots/pillar/kong.sls.example salt/roots/pillar/kong.sls
```

Copy vagrant file from `vagrant/examples/` and then create the vagrant box (you can change to `--provider=libvirt` if you want to use Libvirt provider):
```
$ cp -v vagrant/examples/Vagrantfile.kong-box.fedora-33.x86_64.example vagrant/Vagrantfile.kong-box
$ vagrant up --provider=virtualbox kong-box
```

Provision the vagrant box which will automatically disable swap memory, install Podman, and install Cockpit:
```
$ vagrant ssh kong-box -- sudo salt-call state.highstate
```


## First-time Initializations

This Section contains instruction that will be applied for the first-time initialization only, after finished creating Vagrant box.

Transfer files and configure using the following command:
```
$ vagrant ssh kong-box -- sudo salt-call state.sls kong.config
```

Deploy Postgres container:
```
$ vagrant ssh kong-box -- sudo salt-call state.sls kong.service.postgres
```

Try login and then logout to check whether Postgres has successfully deployed:
```
$ vagrant ssh kong-box -- podman run -it --rm --network=kongnet docker.io/postgres:13.1-alpine psql --host=postgres-pod.kongnet --username=kong --dbname=kongdb
```

Bootstrap Kong database for the first time:
```
$ vagrant ssh kong-box -- podman run --rm --pod postgres-pod --volume="/opt/kong/kong.conf:/etc/kong/kong.conf:ro,z" -e "KONG_PG_HOST=localhost" docker.io/library/kong:2.3.0 kong migrations bootstrap
```

Deploy Kong container:
```
$ vagrant ssh kong-box -- sudo salt-call state.sls kong.service.kong
```

Try accessing Kong services to ensure Kong has successfully deployed:
```
$ vagrant ssh kong-box -- podman run -it --rm --network=kongnet docker.io/curlimages/curl curl http://kong-pod.kongnet:8001
$ vagrant ssh kong-box -- podman run -it --rm --network=kongnet docker.io/curlimages/curl curl --insecure https://kong-pod.kongnet:8444
```

On RedHat and SUSE families, execute the following commands to forward port 8443 to port 443:
```
$ vagrant ssh kong-box -- sudo dnf install -y firewalld
$ vagrant ssh kong-box -- sudo systemctl start firewalld
$ vagrant ssh kong-box -- sudo systemctl enable firewalld
$ vagrant ssh kong-box -- sudo firewall-cmd --zone=public --add-forward-port=port=443:proto=tcp:toport=8443:proto=tcp --permanent
$ vagrant ssh kong-box -- sudo firewall-cmd --zone=public --add-service=https --permanent
$ vagrant ssh kong-box -- sudo firewall-cmd --reload
```

Finally, try access Kong from host and make sure the command success (for example, it should display `{"message":"no Route matched with those values"}`):
```
$ curl --insecure https://kong-box
```

At this point, Kong has been successfully deployed.


## Deploy Filebeat pod for ELK (optional)

```
$ vagrant ssh kong-box -- sudo salt-call state.sls filebeat
```


## Regular Usage

This Section is only applied after finished the first-time initialization procedure.

To stop Kong and Postgres containers:
```
$ vagrant ssh kong-box -- sudo salt-call state.sls kong.service.kong.dead
$ vagrant ssh kong-box -- sudo salt-call state.sls kong.service.postgres.dead
```

To start Kong and Postgres containers:
```
$ vagrant ssh kong-box -- sudo salt-call state.sls kong.service.postgres
$ vagrant ssh kong-box -- sudo salt-call state.sls kong.service.kong
```

To list services and routes:
```
$ vagrant ssh kong-box -- curl -i http://127.0.0.1:8001/services
$ vagrant ssh kong-box -- curl -i http://127.0.0.1:8001/routes
```


## Simple Example: Forward to CVE Search via Kong

Register CVE Search webservice as `cvesearch` and setup routing with prefix `/cvesearch`:
```
$ vagrant ssh kong-box -- curl -i -X POST --url http://127.0.0.1:8001/services/ --data 'name=cvesearch' --data 'url=https://cve.circl.lu'
$ vagrant ssh kong-box -- curl -i -X POST --url http://127.0.0.1:8001/services/cvesearch/routes --data 'name=cvesearch-route' --data 'paths[]=/'
```

Execute the following command from host and make sure it works:
```
$ curl --insecure https://kong-box/api/browse/microsoft
```

To change service URL:
```
$ vagrant ssh kong-box -- curl -i -X PATCH --url http://127.0.0.1:8001/services/cvesearch --data 'url=https://www.cve-search.org'
```

To delete a service, delete their routes first. For example:
```
$ vagrant ssh kong-box -- curl -i -X DELETE --url http://127.0.0.1:8001/routes/cvesearch-route
$ vagrant ssh kong-box -- curl -i -X DELETE --url http://127.0.0.1:8001/services/cvesearch
```
