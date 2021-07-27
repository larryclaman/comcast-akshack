# Challenge 12: Secrets

[< Previous Challenge](11-opsmonitoring.md) - **[Home](../README.md)** - [Next Challenge >](13-secrets-akv.md)


In this challenge, you will need to refactor your MongoDB deployment to require a password when accessing database.  You will first inject the password into your deployment using Kubernetes Secrets. 
## Introduction: Redeploy MongoDB to require authenication

During an architecture review, your CISO noticed that MongoDB is deployed without a password.  She has asked your team to mitigate this risk in the next sprint. For Part 1 of this challenge, you will need to:
- Redeploy MongoDB in a manner that requires a password to access the database
- Store the credentials (username/password) in a Kubernetes Secret
- Refactor your deployment manifests to use the Secret
  - Inject the secrets into your deployments as environment variables

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

Now it's up to you to refactor your deployment manifests to use secrets!
* Note: You will likely need to delete your MongoDB deployment in order to iterate on your new mongodb manifests (don't worry, the data is safe in the PVC!):  `kubectl delete deployment <mongodb deployment name>`
## Success Criteria
- MongoDb is deployed such that authentication is required.
- The MongoDB password is stored as a Kubernetes secret
- The Fabmedical website is fully functional

### Hints / Readings:
- [Using Secrets as environment variables](https://kubernetes.io/docs/concepts/configuration/secret/#using-secrets-as-environment-variables)
- [How to Deploy MongoDB on Kubernetes](https://www.cloudytuts.com/guides/kubernetes/how-to-deploy-mongodb-on-kubernetes/) 
- A full MongoDb connection string has the format `mongodb://<USERNAME>:<PASSWORD>@<hostname>:27017/contentdb?authSource=admin`
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


