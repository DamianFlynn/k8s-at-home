# Debugging Tips

## Helm Charts

kubectl get hr --all-namespaces | grep False | awk '{print $2, $1}'

helm ls -A
list all Helm charts


flux suspend hr -n kube-system metrics-server
flux resume hr -n kube-system metrics-server


helm history -n kube-system metrics-server
helm rollback -n kube-system metrics-server 1


## PodInfo

https://github.com/stefanprodan/podinfo

```sh

[ "$1" == "-d" ] && read -p "Press [Enter] key to continue and add podinfo git repo..."

echo "Add podinfo repository to Flux"
flux create source git podinfo \
  --url=https://github.com/stefanprodan/podinfo \
  --branch=master \
  --interval=30s \
  --export > podinfo-source.yaml
git add -A && git commit -m "Add podinfo GitRepository"
git push

[ "$1" == "-d" ] && read -p "Press [Enter] key to continue and add podinfo application..."

echo "Deploy podinfo application"
flux create kustomization podinfo \
  --target-namespace=default \
  --source=podinfo \
  --path="./kustomize" \
  --prune=true \
  --wait=true \
  --interval=5m \
  --export > podinfo-kustomization.yaml
git add -A && git commit -m "Add podinfo Kustomization"
git push

echo "Waiting for PodInfo deploying"
ATTEMPTS=0
ROLLOUT_STATUS_CMD="kubectl rollout status deployment/podinfo -n default"
FLUX_RECONCILE_CMD="flux reconcile kustomization flux-system --with-source"

$FLUX_RECONCILE_CMD &
until $ROLLOUT_STATUS_CMD || [ $ATTEMPTS -eq 120 ]; do
  $ROLLOUT_STATUS_CMD
  ATTEMPTS=$((ATTEMPTS + 1))
  sleep 1
done

echo "opening http://localhost:9898 in your browser (only on Mac - do it manually on other systems)"
kubectl port-forward deployment/podinfo 9898:9898 &
PID=$!
open http://localhost:9898

[ "$1" == "-d" ] && read -p "Press [Enter] key to continue and change podinfo background via kustomize patch..."

# kill -9 $PID
echo "adding kustomize patch to alter podinfo background"
cd ${CLONE_PATH}
IFS= read -rd '' podinfopatch << EOF
  patches:
    - patch: |-
        apiVersion: apps/v1
        kind: Deployment
        metadata:
          name: podinfo
        spec:
          template:
            spec:
              containers:
              - name: podinfod
                env:
                - name: PODINFO_UI_COLOR
                  value: '#ffff00'
        target:
          name: podinfo
          kind: Deployment
EOF
echo -e "$podinfopatch" >> podinfo-kustomization.yaml
git add -A
git commit -m "changed color in podinfo deployment via kustomize patch"
git push
$FLUX_RECONCILE_CMD # flux will wait for the new deploy to become ready, but we cannot create a new
$ROLLOUT_STATUS_CMD # port-forward on the same port before it finished terminating! wait for that,
fg %2               # then also wait for port-forward to exit, to be sure it closes its port here.

#we can now also get away with opening the browser just once!
# echo "open http://localhost:9898 in your browser"
exec kubectl port-forward deployment/podinfo 9898:9898 &
PID=$!
# open http://localhost:9898

echo; echo
# [ "$1" == "-d" ] &&
read -p "Press [Enter] key to continue and close portforward, ending script (or ^C to leave it running)"
kill -9 $PID

```