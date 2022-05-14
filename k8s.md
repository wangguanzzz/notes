## ingress
https://www.youtube.com/watch?v=GhZi4DxaxxE&t=43s
* ingress controller ( need to installed by helm)
  many solutions:
  - nginx
  - contour
  - haproxy
  - trafik
  - istio
* ingress resource
  it has two parts, backedn service and host
  normally, you can define multiple DNS name to same load balancer
  in helm, need to define the host