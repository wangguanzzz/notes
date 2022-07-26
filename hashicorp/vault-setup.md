Solution
Log in to the server using the credentials provided:

ssh cloud_user@<PUBLIC_IP_ADDRESS>
Note: At times, you will be making changes with root permissions. When doing so, use the password provided for the lab server if prompted to enter a password.

Install and Configure the Consul Service
Download, Unpack, and Move Consul to the Bin Directory
Download Consul:
```
wget https://releases.hashicorp.com/consul/1.7.3/consul_1.7.3_linux_amd64.zip
```
Install unzip:
```
sudo apt install unzip
```
Unpack Consul:
```
unzip consul_1.7.3_linux_amd64.zip
```
Move the consul installation executable to the bin directory:
```
sudo mv consul /usr/bin
```
Verify the installation was successful:
```
consul
```
The output should list all of the available Consul commands.

Create and Configure the Consul systemd Service
Get and copy the IP address of the server:
```
ip addr show
```
Note: You will use this IP address again when configuring Vault, so you may want to paste it into a file or keep it copied in your clipboard for easier access later on.

Using vim, create a systemd service file:
```
sudo vim /etc/systemd/system/consul.service
```
In the file, paste in the following configuration, replacing IP.ADDRESS.OF.SERVER with the IP address you just obtained:
```
[Unit]
Description=Consul
Documentation=https://www.consul.io/

[Service]
ExecStart=/usr/bin/consul agent -server -ui -data-dir=/temp/consul -bootstrap-expect=1 -node=vault -bind=IP.ADDRESS.OF.SERVER -config-dir=/etc/consul.d/
ExecReload=/bin/kill -HUP $MAINPID
LimitNOFILE=65536

[Install]
WantedBy=multi-user.target
```
Press Esc, type :wq, and hit Enter to save and exit the file.

Create a directory for the configuration files:
```
sudo mkdir /etc/consul.d
```
Note: Creating a configuration file for the UI is optional. If you choose not to perform the following steps to do so, proceed to reloading the system.

Using vim, create a JSON configuration file for the UI:
```
sudo vim /etc/consul.d/ui.json
```
In the file, paste in the following configuration:
```
{
  "addresses": {
   "http": "0.0.0.0"
   }
}
```
Press Esc, type :wq, and hit Enter to save and exit the file.

Reload, Start, Enable, and Verify the Consul Service
Reload the system:
```
sudo systemctl daemon-reload
```
Start the Consul service:
```
sudo systemctl start consul
```
Enable the Consul service:

sudo systemctl enable consul
Verify the status of the Consul service:
```
sudo systemctl status consul
```
The output should indicate that the Consul service is active and running.

Install and Configure the Vault Service
Download, Unpack, and Move Vault to the Bin Directory
Download Vault:
```
wget https://releases.hashicorp.com/vault/1.5.0/vault_1.5.0_linux_amd64.zip
```
Unpack Vault:
```
unzip vault_1.5.0_linux_amd64.zip
```
Move the Vault binary to the bin directory:
```
sudo mv vault /usr/bin
```
Create and Configure the Vault systemd Service
Create a directory for the configuration file:
```
sudo mkdir /etc/vault/
```
Get and copy the IP address of the server:

ip addr show
Note: Alternately, if you still have the IP address accessible from when you configured Consul, you could just copy/paste the address from where you saved it.

Using vim, create the configuration file:
```
sudo vim /etc/vault/config.hcl
```
In the file, paste in the following configuration, replacing Consul.IP.ADDRESS with the IP address you just obtained:
```
storage "consul" {
        address = "Consul.IP.ADDRESS:8500"
        path = "vault/"
}
listener "tcp" {
        address = "0.0.0.0:80"
        tls_disable = 1
}
ui = true
```
Press Esc, type :wq, and hit Enter to save and exit the file.

Using vim, create a systemd service file:
```
sudo vim /etc/systemd/system/vault.service
```
In the file, paste in the following configuration:
```
[Unit]
Description=Vault
Documentation=https://www.vault.io/

[Service]
ExecStart=/usr/bin/vault server -config=/etc/vault/config.hcl
ExecReload=/bin/kill -HUP $MAINPID
LimitNOFILE=65536

[Install]
WantedBy=multi-user.target
```
Press Esc, type :wq, and hit Enter to save and exit the file.

Reload, Start, Enable, and Verify the Vault Service
Reload the system:
```
sudo systemctl daemon-reload
sudo systemctl start vault
sudo systemctl enable vault
sudo systemctl status vault
```
Connect to the Vault Service
Using the dig command and the public IP address of the lab server (found on the lab credentials page), get and copy the fully qualified domain name (FQDN) of the server:

dig -x <PUBLIC_IP_ADDRESS>
Set the Vault address for the current session, replacing DOMAIN with the FQDN you just obtained:

export VAULT_ADDR="http://<DOMAIN>"
Set the Vault address to be persistent upon each system boot, replacing DOMAIN with the same FQDN:

echo "export VAULT_ADDR=http://<DOMAIN>" >> ~/.bashrc
Install autocomplete for Vault commands:

vault -autocomplete-install
And then enable autocomplete:

complete -C /usr/bin/vault vault
Initialize and Access the Vault
Initialize the Vault:

vault operator init
The output will include five Unseal Keys and an Initial Root Token that you will use to access your Vault. You will need to supply three keys to unseal the Vault.

Note: It is important to save the keys and root token. If lost, access to the Vault will be lost. Best practice is to save the keys and token in a secure location.

Run the unseal command with no arguments:

vault operator unseal
When prompted with Key (will be hidden):, copy and paste one of the keys provided and hit Enter.

Repeat the process using two additional keys to unseal the Vault.

Note: When the Vault has been successfully unsealed, the Sealed parameter will display as false in the returned output.

Log in to the Vault using the root token that was provided:

vault login <ROOT_TOKEN>
Vault will notify you if you have successfully authenticated and are logged in.

Conclusion
Congratulations — you've completed this hands-on lab!
