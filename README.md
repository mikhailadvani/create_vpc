create_vpc
-------

[![Build Status](https://travis-ci.org/mikhailadvani/create_vpc.svg?branch=master)](https://travis-ci.org/mikhailadvani/create_vpc) [![Galaxy](https://img.shields.io/badge/ansible--galaxy-mikhailadvani.create_vpc-blue.svg)](https://galaxy.ansible.com/mikhailadvani/create_vpc)


Ansible role to setup basic networking components of an AWS VPC viz. VPC, Subnet, Internet Gateway, NAT Gateway and Routing tables

### Required variables

Setup the following vars and include them in the playbook before execution

    api_keys:
      vpc:
        aws_access_key_id: AWS_ACCESS_KEY_ID
        aws_secret_access_key: AWS_SECRET_ACCESS_KEY
    aws_region: us-east-1
    vpc_name: vpc-1                     # Name of the VPC 
    vpc:
      cidr_block: 172.16.0.0/16         # CIDR block of the VPC
      tenancy: default                  # Tenancy of the VPC 
      nat_gateway_subnet_name: public-1 # Name tag of the subnet where the NAT gateway iss to be created 
      subnets:
      - availability_zone: us-east-1a   # AZ of the subnet
        cidr_block: 172.16.0.0/24       # CIDR block of the subnet 
        name: public-1                  # Name tag of the subnet
        gateway: igw                    # The type of gateway, if any, whose entry should be in the routing table(s)
      - availability_zone: us-east-1a
        cidr_block: 172.16.1.0/24
        name: private-1
        gateway: nat
      - availability_zone: us-east-1a
        cidr_block: 172.16.2.0/24
        name: private-2

- If `gateway` is undefined for a subnet, the subnet is a private subnet without inbound/outbound access to the internet
- If `gateway` is `igw`, the subnet is a public subnet
- If `gateway` is `nat`, the subnet is a private subnet with outbound internet access

- It might be advantageous to specify the vpc configurations in a file named as the `vpc_name`, pass the value as an extra var, and include the particular vars file



### Example Playbook

    - hosts: localhost
      roles:
         - { role: mikhailadvani.create_vpc }

License
-------

Apache