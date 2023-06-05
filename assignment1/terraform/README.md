## Cloud Deployment

### Prerequisite
1. Install terraform - [download](https://developer.hashicorp.com/terraform/downloads)
2. GCP Cloud Account

### Resources Provisioned
* Compute Instance of the type `e2-standard-4`
* OS : Ubuntu
* Disk Size 50 GB
* Assign's static IP
* Firewall rules on port `"80", "8080", "8086", "3000", "8095"` to enable traffic
* Install docker using the install script, refer [install.sh](./install.sh)


### Instruction

1. Change working directory to `terraform`

2. Generate SSH key pair
    ```
    ssh-keygen -t rsa -f ce-tf -C ubuntu -b 2048
    ```
    Press `enter` for the `Enter passphrase (empty for no passphrase):` prompt </br>This generates the public and private SSH key pair file name `ce`

    Expected Output:
    ```bash
    $ ls -l ce*
    -rw------- 1 anku anku 1811 Apr 24 21:43 ce-tf
    -rw-r--r-- 1 anku anku  388 Apr 24 21:43 ce-tf.pub
    ```

3. Login into GCP Cloud, create a new Service Account and download the key as JSON file</br>
   Role : Compute Admin </br>
   Copy the contents of the key into `tfkey.json` in current directory

4. Define the variables in the `terraform.tfvars` file
    ```bash
    # terraform.tfvars
    
    project_id = "vertical-set-375108"
    region = "us-east1"
    zone = "us-east1-b"
    name = "nyc-food-inspection"
    ssh_key_filename = "ce"
    machine_type = "e2-standard-4"
    storage = 100
    ```

5. Run the terraform init command
   ```bash
    terrform init
   ```

   Expected Output:
   ```bash
   Terraform has been successfully initialized!
   ```

6. Run the terraform plan command
    ```bash
    terraform plan
    ```
    Expect Output:
    ```bash
    Plan: 3 to add, 0 to change, 0 to destroy.

    Changes to Outputs:
    + ExternalIP = (known after apply)
    ```

7. Run the terraform apply command
    ```bash
    terraform apply
    ```
    Enter `yes` to confirm the execution
    Expect Output:
    ```bash
    Apply complete! Resources: 3 added, 0 changed, 0 destroyed.

    Outputs:
    ExternalIP = "34.139.43.201"
    ```

    > Note: Going forward replace `34.139.43.201` with the IP returned for your instance.

8.  SSH into the instance using the ssh key generated previously
    ```
    ssh -i ce ubuntu@34.139.43.201
    ```
    Enter `yes` in the prompt `Are you sure you want to continue connecting (yes/no/[fingerprint])?`

9. Clone the repository in local
    ```bash
    git clone https://github.com/piyush-an/DAMG7245-Summer23
    ```
    Change working directory to `DAMG7245-Summer23`

10. Check docker is running

    Command:
    ```bash
    docker --version
    ```
    Expected Output:
    ```bash
    $ docker --version
    Docker version 23.0.4, build f480fb1
    ```

11. Run your services

22. Delete the cloud resources using the terraform delete command
    ```bash
    terraform destroy
    ```
    Enter `yes` to confirm the execution

    Expected Output:
    ```bash
    Destroy complete! Resources: 3 destroyed.
    ```

