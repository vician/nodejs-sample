git clone git@github.com:vician/nodejs-sample.git

cd nodejs-samplea/tkn

oc create -f 01_apply_manifest_task.yaml
oc create -f 02_update_deployment_task.yaml
oc create -f 04_pipeline.yaml


tkn pipeline start build-and-deploy \
    -w name=shared-workspace,volumeClaimTemplateFile=https://raw.githubusercontent.com/openshift/pipelines-tutorial/pipelines-1.18/01_pipeline/03_persistent_volume_claim.yaml \
    -p deployment-name=my-nodejs \
    -p git-url=https://github.com/vician/nodejs-sample.git \
    -p IMAGE="image-registry.openshift-image-registry.svc:5000/$(oc project -q)/nodejs-sample" \
    --use-param-defaults
