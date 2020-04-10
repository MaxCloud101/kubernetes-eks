## Kubernetes Dashboard
We are installing Kubernetes Dashboard v2 , which is still in beta state.
Main change:
* Heapster has been replaced by MetricsServer, to collect metrics

### Deploy the K8s Metrics Server
```
sudo apt install jq curl

DOWNLOAD_URL=$(curl --silent "https://api.github.com/repos/kubernetes-sigs/metrics-server/releases/latest" | jq -r .tarball_url) 
DOWNLOAD_VERSION=$(grep -o '[^/v]*$' <<< $DOWNLOAD_URL)
curl -Ls $DOWNLOAD_URL -o metrics-server-$DOWNLOAD_VERSION.tar.gz
mkdir metrics-server-$DOWNLOAD_VERSION
tar -xzf metrics-server-$DOWNLOAD_VERSION.tar.gz --directory metrics-server-$DOWNLOAD_VERSION --strip-components 1
kubectl apply -f metrics-server-$DOWNLOAD_VERSION/deploy/1.8+/
```
Verification

```
kubectl get deployment metrics-server -n kube-system
```

### Deploy the K8s dashboard

grab latest release of v2.xxx here: https://github.com/kubernetes/dashboard/releases
```
export DASHBOARD_RELEASE=v2.0.0-beta1
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/$DASHBOARD_RELEASE/aio/deploy/recommended.yaml
```


### create an admin user account
```
kubectl apply -f admin-service-account.yaml
```

### access the dashboard
* get a security token
```kubectl -n kube-system describe secret $(kubectl -n kube-system get secret | grep eks-course-admin | awk '{print $1}')
```
record the output of ```token:```

* start kube proxy via ```kubectl proxy```

* open browser at `http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/#!/login`

and choose _token_ as login method, where you have to provide the token you recorded two steps before

