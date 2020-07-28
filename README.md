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

1. Install Elastic Search Operator
2. Installing the Jaeger Operator
3. Installing the Kiali Operator
4. Installing the Red Hat OpenShift Service Mesh Operator

![OCP operator here](https://drive.google.com/uc?id=1Zt5Nv4f2vOBzk4yOcbONJ30uqlXaNvvw)
## Install Control Plane

[Install Control Plane](https://docs.openshift.com/container-platform/4.3/service_mesh/service_mesh_install/installing-ossm.html#ossm-control-plane-deploy-cli_installing-ossm)

    oc new-project istio-system
  
-   Create a `ServiceMeshControlPlane` file named `/rhosmapi/osm/service-mesh.yaml ` using the example found in "Customize the Red Hat OpenShift Service Mesh installation". You can customize the values as needed to match your use case. 
    
-   Run the following command to deploy the control plane:
    
    oc create -n istio-system -f service-mesh.yaml

    
-   Execute the following command to see the status of the control plane installation.
    
    $ oc get smcp -n istio-system
    
    The installation has finished successfully when the READY column is true.
    
    NAME           READY
    basic-install   True
    
-   Run the following command to watch the progress of the Pods during the installation process:
    
    $ oc get pods -n istio-system -w
    
    You should see output similar to the following:
    
    NAME                                     READY   STATUS             RESTARTS   AGE
    grafana-7bf5764d9d-2b2f6                 2/2     Running            0          28h
    istio-citadel-576b9c5bbd-z84z4           1/1     Running            0          28h
    istio-egressgateway-5476bc4656-r4zdv     1/1     Running            0          28h
    istio-galley-7d57b47bb7-lqdxv            1/1     Running            0          28h
    istio-ingressgateway-dbb8f7f46-ct6n5     1/1     Running            0          28h
    istio-pilot-546bf69578-ccg5x             2/2     Running            0          28h
    istio-policy-77fd498655-7pvjw            2/2     Running            0          28h
    istio-sidecar-injector-df45bd899-ctxdt   1/1     Running            0          28h
    istio-telemetry-66f697d6d5-cj28l         2/2     Running            0          28h
    jaeger-896945cbc-7lqrr                   2/2     Running            0          11h
    kiali-78d9c5b87c-snjzh                   1/1     Running            0          22h
    prometheus-6dff867c97-gr2n5              2/2     Running            0          28h   

# 3scale API Management

# 3scale Mixer Adapter 

## Repository reference
https://github.com/RedHatWorkshops/dayinthelife-integration
https://gist.github.com/hodrigohamalho
https://github.com/hodrigohamalho

<!--stackedit_data:
eyJoaXN0b3J5IjpbMTM5NTgyMjI2OCwtNzQ3NDA3OTg3LC0yNT
k0NjQ2MjcsLTEzNTM0MjI0NTcsNzkyODY5NzA3LDIxNDE5NjM5
NDYsNDA5MTg0ODU0LC0xNjM0NTA4NTk4LC0xOTk5OTY5NzAxLD
kyMzA2NzAyNiwtMTQ2NTk1NzQ1LC0xMjU5Mzk0ODI1LC0xMTMx
NDU3Mjk4LC0xNTExNTA2OTMyLC04OTI3MzYxNjIsOTE4MjgxND
EzLC00Mzk4Mjc2MDIsMTcyNzg5Mzg0NywtMTg4NTcwMzA3Nywt
MTE3MzEyMDM0NF19
-->