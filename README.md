# logging-ocp

install logging operator, loki operator and cluster observability operator 



$ oc project openshift-logging

$ oc adm policy add-cluster-role-to-user collect-application-logs -z collector

$ oc adm policy add-cluster-role-to-user collect-audit-logs -z collector

$ oc adm policy add-cluster-role-to-user collect-infrastructure-logs -z collector

$ oc adm policy add-cluster-role-to-user logging-collector-logs-writer -z collector


$ oc -n openshift-logging create serviceaccount collector

$ oc create clusterrolebinding collect-application-logs --clusterrole=collect-application-logs --serviceaccount openshift-logging:collector

$ oc create clusterrolebinding collect-infrastructure-logs --clusterrole=collect-infrastructure-logs --serviceaccount openshift-logging:collector

$ oc create clusterrolebinding collect-audit-logs --clusterrole=collect-audit-logs --serviceaccount openshift-logging:collector


### AWS secret example:-
apiVersion: v1
kind: Secret
metadata:
  name: logging-loki-s3
  namespace: openshift-logging
stringData:
  access_key_id: AKIAIOSFODNN7EXAMPLE
  access_key_secret: wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY
  bucketnames: s3-bucket-name
  endpoint: https://s3.eu-central-1.amazonaws.com
  region: eu-central-1

### Azure secret example :  // azure blob storage needs to be created.
oc create secret generic logging-loki-azure --from-literal=container="<azure_container_name>" --from-literal=environment="<azure_environment>" --from-literal=account_name="<azure_account_name>" --from-literal=account_key="<azure_account_key>"

Supported environment values are AzureGlobal, AzureChinaCloud, AzureGermanCloud, or AzureUSGovernment.
