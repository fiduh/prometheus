
### Prometheus Installation using HashiCorp Packer and AWS AMI

### Create a Prometheus User that's incapable of logging in
`sudo useradd --no-create-home --shell /bin/false Prometheus`

### Create a folder in /etc to store the configuration file
`sudo mkdir /etc/prometheus` 

### Create another folder in /var/lib to store the Prometheus data
`sudo mkdir /var/lib/prometheus`

### Change both folders permissions to be the Promethues user
`sudo chown prometheus:prometheus /etc/prometheus`
`sudo chown prometheus:prometheus /var/lib/prometheus `

