# Red Hat Integration + Openshift Service Mesh 

This demo describes how to show the life cycle of an API in online fuse and how it is synchronized in an ecosystem of Openshift Service Mesh and Securitized under 3Scale.


![Contexto Demo](https://drive.google.com/uc?id=1qH6bAffCI2dysmdxYwFmdl1LXVWHazwn)
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

Remove 
	excluded_workloads:
	        DeploymentConfigs

![Kialia excluser](https://drive.google.com/uc?id=1ssz2oIEoXemP5GxcovM8-kCqcCVBs4Rs)
## Observe



# 3scale API Management

## Configure API Backend 

## Configure Product




# 3scale Mixer Adapter 

## [1] Desplegar 3scale-istio-adapter

git clone [https://github.com/3scale/3scale-istio-adapter](https://github.com/3scale/3scale-istio-adapter)

oc create -f deploy -n istio-system

  Verificar que istio tenga policycheck 

$ kubectl -n istio-system get cm istio -o jsonpath="{@.data.mesh}" | grep disablePolicyChecks

  

## [2] Crear configuraciones Handler, Service e Instance

Desde 3Scale se debe recuperar tanto la URL de admin, el Service ID del API creado en 3Scale y el Token generado al momento de crear la autenticación mediante Istio.

- En Openshift (WebConsole o mediante comando rsh) ir al Pod 3scale-istio-adapter (ssh) y ejecutar:

./3scale-config-gen --url "https://3scale-admin.apps.3scale.com:443" --service "replace-me" --token "access_token_change_me" --name=“miapp-3scale-istio”

 - Copiar la configuracion arrojada por comando y pegarla en un archivo yaml (ej: istio-3scale-adapter.yaml)

- Crear archivo istio-3scale-adapter.yaml con las configuraciones handler, service e instance arrojadas con el comando anterior (copy/paste).

Luego:

oc create -f istio-3scale-adapter.yaml -n istio-system

## [3] Agregar labels a DeploymentConfig de la app

SERVICE_ID es el ID del API en 3Scale

- CREDENTIALS_NAME corresponde al nombre de las credenciales generadas con el comando en el paso 2 (--name=“miapp-3scale-istio”)

- DEPLOYMENT es el nombre del DeploymentConfig de la app

export CREDENTIALS_NAME="ste-3scale-istio”

export SERVICE_ID="replace-me”

export DEPLOYMENT=“nombre-del-deployment-config”

  
patch="$(oc get deployment "${DEPLOYMENT}" --template='{"spec":{"template":{"metadata":{"labels":{ {{ range $k,$v := .spec.template.metadata.labels }}"{{ $k }}":"{{ $v }}",{{ end }}"[service-mesh.3scale.net/service-id":"'"${SERVICE_ID}"'","service-mesh.3scale.net/credentials":"'"${CREDENTIALS_NAME}"'"}}}}}'](http://service-mesh.3scale.net/service-id%22:%22'%22$%7BSERVICE_ID%7D%22'%22,%22service-mesh.3scale.net/credentials%22:%22'%22$%7BCREDENTIALS_NAME%7D%22'%22%7D%7D%7D%7D%7D') )”

  

oc patch deployment "${DEPLOYMENT}" --patch ''"${patch}"’'  

  
-  Estos labels son los que se agregan el DeploymentConfig (y a cada pod cuando es creado). Necesarios para que istio sepa que credenciales y que Servicio utilizar para obtener las configuraciones desde 3Scale:

-  [service-mesh.3scale.net/credentials](http://service-mesh.3scale.net/credentials)

- [service-mesh.3scale.net/service-id](http://service-mesh.3scale.net/service-id)



## Repository reference
https://github.com/RedHatWorkshops/dayinthelife-integration
https://gist.github.com/hodrigohamalho
https://github.com/hodrigohamalho

<!--stackedit_data:
eyJoaXN0b3J5IjpbMTQ0ODQ2Mzg1OSwxMTQxMzcwOTQsODExNz
U0MDgwLC0xMjQzNDQzNjUyLDExNzczNDg3NzQsMTAzNzk5NDg2
OSwtMTYwNzgwOTY5MSwtMTk3NjkwOTA1MiwxNTk0NDYzMDksMT
U5MDk4Mzc0NCwxOTA1Mzg3NzUyLC0xMDU2MDI1NjQ0LC0xMzUw
Nzk5MzUsLTIwMDY2NTYyMjAsLTEwODg1MTA4NjAsLTExMjg0OT
gwMjgsLTc0NzQwNzk4NywtMjU5NDY0NjI3LC0xMzUzNDIyNDU3
LDc5Mjg2OTcwN119
-->