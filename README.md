# ocp-iostat


### Run
Execute the `DaemonSet` resource:
```
oc apply -k https://github.com/gmeghnag/ocp-iostat.git
```


### Troubleshooting 
#### Check for high values (> 99ms) of `r_await` and/or `w_await`:
```
oc get po -n ocp-iostat -o json | jq '.items[] | .metadata.name + " " + .spec.nodeName' -r | while read PO NODE; do oc logs --timestamps -n ocp-iostat $PO  | egrep "sd|rb|nvme" | awk -v node="$NODE" '{print node,$1,$2,$7,$13}' | column -t | egrep -v await | egrep " [0-9][0-9][0-9]+\.[0-9][0-9]"; done | column -t
```
P.S.: If your device's name does not start with `sd`, `rb`, or `nvme`, you must modify the `egrep` in the above command.

#### Example Output:
```
ip-10-0-21-248.eu-central-1.compute.internal  2024-11-27T02:17:09.489178664Z  nvme0n1  0.00    113.00
ip-10-0-38-143.eu-central-1.compute.internal  2024-11-26T16:14:34.705628150Z  nvme0n1  0.00    122.00
ip-10-0-74-81.eu-central-1.compute.internal   2024-11-26T16:04:52.897154385Z  nvme0n1  11.00   119.37
ip-10-0-74-81.eu-central-1.compute.internal   2024-11-26T16:04:54.897028960Z  nvme0n1  14.00   114.91
```
