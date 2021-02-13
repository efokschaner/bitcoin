# Holdcoin seeder node
A linux container with a full node of Holdcoin to serve as a seed for the Holdcoin p2p network.

## Build
```
cd docker
docker build -t efokschaner/holdcoin-seeder .
```

## First Ever Deploy
```
docker tag efokschaner/holdcoin-seeder us.gcr.io/just-a-shell/holdcoin-seeder
docker push us.gcr.io/just-a-shell/holdcoin-seeder
gcloud compute instances create-with-container holdcoin-seeder-001 `
    --container-image us.gcr.io/just-a-shell/holdcoin-seeder:latest `
    --machine-type e2-small `
    --boot-disk-size 10GB `
    --zone us-east1-b `
    --network-tier STANDARD `
    --tags holdcoin-seeder

gcloud compute firewall-rules create allow-holdcoin `
    --direction ingress `
    --action allow `
    --target-tags holdcoin-seeder `
    --rules tcp:8565
```

## Redeploy
```
docker tag efokschaner/holdcoin-seeder us.gcr.io/just-a-shell/holdcoin-seeder
docker push us.gcr.io/just-a-shell/holdcoin-seeder
```
### Reboot the target instance to trigger update of container
```
gcloud compute ssh holdcoin-seeder-001 --command='sudo shutdown -r now'

```