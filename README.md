![worpress_operator_CI_Prod](https://github.com/nk-designz/wordpress-operator/workflows/worpress_operator_CI_Prod/badge.svg)
# Wordpress Operator ( working on OpenShift)
This is an operator for wordpress on K8s build with OpenShift in mind.

# Deployment
If you use OpenShift just change kubectl to oc.
```bash
git clone git@github.com:nk-designz/wordpress-operator.git
cd wordpress-operator
kubectl apply -f wordpress-operator/deploy/service_account.yaml
kubectl apply -f wordpress-operator/deploy/role.yaml
kubectl apply -f wordpress-operator/deploy/role_binding.yaml 
kubectl apply -f wordpress-operator/deploy/crds/wordpress.netzlink.com_wordpresses_crd.yaml
kubectl apply -f wordpress-operator/deploy/operator.yaml
kubectl apply -f wordpress-operator/deploy/crds/wordpress.netzlink.com_v1alpha1_wordpress_cr.yaml
```
verify it's woriking
```bash
kubectl get pods -w
```
you should be ready to go!
# Example

```bash
touch my-wordpress.yaml
vim my-wordpress.yaml
```
This is a example configuration.
It utilizes a external mariadb
```yaml
apiVersion:  wordpress.netzlink.com/v1alpha1
kind:         Wordpress
metadata:
  name: netzlink
spec:
  externalDatabase:
    database: netzlink
    host: wordpressdb-mariadb
    password: <secret>
    port: 3306
    user: netzlink
  persistence:
    enabled:      true
    size:         10Gi
  resources:
    requests:
      cpu:     300m
  securityContext:
    enabled: false
  wordpressBlogName:     User's Blog!
  wordpressEmail:         user@example.com
  wordpressFirstName:    FirstName
  wordpressLastName:     LastName
  wordpressUsername:      user
```
apply it to our cluster
```bash
kubectl apply -f my-wordpress.yaml
```
verify it's woriking again
```bash
kubectl get pods -w
kubectl port-forward svc/netzlink-wordpress :80
```
Open your browser on the port shown
# Todo
- The database configuration for this operator in OpenShift is broken due to security contexts in the bitnami mariadb file. You can only use external ones.
# Thanks
- RedHat for the awesome operator-framework
- ASO for introducing me to it
- Bitnami for there Helm file for Wordpress
- CNCF for K8S 
