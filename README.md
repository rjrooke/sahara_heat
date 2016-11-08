#Sahara cluster with HEAT
```
glance image-create --name sahara-vanilla-2.7.1-centos7 --disk-format qcow2 --container-format bare \
--is-public true --progress --file sahara-newton-vanilla-2.7.1-centos7.qcow2 

openstack stack create -t sahara_vanilla_hadoop_image.yaml register_vanilla_image
openstack stack create -t vanilla_hadoop_node_templates.yaml vanilla_node_templates
openstack stack create -t vanilla_cluster_template.yaml cluster_template
openstack stack create -t vanilla_cluster.yaml vanilla_cluster
```