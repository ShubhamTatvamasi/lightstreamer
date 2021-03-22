# lightstreamer

create a deployment and service:
```bash
kubectl create deployment lightstreamer --image=lightstreamer
kubectl expose deployment lightstreamer --port=8080 --name=lightstreamer
```

Ingress value for lightstreamer:
```bash
kubectl apply -f - << EOF
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: lightstreamer
  annotations:
    nginx.org/websocket-services: lightstreamer
    cert-manager.io/cluster-issuer: letsencrypt
spec:
  tls:
  - hosts:
      - lightstreamer.k8s.shubhamtatvamasi.com
    secretName: lightstreamer-tls
  rules:
  - host: lightstreamer.k8s.shubhamtatvamasi.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: lightstreamer
            port:
              number: 8080
EOF
```

delete everything:
```bash
kubectl delete deploy/lightstreamer svc/lightstreamer ing/lightstreamer
```
