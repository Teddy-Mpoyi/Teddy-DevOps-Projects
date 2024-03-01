# PROJECT-16-AUTOMATE-INFRASTRUCTURE-WITH-IAC-USING-TERRAFORM-PART-1


# AUTOMATE INFRASTRUCTURE WITH IAC USING TERRAFORM PART 1

After you have built AWS infrastructure for 2 websites manually, it is time to automate the process using Terraform.

Let us start building the same set up with the power of Infrastructure as Code (IaC)

![tooling_project_16](https://user-images.githubusercontent.com/10243139/137618206-499335c6-0bef-4e15-b102-b2130cb0521e.png)

### Prerequisites before you begin writing Terraform code

- You must have completed Terraform course from the Learning dashboard

- Create an IAM user, name it terraform (ensure that the user has only programatic access to your AWS account) and grant this user AdministratorAccess permissions.

- Copy the secret access key and access key ID. Save them in a notepad temporarily.

- Configure programmatic access from your workstation to connect to AWS using the access keys copied above and a Python SDK (boto3). You must have Python 3.6 or higher on your workstation.

![P4](https://user-images.githubusercontent.com/10243139/137618211-31df33e2-8caa-4e39-b0a7-a66a114d6d66.png)

- Create an S3 bucket to store Terraform state file. You can name it something like yourname-dev-terraform-bucket

![P5](https://user-images.githubusercontent.com/10243139/137618228-26d6b027-4935-4281-9755-d746978ac315.png)

- When you have configured authentication and installed boto3, make sure you can programmatically access your AWS account by running following commands in python:

        import boto3
        s3 = boto3.resource('s3')
        for bucket in s3.buckets.all():
            print(bucket.name)

![P6](https://user-images.githubusercontent.com/10243139/137618233-a5bbc704-679b-4f38-8090-3086d8fbe661.png)


## Part 1 – VPC | Subnets | Security Groups

### Let us create a directory structure

Open your Visual Studio Code and:

- Create a folder called PBL

- Create a file in the folder, name it main.tf

Your setup should look like this:

![1b](https://user-images.githubusercontent.com/10243139/137618298-68d92f18-26d3-4222-b4e4-8d54e641be39.png)

Set up Terraform CLI as per this instruction below.

- Add AWS as a provider, and a resource to create a VPC in the main.tf file.

- Provider block informs Terraform that we intend to build infrastructure within AWS.

- Resource block will create a VPC.

        provider "aws" {
          region = "us-east-2"
        }

        # Create VPC
        resource "aws_vpc" "main" {
          cidr_block                     = "172.16.0.0/16"
          enable_dns_support             = "true"
          enable_dns_hostnames           = "true"
          enable_classiclink             = "false"
          enable_classiclink_dns_support = "false"
        }

![1c](https://user-images.githubusercontent.com/10243139/137618332-7144a484-485a-4f23-99f3-a870734b9a72.png)

The next thing is to download necessary plugins for Terraform to work. Lets accomplish this with terraform init command as seen in the below demonstration.

![1d](https://user-images.githubusercontent.com/10243139/137618349-cda7d5f4-af5f-4323-9374-3138c9d63560.png)

Next, let us create the only resource we just defined: aws_vpc.

Run terraform plan

![1e1](https://user-images.githubusercontent.com/10243139/137618371-19b411b0-c510-451b-a084-584db3994df2.png)

Then, if you are happy with changes planned, execute terraform apply

![1e2](https://user-images.githubusercontent.com/10243139/137618381-cb8faa6b-b8ae-40a2-99a7-3f4afdfc8000.png)


## Part 2 – Subnets resource section

According to our architectural design, we require 6 subnets:

- 2 public
- 2 private for webservers
- 2 private for data layer

Let us create the first 2 public subnets.

Add below configuration to the main.tf file:

        # Create public subnets1
            resource "aws_subnet" "public1" {
            vpc_id                     = aws_vpc.main.id
            cidr_block                 = "172.16.0.0/24"
            map_public_ip_on_launch    = true
            availability_zone          = "us-east-2a"

        tags = {
          Name = "Public Subnet 1"
        }
        }

        # Create public subnet2
            resource "aws_subnet" "public2" {
            vpc_id                     = aws_vpc.main.id
            cidr_block                 = "172.16.1.0/24"
            map_public_ip_on_launch    = true
            availability_zone          = "us-east-2b"

        tags = {
          Name = "Public Subnet 2"
        }
        }

We are creating 2 subnets, therefore declaring 2 resource blocks – one for each of the subnets.

To execute the command and see results, run the following command:

        Run terraform plan 

![2b1](https://user-images.githubusercontent.com/10243139/137618589-72bbe848-0547-48ea-8d8d-a12732b7733a.png)

        Run terraform apply

![2b2](https://user-images.githubusercontent.com/10243139/137618596-3a94f1f8-144e-4a6b-8fdf-a54fc8711668.png)


## Part 3 – Fixing The Problems By Code Refactoring

In this part, we will introduce variables and remove hard coding.

Starting with the provider block, declare a variable named region, give it a default value, and update the provider section by referring to the declared variable.

    variable "region" {
        default = "us-east-2"
    }

    provider "aws" {
        region = var.region
    }

Do the same to cidr value in the vpc block, and all the other arguments.

        variable "region" {
            default = "us-east-2"
        }

        variable "cidr_block" {
            default = "172.16.0.0/16"
        }

        variable "enable_dns_support" {
            default = "true"
        }

        variable "enable_dns_hostnames" {
            default ="true" 
        }

        variable "enable_classiclink" {
            default = "false"
        }

        variable "enable_classiclink_dns_support" {
            default = "false"
        }

        provider "aws" {
            region = var.region
        }

        # Create VPC
        resource "aws_vpc" "main" {
            cidr_block                     = var.vpc_cidr
            enable_dns_support             = var.enable_dns_support 
            enable_dns_hostnames           = var.enable_dns_support
            enable_classiclink             = var.enable_classiclink
            enable_classiclink_dns_support = var.enable_classiclink


        tags = {
          Name = "Main VPC"
        }
        }

![3b](https://user-images.githubusercontent.com/10243139/137618761-9181c1c5-d256-4ed6-befb-1bccdfd18c6b.png)

Terraform has a functionality that allows us to pull data which exposes information to us. 

Let us fetch Availability zones from AWS, and replace the hard coded value in the subnet’s availability_zone section.

        # Get list of availability zones
        data "aws_availability_zones" "available" {
        state = "available"
        }

To make use of this new data resource, we will need to introduce a count argument in the subnet block: Something like this.

    # Create public subnet1
    resource "aws_subnet" "public" { 
        count                   = 2
        vpc_id                  = aws_vpc.main.id
        cidr_block              = "172.16.1.0/24"
        map_public_ip_on_launch = true
        availability_zone       = data.aws_availability_zones.available.names[count.index]

    }

To understand what is going on here:

- The count tells us that we need 2 subnets. Therefore, Terraform will invoke a loop to create 2 subnets.

- The data resource will return a list object that contains a list of AZs. Internally, Terraform will receive the data like this
  
        ["us-east-2a", "us-east-2b"]

- Each of them is an index, the first one is index 0, while the other is index 1. If the data returned had more than 2 records, then the index numbers would continue to increment.


- Therefore, each time Terraform goes into a loop to create a subnet, it must be created in the retrieved AZ from the list. Each loop will need the index number to determine what AZ the subnet will be created. That is why we have data.aws_availability_zones.available.names[count.index] as the value for availability_zone. When the first loop runs, the first index will be 0, therefore the AZ will be eu-central-1a. The pattern will repeat for the second loop.

- But if we run Terraform with this configuration, it may succeed for the first time, but by the time it goes into the second loop, it will fail because we still have cidr_block hard coded. The same cidr_block cannot be created twice within the same VPC. So, let’s make cidr_block dynamic.

We will introduce a function cidrsubnet() to make this happen. 

    # Create public subnet1
    resource "aws_subnet" "public" { 
        count                   = 2
        vpc_id                  = aws_vpc.main.id
        cidr_block              = cidrsubnet(var.vpc_cidr, 4 , count.index)
        map_public_ip_on_launch = true
        availability_zone       = data.aws_availability_zones.available.names[count.index]

    }
    
A closer look at cidrsubnet – this function works like an algorithm to dynamically create a subnet CIDR per AZ. Regardless of the number of subnets created, it takes care of the cidr value per subnet.

Its parameters are cidrsubnet(prefix, newbits, netnum)

- The prefix parameter must be given in CIDR notation, same as for VPC.

- The newbits parameter is the number of additional bits with which to extend the prefix. For example, if given a prefix ending with /16 and a newbits value of 4, the resulting subnet address will have length /20

- The netnum parameter is a whole number that can be represented as a binary integer with no more than newbits binary digits, which will be used to populate the additional bits added to the prefix

Lets try how this works by entering the terraform console and keep changing the figures to see the output.

        On the terminal, run terraform console
        type cidrsubnet("172.16.0.0/16", 4, 0)
        Hit enter
        See the output
        Keep change the numbers and see what happens.
        To get out of the console, type exit

![3i](https://user-images.githubusercontent.com/10243139/137619018-41009b9f-dae4-49bd-8898-bd752a3e583f.png)

The final problem to solve is removing hard coded count value. To solve this, we can introuduce length() function, which basically determines the length of a given list, map, or string.

Since data.aws_availability_zones.available.names returns a list like ["eu-central-1a", "eu-central-1b", "eu-central-1c"] we can pass it into a lenght function and get number of the AZs.

    length(["us-east-2a", "us-east-2b", "us-east-2c"])

Now we can simply update the public subnet block like this

        # Create public subnet1
            resource "aws_subnet" "public" { 
                count                   = length(data.aws_availability_zones.available.names)
                vpc_id                  = aws_vpc.main.id
                cidr_block              = cidrsubnet(var.vpc_cidr, 4 , count.index)
                map_public_ip_on_launch = true
                availability_zone       = data.aws_availability_zones.available.names[count.index]

            }

### Observations:

What we have now, is sufficient to create the subnet resource required. But if you observe the length function will return number 3 to the count argument, but what we actually need is 2.

To fix this declare a variable to store the desired number of public subnets, and set the default value

        variable "preferred_number_of_public_subnets" {
          default = 2
        }

Next, update the count argument with a condition. Terraform needs to check first if there is a desired number of subnets. Otherwise, use the data returned by the lenght function.

        # Create public subnets
        resource "aws_subnet" "public" {
          count  = var.preferred_number_of_public_subnets == null ? length(data.aws_availability_zones.available.names) : var.preferred_number_of_public_subnets   
          vpc_id = aws_vpc.main.id
          cidr_block              = cidrsubnet(var.vpc_cidr, 4 , count.index)
          map_public_ip_on_launch = true
          availability_zone       = data.aws_availability_zones.available.names[count.index]

        }

Now lets break it down:

- The first part var.preferred_number_of_public_subnets == null checks if the value of the variable is set to null or has some value defined.

- The second part ? and length(data.aws_availability_zones.available.names) means, if the first part is true, then use this. In other words, if preferred number of public subnets is null (Or not known) then set the value to the data returned by lenght function.

- The third part : and  var.preferred_number_of_public_subnets means, if the first condition is false, i.e preferred number of public subnets is not null then set the value to whatever is definied in var.preferred_number_of_public_subnets

Now your entire configuration should now look like this

        # Get list of availability zones
        data "aws_availability_zones" "available" {
        state = "available"
        }

        variable "region" {
              default = "us-east-2"
        }

        variable "vpc_cidr" {
            default = "172.16.0.0/16"
        }

        variable "enable_dns_support" {
            default = "true"
        }

        variable "enable_dns_hostnames" {
            default ="true" 
        }

        variable "enable_classiclink" {
            default = "false"
        }

        variable "enable_classiclink_dns_support" {
            default = "false"
        }

          variable "preferred_number_of_public_subnets" {
              default = 2
        }

        provider "aws" {
          region = var.region
        }

        # Create VPC
        resource "aws_vpc" "main" {
          cidr_block                     = var.vpc_cidr
          enable_dns_support             = var.enable_dns_support 
          enable_dns_hostnames           = var.enable_dns_support
          enable_classiclink             = var.enable_classiclink
          enable_classiclink_dns_support = var.enable_classiclink

        tags = {
          Name = "Main VPC"
        }

        }

        # Create public subnets
        resource "aws_subnet" "public" {
          count  = var.preferred_number_of_public_subnets == null ? length(data.aws_availability_zones.available.names) : var.preferred_number_of_public_subnets   
          vpc_id = aws_vpc.main.id
          cidr_block              = cidrsubnet(var.vpc_cidr, 4 , count.index)
          map_public_ip_on_launch = true
          availability_zone       = data.aws_availability_zones.available.names[count.index]

        tags = {
            Name = "Subnet ${count.index}"
          }

        }


## Part 4 – Introducing variables.tf & terraform.tfvars

Instead of having a long list of variables in main.tf file, we can actually make our code a lot more readable and better structured by moving out some parts of the configuration content to other files.

We will put all variable declarations in a separate file and provide non default values to each of them

- Create a new file and name it variables.tf
- Copy all the variable declarations into the new file.
- Create another file, name it terraform.tfvars
- Set values for each of the variables.

  - Maint.tf

        # Get list of availability zones
        data "aws_availability_zones" "available" {
        state = "available"
        }

        provider "aws" {
          region = var.region
        }

        variable "additional_tags" {
          default     = {}
          description = "Additional resource tags"
        }

        # Create VPC
        resource "aws_vpc" "main" {
          cidr_block                     = var.vpc_cidr
          enable_dns_support             = var.enable_dns_support 
          enable_dns_hostnames           = var.enable_dns_support
          enable_classiclink             = var.enable_classiclink
          enable_classiclink_dns_support = var.enable_classiclink
        tags = {
          Name = "Main VPC"
        }
        }

        # Create public subnets
        resource "aws_subnet" "public" {
          count  = var.preferred_number_of_public_subnets == null ? length(data.aws_availability_zones.available.names) : var.preferred_number_of_public_subnets   
          vpc_id = aws_vpc.main.id
          cidr_block              = cidrsubnet(var.vpc_cidr, 4 , count.index)
          map_public_ip_on_launch = true
          availability_zone       = data.aws_availability_zones.available.names[count.index]

        tags = merge(
            var.additional_tags,
            {
              Name = format("PrivateSubnet-%s", count.index)
            } 
          )
        }

  - variables.tf



        variable "region" {
            default = "us-east-2"
        }

        variable "vpc_cidr" {
            default = "172.16.0.0/16"
        }

        variable "enable_dns_support" {
            default = "true"
        }

        variable "enable_dns_hostnames" {
            default ="true" 
        }

        variable "enable_classiclink" {
            default = "false"
        }

        variable "enable_classiclink_dns_support" {
            default = "false"
        }

        variable "preferred_number_of_public_subnets" {
            default = null
        }



  - terraform.tfvars

        region = "us-east-2"

        vpc_cidr = "172.16.0.0/16" 

        enable_dns_support = "true" 

        enable_dns_hostnames = "true"  

        enable_classiclink = "false" 

        enable_classiclink_dns_support = "false" 

        preferred_number_of_public_subnets = 2


You should also have this file structure in the PBL folder.

        └── PBL
            ├── main.tf
            ├── terraform.tfstate
            ├── terraform.tfstate.backup
            ├── terraform.tfvars
            └── variables.tf

Run terraform plan and apply to ensure everything works

![4 1](https://user-images.githubusercontent.com/10243139/137619749-aa3e5398-730c-4220-8d6f-3f9fa046f2da.png)
![4 2](https://user-images.githubusercontent.com/10243139/137619756-17867c91-a64c-4589-bdfb-20d927496114.png)

## Congratulations!
### You have learned how to create and delete AWS Network Infrastructure programmatically with Terraform!

