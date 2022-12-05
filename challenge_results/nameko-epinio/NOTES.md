# Deploying nameko-devex docker envinroment
![Airship Ltd](airship.png)


## Important Notes
* To solve IP comunication issue the k3d on mac should be implemented with this command:
```sh
k3d cluster create epinio -p '80:80@loadbalancer' -p '443:443@loadbalancer'
```

### Epinio Install

```sh
atila@MacBook-Pro-de-Saulo  ~/Projects/epinio-sample/helm-charts-epinio-0.7.1  helm install epinio -n epinio --create-namespace epinio/epinio --set global.domain=epinio.127.0.0.1.sslip.io 
NAME: epinio
LAST DEPLOYED: Wed Nov 30 17:51:32 2022
NAMESPACE: epinio
STATUS: deployed
REVISION: 1
NOTES:
To interact with your Epinio installation download the latest epinio binary from https://github.com/epinio/epinio/releases/latest.

Login to the cluster with any of

    `epinio login -u admin https://epinio.epinio.127.0.0.1.sslip.io`
    `epinio login -u epinio https://epinio.epinio.127.0.0.1.sslip.io`

or go to the dashboard at: https://epinio.epinio.127.0.0.1.sslip.io

If you didn't specify a password the default one is `password`.

For more information about Epinio, feel free to checkout https://epinio.io/ and https://docs.epinio.io/.
```

### Epinio Login

```sh
atila@MacBook-Pro-de-Saulo  ~/Projects/epinio-sample/helm-charts-epinio-0.7.1  epinio login -u admin https://epinio.epinio.127.0.0.1.sslip.io

🚢  Login to your Epinio cluster [https://epinio.epinio.127.0.0.1.sslip.io]
Password: 

⚠️  Certificate signed by unknown authority

|     KEY     |      VALUE       |
|-------------|------------------|
| Issuer Name | CN=epinio-ca     |
| Common Name | epinio-ca        |
| Expiry      | 2023-February-28 |

Do you want to trust it (y/n): y

✔️  Trusting certificate for address https://epinio.epinio.127.0.0.1.sslip.io...

⚠️  Epinio server version (v1.5.0) doesn't match the client version (1.5.0)

⚠️  Update the client manually or run `epinio client-sync`

✔️  Login successful
```

## Issue after launch Epinio Plataform

Error:
failed to get replica details: the server is currently unable to handle the request (get pods.metrics.k8s.io)

# Solution
```sh
kubectl edit deployments.apps -n kube-system metrics-server
Insert this line on the file, after dnsPolicy: ClusterFirst: hostNetwork: true
```

### References

* Issue Solution:
    - [Stackoverflow](https://stackoverflow.com/questions/71843068/metrics-server-is-currently-unable-to-handle-the-request)


### Importante note

After the app nameko-devex was launched on Epinio Plataform, all of the backing services as well and binded them, the endpoint route was created and working, but the application wasn't working. Perhaps I needed more time to solve this issue by don't know deeper the Epinio Plataform.