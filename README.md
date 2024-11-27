# ocp-iostat
```
oc apply -k https://github.com/gmeghnag/ocp-iostat.git
```
If the pods in namespace `ocp-iostat` are not coming up because of the following event:
```
$ oc get ev -n ocp-iostat
LAST SEEN   TYPE      REASON         OBJECT                 MESSAGE
10s         Warning   FailedCreate   daemonset/ocp-iostat   Error creating: pods "ocp-iostat-" is forbidden: error looking up service account ocp-iostat/ocp-iostat: serviceaccount "ocp-iostat" not found
```
Remove the `daemonset/ocp-iostat` and reapply the resource:
```
oc delete -n ocp-iostat daemonset/ocp-iostat
oc apply -k https://github.com/gmeghnag/ocp-iostat.git
```
