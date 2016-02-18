# RVM (Ruby Version Manager)

###Installing RVM
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

***NOTE:*** To avoid installing the documentation of every gem you install, create a file 
called ```/etc/gemrc``` with this content ```gem: --no-document```  From that point on,
every time you install a new gem, it will install without documentation.


***NOTE:*** You cannot share gems between ruby-versions


###Gemsets

A Gemset is a container you can use to keep gems separate from each other.
The idea is to create a gemset per project to allow changing gems (amd gem versions) for
one project without breaking all of your other projects.
Eash project need only worry about its own gems. This is a good idea and the wait time
for installing large gems like rails is usually worth it.

When you install a new ruby, RVM will create 2 gemsets
- default gemset (empty)
- global gemset

it also uses a set of user-editable files to determine which gems to install.

When working in ```~/.rvm/gemsets``` 

rvm searches for global.gems and default.gems using a tree-hierachy based on the 
ruby string being installed. 
Using the example of ree-1.8.7-p2010.02, rvm will check (and import from) the following files:

```
- ~/.rvm/gemsets/ree/1.8.7/p2010.02/global.gems
- ~/.rvm/gemsets/ree/1.8.7/p2010.02/default.gems
- ~/.rvm/gemsets/ree/1.8.7/global.gems
- ~/.rvm/gemsets/ree/1.8.7/default.gems
- ~/.rvm/gemsets/ree/global.gems
- ~/.rvm/gemsets/ree/default.gems
- ~/.rvm/gemsets/global.gems
- ~/.rvm/gemsets/default.gems
```

For example, if you edited ```~/.rvm/gemsets/global.gems``` by adding these two lines:

```
bundler
awesome_print
```
every time you install a new ruby, these two gems are installed into your global gemset.
Using the default or global gemsets, you can also make RVM include a specific version of 
a given gem. Here's how:

```
bundler -v~>1.0.0
awesome_print
hirb -v0.4.5
```

In short: edit a file called ~/.rvm/gemsets/global.gems to contain the list of gems you 
want to be there for each ruby-version.

***Warning***

default.gems and global.gems files are usually overwritten during update of rvm (rvm get ...).

It is however possible to override this behavior by either using ***after_install*** hook or 
overriding with ***--with-default-gems/--with-gems*** flags during install / upgrade.


###Creating Gemsets

Gemsets must be created before being used. 
To create a new gemset for the current ruby, do this:

```
$ rvm 2.1.1
$ rvm gemset create teddy
Gemset 'teddy' created.
```

You can also create multiple gemsets in a single command.

```
$ rvm 2.1.1
$ rvm gemset create teddy rosie
Gemset 'teddy' created.
Gemset 'rosie' created.
```

Alternatively, if you prefer the shorthand syntax offered by rvm use, 
employ the --create option like so:

```
$ rvm use 2.1.1@teddy --create
```

If you would like to create gemsets automatically when used, export this flag in 
your ```~/.rvmrc``` or ```/etc/rvmrc``` file:

```
$ rvm_gemset_create_on_use_flag=1
```

---

With the latest RVM version (1.17.0 and newer) just type:

```
$ rvm @global do gem install passenger
```

or

```
$ rvm use <ruby version>@global --create
```

or

```
$ rvm 1.9.3@global do gem install passenger 
```

if you need it only for a specific version of ruby.

Install gems you want to share between gemsets:

```
$ bundle install <gem name>
```

but these gems can only be shared between gemsets of the ***same ruby version***.


####Interpreter global gemsets

RVM provides (>= 0.1.8) a @global gemset per ruby interpreter.

Gems you install to the ```@global``` gemset for a given ruby are available to all 
other gemsets you create in association with that ruby.

This is a good way to allow all of your projects to share the same installed gem for 
a specific ruby interpreter installation.

To install a gem in the global gemset
```
$ rvm @global do gem install ...
```
###Resources
- [RVM](https://rvm.io/)
- [RVM Gemsets](https://rvm.io/gemsets/)
