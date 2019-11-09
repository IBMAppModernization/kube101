# Lab 1. Deploy your first application

Learn how to deploy an application to a Kubernetes cluster running on the OpenShift container platform

## 1. Deploy the guestbook application

In this part of the lab we will deploy an application called `guestbook` that has already been built and uploaded to DockerHub under the name `ibmcom/guestbook:v1`. If you have not done so already, create a project for your work and make it your active project.

1. Start by running `guestbook`:

   ```$ oc create deployment guestbook --image=ibmcom/guestbook:v1```

   This action will take a bit of time. To check the status of the running application,
   you can use `$ oc get pods`.

   You should see output similar to the following:

   ```console
   $ oc get pods
   NAME                          READY     STATUS              RESTARTS   AGE
   guestbook-59bd679fdc-bxdg7    0/1       ContainerCreating   0          1m
   ```

   Eventually, the status should show up as `Running`.

   ```console
   $ oc get pods
   NAME                          READY     STATUS              RESTARTS   AGE
   guestbook-59bd679fdc-bxdg7    1/1       Running             0          1m
   ```

   The end result of the run command is not just the pod containing our application containers,
   but a Deployment resource that manages the lifecycle of those pods.

1. Once the status reads `Running`, we need to expose that deployment as a
   service so we can access it through the IP of the worker nodes.
   The `guestbook` application listens on port 3000.  Run:

   ```console
   $ oc expose deployment guestbook --type="NodePort" --port=3000
   service "guestbook" exposed
   ```

1. To find the port used on that worker node, examine your new service:

   ```console
   $ oc get service guestbook
   NAME        TYPE       CLUSTER-IP     EXTERNAL-IP   PORT(S)          AGE
   guestbook   NodePort   172.21.41.226   <none>        3000:30732/TCP   13s
   ```

   We can see that our `<nodeport>` is `30732`. We can see in the output the port mapping from 3000 inside 
   the pod exposed to the cluster on port 30732. This port in the 31000 range is automatically chosen, 
   and could be different for you.

1. `guestbook` is now running on your cluster, and exposed to the internet. We need to find out where it is accessible.
   The worker nodes running in the container service get external IP addresses.
   Get the workers for your cluster and note one (any one) of the public IPs listed on the `<public-IP>` line.

   ```console
   $ ibmcloud ks workers $MYCLUSTER
   Kubernetes version 1.16 has removed deprecated APIs. For more information, see <http://ibm.biz/k8s-1-16-apis>

   OK
   ID                                                     Public IP       Private IP     Flavor               State    Status   Zone    Version
   kube-bmsoq81w0o49g5j6mvpg-timrokata-default-00000136   169.62.40.202   10.191.61.67   b3c.4x16.encrypted   normal   Ready    wdc07   3.11.153_1529_openshift
   kube-bmsoq81w0o49g5j6mvpg-timrokata-default-0000025d   169.62.47.221   10.191.61.69   b3c.4x16.encrypted   normal   Ready    wdc07   3.11.153_1529_openshift
   ```

   In this example one of the workers `<public-IP>` is `169.62.40.202`. So to access this directly over tcp you could open `http://169.62.40.202:30732/ in your browser.

1. OpenShift also provides a simple way to expose a kubernetes service through a named http route, which defaults to be the service name plus the project name, appended with the base domain for the cluster. Create a route like this using:

    ```console
    $ oc expose service guestbook
    route.route.openshift.io/guestbook exposed
    ```

1. Get the hostname for the exposed application and create a URL:

    ```console
    $ GUESTBOOK=$(oc get route guestbook -o jsonpath='{ .spec.host }')
    $ echo http://$GUESTBOOK
    http://guestbook-user001.timro-kata-f0093114134cf555e1a213f3756140db-0001.us-east.containers.appdomain.cloud
    ```

    Open the URL in a new browser tab to see the application.

Congratulations, you've now deployed an application to Kubernetes!

When you're all done, continue to the
[next lab of this course](../Lab2/README.md).

