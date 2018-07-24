# KQueen-ha-kompose
KQueen deployment for K8s HA cluster ported with `Kompose utility <https://github.com/kubernetes/kompose>`_.

Sources
-------

* Kqueen API:
https://github.com/Mirantis/kqueen

* Kqueen UI:
https://github.com/Mirantis/kqueen-ui



Deployments
___________


Openstack
---------
* HA cluster needed

.. code-block:: bash

    kubectl create ns kqueen

    ## deploy os-pvc
    kubectl apply -f ./kqueen-kompose/storage_step1/openstack_cloud/pvc/ -n kqueen

    ## deploy etcd with os-pvc
    kubectl apply -f ./kqueen-kompose/storage_step1/openstack_cloud/ -n kqueen

    ## deploy kqueen-addons
    kubectl apply -f ./kqueen-kompose/kqueen-addons_step2/ -n kqueen

    ## deploy kqueen api/ui
    kubectl apply -f ./kqueen-kompose/api_ui_step3/ -n kqueen
