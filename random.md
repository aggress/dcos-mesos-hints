## Random

### Get auth Token

Grabs the auth token from the API allowing you use it in other API commands. Replace `<dcos_url>` with your accessible URL, `<user>` & `<password>`
with correct credentials.

```
$ DCOS_AUTH_TOKEN=$( curl -s -X POST http://<dcos_url>/acs/api/v1/auth/login \
   -d '{"uid": "<user>", "password": "<password>"}' \
   -H 'Content-Type: application/json' \
   | grep token | cut -d ':' -f2 | cut -d '"' -f2)
```

### Get agent resource configuration

List the resource configurations for all public and private agents, requires $DOCS_AUTH_TOKEN as an environment variable and `<dcos_url>`

```
$ curl -H "Content-Type: application/json" -H "Authorization: token=$DCOS_AUTH_TOKEN" -X POST http://<dcos_url>/mesos/slaves | jq
```

### Unreserve static resources

To unreserve static resources on an agent, change `<role>` & `<principle>` where required, `slaveId` needs replacing with each agent/slave ID.

Reference: https://mesos.apache.org/documentation/latest/reservation/

```
$ curl -i \

 -d slaveId=df86aa26-aba2-4140-b16a-5e7ea797bbbb-S5 \
 -d resources='[
     {
         "name": "cpus",
         "type": "SCALAR",
         "scalar": {
             "value": 3
         },
         "role": "<role>",
         "reservation": {
             "principal": "<principle>"
         }
     },
     {
         "name": "mem",
         "type": "SCALAR",
         "scalar": {
             "value": 4096
         },
         "role": "<role>",
         "reservation": {
             "principal": "<principle>"
         }
     }
]' \
-X POST http://master.mesos:5050/master/unreserve
```