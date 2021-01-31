## Redeploy after source code update

### Rollout
With a rollout deployment, we can quickly propagate a update to our users, without impacting the continued performance of our application.

1. Build the updated image, and store it in localhost:32000 with a version tag: `[VERSION]`.
2. For a new todo-api image, run: <br/>
`kubectl --record deployment.apps/todo-api-deployment set image deployment.v1.apps/todo-api-deployment todo-api-container=localhost:32000/todo-api:[VERSION]` <br/>.
3. For a new todo-ui image, run: <br/>
`kubectl --record deployment.apps/todo-ui-deployment set image deployment.v1.apps/todo-ui-deployment todo-ui-container=localhost:32000/todo-ui:[VERSION]` <br/>.
4. This will trigger a rollout. The rollout can be monitored with the following command: <br/>
`kubectl rollout status deployment/todo-ui-deployment` <br/>.
5. The following command can be used to verify that all pods are running correctly on the same deployment: <br/>
`kubectl get pods --show-labels` <br/>

#### Canary
Through a canary deployment, the new version can be tested out on subsets of the users, to allow for small scale testing of the application. As the new deployment passes its tests, it can be slowly scaled up to be serviced to more and more users, eventually completely replacing the old version.

1. Build the updated image, and store it in localhost:32000 with a version tag: `[VERSION]`.
2. For a new todo-api image, run: <br/>
`kubectl --record deployment.apps/todo-api-deployment-canary set image deployment.v1.apps/todo-api-deployment-canary todo-api-container=localhost:32000/todo-api:[VERSION]` <br/>.
3. For a new todo-ui image, run: <br/>
`kubectl --record deployment.apps/todo-ui-deployment-canary set image deployment.v1.apps/todo-ui-deployment-canary todo-ui-container=localhost:32000/todo-ui:[VERSION]` <br/>.
4. The canary deployments will now be running the new images, but will have 0 pods running, as their replica count is defealt set to 0. To test our new image, we will set the replica count of the corresponding canary deployment to `[N]` for `N=n/10` for `[n]` amount of replicas normally running, rounded to the nearest whole positive number larger than 0.
5. For the API deployment, run: <br/>
`kubectl scale --replicas=[N] deploy todo-api-deployment-canary` <br/>.
6. 5. For the UI deployment, run: <br/>
`kubectl scale --replicas=[N] deploy todo-ui-deployment-canary` <br/>.
7. As the new version gains approval, the above command can be repeated with different values of `[N]` until it is equal to `[n]` the total amount of desired pods running the application.
8. As the amount of pods running the new version are increased, at the same time, the amount of pods can be decreased by running the same command but without the word "-canary" added at the end, and with `[N]` set to the lowered amount of pods desired to run.
