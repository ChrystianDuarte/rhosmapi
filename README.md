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









Queries

	select * from users;
	INSERT INTO USERS(NAME,PHONE,AGE) VALUES(:#NAME,:#PHONE,:#AGE);

## Repository reference
https://github.com/RedHatWorkshops/dayinthelife-integration
https://gist.github.com/hodrigohamalho
https://github.com/hodrigohamalho

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE0NjgzNTQzODgsLTE4ODU3MDMwNzcsLT
ExNzMxMjAzNDQsLTE3MzYxOTQwMTYsMTg0NDY0NzIzNiwtMjcz
MDgwNTYyLC03ODA5OTM4MDcsLTE2MzM2ODEwNDksLTEzODU1NT
cwOTQsMTkxMjUyMTA5MywxNDA3MjM4MDg1LDExMTUxNTE2Mjcs
LTEzMzM5NDE2MTMsLTI0NTMzNjM2NSwxNTczMjMzODU0LC0yMT
E2Njc5NjUyLDgyNDEwMjk1MCwtNTc2MDI4ODU0XX0=
-->