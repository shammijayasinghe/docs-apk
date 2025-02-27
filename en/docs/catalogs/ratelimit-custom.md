# Custom Rate Limit Policy

The following define Rate Limiting Policies at the Operation-level via a Rate Limiting Policy Custom Resource (CR) definition.

```
apiVersion: dp.wso2.com/v1alpha1
kind: RateLimitPolicy
metadata:
  name: http-bin-ratelimit-usergroup
spec:
  override:
    type: Custom
    custom:
      key: rlkey_usergroup
      value: admin
      rateLimit:
        requestsPerUnit: 20
        unit: Minute
    organization: default
  targetRef:
    kind: Gateway
    name: default
    group: gateway.networking.k8s.io

```