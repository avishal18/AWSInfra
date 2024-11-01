# Automation of AWS Infrastructure
We go to the security credentials tab from the management console and create a new access key ID and secret access key.

We create a new VPC and set a CIDR block range with enough IP addresses.

Create a public subnet within the VPC and add a tag.

Next, create a private subnet and add an internet gateway by attaching it to the VPC and add an 'igw' tag.

We also need a route table and we route it via the internet gateway. We associate that route table to the public subnet so clients can gain access via the internet.

We launch an EC2 instance using an AMI ID with an instance type of t2.micro and create a security group that allows inbound traffic on port 22 and port 80 for SSH and HTTP respectively.

We then use shell scripting to host a website using apache
    user_data = <<-EOF
        #!/bin/bash
        sudo apt-get update -y
        sudo apt-get install apache2 -y
        sudo systemctl start apache2
        sudo systemctl enable apache2
        echo "<html><body><h1>Welcome to my website!</h1></body></html>" > /var/www/html/index.html
        sudo systemctl restart apache2
    EOF

We create an aws_eip that makes an elastic IP and associated it to the EC2 instance and tagged it accordingly.

We use cloudfront using an s3 bucket with GET and HEAD to make the static content read-only.
