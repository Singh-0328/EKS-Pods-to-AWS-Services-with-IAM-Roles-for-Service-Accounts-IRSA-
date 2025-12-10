Key Components of IRSA

Kubernetes Service Account (SA):
A Service Account in Kubernetes is used to provide an identity for a pod or group of pods to interact with the Kubernetes API or other services.

IAM Role:
An IAM role is an AWS identity with a set of permissions. Using IRSA, you associate an IAM role with a Kubernetes service account, giving specific pods permissions to access AWS resources.

OIDC (OpenID Connect) Identity Provider:
IRSA uses an OpenID Connect (OIDC) provider to establish a trust relationship between the Kubernetes cluster and AWS IAM. This allows AWS to verify the identity of the Kubernetes service account.

How IRSA Works

OIDC Setup:
When an Amazon EKS cluster is created, an OIDC identity provider URL is associated with the cluster. You need to configure IAM to trust this identity provider.

Service Account and IAM Role Association:
You create a Kubernetes service account and annotate it with an IAM role. This annotation links the Kubernetes service account with the specific IAM role.

Authentication Flow:
When a pod uses the service account, it automatically inherits the permissions defined in the associated IAM role. AWS validates the identity of the service account via the OIDC provider.

Temporary Credentials:
The pod does not need static AWS credentials. Instead, it receives temporary credentials from the IAM role, which are rotated automatically.

Why Use IRSA?

Fine-Grained Access Control:
You can assign specific permissions to specific workloads, avoiding over-permissioning.
Improved Security:

Reduces the blast radius in case of a breach by ensuring pods only have the permissions they need.
Eliminates the need for AWS credentials stored in containers or environment variables.

Simplified Management:
Node-level roles (like an EC2 instance profile) are no longer necessary for pod access to AWS resources, making permissions easier to manage.

Least Privilege Principle:
Each pod can operate with the minimum permissions required, adhering to security best practices.
