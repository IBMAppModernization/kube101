# Lab 0. Set up your kubernetes / OpenShift environment

For the hands-on labs in this tutorial repository, you will need an OpenShift cluster. You can either create one as-a-service from the IBM Cloud Kubernetes Service, use a hosted environment or install one locally on your workstation.

## Use Red Hat OpenShift on the IBM Cloud Kubernetes Service

You will need a paid IBM Cloud account. Use the [Getting Started Guide](https://cloud.ibm.com/docs/openshift?topic=openshift-getting-started) to create your cluster.

## Use a hosted trial environment

There are a few services that are accessible over the Internet for temporary use. As these are free services, they can sometimes experience periods of limited availablity/quality. On the other hand, they can be a quick way to get started!

* [OpenShift playground on Katacoda](https://www.katacoda.com/openshift/courses/playgrounds) This environment starts with a master and worker node pre-configured. You can run the steps from Labs 1 and onward from the master node.

* [Red Hat Try OpenShift](https://try.openshift.com/) various options to try out OpenShift 4

## Set up on your own workstation

If you would like to configure kubernetes to run on your local workstation for non-production, learning use, there are several options.

* [Minishift](https://www.okd.io/minishift/) This solution requires the installation of a supported VM provider (KVM, VirtualBox, HyperKit, Hyper-V - depending on platform). Minishift is based on version 3.11 of OKD, the open source upstream for OpenShift

* [Code Ready Containers](https://kind.sigs.k8s.io/) Run OpenShift 4.x in a vm on your laptop. This solution requires the installation of a supported VM provider (KVM, VirtualBox, HyperKit, Hyper-V - depending on platform). Requires registration with Red Hat for the pull secret to configure the cluster.
