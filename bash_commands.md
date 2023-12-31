# K8s commands to remember before exam
- To check the iptables entry in kube-proxy if exists `iptables -L -t nat | grep db-service`
- check logs or configmap to check kube-proxy mode.i.e. iptables, ipvs etc
- service cidr is in kube-apiserver and controller manager args `--service-cluster-ip-range`
- pod cidr in `--pod-network-cidr` in kubeadm init.
- To fetch a page in busybox `wget -O (filename|-) [url]`  - means stdout e.g. `wget -O- https://stackoverflow.com` to write the file to std output. Or `wget -O index.html https://stackoverflow.com` or `wget https://stackoverflow.com` also download the index.html at current path. To set timeout `wget -T 2 -O- https://stackoverflow.com` will downloadand print output to stdout with timeout 2 secs.
- To fetch a page using curl. nginx has curl but busybox has wget. `curl https://stackoverflow.com` or `curl -m 2 -o index.html https://stackoverflow.com`  `-m` for max time. To get curl output silent `curl -s <ip:port>`
- To check if user or sa has permission `k auth can-i create secret --as system:serviceaccount:ns:sa` . note that sa is checked using `system:serviceaccount:_:_` _:_ is namespace:serviceaccountname
- To ssh into node and run command to get the output back on same node. `ssh node1 'crictl logs e3e2fr4' &>/data/c1.log` this will write the node on node doing the ssh.
- To verify the env var `k exec secret-pod -- env | grep APP`
- To verify the secrets `k exec secret-pod -- find /tmp/secret2`
- If `kubeadm join` fails, then call `kubeadm reset`
- To get a sh terminal for testing. This will give you a terminal to run commands. `k run tmp -i -t --rm=true --restart=Never --image=busybox -- sh`
- To run a command for testing. Note the removed `-t` from above command. `k run tmp -i --rm=true --restart=Never --image=busybox -- curl -m 2 svc:port`
- Another way to find the node of the running pod. `k get po -o wide` or `k get po pod1 -o yaml | grep nodeName`

  
# Vi Commands
- y to copy, p to paste, d to delete
- v to mark, arrow to select and move up down, > to ident right, < to ident left, . to continue
- to go to top of the file `gg`, to end of file `shift + g`
- To search `/hello` and then `n` for next occurence or enter to stop and search backward `?hello`
- To search and replace all occurence `:%s/hello/hola/g` to replace
- To search and replace first occurence in current line `:s/hello/hola/` to replace . `%` mean all line in file to search and `g` means all occuerence to replace in current (`s`) line or all lines (`%s`)
- to delete all occurences in file `%s/hello//g`
- To turn on/off line number `:set number` or `:set nonumber`. to go to line 22 `:22`
- To replace the text using `sed`. `sed -e "s/hello/holla/"`
- vi ~/.vimrc
  ````
  set et
  set ts=2
  set sw=2
  set sts=2
  ````
- vi ~/.bashrc
  ````
  alias kc="k create -f"
  alias ka="k apply -f"
  alias kr="k replace -f"
  alias kd="k describe"
  alias kgp="k get po"
  alias kga="k get all"
  alias kdp="k delete po"
  alias kg="k get"
  alias kgn="k get nodes"
  alias kgs="k get componentstatuses"
  export do="--dry-run=client -o yaml"
  export now="--force --grace-period=0"
  nk="-n kube-system"
  alias kn="k config set-context --current --namespace"
  alias knginx="k run tmpnginx -i --rm=true --restart=Never --image=nginx:alpine"
  alias kbusybox="k run tmpbusybox -i --rm=true --restart=Never --image=busybox"
  ````

# General
- To apend logs to a file. this will append instead of overriding `k logs pod >>pod_error.log`

# Systemd

## to check init system
`ps -p 1`
output contains  systemd if init system is systemd.

# for loop

## iterate over range 1... 10
`for i in $(seq 1 10); do echo $i; done;`
prints 1 2 3 4 5 6 ... in each line

## iterate over provided value in condition
`for i in hello world 1 10; do echo $i; done;`
prints `hello world 1 10` in each line

# if

## if only
`status="true" && if "$status" == "true"; then echo status is set; fi`

## if else
`status="false" && if "$status" == "true"; then echo status is set; else echo status is not set; fi`

### if else if
`type="color" && if [ "$type" == "color" ]; then echo orange; elif [ "$type" == "hero" ]; then echo spiderman; else echo type not known; fi`


# Install K8s
- Follow k8s documentation
- Install prerequisites
- Follow install kubeadm on k8s docmentation
- Create cluster. add advertise ip, extra sans, pod cidr based on network plugin
- add pod network, check urls . ensure interface name set for flannel in kube-flannel.yml
- check nodes status
