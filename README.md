# rpi-k8s-ansible
RaspberryPi Ansible Setup

## Raspberry Pi headless mode setup

* Raspbian installed on SD cards
* Add empty ssh to root directory to allow ssh connectivity
* Add wpa_supplicant.conf to /boot to configure wifi
```
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1
country=GB

network={
 ssid="<<WIRELESS_SSID>>"
 psk="<<WIRELESS_PASS>>"
}
```
* Boot the raspberry pi
* Login to wireless router to find the IP address of the raspberry pi
* SSH into the raspberry pi 
```
ssh pi@raspberry.local      # Password is raspberry
```
* Change hostname of raspberry pi using raspi-config
```
sudo raspi-config
-> Network Options -> Hostname
```

# Configure ansible inventory

```
[master]
rpi-k8s-01  ansible_host=192.168.1.36  ansible_user=pi

[workers]
rpi-k8s-02  ansible_host=192.168.1.33  ansible_user=pi
rpi-k8s-03  ansible_host=192.168.1.38  ansible_user=pi
```

# Running ansible playbook

```
ansible-playbook -i inventory -v site.yaml
```

