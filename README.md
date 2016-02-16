# RubyNotes

###Installing Ruby with RVM
Refer to [RVM install](https://rvm.io/rvm/install)

* Installing the stable release of RVM

```$ \curl -sSL https://get.rvm.io | bash -s stable --ruby```

* Installing a specific version of RVM

```$ \curl -sSL https://get.rvm.io | bash -s -- --version latest```

Prefix the 'bash' portion with 'sudo', of course, if you wish to apply this to a Multi_user Install.

Refer to [RVM Multi-User Installations](https://rvm.io/support/troubleshooting#sudo)

Multi-User Install Location: /usr/local/rvm

Single-User Install Location: ~/.rvm/

* Upgrading RVM

```$ rvm get stable```

###RVM Commands
* List Ruby interpreters available for installation.

```$ rvm list known```

* Install a version of Ruby (e.g. 2.1.1) and verify installation

```
$ rvm use 2.1.1
$ ruby -v
ruby 2.1.1p76 (2014-02-24 revision 45161) [x86_64-darwin12.0]
$ which ruby
/Users/me/.rvm/rubies/ruby-2.1.1/bin/ruby
```

* Use newly installed Ruby version

```$ rvm use 2.1```

* Set a version of Ruby as the default

```$ rvm use 2.1 --default```



NOTE: If you use just the Major.Minor version numbers, RVM checks for, and uses, 
what is the most current patchlevel in its $rvm_path/config/db for that Major.Minor version. 
For example, if you only have 2.0.0-p451 installed, and 2.0.0-p481 is the lastest, 
RVM will attempt to download, install, and set 2.0.0-p481 as your default 2.0.0 for you. 
This behaviour may, or may not, be what you want. 
If its not, then be sure to include the patch level when you specify which Ruby RVM should 
use when setting a default. RVM updates the known list ($rvm_path/config/db 
and $rvm_path/config/known) everytime you update RVM, so the 'current' version of a 
Major.Minor (e.g. 2.1.2 at the time of writing) can change on you. 
RVM does not go by what you have installed for this reason. It goes by what is 
maintained in those two files.

NOTE: Ruby switched to a Semantic Versioning scheme as of Ruby 2.1.1, which might 
affect your use of rvm to manage it.
[Ruby Semantic Versioning](https://www.ruby-lang.org/en/news/2013/12/21/ruby-version-policy-changes-with-2-1-0/)

* Uninstall RVM installed 2.0.0 version

```$ rvm uninstall 2.0..0```

* List Ruby interpreters you have already installed

```$ rvm list```

* Ruby information for the current shell

```$ rvm info```

* Switch to gems directory for current Ruby

```
$ rvm gemdir
/Users/me/.gem/ruby/2.1.1
```

* Switch to the system gems directory

```
$ rvm gemdir system
/Library/Ruby/Gems/2.0.0
```

* Use the Ruby version that came with OS (as if no rvm)

```
$ rvm use system
$ ruby -v
ruby 2.0.0p451 (2014-02-24 revision) [universal.x86_64-darwin]
$ which ruby
/usr/bin/ruby
```

```which``` tells the current shell to return the location of Ruby. In this case, you can see that RVM 
has, in fact, 'hidden' itself from the system, and given you access back to the system 
installed Ruby. Hidden does not mean RVM is gone, instead, what is done is the system 
environment and related variables are set back to what the system Ruby would expect them 
to be as if RVM were not installed at all.
Executing another ```$ rvm use default``` will return RVM to service and load whatever Ruby
you have set as default. You can see what the ruby default is at anytime ```$ rvm alias show default```

PLEASE NOTE: RVM is "hands off" any system ruby that you have installed. To be able to "use" your system ruby you 
can tell RVM to undo the environment changes that it has applied, as follows.


NOTE: RVM, DOES NOT control the system Ruby or its gems. Only rubies and gems installed by RVM
are under its control.

* Install gems from system gem dir (OSX: /Library/Ruby/Gems/2.0.0) using current ruby

```
$ rvm system
$ rvm gemset export system.gems
$ rvm 2.0.0
$ rvm gemset import system
```


