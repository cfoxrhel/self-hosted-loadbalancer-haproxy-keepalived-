# self-hosted-loadbalancer-haproxy-keepalived-
Deploying a self-hosted loadbalancer (haproxy/keepalived) on k8s/openshift

As a means to bring up a self-hosted Loadbalancer with no dependencies on other open shift services. 

static pods are leveraged along with statically reserved IP's for the VIP's which have already had connectivity verified and appropriate DNS entries established. 


#copys required fileset to the needed location(you will only use either the 1936 or the 1937.tgz files not both) before being unpacked. 

cp /tmp/*.tgz /etc/kubernetes/static-pod-resources/; 

 

#unpacks the appropriate files 

cd /etc/kubernetes/static-pod-resources/; 

tar -xvzf keepalived.tgz 

tar -xvzf haproxy.tgz 

 

#this moves the manifest file to the manfiest directory and will automatically start the pods 

mv /etc/kubernetes/static-pod-resources/keepalived/keepalived.yaml /etc/kubernetes/manifests/; 

mv /etc/kubernetes/static-pod-resources/haproxy/haproxy.yaml /etc/kubernetes/manifests/; 

 

#changes file context for selinux if the file context isnt preserved in tar, will only need to do once if at all.  

semanage fcontext -a -t kubernetes_file_t /etc/kubernetes/static-pod-resources/keepalived/keepalived.conf.tmpl; 

semanage fcontext -a -t kubernetes_file_t /etc/kubernetes/static-pod-resources/keepalived/scripts/chk_default_ingress.sh; 

semanage fcontext -a -t kubernetes_file_t /etc/kubernetes/static-pod-resources/keepalived/scripts/chk_ocp_script.sh; 

semanage fcontext -a -t kubernetes_file_t /etc/kubernetes/static-pod-resources/keepalived/scripts/chk_ocp_script_both.sh; 

Semanage fcontext -a -t kubernetes_ file t /etc/kubernetes/static-pod-resources/haprox.cfg.tmpl; 

semanage fcontext -a -t kubernetes_file_t /etc/kubernetes/manifests/haproxy.yaml 

semanage fcontext -a -t kubernetes_file_t /etc/kubernetes/manaifest/keepalived.yaml 

 

 

#will only need to do once 

restorecon -v -R /etc/kubernetes/static-pod-resources/keepalived/scripts/chk_default_ingress.sh; 

restorecon -v -R /etc/kubernetes/static-pod-resources/keepalived/scripts/chk_ocp_ script.sh; 

restorecon -v -R /etc/kubernetes/static-pod-resources/keepalived/scripts/chk_ ocp script both.sh, 

restorecon -v -R /etc/kubernetes/static-pod-resources/haproxy/haproxy.cfg.tmpl; 

restorecon -v -R /etc/kubernetes/manifests/haproxy.yaml 

restorecon -v -R /etc/kubernetes/manifests/keepalived.yaml 

restorecon -v -R /etc/kubernetes/static-pod-resources/keepalived/keepalived.conf.tmpl; 

 
