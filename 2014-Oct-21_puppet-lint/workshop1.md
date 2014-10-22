Workshop 1
==========

Here we will generate a puppet module


## Create Your First Module

###Namespaces
Now we will make a module to modify the /etc/motd file
 

There are two types of modules. Base modules and wrapper modules. Typically I name my base modules like so:

    devops/motd
    
 And I name the wrapper modules like so: 
 
     devops/ac_motd
   
devops is the name of the department I work in.   
`ac` is short for 'Adaptive Computing', the name of my company. 

You can [learn more about wrapper modules here:](http://garylarizza.com/blog/2014/02/17/puppet-workflow-part-2/)

In this example, I'll pretend my name is bob and will use my own name for the namespace


    cd ~
    puppet module generate bob/motd
    
Fill in the questions that you are prompted
   
   Once that has finished, you should have the following directory tree: 
   
   ```
   tree bob-motd/
bob-motd/
├── Gemfile
├── README.md
├── Rakefile
├── manifests
│   └── init.pp
├── metadata.json
├── spec
│   ├── classes
│   │   └── init_spec.rb
│   └── spec_helper.rb
└── tests
    └── init.pp
```

Because of bug [PUP-3124](https://tickets.puppetlabs.com/browse/PUP-3124), I usually rename the directory. It prevents harmless warnings from poping up later. 

    mv bob-motd  motd
    
    
Congradulations, you have just made your first module, give yourself a pat on the back!
 
 
## Manifests
 
 Edit the file called `~/motd/manifests/init.pp`. This is where puppet looks first for puppet code. 
 
 Add the following
 
 ```
 class motd {
   file{"/etc/motd":
     ensure => "file", 
     content => template('motd/my_motd.erb'),                                                                                                                                                                                                                                                                                                                         
     mode => '0644',
      backup => '.puppet-back', 
   }
   
 }
 ```
 (You may notice that this code does not conform to puppet-lint standards, we will cover that in workshop 2). 
 
 Create a file in ~/motd/files/my_motd.erb
 
     mkdir -p ~/motd/templates
     touch ~/motd/templates/my_motd.erb
     
 In the ~/motd/my_motd.erb file, add some content. 
 
     You have connected to the following machine
     Hostname:  <%= @hostname %>
     IpAddress:  <%= @ipaddress %>
     
@hostname and @ipaddress are variables known as facter facts. 

You can [learn more about facter here](http://puppetlabs.com/facter)

## Puppet apply

Validate your code, and run it. 

    puppet parser validate ~/motd/manifests/init.pp ; echo $?
    0

If puppet parser validate exits with code 0 (and doesn't print any warnings, then your manifest is good). 

Apply you code in simulation mode (--noop)

    sudo puppet apply -e  'include motd'  --modulepath=~ --debug --noop

If everything worked, remove the --noop to apply your changes

    sudo puppet apply -e  'include motd'  --modulepath=~ --debug
    
    
Congratulations, you have now managed a file with puppet!

You can verify this by typing in the following command

    cat /etc/motd

If you get errors, make sure that `--modulepath` is pointing to the directory where the motd folder is located. 
You should be able to run the following command and see 1 module (your motd module)

    puppet module list --modulepath=~

In the next workshop, we will learn how to improve this module with puppet-lint
