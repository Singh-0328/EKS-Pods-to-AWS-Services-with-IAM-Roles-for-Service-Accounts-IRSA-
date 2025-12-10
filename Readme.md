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
<img width="1366" height="669" alt="image" src="https://github.com/user-attachments/assets/e626d203-1217-4c58-a832-f3e2b7f19b87" />


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

aws-iam-authenticator specifically on Ubuntu EC2 when working with AWS EKS.
<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/556e120e-88eb-48e3-afd2-125f0e0a4192" />

Because EKS uses IAM-based authentication, it requires the aws-iam-authenticator binary to generate authentication tokens for Kubernetes API. Amazon Linux has it by default, but Ubuntu does not, so we must install it manually on any Ubuntu EC2 used as an EKS admin/bastion host.
