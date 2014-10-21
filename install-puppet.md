##Install puppet
If you don’t already have puppet 3 installed

#### Fedora / Cent
```
sudo rpm -Uvh http://yum.puppetlabs.com/el/6/products/i386/puppetlabs-release-6-7.noarch.rpm
yum install puppet
```
 
#### Ubuntu / Debian

 ```
 cd /tmp
 wget http://apt.puppetlabs.com/puppetlabs-release-precise.deb
 sudo dpkg -i puppetlabs-release-precise.deb
 apt-get update 
 apt-get install -y puppet-common 
 ```

#### OpenSuse / SLES

```
gem install facter
gem install puppet
```

#### Windows

Windows environments are very different, we won't cover how to use puppet with windows for now. 

[Additional information](https://docs.puppetlabs.com/guides/install_puppet/install_windows.html)