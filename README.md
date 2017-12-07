# Conduit Examples

## Quickstart

### Install Conduit

@TODO(zoidbergwill): I need to test these on better WiFi, because I had to add `~/.conduit/bin` to my `$PATH` manually.

```
curl https://run.conduit.io/install | bash
conduit install | kubectl apply -f -

Wait for the pods to be ready (`RUNNING`):
```
kubectl get po --all-namespaces -w
```

Open the dashboard:
```
conduit dashboard
```

### Getting Started Example

Source: https://conduit.io/getting-started/

Create the example and open it's NodePort in our browser:

```
curl https://raw.githubusercontent.com/runconduit/conduit-examples/master/emojivoto/emojivoto.yml | conduit inject - --skip-inbound-ports=80 | kubectl apply -f -
minikube -n emojivoto service web-svc
```

Then generate traffic for us to see the pretty graphs

```
INGRESS=$(minikube -n emojivoto service web-svc --url | cut -f 3 -d / -)
while true; do curl $INGRESS; done
```

We can look at stuff with the CLI too:

Like "tap" our deployment / service and see the tcpdump-like traffic
going too and from it, while we make votes:
```
conduit tap deployment emojivoto/voting-svc

e.g.:
```
[172.17.0.11:52152 -> 172.17.0.8:8080]
HTTP Request
Stream ID: (0, 51)
HTTP POST voting-svc.emojivoto:8080/emojivoto.v1.VotingService/VoteGhost

[172.17.0.11:52152 -> 172.17.0.8:8080]
HTTP Response
Stream ID: (0, 51)
Status: 200
Latency (us): 3279

[172.17.0.11:52152 -> 172.17.0.8:8080]
HTTP Response End
Stream ID: (0, 51)
Grpc-Status: 0
Duration (us): 29
Bytes: 5
```

Or see some statistics for that service now that we've made some votes

```
conduit stat deployments emojivoto/voting-svc
```

### Coming Soon: Our Own Example
