<p>You can grab your key and password by reading the secret keys and decode them.<br>
In my example we have the following secret created by the installer:</p>
<pre><code>christofoletti@bf:~/charts/stable/minio$ kubectl get secrets 
NAME                                TYPE                                  DATA   AGE
crusty-mongoose-minio               Opaque                                2      15m
crusty-mongoose-minio-token-jqbcb   kubernetes.io/service-account-token   3      11m
</code></pre>
<p>You can check your encoded key by running:</p>
<pre><code>user@bf:~/charts/stable/minio$ kubectl get secret --namespace default crusty-mongoose-minio -o yaml
apiVersion: v1
data:
  accesskey: bXlhY2Nlc3NrZXk=
  secretkey: bXlzZWNyZXRrZXk=   
kind: Secret
metadata:
  creationTimestamp: "2019-10-31T14:27:52Z"
  labels:
    app: minio
    chart: minio-2.5.16
    heritage: Tiller
    release: crusty-mongoose
  name: crusty-mongoose-minio
  namespace: default
  resourceVersion: "358025"
  selfLink: /api/v1/namespaces/default/secrets/crusty-mongoose-minio
  uid: af8ed190-4e59-49df-b584-824a4eb14439
type: Opaque
</code></pre>
<p>From here you can see my encoded access and secure keys:</p>
<pre><code>accesskey: bXlhY2Nlc3NrZXk=
secretkey: bXlzZWNyZXRrZXk=   
</code></pre>
<p>Now that we have it we can decode using the following command:</p>
<pre><code>$ echo bXlhY2Nlc3NrZXk= | base64 --decode
mysecretkey
 echo bXlzZWNyZXRrZXk= | base64 --decode
mysecretkey
</code></pre>
<p>Optionally you can grab using the following command:</p>
<pre><code>$ kubectl get secret --namespace default fashionable-elk-minio -o jsonpath="{.data.accesskey}" |e 
myaccesskey
$ kubectl get secret --namespace default fashionable-elk-minio -o jsonpath="{.data.secretkey}" | base64 --decode 
mysecretkey
</code></pre>

