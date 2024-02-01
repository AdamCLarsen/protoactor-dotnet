# Setup
Was build and testing using k3s and Rancher Desktop and registry for local development.

## Local registry
Setup a local registry, so we can push and pull images from it.
```sh
docker volume create registry_data
docker run -d -p 5000:5000 --name registry --restart=always -v registry_data:/var/lib/registry registry:2
```

## Build and push images

Run from the root of the repository to build and push the images to the local registry.
```sh
docker build -t protoactor.example.k8s.grains.node1:latest -f .\examples\ClusterK8sGrains\Node1\Dockerfile .
docker tag protoactor.example.k8s.grains.node1:latest localhost:5000/protoactor.example.k8s.grains.node1:latest
docker push localhost:5000/protoactor.example.k8s.grains.node1:latest

docker build -t protoactor.example.k8s.grains.node2:latest -f .\examples\ClusterK8sGrains\Node2\Dockerfile .
docker tag protoactor.example.k8s.grains.node2:latest localhost:5000/protoactor.example.k8s.grains.node2:latest
docker push localhost:5000/protoactor.example.k8s.grains.node2:latest
```

## Deploy to k8s

From the ClusterK8sGrains folder run the following commands to deploy the application to k3s.
```sh
helm install protoactor-example-k8s-grains ./chart
```

To remove the application from k3s run the following command.
```sh
helm uninstall protoactor-example-k8s-grains
```