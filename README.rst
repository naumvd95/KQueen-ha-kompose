# KQueen-ha-kompose


KQueen deployment for K8s HA cluster ported with `Kompose utility <https://github.com/kubernetes/kompose>`_.

KQueen chart for K8s HA cluster ported with `Helm <https://github.com/helm/helm>`_.

Sources
-------

* `Kqueen API <https://github.com/Mirantis/kqueen>`_.

* `Kqueen UI <https://github.com/Mirantis/kqueen-ui>`_.

p.s. Yep i use KubernetesQueen to deploy kubernetes environment to deploy KubernetesQueen for kubernetes clusters deployments.
Yep, its weird. But fun=)


Reqirements
-----------

* Works properly with:

    .. code-block:: bash
    
        ➜  KQueen-ha-kompose git:(master) ✗ helm version
        Client: &version.Version{SemVer:"v2.9.1", GitCommit:"20adb27c7c5868466912eebdf6664e7390ebe710", GitTreeState:"clean"}
        Server: &version.Version{SemVer:"v2.9.1", GitCommit:"20adb27c7c5868466912eebdf6664e7390ebe710", GitTreeState:"clean"}

        ➜  KQueen-ha-kompose git:(master) ✗ kubectl version
        Client Version: version.Info{Major:"1", Minor:"10", GitVersion:"v1.10.3"}
        Server Version: version.Info{Major:"1", Minor:"9+", GitVersion:"v1.9.7-gke.6"}


*Kqueen OS-cluster setup:*


* LBaaS should be configured on OS-cloud
* 3-master/3-worker cluster `Instruction <http://kqueen.readthedocs.io/en/latest/kqueen.html#provision-a-kubernetes-cluster-using-openstack-kubespray-engine>`_.
* Go to one of master nodes
* Verify that all k8s cluster components working properly:


    .. code-block:: bash
    
        calicoctl node status 
        kubectl get nodes
        kubectl get all --all-namespaces


*Kqueen GKE-cluster setup:*


* 3-worker cluster `Instruction <https://kqueen.readthedocs.io/en/latest/kqueen.html#provision-a-kubernetes-cluster-using-google-kubernetes-engine>`_.


Kompose-guide
~~~~~~~~~~~~~


#. Export k8s config or upload repo directly on master node
#. Deploy KQueen


    .. code-block:: bash
    
        kubectl create ns kqueen 
   
        ## deploy pvc
        kubectl apply -f ./kqueen-kompose/storage_step1/XXX_cloud/
    
        ## deploy etcd with pvc
        kubectl apply -f ./kqueen-kompose/storage_step1/
    
        ## deploy kqueen-addons
        kubectl apply -f ./kqueen-kompose/kqueen-addons_step2/
    
        ## deploy kqueen api/ui
        kubectl apply -f ./kqueen-kompose/api_ui_step3/


#. Get ``EXTERNAL_IP`` for UI service:

 
    .. code-block:: bash
    
        kubectl get svc ui -n kqueen



Helm-guide
~~~~~~~~~~


#. Export k8s config or upload repo directly on master node
#. Overwrite helm values in `charts/XXX/values.yaml` (`GKE` provider defined by default)
#. Verify charts


    .. code-block:: bash

        ## verify syntax
        helm lint charts/XXX


#. Dry-run


    .. code-block:: bash

        ## dry-run
        helm dep build charts/kqueen-chart
        helm install --dry-run --debug charts/kqueen-chart -n kqueen


#. Build chart and dependency-charts


    .. code-block:: bash

        helm package charts/kqueen-chart -u


#. Deploy KQueen


    .. code-block:: bash

        kubectl create ns kqueen
        helm install --debug kqueen-chart-*VERSION*.tgz -n kqueen --namespace kqueen
