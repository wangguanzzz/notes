## what is ISTIO and what can it do
1. traffic management (envoy proxy, which is a sidecar)
2. auth / encrypt
3. loggging

istio is a proxy betweeen your container and underlying hardware, it can run on docker or k8s

## overview of istio components
在每个pod里面有proxy(envoy (layer 7 ))作为sidecar
- dynamic service discovery
- LB
- tls terminiation
- health checks
- staged rollouts
- fault injection

in controll plain API, there are
- Pilot: sends configuration to proxy
  1. service discovery
  2. intelligent routing
  3. resiliency
- Citadel
  1. user authenticaiton
  2. credetnial management
  3. cert management
  4. traffic enryption
- Mixer
  1. accessc control
  2. usage policies
  3. telemetry data
  it also has a lot of plugins


## how Istio does its job
- ingress
- egress
- svc
- proxy  (configured by pilot) (mixier contain the routing rules) (encrypt by citadel)

- pilot, citadel, mixer

```
kubectl get endpoints
kubectl get vs -o yaml
```

## istio with k8s

typical inject usage istio case
```
kubectl apply -f < (istioctl kube-inject -f <original_k8s.yaml>)
```

when need to access the real app, need to apply an istio gateway, (virutal svc)


## inspecting the installation
virutual service

```yaml
apiVersion: ..
kind: VirtualService
metadata: 
  name: productpage
spec: 
  hosts:
  - productpage
  http:
  - route:
    - destination:
        host: productpage
        subset: v1
```

destniation role
```yaml
apiVersion: ..
kind: DestinationRule
metadata: 
  name: productpage
spec: 
  hosts: productpage
  subsets:
  - name: v1
    labels:
      version: v1
  - name: v2
    labels:
      version: v2
```

after istio is installed, it will have a istio-system namespace

## istio routing
istio has an ingressgateway in istio-system, that can be configured as nodeport, loadbalancer, etc.
```
kubectl get gateway
```
gateway pass to virtual service

```
kubectl get destinationrules
```

destination rules 定义所有的destination 和他们背后的version

service rule 是route到destination的设定


## istio policies
mixer policy
canary testing金丝雀test
可以限制某个服务的quota,例如多少时间内访问多少次。rate limit
```
# check the rule
kubectl get rule
```

## loging in istio


## lab of istio
Getting Logged In
On the lab overview page, we'll see three servers: a Kube Master ( we'll call it km), and two Kube Nodes (kn1 and kn2). We can log into any of them using either an SSH client of our own, or by using Linux Academy's built in Instant Terminal program.

Install Istio in Kubernetes and Deploy the bookinfo Application
Get the Istio installation package and unpack it:

[cloud_user@km]$ wget https://github.com/istio/istio/releases/download/1.0.6/istio-1.0.6-linux.tar.gz
[cloud_user@km]$ tar -xvf istio-1.0.6-linux.tar.gz
Add istioctl to our path:

[cloud_user@km]$ export PATH=$PWD/istio-1.0.6/bin:$PATH
Set Istio to NodePort type, and set the nodePort to port 30080:

[cloud_user@km]$ sed -i 's/LoadBalancer/NodePort/;s/31380/30080/' ./istio-1.0.6/install/kubernetes/istio-demo.yaml
Bring up the Istio control plane:

[cloud_user@km]$ kubectl apply -f ./istio-1.0.6/install/kubernetes/istio-demo.yaml
Verify that the control plane is running, but note that it may take a minute or two:

[cloud_user@km]$ kubectl -n istio-system get pods
Install the bookinfo application with manual sidecar injection:

kubectl apply -f <(istioctl kube-inject -f istio-1.0.6/samples/bookinfo/platform/kube/bookinfo.yaml)
Verify that the application is running and that there are 2 containers per pod. This may take another minute or so:

[cloud_user@km]$ kubectl get pods
Create an ingress and virtual service for the application:

[cloud_user@km]$ kubectl apply -f istio-1.0.6/samples/bookinfo/networking/bookinfo-gateway.yaml
Now, in a web browser, let's verify the page loads at the URL http://<kn1_PUBLIC_IP ADDRESS>:30080/productpage.

Configure the Forwarder, Install Nginx, and Access the Grafana Dashboard
Configure Grafana to forward to localhost:

[cloud_user@km]$ kubectl -n istio-system port-forward $(kubectl -n istio-system get pod -l app=grafana -o jsonpath='{.items[0].metadata.name}') 3000:3000 &
Install and configure Nginx to proxy to the Grafana port:

[cloud_user@km]$ sudo apt install -y nginx
Change the Nginx config by editing /etc/nginx/sites-enabled/default with whichever text editor you like (but remember privileges, and run something like sudo vi...). Comment out all existing lines in the location / directive, and add proxy_pass http://127.0.0.1:3000. The results should look like this:

location / {
    # First attempt to serve request as file, then
    # as directory, then fall back to displaying a 404.
    #try_files $uri $uri/ =404;
    proxy_pass http://127.0.0.1:3000;
}
Then restart Nginx:

[cloud_user@km]$ sudo systemctl restart nginx
Load Grafana at http://<km_PUBLIC_IP ADDRESS/dashboard/db/istio-mesh-dashboard and review the installed Istio dashboards.

If the mesh traffic dashboard does not populate when the application is refreshed, then rerun this command to re-apply the mixer configuration:

[cloud_user@km]$ kubectl apply -f ./istio-1.0.6/install/kubernetes/istio-demo.yaml