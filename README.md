#HEAT templates to build Sahara vanilla Hadoop cluster


Create individual stacks for each component (requires parameter updates for each stack based on dependencies)
```
openstack stack create -t sahara_vanilla_hadoop_image.yaml register_vanilla_image \
--parameter image_url=http://sahara-files.mirantis.com/images/upstream/newton/sahara-newton-vanilla-2.7.1-centos7.qcow2
 
openstack stack create -t vanilla_hadoop_node_templates.yaml vanilla_node_templates --parameter image_id=<IMAGE_ID>
 
# Get node template ids to use in cluster template stack
openstack dataprocessing node group template list
 
openstack stack create -t vanilla_cluster_template.yaml cluster_template \
    --parameter master_node_template_id=0326df32-c333-4972-882e-b5be86cff32a \
    --parameter worker_node_template_id=c1ab821d-9d94-4317-a9a1-5473b8df0ff5
    
# Get cluster template id to use with cluster stack
openstack dataprocessing cluster template list
 
openstack stack create -t vanilla_cluster.yaml vanilla_cluster
```

All-in-one cluster template (Update environment file)
```
cat vanilla_hadoop_environment.yaml
 
parameter_defaults:
  node_key_name: mykey
  image_url: http://sahara-files.mirantis.com/images/upstream/newton/sahara-newton-vanilla-2.7.1-centos7.qcow2
  image_name: vanilla-hadoop-2.7.1
  admin_id: centos
  image_tags: [ "vanilla", "2.7.1" ]
  
openstack stack create -t vanilla_hadoop_whole_cluster.yaml -e vanilla_hadoop_environment.yaml vanilla_hadoop_cluster
```