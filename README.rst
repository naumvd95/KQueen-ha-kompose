# KQueen-ha-kompose


KQueen deployment for K8s HA cluster ported with `Kompose utility <https://github.com/kubernetes/kompose>`_.

Sources
-------

* `Kqueen API <https://github.com/Mirantis/kqueen>`_.

* `Kqueen UI <https://github.com/Mirantis/kqueen-ui>`_.


Openstack
~~~~~~~~~

HA cluster needed

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