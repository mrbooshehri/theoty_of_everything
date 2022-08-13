## https://stackoverflow.com/questions/64241970/how-to-remove-a-master-node-from-a-ha-cluster-and-also-from-etcd-cluster?noredirect=1#comment113612607_64241970
kubectl exec etcd-< nodeNameMasterNode > -n kube-system -- etcdctl --cacert /etc/kubernetes/pki/etcd/ca.crt --cert /etc/kubernetes/pki/etcd/peer.crt --key /etc/kubernetes/pki/etcd/peer.key member list

kubectl exec etcd-nodeNameMaster1 -n kube-system -- etcdctl --cacert /etc/kubernetes/pki/etcd/ca.crt --cert /etc/kubernetes/pki/etcd/peer.crt --key /etc/kubernetes/pki/etcd/peer.key member remove b0c50c50d563ed51
