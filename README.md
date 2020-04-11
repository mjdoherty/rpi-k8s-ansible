# rpi-k8s-ansible
RaspberryPi Ansible Setup

## Notes

The version of raspian lite used for this kubernetes installation
was 2019-09-26-raspbian-buster-lite.

It has been tested using later versions of raspbian and the boot sequence
hangs after usb setup - so some issues to triage.

## Raspberry Pi headless mode setup

* Raspbian installed on SD cards (see INSTALL.md)
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

# Configure passwordless SSH access to each raspberry pi

On the host machine generate ssh keys, do not enter a password 
when prompted

```
    ssh-keygen
```

Now copy these keys to each raspberry pi so that ansible can
access nodes without credentials

```
    ssh-copy-id pi@rpi-k8s-01
    ssh-copy-id pi@rpi-k8s-02
    ssh-copy-id pi@rpi-k8s-03
```

# Configure ansible inventory

```
    [master]
    rpi-k8s-01  ansible_host=192.168.1.36  ansible_user=pi
    
    [workers]
    rpi-k8s-02  ansible_host=192.168.1.33  ansible_user=pi
    rpi-k8s-03  ansible_host=192.168.1.38  ansible_user=pi
```

# Verify the ansible inventory 

```
    ansible -i inventory all -m ping
```

# Run the ansible playbook

```
    ansible-playbook -i inventory -v site.yaml
```

# Restarting the playbook after a failed task
```
    ansible-playbook -i inventory -v site.yaml --start-at-task="Kubernetes join cluster"
```

# Copy the kubernetes config into the correct location on the host to access the cluster with kubectl
```
    cp /tmp/kubeconfig $HOME/.kube/config
    kubectl get nodes
    NAME         STATUS   ROLES    AGE   VERSION
    rpi-k8s-01   Ready    master   23m   v1.18.1
    rpi-k8s-02   Ready    <none>   22m   v1.18.1
    rpi-k8s-03   Ready    <none>   22m   v1.18.1
```
