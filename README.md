# VPC - AWS

## Set-up

1) AWS > VPCs > Create VPC
2) Name, IPv4 CIDR
3) Create subnets
	- Using VPC ID
	- Availabiltity zone
		- Common to make individual subnets across multiple zones
	- IP with /24
	- Public, private
4) Internet gateway
	- Attatch to VPC
5) Route table
	- Pre-defined exists - shell
	- Default = Main
	- Rename, shape as req
	- Does not connect to internet, only VPC
	- Public, Private
6) Security groups
	- Specific ports for specific groups
	- Different groups for each subnet
	- PRIVATE - DB
	- PUBLIC - WebApp
7) NACL
	- Defaults ALL TRAFFIC for inbound outbound.
	- Add new inbound:
		- Rule 110 - SSH - MY IP

###### SEE VPC DIAGRAM


### Unable to resolve host ip?
- Private IP requires adding to localhost file
- `cd etc/`
- `sudo nano hosts`
- Add new line:
	- `127.0.1.1 ip-[PrivateIP]`


### Connecting Mongodb?

- Bad gateway error 502 can be fixed as such:

- Check for correct ip (private - DB instance):
	1) DB_HOST (in `$ env`)
	2) Provision file - export line 
	3) .bashrc

- Run npm instead of pm2 - more output for troubleshooting

## Bastion Server Set-up

1) Switch off MongoDB server first from inside DB
	- `sudo systemctl stop mongod`
	- `service mongod status` <-- Check

2) Security groups - Private
	- Remove SSH access
3) Create public instance
	- New security group - Bastion
		- Inbound: SSH - My IP
		- Outbound: All Traffic 0.0.0.0
4) Append private (DB) security group
	-  Inbound: Bastion SG AND IP
5) scp key into Bastion
	- In new /.ssh folder
6) ssh into DB, through bastion
7) Start up mongodb server
	- `sudo systemctl start mongod`
	- `systemctl status mongod` <-- Check
8) Go to domain
9) Go to domain/posts
