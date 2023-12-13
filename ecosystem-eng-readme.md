## EcoSystem Engineering team specific instructions.

This project is having helm chart to  set up the janus IDP backstage for Ecosystem engineering team.

The default configuration from the helm chart is overridden [appeng-helm-local-override-values.yaml](appeng-helm-local-override-values.yaml) to customize backstage as per our team needs and included the necessary plugins.
You need to pass this helm file while installing the backstage instance for our team.

Our backstage lab instance is configured with github, kubernetes, techdocs plugins. All these plugins required configurations and some are sensitive secrets. Due to that reason we are not providing actual secret config here.
Before installing backstage configure config maps - [appeng-backstage-config-map.yaml](appeng-backstage-config-map.yaml) and [backstage-s3-bucket-config-map.yaml](backstage-s3-bucket-config-map.yaml) with relevant values and install on the cluster.

```shell
# Create the config maps which are used by the backstage instance.
# Skip this step if you are installing backstage second time as these configs are already available.
oc apply -f appeng-backstage-config-map.yaml backstage-s3-bucket-config-map.yaml

# > Installing the backstage using helm chart with local changes.
# Make sure you have access to our openshift lab cluster.
oc project backstage
cd charts
helm upgrade -i appeng-backstage backstage -f ./../appeng-helm-local-override-values.yaml

# To delete the existing backstage cluster
 helm delete appeng-backstage
```

Some other examples.

```shell
# Consider our team's config but override the backstage image tag only.
helm upgrade -i appeng-backstage backstage -f ./../appeng-helm-local-override-values.yaml --set upstream.backstage.image.tag=next-7e85b09

# Override the backstage image tag only.
helm upgrade -i appeng-backstage backstage --set upstream.backstage.image.tag=next-7e85b09
```
