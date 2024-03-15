# Creating a pod with a UBI postgresql container

## Finding the image

```
# search for an available container image on UBI
podman search registry.redhat.io/postgresql

# login into red hat registry - I used the [Registry Service Account](https://access.redhat.com/terms-based-registry/)
podman login registry.redhat.io

# pull the postgresl UBI image
podman pull registry.redhat.io/rhel9/postgresql-15
```

## Standalone container

```
# run the container locally
podman run -it -d --name postgresql_database \
-e POSTGRESQL_USER=user \
-e POSTGRESQL_PASSWORD=pass \
-e POSTGRESQL_DATABASE=my_db \
-p 5432:5432 \
registry.redhat.io/rhel9/postgresql-15

# access the postgresql_database container
podman run -it postgresql_database bash

# stop the container
podman ps
podman stop <container_id>

# remove the container
podman rm <container_id>
```

## In a pod

```
# create a pod to run the container image in and expose the pod port5432 and map to port 80
podman pod create --name=pgsql -p 5432:80

# run the container locally
podman run --pod=pgsql --name postgresql_database \
-e POSTGRESQL_USER=user \
-e POSTGRESQL_PASSWORD=pass \
-e POSTGRESQL_DATABASE=my_db \
registry.redhat.io/rhel9/postgresql-15

# in a new terminal check pod stats
podman pod stats

# access the postgresql_database container
podman run -it postgresql_database bash

# stop the pod
podman pod ps
podman pod stop <pod_id>

# remove the pod
podman pod rm <pod_id>
```

Resources:
1. https://catalog.redhat.com/software/containers/rhel9/postgresql-15/63f763f779eb1214c4d6fcf6?architecture=amd64&image=65e0af6ed6fed9d9cb59fffd