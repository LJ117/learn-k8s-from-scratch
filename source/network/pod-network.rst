Pod Networking
==================

Basic
---------

- Pod share a network namespace
- Containers in a Pod communicate over localhost

这一节需要安装的一些包(以Ubuntu为例)

- ``bridge-utils``
- ``net-tools``

在集群的所有节点上安装

.. code-block:: bash

    $ sudo apt install bridge-utils net-tools



.. image:: ../_static/network/pod-network.PNG
   :alt: pod-network



Container to Container in Pod
--------------------------------


.. code-block:: bash

  kubectl apply -f https://raw.githubusercontent.com/xiaopeng163/learn-k8s-from-scratch/master/source/_code/network/container-to-container.yml


.. literalinclude:: ../_code/network/container-to-container.yml
   :language: yaml
   :linenos:


Pod to Pod (single node)
-----------------------------

.. code-block:: yaml

    apiVersion: v1
    kind: Pod
    metadata:
      name: mypod1
    spec:
      nodeName: 'k8s-worker1'
      containers:
      - name: pod1
        image: xiaopeng163/net-box
        command: ["sh", "-c", "while true; do echo $(date) >> /tmp/index.html; sleep 60; done"]
    ---
    apiVersion: v1
    kind: Pod
    metadata:
      name: mypod2
    spec:
      nodeName: 'k8s-worker1'
      containers:
      - name: pod2
        image: xiaopeng163/net-box
        command: ["sh", "-c", "while true; do echo $(date) >> /tmp/index.html; sleep 60; done"]


Pod to Pod (multi-Node)
-----------------------------

.. code-block:: yaml

    apiVersion: v1
    kind: Pod
    metadata:
      name: mypod1
    spec:
      containers:
      - name: pod1
        image: xiaopeng163/net-box
        command: ["sh", "-c", "while true; do echo $(date) >> /tmp/index.html; sleep 60; done"]
    ---
    apiVersion: v1
    kind: Pod
    metadata:
      name: mypod2
    spec:
      containers:
      - name: pod2
        image: xiaopeng163/net-box
        command: ["sh", "-c", "while true; do echo $(date) >> /tmp/index.html; sleep 60; done"]


References
-------------------

https://kubernetes.io/docs/concepts/cluster-administration/networking/

https://medium.com/@anilkreddyr/kubernetes-with-flannel-understanding-the-networking-part-2-78b53e5364c7
