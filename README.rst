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

*Kqueen OS-cluster setup:*


* LBaaS should be configured on OS-cloud
* 3 master 3 worker `Instruction <http://kqueen.readthedocs.io/en/latest/kqueen.html#provision-a-kubernetes-cluster-using-openstack-kubespray-engine>`_.
* Go to one of master nodes
* Verify that all k8s cluster components working properly:


    .. code-block:: bash
    
        calicoctl node status 
        kubectl get nodes
        kubectl get all --all-namespaces


*Kqueen GKE-cluster setup:*


* 3 worker `Instruction <https://kqueen.readthedocs.io/en/latest/kqueen.html#provision-a-kubernetes-cluster-using-google-kubernetes-engine>`_.


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
#. Overwrite helm values if necessary (`charts/XXX/values.yaml`)
#. Verify charts


    .. code-block:: bash

        ## verify syntax
        helm lint charts/XXX


#. Dry-run


    .. code-block:: bash

        ## dry-run
        helm install --dry-run --debug charts/kqueen-chart/ -n kqueen


#. Build chart and dependency-charts


    .. code-block:: bash

        helm package charts/kqueen-chart -u


#. Deploy KQueen


    .. code-block:: bash

        kubectl create ns kqueen
        helm install --debug kqueen-chart-*VERSION*.tgz -n kqueen --namespace kqueen
