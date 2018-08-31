# Deploying MQ in container environments

## Immutable containers
While MQ per se is not fundamantally different when packaged in a container, the adoption of containers and realization of its benefits leads to a different method of deploying and generally treating containeraized software along the "cattle vs pets" paradigm. 
One can deploy an MQ container and treat is as traditional middleware server (a "pet") by connecting to it via command shell or MQ Explorer and adding configurations, creating channles, queues, topics and other objects. This may be quite handy in development process when a developer would self-provision an MQ instance and need to update queues configuration at will in an agile manner.
Deploying MQ in containers in higher invironments will more likely follow the "cattle" analogy when the container has all the contents it needs and becomes immutable and therefore disposable and easily replacable in the case of failure, or need of upgrade. This approach is illustrated below.

![Deploying immutable containers](../static/imgs/mq-immutable-containers.png?raw=true)

## Managing MQ configurations 
But we don't want the developers to be responsible for defining real life MQ server configurations, don't we. Instead, we want to reuse existing MQ configuration and hardening scripts that MQ admin have already defined.
So how do we reconcile the need of additional not-out-of-box MQ configuration with the immutable containers deployment idea?
The approach is outlines below

![Deploying immutable containers](../static/imgs/mq-configuring-containers.png?raw=true)

The solution begins with using IBM provided Docker images for MQ, available e.g. from public Docker hub repo ibmcom/mq.  For production air gap environment, IBM MQ image is provided on the portable media and loaded in your private Docker repo, but this does not change the approch here.  This step is a top level part of the diagram.

Next step, MQ admin provides appropriate MQ config scripts that are intented to all or a group of applications, but don't have application specific contents, e.g. creating queues/topis and their parameters. A simple fragment for illustation only shown below would be part of a security hardening configuration

```
SET CHLAUTH('*') TYPE(ADDRESSMAP) ADDRESS('*') USERSRC(NOACCESS) DESCR('Back-stop rule - Blocks everyone') ACTION(REPLACE)
SET CHLAUTH('EXAMP.APP.SVRCONN') TYPE(ADDRESSMAP) ADDRESS('*') USERSRC(CHANNEL) DESCR('Allows connection via APP channel') ACTION(REPLACE)
SET CHLAUTH('EXAMP.ADMIN.SVRCONN') TYPE(ADDRESSMAP) ADDRESS('*') USERSRC(CHANNEL) DESCR('Allows connection via ADMIN channel') ACTION(REPLACE)
SET CHLAUTH('EXAMP.ADMIN.SVRCONN') TYPE(BLOCKUSER) USERLIST('nobody') DESCR('Allows admins on ADMIN channel') ACTION(REPLACE)
```

So how to deploy such script to an immutable MQ server container? The answer is to create another Docker image, derived from the one provided by IBM. One would need to create a Dockerfile for this, such as below

```
# Use IBM image as a base
FROM ibmcom/mq
# Disable IBM-provided configuration for dev purposes
ENV MQ_DEV=false
# Add your MQ admin configuration
COPY admin-config.mqsc /etc/mqm/
```

Then build your MQ image locally and tag it to differntiate from IBM stock image e.g.
```
docker build -t mq9-server .
```
Now you are ready to load it into your private Docker repo. In pseudocode:
```
docker tag mq9-server  myrepo:8500/mq9-server
docker push myrepo:8500/mq9-server
```
Now we have a pre-configured MQ container image which is ready to be used by developers.

Developer needs to add some application-specific MQ configuration such as define some queues and/or topics.
To build immutable image the dev team will follow the same approch of creating application-specific MQ image based on pre-configured MQ image built in earlier step.

```
FROM myrepo:8500/mq9-server
COPY app-config.mqsc /etc/mqm/
```

where app-config.mqsc  is something like
```
* Create Local Queues that my application(s) can use.
DEFINE QLOCAL('EXAMPLE.QUEUE.1') REPLACE
DEFINE QLOCAL('EXAMPLE.QUEUE.2') REPLACE
```

Dev will build the Docker image from this, tag it an push to the private Docker repo:
```
docker build -t myapp-mq9-server .
docker tag myapp-mq9-server  myrepo:8500/myapp-mq9-server
docker push myrepo:8500/myapp-mq9-server
```

Now we have fully self contained MQ image for myapp application ready to be deployed using whatever application deployment pattern necessary.

The app deployment will deploy at least 2 containers now, one with App itself, another with app-specific MQ instance we just built above.

## Deployment patterns
In the simple case when the app only uses MQ as a messaging component between different parts of itself, using only local queues, the deployment pattern can be just app pod and standalone MQ pod.  
In more compex case when the app needs MQ to communicate with other Apps, or legacy systems using remote queues, the deployment pattern may include joing an MQ cluster for ease of connectivity and discovery. 
The example of deploying MQ in a cluster in ICP is provided  here 
<TODO: scrub and move Rob's repo to public github>

![Deploying immutable containers](../static/imgs/MQCluster.png?raw=true)

## Closing comments
This approch builds on the idea of decoupling MQ services from traditional shared MQ monolith to a dedicated MQ service just for my app.  This is a major shift in using MQ in containers.  This repo is not a right place to  answer questions such as does this mean I have to pay more for MQ now that I have my own MQ instance, but with IBM Cloud Private as the deployment tagret, the consumption of MQ is not defined by number of deployed containers.
