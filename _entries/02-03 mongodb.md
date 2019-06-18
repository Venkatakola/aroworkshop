---
sectionid: mongodb
sectionclass: h2
title: Deploy mongoDB
parent-id: labs
---

### Create mongoDB from template

Azure Red Hat OpenShift provides a container image and template to make creating a new MongoDB database service easy. The template provides parameter fields to define all the mandatory environment variables (user, password, database name, etc) with predefined defaults including auto-generation of password values. It will also define both a deployment configuration and a service.

There are two templates available:

* `mongodb-ephemeral` is for development/testing purposes only because it uses ephemeral storage for the database content. This means that if the database pod is restarted for any reason, such as the pod being moved to another node or the deployment configuration being updated and triggering a redeploy, all data will be lost.

* `mongodb-persistent` uses a persistent volume store for the database data which means the data will survive a pod restart. Using persistent volumes requires a persistent volume pool be defined in the Azure Red Hat OpenShift deployment.

> **Hint** You can retrieve a list of templates using the command below. The templates are preinstalled in the `openshift` namespace.
> ```sh
> ./oc get templates -n openshift
> ```

Create a mongoDB deployment using the `mongodb-persistent`. You're passing in the values to be replaced (username, passwor and database) which generates a YAML/JSON file. You then pipe it to the `oc create` command.

```sh
./oc process openshift//mongodb-persistent \
    -p MONGODB_USER=ratingsuser \
    -p MONGODB_PASSWORD=ratingspassword \
    -p MONGODB_DATABASE=ratingsdb \
    -p MONGODB_ADMIN_PASSWORD=ratingspassword | ./oc create -f -
```

If you now head back to the web console, you should see a new deployment for mongoDB.

![MongoDB deployment](media/mongodb-overview.png)

> **Resources**
> * <https://docs.openshift.com/aro/using_images/db_images/mongodb.html>
> * <https://docs.openshift.com/aro/dev_guide/templates.html>