# Challenge 12: Secrets

[< Previous Challenge](./10-networking.md) - **[Home](../README.md)** - [Next Challenge >](12-secrets-p2.md)

## Introduction

This challenge will cover AKS secrets, and secret injection in AKS from Azure Key Vault.

In this challenge, you will need to refactor your MongoDB deployment to require a password when accessing database.  You will first inject the password into your deployment using Kubernetes Secrets.  You will then improve the security of your application by integrating Azure Key Vault with AKS to securely store your secrets.


## Part 1:  Redeploy MongoDB with a password

During an architecture review, your CISO noticed that MongoDB is deployed without a password.  He has asked your team to mitigate this risk in the next sprint. For Part 1 of this challenge, you will need to:
- Redeploy MongoDB in a manner that requires a password to access the database
- Store the credentials (username/password) in a Kubernetes Secret

- Refactor your deployment manifests to use the Secret
  - Inject the secrets into your deployments as environment variables
  - _Don't forget about content-init-job.yml!_

### Pre-req -- add admin user & password to existing database
Prior to redeploying your database, **and before you do anything else**, you need to add authentication to your existing database. Use the following steps:
1. From a shell, connect into your MongoDB pod:  `kubectl exec -it $(kubectl get pods -o  custom-columns=NAME:.metadata.name|grep mongo) -- /bin/bash`
2. Run the following commands to set a username and password (replacing the password with your own secret!):
   - Run `mongo`
   - Run `use admin`
   - Run the following to set a username & password on the database:
	```
	db.createUser(
  	{
    	user: "useradmin",
    	pwd: "my-super-secret-password",
    	roles: [ { role: "userAdminAnyDatabase", db: "admin" } ]
  	}
	)
	db.grantRolesToUser("useradmin", [ { role: "readWrite", db: "contentdb" } ] )
	```
3. Exit out of mongo, then exit out of the pod
4. You will need to delete your MongoDB deployment (don't worry, the data is safe in the PVC!):  `kubectl delete deployment <mongodb deployment name>`


Now it's up to you to refactor your deployment manifests to use secrets!


### Hints / Readings:
- [Using Secrets as environment variables](https://kubernetes.io/docs/concepts/configuration/secret/#using-secrets-as-environment-variables)
- [How to Deploy MongoDB on Kubernetes](https://www.cloudytuts.com/guides/kubernetes/how-to-deploy-mongodb-on-kubernetes/) 
- A full MongoDb connection string has the format `mongodb://<USERNAME>:<PASSWORD>@<hostname>:27017/contentdb`
  - [Concatenating strings & environment variables](https://joeblogs.technology/2020/12/concatenating-kubernetes-secrets-for-environment-variables/)
- You can confirm MongoDb is running correctly with the following commands:
	```
	kubectl exec -it <mongo pod name> -- /bin/bash
	mongo --version
	mongo -u $MONGO_INITDB_ROOT_USERNAME -p $MONGO_INITDB_ROOT_PASSWORD
	show dbs
	use contentdb
	show collections
	db.sessions.find()
	```

## Success Criteria

- MongoDb is deployed such that authentication is required.
- The MongoDB password is stored as a Kubernetes secret
- The Fabmedical website is fully functional

