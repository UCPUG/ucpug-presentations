Puppet-lint
===========

In workshop 1, we made a puppet module. Now we will improve upon the previous module with puppet-lint

    gem install puppet-lint
    

Run puppet-lint against your manifest, it should report some errors. 

```
puppet-lint /tmp/motd/manifests/*.pp
WARNING: double quoted string containing no variables on line 35
WARNING: double quoted string containing no variables on line 36
ERROR: trailing whitespace found on line 36
WARNING: line has more than 80 characters on line 10
WARNING: line has more than 80 characters on line 17
ERROR: two-space soft tabs not used on line 35
ERROR: two-space soft tabs not used on line 36
ERROR: two-space soft tabs not used on line 37
ERROR: two-space soft tabs not used on line 38
ERROR: two-space soft tabs not used on line 39
WARNING: indentation of => is not properly aligned on line 36
WARNING: indentation of => is not properly aligned on line 38
```

We can fix some of these errors, by using the --fix option. 

The manifest before

```
 class motd {
   file{"/etc/motd":
     ensure => "file", 
     content => '/tmp/my_motd.erb',
     mode => '0644',
   }
 }
 ```
 
     puppet lint --fix /tmp/motd/manifests/*.pp
     

The manifest after

```
class motd {

   file{'/etc/motd':
     ensure  => 'file',
     content => '/tmp/my_motd.erb',
     mode    => '0644',
   }
}
```

Congraulations, you are now a puppet-lint master. 

You may want to ignore a few of the more dumb checks. 

```
cat ~/.puppet-lint.rc i
--no-80chars-check 
```

Or simply from the command line

    puppet-lint --no-80chars-check *.pp