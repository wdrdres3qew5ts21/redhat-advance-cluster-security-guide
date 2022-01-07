docker push quay.io/linxianer12/java-danger-log4j:0.0.1 

docker run quay.io/linxianer12/java-danger-log4j:0.0.1 


https://access.redhat.com/node/5822721


```
roxctl image scan --endpoint central-rhacs-operator.itzroks-666000ldq2-7q7o5f-4b4a324f027aea19c5cbc0c3275c4656-0000.hkg02.containers.appdomain.cloud:443 --image quay.io/linxianer12/java-danger-log4j:0.0.4  --token-file token
```
https://docs.openshift.com/acs/3.66/installing/install-ocp-operator.html

On the RHACS portal, navigate to Platform Configuration → Integrations.

Under the Authentication Tokens section, click on Cluster Init Bundle.

Click New Integration.

Enter a name for the cluster init bundle and click Generate.

Click Download Kubernetes Secret File to download the generated bundle.


```
oc run shell --labels=app=shellshock,team=test-team --image=quay.io/linxianer12/vulnerables:cve-2014-6271 

oc run samba --labels=app=rce  --image=docker.io/vulnerables/cve-2017-7494 

roxctl image scan --endpoint central-rhacs-operator.itzroks-666000ldq2-2vqn82-6ccd7f378ae819553d37d5f2ee142bd6-0000.jp-tok.containers.appdomain.cloud:443 --image quay.io/linxianer12/java-danger-log4j:0.0.1  -f  --token-file token

roxctl image scan --endpoint central-rhacs-operator.itzroks-666000ldq2-2vqn82-6ccd7f378ae819553d37d5f2ee142bd6-0000.jp-tok.containers.appdomain.cloud:443 --image quay.io/linxianer12/im-in-frontend:0.0.3  -f  --token-file token 

roxctl image scan --endpoint central-rhacs-operator.itzroks-666000ldq2-2vqn82-6ccd7f378ae819553d37d5f2ee142bd6-0000.jp-tok.containers.appdomain.cloud:443 --image quay.io/linxianer12/vulnerables:cve-2014-6271  -f  --token-file token 

#  have >=High Severity Vulnerable but Fixable in next release already

oc create deployment java-danger  --image quay.io/linxianer12/java-danger-log4j:0.0.1 

# have >=High Severity Vulnerable but Fixable in next release already
oc create deployment im-in-frontend --image quay.io/linxianer12/im-in-frontend:0.0.3

# have >=High Severity Vulnerable but Fixable in next release already
oc create deployment todoapp-frontend --image quay.io/linxianer12/todoapp-frontend:1.0.0

# Not have Fixable yet
oc create deployment shellshock --image quay.io/linxianer12/vulnerables:cve-2014-6271

```