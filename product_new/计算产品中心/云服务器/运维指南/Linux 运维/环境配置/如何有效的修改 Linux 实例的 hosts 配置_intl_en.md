## Scenario

From March 1st, 2018, the Linux public image provided by Tencent Cloud has the fully open-source tool Cloud-Init pre-installed, implementing all the initialization operations of an instance through Cloud-Init, making the internal operations of the entire instance more transparent. For more information, see Cloud-Init.
In **each launch**, Cloud-Init generates a new `/etc/hosts` file according to the `/etc/cloud/templates/hosts.${os_type}.tmpl` template and overwrite the original `/etc/hosts` file of instance. After the user has manually modified the internal `/etc/hosts` configuration of the instance, and restarted the instance, the `/etc/hosts` configuration becomes the original default configuration.

##  Prerequisites
Tencent Cloud has fixed this problem for instance created after September, 2018, the `/etc/hosts` configuration will not be overwritten.
For instance created **before September, 2018**, please modify the as instructed below.

## Steps

### Solution 1 
1. Log into the Linux CVM.
2. Execute the following command to change the `- update_etc_hosts` in the `/etc/cloud/cloud.cfg` configuration file to `- ['update-etc-hosts', 'once-per-instance']`.
```
sed -i "/update_etc_hosts/c \ - ['update_etc_hosts', 'once-per-instance']" /etc/cloud/cloud.cfg
```
3. Execute the following command to create a `config_update_etc_hosts` file under the `/var/lib/cloud/instance/sem/` path.
```
touch /var/lib/cloud/instance/sem/config_update_etc_hosts
```

### Solution 2
>This solution takes the CentOS 7.2 operating system as an example.
>
#### Obtaining `hosts` Template File Path
1. Log into the Linux CVM.
2. Execute the following command to view the system `hosts` template file.
```
cat /etc/hosts
```
The `hosts` template file is as shown in the following figure:
![](https://main.qcloudimg.com/raw/f51f9c53004574f72d32f5ed790c8563.png)


#### Modifying the `hosts` Template File
>Taking adding `127.0.0.1 test test` as an example, you can modify the `hosts` template and `/etc/hosts` file as needed.
>
1. Execute the following command to modify the `hosts` template file.
```
vim /etc/cloud/templates/hosts.redhat.tmpl
```
2. Press **i** or **Insert** to switch to editing mode.
3. Add the following content to the end of the file.
```
127.0.0.1 test test
```
4. Press **Esc**, enter **:wq**, save the file and return.

#### Modifying the /etc/hosts File
1. Execute the following command to modify the `/etc/hosts` file.
```
vim /etc/hosts
```
2. Press **i** or **Insert** to switch to editing mode.
3. Add the following content to the end of the file.
```
127.0.0.1 test test
```
4. Press **Esc**, enter **:wq**, save the file and return.
