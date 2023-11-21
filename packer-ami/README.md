
### Prometheus Installation using HashiCorp Packer and AWS AMI

### Create a Prometheus User that's incapable of logging in
`sudo useradd --no-create-home --shell /bin/false Prometheus`

### Create a folder in /etc to store the configuration file
`sudo mkdir /etc/prometheus` 

### Create another folder in /var/lib to store the Prometheus data
`sudo mkdir /var/lib/prometheus`

### Change both folders permissions to be the Promethues user
```bash
sudo chown prometheus:prometheus /etc/prometheus
sudo chown prometheus:prometheus /var/lib/prometheus
```

### Download Prometheus and untar it and copy the binary to user/local/bin
`sudo cp prometheus /usr/local/bin/`

### Copy Promtool as well
`sudo cp promtool /usr/local/bin/`

### Update the permissions for the two files
```bash
sudo chown prometheus:prometheus /usr/local/bin/prometheus
sudo chown prometheus:prometheus /usr/local/bin/promtool

```


### Cope the consoles folder meant for dashboarding and visualization
```bash
sudo cp -r consoles /etc/prometheus
sudo cp -r console_libraries /etc/prometheus
```

### Update the permissions (the -R is for changing permissions for directory and all of the files within it)
```bash 
sudo chown -R prometheus:prometheus /etc/prometheus/consoles
sudo chown -R prometheus:prometheus /etc/prometheus/consoles_libraries
```

## Copy the configuration file into the /etc/prometheus folder and change the permissions to that of the Prometheus user) 
```bash
sudo cp prometheus.yaml /etc/prometheus/prometheus.yml
sudo chown prometheus:prometheus /etc/prometheus/prometheus.yml
```
### The command to start prometheus

```bash
sudo -u prometheus /usr/local/bin/prometheus \
--config.file /etc/prometheus/prometheus.yml \
--storage.tsdb.path /var/lib/prometheus/ \
--web.console.templates=/etc/promethues/consoles \
--web.console.libraries=/etc/promethues/console_libraries
```


### Create a unit service file for the Prometheus server 
`sudo vi /etc/systemd/system/prometheus.service`

```
[Unit]
Description=Promethues
Wants=network-online.target
After=network-online.target

[Service]
User=promethues
Group=promethues
Type=simple
ExecStart= /usr/local/bin/prometheus \
  --config.file /etc/prometheus/prometheus.yml \
  --storage.tsdb.path /var/lib/prometheus/ \
  --web.console.templates=/etc/promethues/consoles \
  --web.console.libraries=/etc/promethues/console_libraries

[Install]
Wanted=multi-user.target

```

### Since we change the unit file, we have to relaod systemctl daemon
```bash
sudo systemctl daemon-reload
sudo systemctl start prometheus
```































