To print a join command for a new worker node use:

    kubeadm token create --print-join-command

But if you need to join a new control plane node, you need to recreate a new key for the control plane join command. This can be done with three simple steps:

Re upload certificates in the already working master node with : 
    kubeadm init phase upload-certs --upload-certs
That will generate a new certificate key.

Print join command in the already working master node with 
    kubeadm token create --print-join-command

Join a new control plane node with 
    $JOIN_COMMAND_FROM_STEP2 --control-plane --certificate-key $KEY_FROM_STEP1
