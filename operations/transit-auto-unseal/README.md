# [Vault 1.1 Beta] Transit Auto-unseal



**NOTE:** Currently, Vault 1.1 is in _beta_.

---


## Demo Steps

>**NOTE:** The example Terraform in this repository is created for the demo purpose, and not suitable for production use.


1. Set this location as your working directory

1. Set your AWS credentials as environment variables: `AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY`

1. Set the Terraform variable values in a file named `terraform.tfvars` (use `terraform.tfvars.example` as a base)

    **Example:**

    ```shell
    # SSH key name to access EC2 instances (should already exist) in the region you are using
    key_name = "vault-test"

    # All resources will be tagged with this
    environment_name = "va-demo"

    # If you want to use a different AWS region
    aws_region = "us-west-1"
    availability_zones = "us-west-1a"
    ```

1. Run Terraform:

    ```shell
    # Pull necessary plugins
    $ terraform init

    $ terraform plan

    # Output provides the SSH instruction
    $ terraform apply -auto-approve
    ```

### Vault Server

1. SSH into the Vault **server** instance: `ssh -i <path_to_key> ubuntu@<public_ip_of_server>`

1. On the **server** instance, run the following commands:

    ```shell
    # Initialize Vault
    $ vault operator init > key.txt

    # Check the Vault server status
    $ vault status

    # Log in with initial root token
    $ vault login $(grep 'Initial Root Token:' key.txt | awk '{print $NF}')    
    ```


## Clean up

Clean up the cloud resources

    ```plaintext
    $ terraform destroy -auto-approve
    $ rm -rf .terraform terraform.tfstate* private.key
    ```
