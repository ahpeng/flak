# flak.io
The flak.io Project is a microservices sample application.

## Setup instructions

Before installing and running flak.io sample in a cluster you will need to apply a temporary fix for minutemant.
```
for i in $(curl -sS master.mesos:5050/slaves | jq '.slaves[] | .hostname' | tr -d '"'); do ssh "$i" -oStrictHostKeyChecking=no "sudo sysctl -w net.netfilter.nf_conntrack_tcp_be_liberal=1"; done
```

### Install elasticsearch from universe
Open the DC/OS management portal and install elasaticsearch from the portal. Feel free to adjust the CPU and memory settings.  If you change the executor name or ports you will need to re-configure the catalog service configuration.
### Install the catalog service
From the console on the master execute the following script.  You can copy and paste this into the terminal window with the SSH connection to a machine in your cluster
```
curl https://raw.githubusercontent.com/flakio/catalog/master/marathon.json | curl -qs -XPOST master.mesos/marathon/v2/apps -d@- -H "Content-Type: application/json"
```
### Install the gateway
From the console on a node in the cluster, run the following script which will download and install the gateway
```
curl https://raw.githubusercontent.com/flakio/infrastructure/master/gateway/marathon.json | curl -qs -XPOST master.mesos/marathon/v2/apps -d@- -H "Content-Type: application/json"
```

### Install the front end
From the console on a node in the cluster, run the following script which will download and install the client
```
curl https://raw.githubusercontent.com/flakio/frontend/master/marathon.json | curl -qs -XPOST master.mesos/marathon/v2/apps -d@- -H "Content-Type: application/json"
```
### Install the order service
From the console on a node in the cluster, run the following script which will download and install the gateway
```
curl https://raw.githubusercontent.com/flakio/order/master/marathon.json | curl -qs -XPOST master.mesos/marathon/v2/groups -d@- -H "Content-Type: application/json"
```
Initialize the order service
```
curl 192.168.3.1:80/order/install
```
