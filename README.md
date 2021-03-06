# Snyk controller for Amazon EKS

**DEVELOPER PREVIEW** - This is a preview release and not intended for production use.

---

> This Quick Start was created by [Snyk](https://snyk.io) in collaboration with 
> Amazon Web Services (AWS). [AWS Quick Starts](https://aws.amazon.com) are automated 
> reference deployments that use AWS CloudFormation templates to deploy key technologies on AWS, following AWS best 
> practices. 

## Overview
Snyk controller for Amazon EKS enables you to import and test your running EKS workloads and identify vulnerabilities in 
their associated images and configurations that might make those workloads less secure. Once imported, Snyk continues 
to monitor those workloads, identifying additional security issues as new images are deployed and the workload 
configuration changes.

This Quick Start reference deployment guide provides step-by-step instructions for deploying Snyk on Amazon EKS. 

> Please know that we may share who uses AWS Quick Starts with the AWS Partner Network (APN) Partner that collaborated 
> with AWS on the content of the Quick Start.

## Target Audience 

Developers, DevOps, Security teams, or any role within an organization building, deploying and maintaining applications 
on Amazon EKS.

## Architecture

Deploying this Quick Start with default parameters into an existing Amazon EKS cluster builds the following environment. 
For a diagram of the new VPC and new EKS cluster deployment options, see the 
[Amazon EKS Quick Start documentation](https://docs.aws.amazon.com/quickstart/latest/amazon-eks-architecture/architecture.html).
 
![Snyk architecture diagram](docs/images/architecture.png)
*Figure 1: Quick Start architecture for Snyk on Amazon EKS*

As shown in Figure 1, the Quick Start sets up the following:
* Kubernetes namespace for Snyk
* Kubernetes secret containing Skyk integration ID and docker config json.
* Snyk monitor pod

## Cost and licenses

You are responsible for the cost of the AWS services used while running this Quick Start reference deployment. 
There is no additional cost for using the Quick Start.

The AWS CloudFormation template for this Quick Start includes configuration parameters that you can customize. 
Some of these settings may affect the cost of deployment. For cost estimates, see the pricing pages for each AWS 
service you will use. Prices are subject to change.

> Tip: We recommend that you enable the AWS Cost and Usage Report. This report delivers billing metrics to an Amazon 
> Simple Storage Service (Amazon S3) bucket in your account. It provides cost estimates based on usage throughout each 
> month and finalizes the data at the end of the month. For more information about the report, see the AWS 
> documentation.

This Quick Start requires a license for Snyk, the license costs and the instructions to obtain a license are available 
[here](https://aws.amazon.com/marketplace/pp/B085VGM85Q?qid=1590170928622&sr=0-1&ref_=srh_res_product_title).

## Planning the deployment

### Specialized knowledge

This Quick Start assumes familiarity with Amazon EKS, AWS CloudFormation and Kubernetes.

### AWS account

If you don’t already have an AWS account, create one at https://aws.amazon.com by following the on-screen instructions. 

### EKS cluster

If you are deploying onto an existing EKS cluster that was **not** created by the Amazon EKS Quick Start, you will need 
to configure the cluster to allow this Quick Start to manage your EKS cluster. The requirements are detailed in Step 2
of the *Deployment steps* section of this document.

	
## Deployment options

This Quick Start provides three deployment options:
* Deploy Snyk into a new VPC (end-to-end deployment). This option builds a new AWS environment consisting of 
the VPC, subnets, NAT gateways, security groups, bastion hosts, EKS cluster, a node group, and other infrastructure 
components. It then deploys Snyk into this new EKS cluster.
* Deploy Snyk into a new EKS cluster in an existing VPC. This option builds a new AWS EKS cluster, a node 
group, and other infrastructure components into an existing VPC. It then deploys Snyk into this new EKS cluster.
* Deploy Snyk into an existing EKS cluster. This option provisions Snyk in your existing AWS infrastructure.


## Deployment steps

### Step 1. Sign in to your AWS account

1. Sign in to your AWS account at https://aws.amazon.com with an IAM user role that has the necessary permissions. 
For details, see Planning the deployment earlier in this guide. 
2. Make sure that your AWS account is configured correctly, as discussed in the *Technical requirements* section.

### Step 2. Prepare existing EKS cluster

> Note: This step is only required if you are launching this Quick Start into an existing EKS cluster that was **not** 
> created using the Amazon EKS Quick Start. If you would like to create a new EKS cluster with your deployment, skip to 
> step 3.

1. Sign in to your AWS account, and launch the 
[cluster preparation template](https://us-east-2.console.aws.amazon.com/cloudformation/home?region=us-east-2#/stacks/create/template?stackName=Amazon-EKS&templateURL=https://s3.amazonaws.com/aws-quickstart/quickstart-amazon-eks/templates/amazon-eks-master-existing-cluster.template.yaml).
2.	The template is launched in the Ohio (us-east-2) AWS Region by default. To change the Region choose another Region 
from list in the upper-right corner of the navigation bar.
3. On the Create stack page, keep the default setting for the template URL, and then choose Next.
4. On the Specify stack details page, change the stack name if needed. Enter the name of the EKS cluster you would like 
to deploy to, as well as the Subnet ID's and security group ID associated with the cluster. These can be obtained from 
the EKS Cluster console.
5. On the options page, you can specify tags (key-value pairs) for resources in your stack and set advanced options. When you’re done, choose Next.
6. On the Review page, review and confirm the template settings. Under Capabilities, select the two check boxes to acknowledge that the template creates IAM resources and might require the ability to automatically expand macros.
7. Choose Create stack to deploy the stack.
8. Monitor the status of the stack until the status reaches CREATE_COMPLETE.
9. From the *Outputs* section of the stack, note the KubernetesRoleArn and the HelmRoleArn.
10. Add the roles to the aws-auth config map in your cluster, specifying *system:masters* for the groups, by following 
the steps in the [EKS documentation](). This allows the Quick Start to manage your cluster via AWS CloudFormation.

### Step 3. Retrieve Snyk Kubernetes integration ID

1. Now, log in to your Snyk account and navigate to Integrations.
2. Search for and click Kubernetes.
3. Click Connect from the page that loads, copy the Integration ID. The Snyk Integration ID is a UUID, similar to this 
format: `abcd1234-abcd-1234-abcd-1234abcd1234`. Save it for use in the next step.

### Step 3. Launch the Quick Start

> Note: You are responsible for the cost of the AWS services used while running this Quick Start reference deployment. 
> There is no additional cost for using this Quick Start. For full details, see the pricing pages for each AWS service 
> used by this Quick Start. Prices are subject to change.

1. Sign in to your AWS account, and choose one of the following options to launch the AWS CloudFormation template. 
For help with choosing an option, see deployment options earlier in this guide.

| [![New VPC](docs/images/deploy1.png)](https://us-east-2.console.aws.amazon.com/cloudformation/home?region=us-east-2#/stacks/create/template?stackName=Amazon-EKS-with-Snyk&templateURL=https://s3.amazonaws.com/aws-quickstart/quickstart-amazon-eks/templates/amazon-eks-master.template.yaml) | [![Existing VPC](docs/images/deploy2.png) ](https://us-east-2.console.aws.amazon.com/cloudformation/home?region=us-east-2#/stacks/create/template?stackName=Amazon-EKS-with-Snyk&templateURL=https://s3.amazonaws.com/aws-quickstart/quickstart-amazon-eks/templates/amazon-eks-master-existing-vpc.template.yaml)    | [![Existingcluster](docs/images/deploy3.png)](https://us-east-2.console.aws.amazon.com/cloudformation/home?region=us-east-2#/stacks/create/template?stackName=Snyk-EKS&templateURL=https://s3.amazonaws.com/aws-quickstart/quickstart-eks-snyk/templates/eks-snyk.template.yaml) |
| :---: | :---: | :---: |
| [Deploy into a new VPC and new EKS cluster](https://us-east-2.console.aws.amazon.com/cloudformation/home?region=us-east-2#/stacks/create/template?stackName=Amazon-EKS-with-Snyk&templateURL=https://s3.amazonaws.com/aws-quickstart/quickstart-amazon-eks/templates/amazon-eks-master.template.yaml) | [Deploy into a new EKS cluster in an existing VPC](https://us-east-2.console.aws.amazon.com/cloudformation/home?region=us-east-2#/stacks/create/template?stackName=Amazon-EKS-with-Snyk&templateURL=https://s3.amazonaws.com/aws-quickstart/quickstart-amazon-eks/templates/amazon-eks-master-existing-vpc.template.yaml) | [Deploy into an existing EKS cluster](https://us-east-2.console.aws.amazon.com/cloudformation/home?region=us-east-2#/stacks/create/template?stackName=Snyk-EKS&templateURL=https://s3.amazonaws.com/aws-quickstart/quickstart-eks-snyk/templates/eks-snyk.template.yaml) |

Each new cluster deployments takes about 2 hours to complete. Existing cluster deployments take around 10 minutes.

2.	The template is launched in the Ohio (us-east-2) AWS Region by default. To change the Region choose another Region 
from list in the upper-right corner of the navigation bar.
3. On the Create stack page, keep the default setting for the template URL, and then choose Next.
4. On the Specify stack details page, change the stack name if needed. Review the parameters for the template, a 
reference is provided in the *Parameters* section of this document. Enter the ID obtained in *Step 3* 
into the parameter labelled `Snyk integration ID`. Provide values for the parameters that require 
input. For all other parameters, review the default settings and customize them as necessary. When you finish reviewing 
and customizing the parameters, choose Next.
5. On the options page, you can specify tags (key-value pairs) for resources in your stack and set advanced options. 
When you’re done, choose Next.
6. On the Review page, review and confirm the template settings. Under Capabilities, select the two check boxes to 
acknowledge that the template creates IAM resources and might require the ability to automatically expand macros.
7. Choose Create stack to deploy the stack.
8. Monitor the status of the stack. When the status is CREATE_COMPLETE, the Snyk cluster is ready.

### Step 4. Test the deployment

1. Configure kubectll command line utility to connect to your EKS cluster, as per the 
[EKS documentation](https://docs.aws.amazon.com/eks/latest/userguide/managing-auth.html)
2. Run `kubectl get namespaces` and verify that a namespace `snyk-monitor` displays with STATUS `Active`.
3. Run `kubectl get pods --namespace snyk-monitor` and verify that a pod with the prefix `snyk-monitor` displays with 
STATUS `Running`.
4. From the Snyk console, verify that your cluster appears and you are able to add workloads as described in the 
[Snyk documentation](https://support.snyk.io/hc/en-us/articles/360003947117#UUID-a0526554-0943-3363-6977-7a11f766ede2)

### Parameter reference

In the following table, parameters are described for the existing EKS cluster option. For parameters in the other 
deployment options see the EKS Quick Start documentation: 
* [Parameters for deploying new VPC and new EKS cluster](https://docs.aws.amazon.com/quickstart/latest/amazon-eks-architecture/step-2.html#option-1-new-vpc)
* [Parameters for deploying EKS cluster into an existing VPC](https://docs.aws.amazon.com/quickstart/latest/amazon-eks-architecture/step-2.html#option-2-existing-vpc)

| Parameter label (name) | Default | Description |
| --- | --- | --- |
| EKS cluster name(KubeClusterName) | *Requires input* | Name of the EKS cluster to deploy snyk into. |
| Snyk integration ID(SnykIntegrationId) | *Requires input* | Snyk kubernetes integration ID. Must be obtained from the Snyk console. For more information see *Step 3. Retrieve Snyk Kubernetes integration ID* in this guide. |
| Namespace(Namespace) | snyk-monitor | Kubernetes namespace to deploy the Snyk controller into. |

### Best practices for using Snyk on AWS
The Snyk Kubernetes integration will monitor workloads and provide details on potential vulnerabilities in container 
images as well as security configuration for your deployments. It is recommended that Kubernetes manifests adhere to 
the workload configuration properties defined in the 
[Snyk documentation](https://support.snyk.io/hc/en-us/articles/360003916178-Viewing-project-details-and-test-results)

## Security
Snyk will alert for common misconfigurations include not setting CPU/Memory limits for your application as well as 
running containers as the root user. Another common misconfiguration is leaving the default file system mounted for the 
container as writable. This may lead to a potential exposure for an attacker who compromises the container and writes 
to the disk, which makes certain kinds of attacks easier. If your containers are stateless then you don’t need a writable 
filesystem. Lastly, not defining capabilities. At a low-level, Linux capabilities control what different processes in 
the container are allowed to do: from being able to write to the disk, to being able to communicate over the network.

## FAQ
**Q**. I encountered a CREATE_FAILED error when I launched the Quick Start. 

**A**. If AWS CloudFormation fails to create the stack, we recommend that you relaunch the template with Rollback on 
failure set to No. (This setting is under Advanced in the AWS CloudFormation console, Options page.) With this setting, 
the stack’s state is retained and the instance is left running, so you can troubleshoot the issue. 

> Important: When you set Rollback on failure to No, you continue to incur AWS charges for this stack. Please make sure 
> to delete the stack when you finish troubleshooting.

For general EKS troubleshooting steps see the 
[EKS Quick Start documentation](https://docs.aws.amazon.com/quickstart/latest/amazon-eks-architecture/). 

For Snyk specific troubleshooting see 
[Snyk troubleshooting documentation](https://support.snyk.io/hc/en-us/articles/360003916138-Kubernetes-integration-overview).

For additional information, see Troubleshooting AWS CloudFormation on the AWS website. 

## Send us feedback

To post feedback, submit feature ideas, or report bugs, use the 
[Issues](https://github.com/aws-quickstart/quickstart-eks-snyk/issues) section of the GitHub repository for this Quick 
Start. If you’d like to submit code, please review the Quick Start Contributor’s Guide.

## Additional resources

### AWS resources

* [Getting Started Resource Center](https://aws.amazon.com/getting-started/)
* [AWS General Reference](https://docs.aws.amazon.com/general/latest/gr/)
* [AWS Glossary](https://docs.aws.amazon.com/general/latest/gr/glos-chap.html)

### AWS services

* [AWS CloudFormation](https://docs.aws.amazon.com/cloudformation/)
* [Amazon EKS](https://aws.amazon.com/eks/)
* [IAM](https://docs.aws.amazon.com/iam/)
* [Amazon VPC](https://docs.aws.amazon.com/vpc/)

### Snyk documentation

* [Snyk Kubernetes integration](https://support.snyk.io/hc/en-us/articles/360003916138-Kubernetes-integration-overview)

### Other Quick Start reference deployments

* [AWS Container Quick Start home page](https://aws.amazon.com/quickstart/?quickstart-all.sort-by=item.additionalFields.updateDate&quickstart-all.sort-order=desc&awsf.quickstart-homepage-filter=categories%23containers)
* [AWS Quick Start home page](https://aws.amazon.com/quickstart/)
