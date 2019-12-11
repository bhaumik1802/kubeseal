# kubeseal

--> You can install the operator with:

kubectl apply -f \
  https://github.com/bitnami-labs/sealed-secrets/releases/download/v0.8.1/controller.yaml


--> You can install the command-line tool (on macOS) with:
brew install kubeseal

k apply -f controller.yaml -n anz

POD should come up : sealed-secrets-controller-5cb5dd5b8f-vvmpk   1/1     Running     0          26m

--> When the operator starts, it generates a private and public key.
The private key stays in the cluster, but you can retrieve the public key with the kubeseal CLI:

$ kubeseal --fetch-cert --controller-namespace anz > mycert.pem

Once you have the public key, you can encrypt all your secrets.
Storing the public key and the secrets in the repository are safe, even if the repo is public, as the public key is used only for encryption.

============

kubectl config set-context --current --namespace=anz
shahs@shahs-JSS1471:~/Kafka/Kubeseal$ kubectl create secret generic mysecret --dry-run --from-literal=password=supersekret -o json | kubeseal --cert kubeseal-docker-desktop.pem > mysealedsecret.json
shahs@shahs-JSS1471:~/Kafka/Kubeseal$ kubectl create -f mysealedsecret.jsonsealedsecret.bitnami.com/mysecret created
shahs@shahs-JSS1471:~/Kafka/Kubeseal$ k apply -f sealedsecret.yaml -n anz
pod/mypod created
shahs@shahs-JSS1471:~/Kafka/Kubeseal$ k exec -it mypod /bin/bash -n anz
root@mypod:/data# cd /etc/foo/
root@mypod:/etc/foo# ls
password
root@mypod:/etc/foo# more password
supersekret
root@mypod:/etc/foo#

