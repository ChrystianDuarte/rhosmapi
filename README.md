# Red Hat Integration + Openshift Service Mesh 

This demo describes how to show the life cycle of an API in online fuse and how it is synchronized in an ecosystem of Openshift Service Mesh and Securitized under 3Scale.


![Contexto Demo](https://drive.google.com/uc?id=1qH6bAffCI2dysmdxYwFmdl1LXVWHazwn)
# Install

THE CURRENT SUPPORTED VERSION of RHMI WORKSHOP is 2.3.0!! PLEASE INSTALL THE CORRECT VERSION!

![RHPDS](https://drive.google.com/uc?id=1B6Vq0URuGPAazpgsqR9qef_CM_AgnCCp)
## Environment

When the lab enviroment its ready, please go to the Solution Explorer and then click in 




# Fuse Online

## Database Users

Prepare the environment in Openshift

	oc new-project fuse-demo
	oc new-app --template=postgresql-persistent --param=POSTGRESQL_PASSWORD=redhat --param=POSTGRESQL_USER=redhat --param=POSTGRESQL_DATABASE=sampledb

Create and populate database

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

Configure a database connector in Fuse Online

	url: jdbc:postgresql://postgresql.fuse-demo:5432/sampledb
	user: redhat
	password: redhat
	
## API DEFINITION

	{
	    "id": 0,
	    "name": "Rodrigo Ramalho",
	    "phone": "11 95474-8099",
	    "age": 30
	}


## INTEGRATIONS


Queries

	select * from users;
	INSERT INTO USERS(NAME,PHONE,AGE) VALUES(:#NAME,:#PHONE,:#AGE);
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTM1NDIzOTEyOCwtMjQ1MzM2MzY1LDE1Nz
MyMzM4NTQsLTIxMTY2Nzk2NTIsODI0MTAyOTUwLC01NzYwMjg4
NTRdfQ==
-->