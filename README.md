oc create project nfs-autoprovisioner

helm repo add nfs-subdir-external-provisioner https://kubernetes-sigs.github.io/nfs-subdir-external-provisioner/

helm repo update

oc apply -f leaderrole.yaml
oc apply -f nfsrole.yaml

oc apply -f leaderrolebinding.yaml
oc apply -f nfsrolebinding.yaml 

oc adm policy add-scc-to-user hostmount-anyuid system:serviceaccount:nfs-autoprovisioner:nfs-client-provisioner



helm install nfs-provisioner \
  nfs-subdir-external-provisioner/nfs-subdir-external-provisioner \
  --set nfs.server=192.168.126.1 \
  --set nfs.path=/export/ocp/ocp/dynamic \
  --set storageClass.name=nfs-provisioner \
  --set serviceAccount.name=nfs-client-provisioner


	
