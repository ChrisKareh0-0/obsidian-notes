## Create IAM User
![[IAM User.png]]
Permission Options is set to : Attach Policies directly
Permission Policy is set to : AdministratorAccess | Type : AWS managed - job function
## Create Access Key
![[Pasted image 20240623225853.png]]
Use Case is set to : Command Line Interface (CLI)
## Create Key Pair
![[keyPair.png]]
Key Pair Type : ED25519
Private Key File Format : .pem
## Terraform Config
```hcl

terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 3.0"
    }
  }
}

provider "aws" {
  region     = "us-east-1"


}

resource "aws_security_group" "mysg" {
  name        = "mysg"
  description = "Allow inbound SSH"

  ingress {
    from_port        = 22
    to_port          = 22
    protocol         = "tcp"
    cidr_blocks      = ["0.0.0.0/0"]
    ipv6_cidr_blocks = ["::/0"]
  }

  ingress {
    description = "HTTP"
    from_port   = 8080
    to_port     = 8080
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
  }
}

resource "aws_instance" "myec2" {
  ami           = "ami-0eaf7c3456e7b5b68"
  instance_type = "t2.micro"
  key_name      = "web-key"

  tags = {
    Name = "instance-1"
  }

  user_data = <<-EOF
    #!/bin/bash

    sudo amazon-linux-extras install java-openjdk11 -y
    sudo wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo
    sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io-2023.key
    sudo yum install jenkins -y
    sudo systemctl start jenkins
    sudo yum install python3 -y
  EOF
}

resource "aws_network_interface_sg_attachment" "sg_attachment1" {
  security_group_id    = aws_security_group.mysg.id
  network_interface_id = aws_instance.myec2.primary_network_interface_id
}

```
## Terraform Apply
![[Pasted image 20240625195249.png]]
## Instance is running 
![[Pasted image 20240625001656.png]]
## Connected to instance using SSH
![[Pasted image 20240625195929.png]]
## Dependencies Check
![[Pasted image 20240625200541.png]]
