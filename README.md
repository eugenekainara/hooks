<h3>1. Install chart </h3>

`helm install --wait --name hooks ./`

<h3>2. Check pre-install hook job logs. It prints Helm release revision number. It should be 1 at this point </h3>

`kubectl logs job/hooks-pre-install`

Output: `1`

<h3>3. Upgrading release and expecting that it will be in failed status</h3>

`helm upgrade --wait --timeout 10 --set command=false hooks ./`

Output: 
```
UPGRADE FAILED
Error: timed out waiting for the condition
Error: UPGRADE FAILED: timed out waiting for the condition
```

<h3>4. Check pre-install hook job logs. There should be 2.</h3>

`kubectl logs job/hooks-pre-install`

Output: `2`

<h3>5. Rollback to successfully deployed release previously.</h3>

`helm rollback hooks 0`

<h3>6. Get logs for rollback job. There also should be revision number.</h3>

`kubectl logs job/hooks-pre-rollback`

Output: `1`

<h3>Expected:</h3> As I expect hooks configs are related to helm release and when I'm running rollback from failed 2 to successful 1 revision,  pre-rollback hooks should be created from spec provided by configuration of revision 2 and not from 1.

