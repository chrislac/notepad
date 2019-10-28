---


---

<p>Your ingress don’t have any IP address assigned to it. It seems like you don’t have ingress enabled on your minikube system.<br>
I’ve reproduced this behavior and I also have this situation:</p>
<pre><code>user@bf:~$ kubectl get ingresses.
NAME              HOSTS              ADDRESS   PORTS   AGE
example-ingress   hello-world.info             80      34s
</code></pre>
<p>I’m not able to communicate to my HTTP server:</p>
<pre><code>user@bf:~$ curl hello-world.info
curl: (7) Failed to connect to hello-world.info port 80: Connection refused
</code></pre>
<p>To check if your ingress is enabled, please run the following command:</p>
<pre><code>$ minikube addons list
</code></pre>
<p>In the output you must see something like that:</p>
<pre><code>user@bf:~$ minikube addons list
- addon-manager: enabled
- dashboard: disabled
- default-storageclass: enabled
- efk: disabled
- freshpod: disabled
- gvisor: disabled
- heapster: disabled
- ingress: disabled
- logviewer: disabled
- metrics-server: disabled
- nvidia-driver-installer: disabled
- nvidia-gpu-device-plugin: disabled
- registry: disabled
- registry-creds: disabled
- storage-provisioner: enabled
- storage-provisioner-gluster: disabled
</code></pre>
<p>As you can see in my output, I have ingress disabled.</p>
<p>If that’s your case, please run the following command in order to enable it:</p>
<pre><code>user@bf:~$ minikube addons enable ingress
✅  ingress was successfully enabled
</code></pre>
<p>This command will create an ingress controller in you minikube system:</p>
<pre><code>user@bf:~$ kubectl get pods --all-namespaces
NAMESPACE     NAME                                       READY   STATUS    RESTARTS   AGE
default       web-6d4657545d-5gjg5                       1/1     Running   0          17m
default       web2-674dd45977-6xhkm                      1/1     Running   0          17m
kube-system   coredns-5c98db65d4-q95qx                   1/1     Running   1          117m
kube-system   coredns-5c98db65d4-qm85g                   1/1     Running   1          117m
kube-system   etcd-minikube                              1/1     Running   0          116m
kube-system   kube-addon-manager-minikube                1/1     Running   0          116m
kube-system   kube-apiserver-minikube                    1/1     Running   0          116m
kube-system   kube-controller-manager-minikube           1/1     Running   0          116m
kube-system   kube-proxy-4rbfh                           1/1     Running   0          117m
kube-system   kube-scheduler-minikube                    1/1     Running   0          116m
kube-system   nginx-ingress-controller-778fcbd96-6bg2b   1/1     Running   0          20s
kube-system   storage-provisioner                        1/1     Running   0          117m
</code></pre>
<p>After enabling it check if your ingress have an IP:</p>
<pre><code>user@bf:~$ kubectl get ingresses
NAME              HOSTS              ADDRESS           PORTS   AGE
example-ingress   hello-world.info   192.168.122.209   80      7m10s
</code></pre>
<p>Now it’s possible to comunicate using the ingress rule:</p>
<pre><code>user@bf:~$ curl hello-world.info
Hello, world!
Version: 1.0.0
Hostname: web-6d4657545d-5gjg5
</code></pre>

