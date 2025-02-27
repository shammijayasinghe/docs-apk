# Message Transformation using Interceptor APIPolicy

You can use `APIPolicy` CR with interceptor configurations to carry out transformations and mediation on the requests and responses. Request interceptor gets triggered before sending the request to the backend. Response interceptor gets triggered before responding to the client. Here, an interceptor is a separate microservice that handles the request, response, or both request and response transformations.

##### Sample Interceptor APIPolicy
```
apiVersion: dp.wso2.com/v1alpha1
kind: APIPolicy
metadata:
  name: interceptor-api-policy
  namespace: ns
spec:
  override:
    requestInterceptor:
      backendRef:
         name: req-interceptor-backend
         namespace: ns
      includes:
      - request_body
    responseInterceptor:
      backendRef:
         name: res-interceptor-backend
         namespace: ns
      - response_headers
  targetRef:
    group: dp.wso2.com
    kind: API
    name: sample-api
```

When you have attahced an Interceptor `APIPolicy` like above example to your `API`, then the request flow (`1`, `2`, `3`, and `4` numbered circles) and response flow (`5`, `6`, `7`, and `8` numbered circles) can be depicted as below.

![Interceptors]({{base_path}}/en/latest/assets/img/api-management/api-policies/interceptors/interceptors-light.png#only-light)
![Interceptors]({{base_path}}/en/latest/assets/img/api-management/api-policies/interceptors/interceptors-dark.png#only-dark)

If you are an API developer, you can write a custom request/response interceptor microservice in any programming language of your choice by following [the Interceptor OpenAPI Definition](https://github.com/wso2/apk/blob/main/developer/resources/interceptor-service-open-api-v1.yaml). Then you can configure the following spec feilds in the `APIPolicy` CR to wire your interceptor serivce with the `API`.

<table>
<thead>
  <tr>
    <th>Policy Spec Field</th>
    <th>Description</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td style="white-space: nowrap;"><a href="#requestInterceptor"><code>requestInterceptor</code></a></td>
    <td>Interceptor service configuration for the request path</td>
  </tr>
  <tr>
    <td style="white-space: nowrap;"><a href="#responseInterceptor"><code>responseInterceptor</code></a></td>
    <td>Interceptor service configuration for the response path</td>
  </tr>
</tbody>
</table>


You can define interceptors on an API level (per API) and at resource level. How to do these are explained below in detail. If a request/response interceptor is on an API level and a resource level, interceptors properties are combined using proirity order defined in the following order. You can define properties under `default` and/or `override` section in API level `APIPolicy` and/or resource level `APIPolicy`. These values are combined using the priority order defined below, with priority decreasing towards the bottom of the table.

<table>
<tbody>
  <tr>
    <td>API level override</td>
  </tr>
  <tr>
    <td>Resource level override</td>
  </tr>
  <tr>
    <td>Resource level default</td>
  </tr>
  <tr>
    <td>API level default</td>
  </tr>
</tbody>
</table>

When you want the policy only to be defined in a single level, then defining the interceptor configurations in either `default` or `override` sections will work.

## Configuring Interceptors

Configuring an interceptor requires the following two steps.

1. [Implement an interceptor microservice adhering to the Interceptor OpenAPI Definition](https://apim.docs.wso2.com/en/latest/deploy-and-publish/deploy-on-gateway/choreo-connect/message-transformation/interceptor-microservice/interceptor-microservice/)

2. Create `APIPolicy` with interceptor configuration and attach it to your `API`.
    
    - [Attach Interceptor APIPolicy via REST API](../interceptors-via-rest-api)
    - [Attach Interceptor APIPolicy via CR](../interceptors-via-crs)
