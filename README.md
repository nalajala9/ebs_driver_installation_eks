# EBS_DRIVER_INSTALLATION_STEPS

## Step-1: Create IAM policy for EBS:

* Go to Services -> IAM
* Create a Policy
* Select JSON tab and copy paste the below JSON

```


{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "ec2:AttachVolume",
        "ec2:CreateSnapshot",
        "ec2:CreateTags",
        "ec2:CreateVolume",
        "ec2:DeleteSnapshot",
        "ec2:DeleteTags",
        "ec2:DeleteVolume",
        "ec2:DescribeInstances",
        "ec2:DescribeSnapshots",
        "ec2:DescribeTags",
        "ec2:DescribeVolumes",
        "ec2:DetachVolume"
      ],
      "Resource": "*"
    }
  ]
}

```


* Click on Review Policy and provide name for the policy (eg:Amazon_EBS_CSI_Driver)
* Click on Create Policy


      
 


## Step-2: Get the IAM role of WorkerNode and attach the above policy to the role

    kubectl -n kube-system describe configmap aws-auth

>  Note the role arn

* Go to Services -> IAM -> Roles
* Search for the role arn that we got earlier
* Click on Permissions tab
* Click on Attach Policies
* Search for created plociy earlier(Amazon_EBS_CSI_Driver) and click on Attach Policy

## Step-3: Deploy Amazon EBS CSI Driver

### To deploy the CSI driver:


    kubectl apply -k "github.com/kubernetes-sigs/aws-ebs-csi-driver/deploy/kubernetes/overlays/stable/?ref=release-1.13"

### Verify driver is running:

    kubectl get pods -n kube-system


## Alternatively, you could also install the driver using Helm:

### References:

   https://github.com/kubernetes-sigs/aws-ebs-csi-driver/blob/master/docs/install.md 

   https://docs.aws.amazon.com/eks/latest/userguide/csi-iam-role.html





