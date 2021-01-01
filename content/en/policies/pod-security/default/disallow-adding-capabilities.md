---
type: "docs"
title: Disallow Adding Capabilities
linkTitle: Disallow Adding Capabilities
weight: 46
description: >
    Adding additional capabilities beyond the default set must not be allowed.
---

## Policy Definition
<a href="https://github.com/kyverno/policies/raw/main//pod-security/default/disallow-adding-capabilities.yaml" target="-blank">/pod-security/default/disallow-adding-capabilities.yaml</a>

```yaml
apiVersion: kyverno.io/v1
kind: ClusterPolicy
metadata:
  name: disallow-adding-capabilities
  annotations:
    policies.kyverno.io/category: Pod Security Standards (Default)
    policies.kyverno.io/description: >-
      Adding additional capabilities beyond the default set must not be allowed.
spec:
  validationFailureAction: audit
  background: true
  rules:
  - name: capabilities
    match:
      resources:
        kinds:
        - Pod
    validate:
      message: >-
        Adding of additional capabilities beyond the default set is not allowed.
        The fields spec.containers[*].securityContext.capabilities.add and 
        spec.initContainers[*].securityContext.capabilities.add must be empty.
      pattern:
        spec:
          containers:
          - =(securityContext):
              =(capabilities):
                X(add): null
          =(initContainers):
          - =(securityContext):
              =(capabilities):
                X(add): null

```