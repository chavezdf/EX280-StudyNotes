Objetivo: Crear un template de proyecto con las Network Policies, Limits and Quotes.

Paso 1: Exportar plantilla inicial

    oc adm create-bootstrap-project-template -o yaml > <filename>

Paso 2: Editar la plantilla:

Se deben colocar la definicion de recursos de NetworkPolicies, Limits and Quotes.

```yaml
# NetworkPolicies
- apiVersion: networking.k8s.io/v1
  kind: NetworkPolicy
  metadata:
    name: allow-from-openshift-ingress
  spec:
    podSelector: {}
    ingress:
    - from:
      - namespaceSelector:
          matchLabels:
            network.openshift.io/policy-group: ingress

- apiVersion: networking.k8s.io/v1
  kind: NetworkPolicy
  metadata:
    name: allow-same-namespace
  spec:
    podSelector: {}
    ingress:
    - from:
      - podSelector: {}

# Limits

- apiVersion: v1
  kind: LimitRange
  metadata:
    name: project-limitrange
  spec:
    limits:
    - default:
        memory: 100Mi
        cpu: 100m
    defaultRequest:
      memory: 30Mi
      cpu: 30m
    type: Container

# Quota

- apiVersion: v1
  kind: ResourceQuota
  metadata:
    name: project-quota
  spec:
    hard:
      pods: '10'
      requests.cpu: '2'
      requests.memory: 1Gi
      limits.cpu: '4'
      limits.memory: 4Gi

```

  
