# microkafka

Creates small zookeeper and kafka docker containers to be used as a
single replica Kubernetes deployment.  The Kubernetes pod is composed of
a zookeeper container and a kafka container, and exposed as a service.

This is meant for being able to quickly spin up a kafka instance on
Kubernetes for development purposes.  Note that state will not survive
across pod restarts.

The images are based on alpine and openjdk are 'small', ~140MB each.


## Example usage

This assumes that you have [minikube](https://github.com/kubernetes/minikube/releases) and [kubectl](https://github.com/kubernetes/kubernetes/releases) available.  Firstly, start up Kubernetes and re-use it's docker daemon:

    minikube start
    eval $(minikube docker-env)

Then build the docker images from the provided Dockerfiles:

    pushd docker/zookeeper && docker build -t microzookeeper:v1 . && popd
    pushd docker/kafka && docker build -t microkafka:v1 . && popd

Note that if you change the image names/tags you'll have to adjust the
Kubernetes deployment yaml accordingly.

Now create the Kubernetes service and deployment:

    kubectl create -f kubernetes/

To test this, describe the service

    kubectl describe service microkafka

    Name:             microkafka
    Namespace:        default
    Labels:           app=microkafka
    Selector:         app=microkafka
    Type:             NodePort
    IP:               10.0.0.191
    Port:             zk 2181/TCP
    NodePort:         zk 32369/TCP
    Endpoints:        172.17.0.2:2181
    Port:             kafka 9092/TCP
    NodePort:         kafka 30844/TCP
    Endpoints:        172.17.0.2:9092
    Session Affinity: None

and get the zookeeper port (NodePort: zk 32369/TCP in the example) from this
under which it is accessible from the outside.  Then create and list a test
topic:

    kafka-topics.sh --list --zookeeper 192.168.99.100:32369

    kafka-topics.sh --create --topic testtopic --partitions 1 --replication-factor 1 --zookeeper 192.168.99.100:32369
    Created topic "testtopic".

    kafka-topics.sh --list --zookeeper 192.168.99.100:32369
    testtopic

This requires that you have kafka available locally (get it from
[here](https://kafka.apache.org/downloads)).
