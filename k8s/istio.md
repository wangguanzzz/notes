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