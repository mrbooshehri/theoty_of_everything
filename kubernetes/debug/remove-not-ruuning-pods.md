# Remove not running pods
```bash
for item in `kubectl get pod -A --no-headers |grep -v Running | awk '{print $1"|"$2}'`; do kubectl delete pod -n ${item%|*} ${item#*|} --grace-period=15; done
```

Tags:
```
#remove_pod #not_running
```
