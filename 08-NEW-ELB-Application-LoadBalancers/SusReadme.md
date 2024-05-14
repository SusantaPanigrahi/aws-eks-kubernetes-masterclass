
# My Notes
## ALB vs Ingress
- https://www.geeksforgeeks.org/difference-between-kubernetes-ingress-and-loadbalancer/
- Mine:-- ALB is external to k8s-cluster and Ingress is internal to the k8s-cluster. ALB sits infront of ingress. ALB route traffic to ingress and ingress which is having multiple rules route traffic to multiple services.
- Load balancers sit between servers and the internet. Ingress can not sit between servers and the internet.
- A Kubernetes application load balancer is a type of service, while Kubernetes ingress is a collection of rules, not a service. Instead, Kubernetes ingress sits in front of multiple services and acts as the entry point for an entire cluster of pods. 
- An Ingress(resource) defines rules that determine how traffic from the external network should be forwarded to the services in your cluster. 
### When To Use Kubernetes Loadbalancer
- A Kubernetes load balancer is the default way to expose a service to the internet.
- Every service you expose using a load balancer will receive its own IP address. This is the main drawback with loadbalancer service. You have to pay for a load balancer for each exposed service, which may grow pricey!
- if you wish to expose a service directly, on the port you designate and the service will receive all kind of incoming traffic,  including HTTP, TCP, UDP, Websockets, gRPC, and so on.. i,e without any need of Filtering, routing, and any other kind of features then you can go for Loadbalancer.
- Load balancers can only route to one service at a time since they are defined per service. This contrasts with an ingress, which can route to several services inside the cluster.
  
### Example of a service with Kubernetes load balancer:
```
apiVersion: v1.5
kind: Service
metadata:
  name: api-service
spec:
  selector:
    app: api-app
  ports:
    - protocol: TCP
      port: 80
      targetPort: 8080
  type: LoadBalancer
```

### When To Use Kubernetes Ingress 
The most effective approach to get your services visible is most likely through ingress, but it may also be the most difficult. There are several varieties of ingress controllers, including Nginx, Contour, Istio, and the Google Cloud Load Balancer. Additionally, there are Ingress controller plugins like the cert manager that enable automated SSL certificate provisioning for your services. If you wish to offer several services under one IP address and they are all using the same L7 protocol (usually HTTP), then ingress is the most helpful option. 

### Example Of A Service With Kubernetes Ingress
```
apiVersion: networking.k8s.io/v1.5
kind: Ingress
metadata:
  name: ingress-example
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  ingressClassName: nginx-example
  rules:
  - http:
      paths:
      - path: /api
        pathType: Prefix
        backend:
          service:
            name: api-service
            port:
              number: 80
```
### One more
```
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: simple-fanout-example
spec:
  rules:
  - host: foo.bar.com
    http:
      paths:
      - path: /foo
        pathType: Prefix
        backend:
          service:
            name: service1
            port:
              number: 4200
      - path: /bar
        pathType: Prefix
        backend:
          service:
            name: service2
            port:
              number: 8080
```
