#Cilium PE labs

# Check staus
cilium status --wait

# Star wars app
endor.yml

# Get service details
kubectl -n endor get svc deathstar

# Adding ingress
root@server:~# kubectl -n endor get ingress deathstar -o yaml | yq '.spec'
ingressClassName: cilium
rules:
  - http:
      paths:
        - backend:
            service:
              name: deathstar
              port:
                number: 80
          path: /
          pathType: Prefix
tls:
  - hosts:
      - deathstar.cilium.rocks
    secretName: cilium-rocks

# API Gateway
root@server:~# kubectl -n endor get gateway tls-gateway
NAME          CLASS    ADDRESS          PROGRAMMED   AGE
tls-gateway   cilium   172.18.255.202   True         14m
root@server:~# kubectl -n endor get gateway tls-gateway -o yaml | yq '.spec'
gatewayClassName: cilium
listeners:
  - allowedRoutes:
      namespaces:
        from: Same
    hostname: deathstar.cilium.rocks
    name: deathstar
    port: 443
    protocol: HTTPS
    tls:
      certificateRefs:
        - group: ""
          kind: Secret
          name: cilium-rocks
      mode: Terminate

# fix dns
kubectl -n kube-system edit configmap/coredns






