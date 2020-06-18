# Jitsi
Jitsi-install on Ubuntu 20.04

Required:

* Ubuntu 20.04 server

* Public ip

* Subdomain assigned to above ip, handy when using let's encrypt ;-)

Update using TLS and add universe repo
```
apt update apt install apt-transport-https 

apt-add-repository universe apt update
```
Add public fqdn to hosts file

Edit /etc/hosts to 127.0.0.1 localhost goto.domain.dk

Get the Jitsi developers public signing key
```
curl https://download.jitsi.org/jitsi-key.gpg.key | sudo sh -c 'gpg --dearmor > /usr/share/keyrings/jitsi-keyring.gpg' 

echo 'deb [signed-by=/usr/share/keyrings/jitsi-keyring.gpg] https://download.jitsi.org stable/' | sudo tee /etc/apt/sources.list.d/jitsi-stable.list > /dev/null

```
Open and enable the firewall
```
ufw allow proto tcp from any to any port 22,80,443,4443
ufw allow proto udp from any to any port 10000 
ufw enable
```
install Jitsi using stable branch from download.jitsi.org
```
apt install jitsi-meet
```
The Jitsi installer has a minor error in refering to certbot-auto - should be certbot

Hattip to vultr.com (https://www.vultr.com/docs/install-jitsi-meet-on-ubuntu-20-04-lts)

```
apt install certbot sed -i 's/./certbot-auto/certbot/g' /usr/share/jitsi-meet/scripts/install-letsencrypt-cert.sh 

ln -s /usr/bin/certbot /usr/sbin/certbot 
```

Get a Let's encrypt certificate
```
/usr/share/jitsi-meet/scripts/install-letsencrypt-cert.sh
```
Busy servers - consider editing /etc/systemd/system.conf

Ensure following minimum settings:

- DefaultLimitNOFILE=65000

- DefaultLimitNPROC=65000

- DefaultTasksMax=65000
