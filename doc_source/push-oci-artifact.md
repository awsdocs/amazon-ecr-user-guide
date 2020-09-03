# Pushing a Helm chart<a name="push-oci-artifact"></a>

Amazon ECR supports pushing Open Container Initiative \(OCI\) artifacts to your repositories\. To display this functionality, use the following steps to push a Helm chart to Amazon ECR\.

For more information about using your Amazon ECR hosted Helm charts with Amazon EKS, see [Installing a Helm chart hosted on Amazon ECR with Amazon EKS](ECR_on_EKS.md#using-helm-charts-eks)\.

**To push a Helm chart to an Amazon ECR repository**

1. Install the Helm client version 3\. For more information, see [Installing Helm](https://helm.sh/docs/intro/install/)\.

1. Enable OCI support in the Helm 3 client\.

   ```
   export HELM_EXPERIMENTAL_OCI=1
   ```

1. Create a repository to store your Helm chart\. For more information, see [Creating a repository](repository-create.md)\.

   ```
   aws ecr create-repository \
        --repository-name artifact-test \
        --region us-west-2
   ```

1. Authenticate your Helm client to the Amazon ECR registry to which you intend to push your Helm chart\. Authentication tokens must be obtained for each registry used, and the tokens are valid for 12 hours\. For more information, see [Registry authentication](Registries.md#registry_auth)\.

   ```
   aws ecr get-login-password \
        --region us-west-2 | helm registry login \
        --username AWS \
        --password-stdin aws_account_id.dkr.ecr.region.amazonaws.com
   ```

1. Use the following steps to create a test Helm chart\. For more information, see [Helm Docs \- Getting Started](https://helm.sh/docs/chart_template_guide/getting_started/)\.

   1. Create a directory named `helm-tutorial` to work in\.

      ```
      mkdir helm-tutorial
      cd helm-tutorial
      ```

   1. Create a Helm chart named `mychart` and clear the contents of the `templates` directory\.

      ```
      helm create mychart
      rm -rf ./mychart/templates/*
      ```

   1. Create a ConfigMap in the `templates` folder\.

      ```
      cd mychart/templates
      cat <<EOF > configmap.yaml
      apiVersion: v1
      kind: ConfigMap
      metadata:
        name: mychart-configmap
      data:
        myvalue: "Hello World"
      EOF
      ```

1. Save the chart locally and create an alias for the chart with your registry URI\.

   ```
   cd ..
   helm chart save . mychart
   helm chart save . aws_account_id.dkr.ecr.us-west-2.amazonaws.com/artifact-test:mychart
   ```

1. Identify the Helm chart to push\. Run the helm chart list command to list the Helm charts on your system\.

   ```
   helm chart list
   ```

   The output should look similar to this:

   ```
   REF                                                         	NAME   	VERSION	DIGEST 	SIZE   	CREATED       
   aws_account_id.dkr.ecr.us-west-2.amazonaws.com/artifact-tes..   mychart	0.1.0  	30e0a03	3.6 KiB	14 seconds    
   mychart                                                  	mychart	0.1.0  	ba3e62a	3.6 KiB	About a minute
   ```

1. Push the Helm chart using the helm chart push command:

   ```
   helm chart push aws_account_id.dkr.ecr.region.amazonaws.com/artifact-test:mychart
   ```

1. Describe your Helm chart\.

   ```
   aws ecr describe-images \
        --repository-name artifact-test \
        --region us-west-2
   ```

   In the output, verify the `artifactMediaType` parameter indicates the proper artifact type\.

   ```
   {
       "imageDetails": [
           {
               "registryId": "aws_account_id",
               "repositoryName": "artifact-test",
               "imageDigest": "sha256:f23ab9dc0fda33175e465bd694a5f4cade93eaf62715fa9390d9fEXAMPLE",
               "imageTags": [
                   "mychart"
               ],
               "imageSizeInBytes": 3714,
               "imagePushedAt": 1597433021.0,
               "imageManifestMediaType": "application/vnd.oci.image.manifest.v1+json",
               "artifactMediaType": "application/vnd.cncf.helm.config.v1+json"
           }
       ]
   }
   ```