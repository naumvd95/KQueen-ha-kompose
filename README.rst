# KQueen-ha-kompose


KQueen deployment for K8s HA cluster ported with `Kompose utility <https://github.com/kubernetes/kompose>`_.

Sources
-------

* `Kqueen API <https://github.com/Mirantis/kqueen>`_.

* `Kqueen UI <https://github.com/Mirantis/kqueen-ui>`_.


Openstack
~~~~~~~~~

*Kqueen OS-cluster setup:*

p.s. Yep i use KubernetesQueen to deploy kubernetes environment to deploy KubernetesQueen. Yep, its weird. But fun=)


#. LBaaS should be configured on OS-cloud 
#. 3 master 3 slaves `Instruction <http://kqueen.readthedocs.io/en/latest/kqueen.html#provision-a-kubernetes-cluster-using-openstack-kubespray-engine>`_.
#. Go to one of master nodes
#. Verify that all k8s cluster components working properly:


    .. code-block:: bash
    
        calicoctl node status 
        kubectl get nodes
        kubectl get all --all-namespaces


#. Export k8s config or upload repo directly on master node
#. Deploy KQueen


    .. code-block:: bash
    
        kubectl create ns kqueen
    
        ## deploy os-pvc
        kubectl apply -f ./kqueen-kompose/storage_step1/openstack_cloud/pvc/
    
        ## deploy etcd with os-pvc
        kubectl apply -f ./kqueen-kompose/storage_step1/openstack_cloud/
    
        ## deploy kqueen-addons
        kubectl apply -f ./kqueen-kompose/kqueen-addons_step2/
    
        ## deploy kqueen api/ui
        kubectl apply -f ./kqueen-kompose/api_ui_step3/


#. Get ``EXTERNAL_IP`` for UI service:

 
    .. code-block:: bash
    
        kubectl get svc ui -n kqueen


