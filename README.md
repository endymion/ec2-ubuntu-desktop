# Utuntu desktop PC in Amazon EC2

This is an example of how to spin up an
[Ubuntu 14.04](http://releases.ubuntu.com/14.04/) LTS desktop instance
in the cloud in [Amazon EC2](http://aws.amazon.com/ec2/) with X11
provided by [Lubuntu](http://lubuntu.net/), using
[Vagrant](https://www.vagrantup.com/) with the
[vagrant-aws](https://github.com/mitchellh/vagrant-aws) plugin.

Then I installed
 and encoded that in the Vagrant provisioning.
The whole thing is fully automated from the ```vagrant up``` command.

When you run ```vagrant up```, you get an Ubuntu instance with a minimal
X Windows desktop, running in EC2.

## Prerequisites

You will need:

* An AWS IAM user, with access to EC2
* An EC2 key pair

## Authentication

To spin up an EC2 machine you'll need a key and secret for accessing your AWS account.
Create an IAM user in a group with full access to EC2, and provide the key and secret
by setting two environment variables:

    export AWS_KEY="[YOUR KEY]"
    export AWS_SECRET="[YOUR SECRET]"

To access the EC2 machine you'll need an [EC2 key pair](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html).  If you have a key pair in a file
called ```key.pem``` in the root of this project, then you can configure that
by setting two environment variables, like this:

    export AWS_KEYPAIR_NAME="key"
    export AWS_KEYPAIR_PATH="key.pem"

If you have an aws-authentication.sh file that contains all four of the above
export commands, then you can set all four of the above environment variables
with it.  But you can't just run it.  You have to source it:

    source aws-authentication.sh

## Ports

You will need to adjust your default security group for your EC2 instance
(or specify a custom group) to open ports for:

* SSH/SFTP (port 22)

## Prerequisites

* Install [Vagrant](http://downloads.vagrantup.com/)
* Install the vagrant-aws plugin with ```vagrant plugin install vagrant-aws```

## Setup

* Clone this project to your development system.
* Add your AWS key, secret and EC2 key pair references to an aws-configuration.sh file
  and source it to set your environment variables.
* Spin up your Vagrant virtual machine in AWS with
  ```vagrant up --provision=aws```.
  Vagrant will automatically use the specified AMI to create a new cloud
  instance, and then provision (configure) the instance.

## Access the desktop with VNC

* Set up an SSH tunnel for the VNC port from your development machine, with
  ```vagrant ssh -- -L 5900:localhost:5900 -i key.pem``` where
  ```key.pem``` is the path of your EC2 key pair file.
* Start a local VNC viewer and connect to ```localhost```
