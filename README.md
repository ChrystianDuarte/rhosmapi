# Red Hat Integration + Openshift Service Mesh 

This demo describes how to show the life cycle of an API in online fuse and how it is synchronized in an ecosystem of Openshift Service Mesh and Securitized under 3Scale.

https://www.cncf.io/blog/2020/03/06/the-difference-between-api-gateways-and-service-mesh/

![enter image description here](https://2tjosk2rxzc21medji3nfn1g-wpengine.netdna-ssl.com/wp-content/uploads/2020/02/Screen-Shot-2020-02-25-at-11.11.14-AM-1024x823.png)
# Install

THE CURRENT SUPPORTED VERSION of RHMI WORKSHOP is 2.3.0!! PLEASE INSTALL THE CORRECT VERSION!

![RHPDS](https://drive.google.com/uc?id=1B6Vq0URuGPAazpgsqR9qef_CM_AgnCCp)
## Environment

When the lab enviroment its ready, please go to the Solution Explorer and then click in Red Hat Fuse Online (version 7.6), Open console

![RHMI Solution Explorer](https://drive.google.com/uc?id=1iJ7OmbZME6A7g_GAUZHvDp0vLf9iv5dJ)
# Fuse Online

When you enter to Fuse Online
![FuseOnlineConsole](https://drive.google.com/uc?id=1hR7rcDa_QZsKNApfjz_2F4pVuVOspQf0)
## Login Openshift Cluster

login with the "evalsx" user using copy login command
 ![OPenshift Console](https://drive.google.com/uc?id=1Dn_mHRe-KnPoRPgtq7Fe1bqCBPTc9Csq)

    oc login --token=<token web> --server=https://api.cluster-xxx-xxxxx.xxxxx.example.opentlc.com:6443


## Database Users

Prepare the environment in Openshift Cluster

	oc new-project evals3-fuse-db
	oc new-app --template=postgresql-persistent --param=POSTGRESQL_PASSWORD=redhat --param=POSTGRESQL_USER=redhat --param=POSTGRESQL_DATABASE=sampledb

When the database pod its ready, we will Create and populate database

    oc get pods
    oc rsh <pod postgress>
    	
	psql -U redhat -d sampledb	
	CREATE TABLE users(
	 id serial PRIMARY KEY,
	 name VARCHAR (50),
	 phone VARCHAR (50),
	 age integer
	);
	INSERT INTO users(name, phone, age) VALUES  ('Rodrigo Ramalho', '(11) 95474-8099', 30);
	INSERT INTO users(name, phone, age) VALUES  ('Rafael Ramalho', '(61) 99988-8029', 32);
	INSERT INTO users(name, phone, age) VALUES  ('Thiago Araki', '(11) 9999-9999', 33);
	INSERT INTO users(name, phone, age) VALUES  ('Gustavo Luszynsk', '(11) 9999-9999', 33);
	INSERT INTO users(name, phone, age) VALUES  ('Rafael Tuelho', '(11) 9999-9999', 33);

verify the inserts;

    sampledb=> select * from users;
    
![Validate select](https://drive.google.com/uc?id=1lQZS721dQJiLF2eJBXucLjVwC3gyz6Hy)

Configure a database connector in Fuse Online

	url: jdbc:postgresql://postgresql.evals3-fuse-db:5432/sampledb
	user: redhat
	password: redhat
	
![fuseonline connect jdbc](https://drive.google.com/uc?id=1D54_JsA-23vqYFaiBh8nAB2NjPArEUAR)
Results

![enter image description here](https://drive.google.com/uc?id=1k3Tg2N4IZt4eSG7pJT1RGR2clYsg3eOY)
## API DEFINITION

Enter to Fuse Online Create Integration

![enter image description here](https://drive.google.com/uc?id=1aD23ooKBPvdJQmhXqS1QtHNxNLqWXbVN)
Click in API provider.

![API Integration](https://drive.google.com/uc?id=14KN2jhRHRXgxfLy1IGhC8SgAZegv0wIp)
select Add a data type , and put a API name, is this case "users" and enter a json example.

### Enter JSON Example 


	{
	    "id": 0,
	    "name": "Rodrigo Ramalho",
	    "phone": "11 95474-8099",
	    "age": 30
	}

in the section "Choose to create a REST Resource with the Data Type" select "Rest Resource", After then click Save

![datatype here](https://drive.google.com/uc?id=183mT2EtQzLrGOoOEad-Ft0irkK11BEPx)
 the Import output looks like this
 
![enter image description here](https://drive.google.com/uc?id=1n4napX4gjRO7YmOD58vv99IBwfDkVcs0)
Click save and the you can see in the # Review Actions, the following 

##### WARNINGS
1. Found operations with non unique operationIds: getusers

2. Operation POST /users does not provide a response schema for code 201

3. Operation PUT /users/{usersId} does not provide a response schema for code 202

4. Operation DELETE /users/{usersId} does not provide a response schema for code 20

Click in Next button 

## INTEGRATIONS

Go to Integration panel

![integration endpoint](https://drive.google.com/uc?id=1-yJ4DVzONcQyNG1VymcjRkaDOPZVzHtp)
### Get User Operation
 Click  "create flow" in the Get operation 
 
![Integration without components](https://drive.google.com/uc?id=1qMydSqoiIL2kTK2ho9_-I9JZtp1dSVVg)

#### log

Add log component

![putlog](https://drive.google.com/uc?id=1m_XUStXRCcFj8llRREPPaBEJPk7W_qVq)
and select  "Message Context" and "Message Body"

#### Database

![databaseselect](https://drive.google.com/uc?id=1QZVFvnUVfVyEtutSIYKPzqBQ4xKRPZj5)



	select * from users;

#### Mapping

Do the mapping between Source and Tarjet

![Mapping](https://drive.google.com/uc?id=1IcjfHlr-892pz-NrNTyGmv4O6mrerNof)

#### Tree Integration

![tree Intregation](https://drive.google.com/uc?id=1pi_ojluCHV28wIaSXqFkm2G4eg7iR-tO)
Click in save, then we will to continous with other operation

### Post User Insert  Operation

![Insert Post Operation](https://drive.google.com/uc?id=12wv5hRY22-1_xadcYmF4j-rSGrQvq08C)
#### log
![Insert log to POST operation](https://drive.google.com/uc?id=1wlDJPKEgNJ14C3udkQvZc7sYgl0XQa7t)
#### Database
![post query insert](https://drive.google.com/uc?id=1-bjZqaWMrPWaxAnW7LcKPnwSgxAxzAMu)
Queries

	INSERT INTO USERS(NAME,PHONE,AGE) VALUES(:#NAME,:#PHONE,:#AGE);


#### Mapping

![Mapping post](https://drive.google.com/uc?id=1_OoFLBmdD5QSQ8kUdkZWGZpEaiqxvhkT)

#### Tree Integration
![Tree Integration post](https://drive.google.com/uc?id=1HKtuKkHsUXlJhfFM6jSXGocyhnflHhDy)

### Publish API on Openshift
Click in save and then press in Publish

![publish api](https://drive.google.com/uc?id=1anK-lo6lRr46dgeRZTKI3Zsm8VY-9v77)
Publish and build in OCP.

![publish and build](https://drive.google.com/uc?id=1frq6KkA3bzUZxkiHK5IEk3uCslLQLuyK)
# Service Mesh

## Install Stack Openshift Service Mesh
To install the stack of service mesh please refer to:
https://docs.openshift.com/container-platform/4.3/service_mesh/service_mesh_install/installing-ossm.html

## Install 

for this demostration we do a "cluster-admin" privilege for the user that we are working.

![clusterrolebinding](https://drive.google.com/uc?id=1xd80R14dgz44CABTPlrGESqiySauSivT)


1. Install Elastic Search Operator
2. Installing the Jaeger Operator
3. Installing the Kiali Operator
4. Installing the Red Hat OpenShift Service Mesh Operator

![OCP operator here](https://drive.google.com/uc?id=1Zt5Nv4f2vOBzk4yOcbONJ30uqlXaNvvw)
## Install Control Plane

- Create Namespace. 

    `oc new-project istio-system`
   
-   Create a `ServiceMeshControlPlane` file named `/rhosmapi/osm/service-mesh.yaml ` using the example found in "Customize the Red Hat OpenShift Service Mesh installation". You can customize the values as needed to match your use case. 
![servicemesh control plane](https://drive.google.com/uc?id=167yn99tnFULe76Y8DEoW0AFpOz4NGl2G)   
-   or Run the following command to deploy the control plane:
    
    `oc create -n istio-system -f service-mesh.yaml`
   
-   Execute the following command to see the status of the control plane installation.
   
    `oc get smcp -n istio-system`
       
    The installation has finished successfully when the READY column is true.
    
    `  NAME                 READY`
   `  Basic-install        True` 
    
-   Run the following command to watch the progress of the Pods during the installation process:
    
    `oc get pods -n istio-system -w`
    
    You should see output similar to the following:
    ![osm-control plane pods](https://drive.google.com/uc?id=1UeGTTzSl8cEky0DzdLU6sAIXqVMMAF63)
## Install Data plane [ServiceMeshMemberRoll]

Go to the Openshift Service mesh operator, and click in Istio Service Mesh Member Roll section, and create a default ServiceMeshMemberRoll.

![MemberRoll here](https://drive.google.com/uc?id=1ySxxkwbY4_JPsbOXvw7kW6HmVRAevS8Z)
Verify the NetworkPolicy in the namespaces of fuseonline.

![Networkpolicy](https://drive.google.com/uc?id=1_YJhO9EiV9mZ-hNrtR30dhAli4UHeajJ)
### Install NetworkPolicy in FuseOnlineNamespace

Now, It is necessary aggregate the following rules:
-  allow-from-all-namespaces.
-  allow-from-ingress-namespace.

![Networkpolicy](https://drive.google.com/uc?id=1-NMjQQzwX7QSX3gXtnfAiUBX5gmPpDbm)
this step its only for demostrations because syndesys webconsole lost for the users.

### Install Sidecar in the API service.
verify the namespace in where api user deploy:
![Namespace](https://drive.google.com/uc?id=1hE1Atgdv4qi6CWMbpse3XUQ7ZkdrjojR)
Select deployment config of "i-user" in evals3-fuse  namespace  and agregate sidecard annotation

    spec:
     template:
       metadata:
         annotations:
           sidecar.istio.io/inject: 'true'

![inject sidecar](https://drive.google.com/uc?id=1SDZVWJHKqMCIWbPnSWAt7n8zor6APE8D)
Repet the same in the Database Postgress
![inject in deployment f postgressdb](https://drive.google.com/uc?id=14TchTkaUz9oecOInCPHDSfv7QPis7kOr)
# Kiali Observe

## Allow read deployment config in Kiali

Select Istio-system (control plane) namespace and then go to Config maps, click in Kiali Config map and the select yaml.

Remove deployment config option:
 
	excluded_workloads:
	        DeploymentConfigs ##delete this line.

![Kialia excluser](https://drive.google.com/uc?id=1ssz2oIEoXemP5GxcovM8-kCqcCVBs4Rs)
## Create Virtual Service and Gateway

for this section its important to read first:

https://istio.io/latest/blog/2018/v1alpha3-routing/

![explain istio object](https://drive.google.com/uc?id=1YYhbgkHu09rJgX-6yq5zqLOPNtnsXREn)


then we create a Gateway and Virtual service. that its in /osm folder.

    oc apply -n evals3-fuse -f gateway-user.yaml


![result apply config gateway and virtual services](https://drive.google.com/uc?id=1bXn4kOzxXwpoDHupwTC0kZe4k99S4nq0)

Nota: in this demo does not create a destinationRule


## Observe

 - First get  the url of istio gateway
 
`export ISTIOGWUSER=$(oc -n istio-system get route istio-ingressgateway -o jsonpath='{.spec.host}')`

 - Curl for API in a loop
      `for i in {1..1000} ; do curl  -s -w "%{http_code}\n" http://$ISTIOGWUSER/users ;sleep 2 ; done `

![Curl result](https://drive.google.com/uc?id=1_Bp1Vkv2feDblRbVao3uEqy1Cy11pbsB)
 - Then go to Kiali

   ` oc get routes kiali -n istio-system`

 - open Kiali and graph option

![graph1](https://drive.google.com/uc?id=1qR_JVVhbZljyNY12ZF3E5Zzn_c8Xjkn0)

![graph2](https://drive.google.com/uc?id=1N1B8Oh0A3USLtRCn-iE37_RkvQqO7ofT)

# 3scale API Management

## Configure 3scale Account

## Configure API Backend 

## Configure Product



# 3scale Mixer Adapter 

By default, Red Hat Service Mesh disables evaluation of all policies.

In order for API Management policies to be applied to service mesh traffic, this default behavior needs to be reversed. The setting for this behavior is in the _istio_ configmap in the istio namespace. This configmap is read by the Envoy proxy upon start-up of an istio enabled pod.

Your lab environment already comes provisioned with service mesh policies (to include API Management policies that will be introduced in this lab) enabled.

You can view state of this setting that disables service mesh policies as follows:

```
$ oc describe cm istio -n istio-system | grep disablePolicyChecks

disablePolicyChecks: false
```
## Desplegar 3scale-istio-adapter

git clone [https://github.com/3scale/3scale-istio-adapter](https://github.com/3scale/3scale-istio-adapter)
  
   ```
oc create -f deploy -n istio-system
   ```

1.  Review 3scale Istio Adapter components in _istio-system_ namespace:
    
    ```
    $ oc get all -l app=3scale-istio-adapter -n istio-system
    ```
    1.  The response should list the deployment, replicaset and pod.
        
    2.  As per the diagram above, the _3scale-istio-adapter_ Linux container includes the following two components:
        
        1.  **3scale-istio-adapter**
            
            Accepts gRPC invocations from Istio ingress and routes to the other side car in the pod: _3scale-istio-httpclient_
            
        2.  **3scale-istio-httpclient**
            
            Accepts invocations from _3scale-istio-adapter_ and invokes the _system-provider_ and _backend-listener_ endpoints of the remote Red Hat 3scale API Management manager.
            
    3.  Its possible that the pod corresponding to the 3scale-istio-adapter is in an _ImagePullBackOff_ error state.
        
        If so, edit the _3scale-istio-adapter_ Deployment such that the URL to the image explicitly includes _quay.io_ as follows:
        
        ```
        image: quay.io/repository/3scale/3scale-istio-adapter:0.5.1
        ```
        
    
2.  View listing of configs that support the 3scale Mixer Adapter:
    
    Embedded in the following YAML files is the 3scale handler that is injected into the Istio Mixer. This handler is written in Golang by the 3scale engineering team as per the [Mixer Out of Process Adapter Dev Guide](https://github.com/istio/istio/wiki/Mixer-Out-Of-Process-Adapter-Dev-Guide). Much of these files consists of the adapter’s configuration [proto](https://developers.google.com/protocol-buffers/docs/proto3).
    
    1.  Adapters:
        
        ```
        $ oc get adapters.config.istio.io -n istio-system
        threescale   7d
        ```
        
    2.  Template:
        
        ```
        $ oc get templates.config.istio.io -n istio-system
        
        threescale-authorization   7d
        ```
 

## [2] Crear configuraciones 3scale Adapter

Now that 3scale Istio Adapter has been verified to exist, various configurations need to be added to the service mesh.

In particular, you will specify the URL of the _system-provider_ endpoint of your 3scale tenant along with the corresponding access token. This is needed so that the Istio Mixer can pull API proxy details from the 3scale API Manager (similar to what the 3scale API Gateway does).

1.  In the details of your _catalog_ service in the Red Hat 3scale API Manager administration console, locate the `ID for API calls … `:

-   Review the `threescale-adapter-config.yaml` file :
    
    ```
    $ less 3scale-istio-adapter/istio/threescale-adapter-config.yaml | more
    ```
    
-   Modify the `threescale-adapter-config.yaml` file with the ID of your catalog service:
    
    ```
    $ sed -i "s/service_id: .*/service_id: \"$CATALOG_SERVICE_ID\"/" \
          3scale-istio-adapter/istio/threescale-adapter-config.yaml
    ```
    
-   Modify the `threescale-adapter-config.yaml` file with the URL to your Red Hat 3scale API Management manager tenant:
    
    ```
    $ sed -i "s/system_url: .*/system_url: \"https:\/\/$TENANT_NAME-admin.$API_WILDCARD_DOMAIN\"/" \
          3scale-istio-adapter/istio/threescale-adapter-config.yaml
    ```
    
-   Modify the `threescale-adapter-config.yaml` file with the administrative access token of your Red Hat 3scale API Management manager administration account:
    
    ```
    $ sed -i "s/access_token: .*/access_token: \"$API_ADMIN_ACCESS_TOKEN\"/" \
          3scale-istio-adapter/istio/threescale-adapter-config.yaml
    ```
    
-   The _rule_ in _threescale-adapter-config.yaml_ defines the conditions that API Management policies should be applied to a request.
    
    The existing default rule is as follows:
    
    ```
    match: destination.labels["service-mesh.3scale.net"] == "true"
    ```
    
    This rule specifies that API Management policies should be applied to the request when the target Deployment includes a label of: `service-mesh.3scale.net`. In this version of the lab, this rule does not apply API Management policies as expected. Further research into the issue is needed.
    
    1.  As a work-around for the current problem, modify the `threescale-adapter-config.yaml` file with a modified rule that specifies that API Management policies should be applied when the target is the catalog-service:
        
        ```
        $ sed -i "s/match: .*/match: destination.service.name == \"catalog-service\"/" \
              3scale-istio-adapter/istio/threescale-adapter-config.yaml
        ```
        
    2.  More information about Istio’s Policy Attribute Vocabulary (used in the creation of rules) can be found [here](https://istio.io/docs/reference/config/policy-and-telemetry/attribute-vocabulary/).
        
    
-   Load the Red Hat 3scale API Management Istio Handler configurations:
    
    ```
    $ oc create -f 3scale-istio-adapter/istio/threescale-adapter-config.yaml
    
    ...
    
    handler.config.istio.io/threescale created
    instance.config.istio.io "threescale-authorization" created
    rule.config.istio.io "threescale" created
    ```
    
    1.  If for whatever reason you want to delete these 3scale Istio mixer adapter configurations, execute the following:
        
        ```
        oc delete rule.config.istio.io threescale -n istio-system
        oc delete instance.config.istio.io threescale-authorization -n istio-system
        oc delete handler.config.istio.io threescale -n istio-system
        ```
        
    
-   Verify that the Istio Handler configurations were created in the istio-system namespace:
    
    ```
    $ oc get handler threescale -n istio-system -o yaml
    
    apiVersion: v1
    items:
    - apiVersion: config.istio.io/v1alpha2
      kind: handler
    
      ....
    
      spec:
        adapter: threescale
        connection:
          address: threescaleistioadapter:3333
        params:
          access_token: fa16cd9ebd66jd07c7bd5511be4b78ecf6d58c30daa940ff711515ca7de1194a
          service_id: "103"
          system_url: evals3-tenant-admin.apps.cluster-chile-6b30.chile-6b30.example.opentlc.com
    ```

## Smoke Test 3scale Istio Mixer Adapter







## Repository reference
https://github.com/RedHatWorkshops/dayinthelife-integration
https://gist.github.com/hodrigohamalho
https://github.com/hodrigohamalho

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTY5MjU2MTQ1NCwtMTQyNTcwOTUyNywtMT
k1NDI1MTczMiwxMjEwMjUxODA3LDEwNjQxNDQ5ODEsMTgxOTUx
ODgyOSwtMTczOTY3NDk0MSw1NDQ1NjMxNjksNjQwNzQyMzA4LD
IxMjMyNDUzMTIsLTE0NzYxMjEyMzAsLTMzNDcyMjQ0LDExNDEz
NzA5NCw4MTE3NTQwODAsLTEyNDM0NDM2NTIsMTE3NzM0ODc3NC
wxMDM3OTk0ODY5LC0xNjA3ODA5NjkxLC0xOTc2OTA5MDUyLDE1
OTQ0NjMwOV19
-->