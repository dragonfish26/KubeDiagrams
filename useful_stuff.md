# Notes for me


Run kubediagrams:

    ../../bin/kube-diagrams cert-manager.yaml --without-namespace

    kube-diagrams default.yml


To run on mac:

    spirals@Spiralss-MacBook-Pro kubedia % python3 -m venv envi
    spirals@Spiralss-MacBook-Pro kubedia % source envi/bin/activate

    (envi) spirals@Spiralss-MacBook-Pro kubedia % pip install KubeDiagrams 
    (envi) spirals@Spiralss-MacBook-Pro kubedia % pip3 install PyYAML diagrams

    (envi) spirals@Spiralss-MacBook-Pro kubedia % 
    (envi) spirals@Spiralss-MacBook-Pro kubedia % bin/nomi examples/wordpress/*.yaml  

    (envi) spirals@Spiralss-MacBook-Pro kubedia % kube-diagrams examples/oai-5g-cn/case1.yaml


Example to work on:

oai-5g-cn : case1-without-namespace


Helmcharts/ elasticsearch:

 - K8s Instance: elasticsearch
   - Helm Chart: elasticsearch
     - Resource elasticsearch-vmump-test/default/Pod/v1
     - K8s Application: elasticsearch-master
       - Resource elasticsearch-master-pdb/default/PodDisruptionBudget/policy/v1
       - Resource elasticsearch-master/default/Service/v1
       - Resource elasticsearch-master-headless/default/Service/v1
       - Resource elasticsearch-master/default/StatefulSet/apps/v1
       - Resource elasticsearch-master-elasticsearch-master/default/PersistentVolumeClaim/v1
 - K8s Application: middle
   - Resource elasticsearch-master-certs/default/Secret/v1
   - Resource elasticsearch-master-credentials/default/Secret/v1



class ResourceCluster:
    """
        Hierarchical clustering of Kubernetes resources.
    """
    def __init__(self, name):
        """
            Initialize a ResourceCluster instance.
        """
        self.name = name
        self.resources = {} # dict<str, resource>
        self.clusters = {} # dict<str, Cluster>
        self.graph_attr = { "tooltip": name }

    def get_or_create_cluster(self, name):
        """
            Get or create a sub cluster.
        """
        cluster = self.clusters.get(name)
        if cluster is None:
            cluster = ResourceCluster(name)
            self.clusters[name] = cluster
        return cluster

    def display(self, ident=0):
        """
            Display sub clusters and Kubernetes resources recursively.
        """
        for rid, _ in self.resources.items():
            print("  " * ident, f"- Resource {rid}")
        for cid, sub_cluster in self.clusters.items():
            print("  " * ident, f"- {cid}")
            sub_cluster.display(ident + 1)



Note -- Selectors: change labels 

Start with algo 3

 - Namespace: default
   - Resource oai-amf/default/ServiceAccount/v1
   - Resource oai-ausf/default/ServiceAccount/v1
   - Resource oai-gnb-sa/default/ServiceAccount/v1
   - Resource oai-nr-ue-sa/default/ServiceAccount/v1
   - Resource oai-nrf/default/ServiceAccount/v1
   - Resource oai-smf/default/ServiceAccount/v1
   - Resource oai-traffic-server/default/ServiceAccount/v1
   - Resource oai-udm/default/ServiceAccount/v1
   - Resource oai-udr/default/ServiceAccount/v1
   - Resource oai-upf/default/ServiceAccount/v1
   - Resource oai-cn5g-mysql-initialization/default/ConfigMap/v1
   - Resource oai-gnb-configmap/default/ConfigMap/v1
   - Resource oai-nr-ue-configmap/default/ConfigMap/v1
   - Resource iperf-pod/default/ConfigMap/v1
   - Resource core/default/ConfigMap/v1
   - Resource oai-traffic-server/default/Deployment/apps/v1
   - Release: oai-cn5g
     - Chart: mysql-9.0.1
       - K8s Application: oai-cn5g-mysql
         - Resource oai-cn5g-mysql/default/Secret/v1
         - Resource mysql/default/Service/v1
   - K8s Instance: oai-cn5g
     - Helm Chart: oai-amf-v2.1.0
       - K8s Application: oai-amf
         - Resource oai-amf/default/Service/v1
         - Resource oai-amf/default/Deployment/apps/v1
     - Helm Chart: oai-ausf-v2.1.0
       - K8s Application: oai-ausf
         - Resource oai-ausf/default/Service/v1
         - Resource oai-ausf/default/Deployment/apps/v1
     - Helm Chart: oai-gnb-2.1.0
       - K8s Application: oai-gnb
         - Resource oai-ran/default/Service/v1
         - Resource oai-gnb/default/Deployment/apps/v1
     - Helm Chart: oai-nrf-v2.1.0
       - K8s Application: oai-nrf
         - Resource oai-nrf/default/Service/v1
         - Resource oai-nrf/default/Deployment/apps/v1
     - Helm Chart: oai-smf-v2.1.0
       - K8s Application: oai-smf
         - Resource oai-smf/default/Service/v1
         - Resource oai-smf/default/Deployment/apps/v1
     - Helm Chart: oai-udm-v2.1.0
       - K8s Application: oai-udm
         - Resource oai-udm/default/Service/v1
         - Resource oai-udm/default/Deployment/apps/v1
     - Helm Chart: oai-udr-v2.1.0
       - K8s Application: oai-udr
         - Resource oai-udr/default/Service/v1
         - Resource oai-udr/default/Deployment/apps/v1
     - Helm Chart: oai-upf-v2.1.0
       - K8s Application: oai-upf
         - Resource oai-upf/default/Service/v1
         - Resource oai-upf/default/Deployment/apps/v1
     - Chart: mysql-9.0.1
       - K8s Application: oai-cn5g-mysql
         - Resource oai-cn5g-mysql/default/Deployment/apps/v1
     - Helm Chart: oai-nr-ue-2.1.0
       - K8s Application: oai-nr-ue
         - Resource oai-nr-ue/default/Deployment/apps/v1