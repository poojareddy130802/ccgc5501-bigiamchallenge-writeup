# CloudFoxable - 1. Do this First!

## Solution

- **Selecting an AWS Account**
  - I created a free tier account in AWS.

- **Creating a Non-Root User**
  - I created a new IAM user with administrative access to use for Terraform operations.
  - Generated an access key for the user.

- **Installing & Configuring AWS CLI**
  - Installed AWS CLI on my system.
  - Configured it using:
    ```bash
    aws configure
    ```
    It asked for the Access ID, key and region.
  - Verified the setup by running:
    ```
    aws sts get-caller-identity
    ```
    This confirmed that my CLI was using the new IAM user profile.
- **Installing Terraform**
  - Downloaded and installed the Terraform binary.
  - Added its location to my system’s path to enable execution from any directory.
- **Cloning Cloudfoxable Repository**
  - Cloned the Cloudfoxable GitHub repository:
    ```
    git clone https://github.com/BishopFox/cloudfoxable
    cd cloudfoxable/aws
    ```
- **Initializing Terraform**
  - Copied the example Terraform variable file:
    ```bash
    cp terraform.tfvars.example terraform.tfvars
    ```
  - Opened terraform.tfvars and updated:
    ```bash
    aws_local_profile = "<MY_LOCAL_PROFILE>"
    ```
  - Initialized Terraform:
    ```bash
    terraform init
    ```
- **Applying Terraform Configuration**
  - Ran an optional plan check:
    ```bash
    terraform plan
    ```
  - Applied the configuration:
    ```bash
    terraform apply
    ```
  - Once completed, the Terraform output provided important details.
- **Extracting the Flag**
  - Carefully reviewed the Terraform output.
  - Found the instructions on setting up AWS CLI for ctf-starting-user.
  - Followed the provided details and retrieved the flag.
  - The flag followed the format:
    ```
    FLAG{challengeName::CamelCaseText}
    ```

## Reflection

* What was your approach?

  I followed a methodical approach, ensuring all setup steps were completed accurately before running Terraform. I carefully examined Terraform outputs and referenced documentation to ensure proper execution.

* What was the biggest challenge?

  The biggest challenge was ensuring the correct AWS profile was used throughout the setup. Initially, I encountered permission issues because I had misconfigured the IAM user’s access settings.

* How did you overcome the challenges?

  I reviewed the AWS IAM user permissions, ensuring that the user had administrator rights. Then, I reconfigured my AWS CLI profile and verified it using aws sts get-caller-identity before proceeding further.

* What led to the break through?

  Reading the Terraform output carefully was key. Initially, I skimmed through it too quickly and missed the crucial details about setting up the AWS CLI for ctf-starting-user. A thorough review helped me identify and retrieve the flag.


* On the blue side, how can the learning be used to properly defend the important assets? 

  Understanding AWS IAM permissions and Terraform outputs is crucial for security. Attackers leverage misconfigurations, so ensuring minimal privilege access and monitoring infrastructure changes is essential. Logging and alerting on Terraform deployments and AWS CLI activity can help detect potential unauthorized actions.


## Overall Experience
This challenge was a great learning experience in Terraform, AWS CLI, and IAM user management. The structured approach helped in efficiently retrieving the flag while reinforcing best security practices.