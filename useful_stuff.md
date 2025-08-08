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

Cluster: K8s Application: cassandra 
for resource cassandra : 
resource labels {'app.kubernetes.io/name': 'cassandra'}


Yes, by updating the labels dictionary in the update_resource_labels function, you are updating the resource item.

This works because labels = query_path(resource, "metadata.labels") returns a reference to the dictionary inside the resource object.
When you modify labels (e.g., with labels[new_label] = value or del labels[label]), you are directly changing the contents of resource["metadata"]["labels"].

Summary:

Changes to labels are reflected in resource["metadata"]["labels"].
You are updating the resource item in-place.


Altered functions:

process_edges -> collect_edges, draw_edges, build_parent_nodes


clusters:
  - label: app.kubernetes.io/instance
    title: K8s Instance
    recommended: true
    recommended_label: None
    graph_attr:
      bgcolor: "#E5F5FD"
  - label: release
    title: Release
    recommended: false
    recommended_label: app.kubernetes.io/instance
    graph_attr:
      bgcolor: "#E5F5FD"
  - label: helm.sh/chart
    title: Helm Chart
    recommended: true
    recommended_label: None
    graph_attr:
      bgcolor: "#EBF3E7"
  - label: chart
    title: Chart
    recommended: false
    recommended_label: helm.sh/chart
    graph_attr:
      bgcolor: "#EBF3E7"
  - label: app.kubernetes.io/name
    title: K8s Application
    recommended: true
    recommended_label: None
    graph_attr:
      bgcolor: "#ECE8F6"
  - label: app
    title: Application
    recommended: false
    recommended_label: app.kubernetes.io/name
    graph_attr:
      bgcolor: "#ECE8F6"
  - label: app.kubernetes.io/component
    title: K8s Component
    recommended: true
    recommended_label: None
    graph_attr:
      bgcolor: "#FDF7E3"
  - label: service
    title: Microservice
    recommended: false
    recommended_label: None
    graph_attr:
      bgcolor: "#FDF7E3"
  - label: tier
    title: Tier
    recommended: false
    recommended_label: None
    graph_attr:
      bgcolor: "#FDF7E3"
  - annotation: helm.sh/hook
    title: "{}"
    recommended: true
    recommended_label: None
    graph_attr:
      bgcolor: "#EBF3E7"
      style: dotted,rounded

for file in charts/*.yaml; do
  bin/nomi3 --without-namespace "$file"
done
