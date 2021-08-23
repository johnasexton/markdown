# chef, ruby, gem & bundler

### Chef Cookbook Authoring with Ruby
* all info is from the: [Chef Ruby Guide](https://docs.chef.io/ruby.html)
* This doc is me rewriting it for myself in markdown format as a way to
ingrain the lesson in my own head.
<br/>
check the syntax of a Ruby file
```
ruby -c my_cookbook_file.rb
```

##### Foodcritic Linting
All cookbooks should pass Foodcritic rules before being uploaded.

```
foodcritic -f all your-cookbook
```

##### this is a variable assignment
```
x = 1
```
##### this is how math works
```
1 + 2           # => 3
2 * 7           # => 14
5 / 2           # => 2   (because both arguments are whole numbers)
5 / 2.0         # => 2.5 (because one of the numbers had a decimal place)
1 + (2 * 3)     # => 7   (you can use parens to group expressions)
```
##### use of strings
```
'single quoted'   # => "single quoted"
"double quoted"   # => "double quoted"
'It\'s alive!'    # => "It's alive!" (the \ is an escape character)
'1 + 2 = 5'       # => "1 + 2 = 5" (numbers surrounded by quotes behave like strings)
```
##### convert case
```
node['hostname'].downcase    # => "foo"
node['hostname'].upcase      # => "FOO"
```
##### embed Ruby in a string
```
x = 'Bob'
"Hi, #{x}"      # => "Hi, Bob"
'Hello, #{x}'   # => "Hello, \#{x}" Notice that single quotes don't work with #{}
```
##### escape character is "\" just like shell scripting
```
'It\'s alive!'                        # => "It's alive!"
"Won\'t you read Grant\'s book?"      # => "Won't you read Grant's book?"
```
##### string interpolation
```
When strings have quotes within quotes, use double quotes `(" ")` on the
# outer quotes, and then single quotes `(' ')` for the inner quotes.
```
##### For example:
```
Chef::Log.info("Loaded from aws[#{aws['id']}]")
"node['mysql']['secretpath']"
"#{ENV['HOME']}/chef.txt"
antarctica_hint = hint?('antarctica')
if antarctica_hint['snow']
  "There are #{antarctica_hint['penguins']} penguins here."
else
  'There is no snow here, and penguins like snow.'
end
```
##### truths
```
true            # => true
false           # => false
nil             # => nil
0               # => true ( the only false values in Ruby are false
                #    and nil; in other words: if it exists in Ruby,
                #    even if it exists as zero, then it is true.)
1 == 1          # => true ( == tests for equality )
1 == true       # => false ( == tests for equality )
```
##### untruths (! means not!):
```
!true           # => false
!false          # => true
!nil            # => true
1 != 2          # => true (1 is not equal to 2)
1 != 1          # => false (1 is not not equal to itself)
```
##### Convert something to either true or false (!! means not not!!):
```
!!true          # => true
!!false         # => false
!!nil           # => false (when pressed, nil is false)
!!0             # => true (zero is NOT false).
```
##### Create lists using arrays
```
x = ['a', 'b', 'c']   # => ["a", "b", "c"]
x[0]                  # => "a" (zero is the first index)
x.first               # => "a" (see?)
x.last                # => "c"
x + ['d']             # => ["a", "b", "c", "d"]
x                     # => ["a", "b", "c"] ( x is unchanged)
x = x + ['d']         # => ["a", "b", "c", "d"]
x                     # => ["a", "b", "c", "d"]
```
##### Whitespace arrays
The %w syntax is a Ruby shortcut for creating an array without
requiring quotes and commas around the elements.

__For example:__
```
if %w{debian ubuntu}.include?(node['platform'])
  # do debian/ubuntu things with the Ruby array %w{} shortcut
end
```
When %w syntax uses a variable, such as |foo|, double quoted strings should be used.

__Right:__
```
%w{openssl.cnf pkitool vars Rakefile}.each do |foo|
  template "/etc/openvpn/easy-rsa/#{foo}" do
    source "#{foo}.erb"
    ...
  end
end
```
__Wrong:__
```
%w{openssl.cnf pkitool vars Rakefile}.each do |foo|
  template '/etc/openvpn/easy-rsa/#{foo}' do
    source '#{foo}.erb'
    ...
  end
end
```
#### Hashes
A Hash is a list with keys and values.
Sometimes they don't have a set order:
```
h = {
  'first_name' => "Bob",
  'last_name'  => "Jones"
}
```
##### And sometimes they do.
For example, first name then last name:
```
h.keys              # => ["first_name", "last_name"]
h['first_name']     # => "Bob"
h['last_name']      # => "Jones"
h['age'] = 23
h.keys              # => ["first_name", "age", "last_name"]
h.values            # => ["Jones", "Bob", 23]
```
##### Perl style Regular Expressions (RegEx)
```
'I believe'  =~ /I/                       # => 0 (matches at the first character)
'I believe'  =~ /lie/                     # => 4 (matches at the 5th character)
'I am human' =~ /bacon/                   # => nil (no match - bacon comes from pigs)
'I am human' !~ /bacon/                   # => true (correct, no bacon here)
/give me a ([0-9]+)/ =~ 'give me a 7'     # => 0 (matched)
```
##### Conditional logic
Use conditions! For example, an if statement
```
if false
  # this won't happen
elsif nil
  # this won't either
else
  # code here will run though
end
```
##### or a case statement:
```
x = 'dog'
case x
when 'fish'
 # this won't happen
when 'dog', 'cat', 'monkey'
  # this will run
else
  # the else is an optional catch-all
end
```
##### If logic
An if statement can be used to specify part of a recipe to be used when
certain conditions are met. else and elseif statements can be used to
handle situations where either the initial condition is not met or when
there are other possible conditions that can be met. Since this behavior
is 100% Ruby, do this in a recipe the same way here as anywhere else.
For example, using an if statement with the platform node attribute:
```
if node['platform'] == 'ubuntu'
  # do ubuntu things
end
```
##### case statement
A case statement can be used to handle a situation where there are a
lot of conditions. Use the when statement for each condition, as many as are required.
For example, using a case statement with the platform node attribute:
```
case node['platform']
when 'debian', 'ubuntu'
  # do debian/ubuntu things
when 'redhat', 'centos', 'fedora'
  # do redhat/centos/fedora things
end
```
##### Methods
Call a method on something with .method_name():
```
x = 'My String'
x.split(' ')            # => ["My", "String"]
x.split(' ').join(', ') # => "My, String"
```
##### Define a Method
Define a method (or a function, if you like):
```
def do_something_useless( first_argument, second_argument)
  puts "You gave me #{first_argument} and #{second_argument}"
end

do_something_useless( 'apple', 'banana')
# => "You gave me apple and banana"
do_something_useless 1, 2
# => "You gave me 1 and 2"
# see how the parens are optional if there's no confusion about what to do
```
##### Ruby Class
```
execute 'apt-get-update' do
  command 'apt-get update'
  ignore_failure true
  only_if { apt_installed? }
  not_if { File.exist?('/var/lib/apt/periodic/update-success-stamp') }
end
```
##### Include a Class
In non-Chef Ruby, the syntax is include (without the : prefix), but  without the : prefix the chef-client will try to find a provider named include. Using the : prefix tells the chef-client to look for the specified class that follows.
Use :include to include another Ruby class. For example:
```
::Chef::Recipe.send(:include, Opscode::OpenSSL::Password)
```
##### Include a Parameter
The include? method can be used to ensure that a specific parameter is
included before an action is taken. For example, using the include? method
to find a specific parameter:
```
if ['debian', 'ubuntu'].include?(node['platform'])
  # do debian/ubuntu things
end

or:

if %w{rhel}.include?(node['platform_family'])
  # do RHEL things
end
```
##### Log Entries

Chef::Log extends Mixlib::Log and will print log entries to the default logger that is configured for the machine on which the chef-client is running. (To create a log entry that is built into the resource collection, use the log resource instead of Chef::Log.)The following log levels are supported:

__Log Level Syntax__
```
Debug Chef::Log.debug('string')
Error Chef::Log.error('string')
Fatal Chef::Log.fatal('string')
Info Chef::Log.info('string')
Warn Chef::Log.warn('string'
```
##### git Etiquette
Although not strictly a Chef style thing, please always ensure your * user.name and user.email are set properly in your .gitconfig file. user.name should be your given name (e.g., Julian Dunn) user.email should be an actual, working e-mail address. This will prevent commit log entries similar to "guestuser <login@Bobs-Macbook-Pro.local>", which are unhelpful.

##### Use of Hyphens
* Cookbook and custom resource names should contain only alphanumeric characters.
* A hyphen (-) is a valid character and may be used in cookbook and custom
* resource names, but it is discouraged. The chef-client will return an error
* if a hyphen is not converted to an underscore "(_)" when referencing from a recipe
* the name of a custom resource in which a hyphen is located.

##### Cookbook Naming
Use a short organizational prefix for application cookbooks that are part of your organization. For example, if your organization is named SecondMarket, use sm as a prefix:
```
sm_postgresql
# or
sm_httpd.
```
##### Cookbook Versioning
* Use semantic versioning when numbering cookbooks.
* Only upload stable cookbooks from master.
* Only upload unstable cookbooks from the dev branch. Merge to master and bump the version when stable.
* Always update CHANGELOG.md with any changes, with the JIRA ticket and a brief description.

##### Cookbook Patterns
Good cookbook examples:
* https://github.com/chef-cookbooks/yum
* https://github.com/chef-cookbooks/mysql
* https://github.com/chef-cookbooks/httpd
* https://github.com/chef-cookbooks/php
* https://github.com/someara/wordpress-uberdemo
* https://github.com/jtimberman/smartmontools-cookbook/

##### Naming
Name things uniformly for their system and component. For example:
* attributes: node['foo']['bar']
* recipe: foo::bar
* role: foo-bar
* directories: foo/bar (if specific to component), foo (if not). For example: /var/log/foo/bar.
* Name attributes after the recipe in which they are primarily used. e.g. node['postgresql']['server'].

##### Parameter Order
Follow this order for information in each resource declaration:
* Source
* Cookbook
* Resource ownership
* Permissions
* Notifications
* Action


__For example:__
```
template '/tmp/foobar.txt' do
  source 'foobar.txt.erb'
  owner  'someuser'
  group  'somegroup'
  mode   '0644'
  variables(
    :foo => 'bar'
  )
  notifies :reload, 'service[whatever]'
  action :create
end
```
##### File Modes
Always specify the file mode with a quoted 3-5 character string that defines the octal mode:
```
mode '755'
mode '0755'
mode '00755'
```
##### Specify Resource Action?
A resource declaration does not require the action to be specified because the chef-client will apply the default action for a resource automatically if it's not specified within the resource block. For example:
```
package 'monit'
```
will install the monit package because the :install action is the default action for the package resource. However, if readability of code is desired, such as ensuring that a reader understands what the default action is for a custom resource or stating the action for a resource whose default may not be immediately obvious to the reader, specifying the default action is recommended:
```
ohai 'apache_modules' do
  action :reload
end
```
##### Symbols or Strings?
Prefer strings over symbols, because they�re easier to read and you don�t need to explain to non-Rubyists what a symbol is. Please retrofit old cookbooks as you come across them. ##### Right:
```
default['foo']['bar'] = 'baz'
```
##### Wrong:
```
default[:foo][:bar] = 'baz'
```
##### String Quoting
Use single-quoted strings in all situations where the string doesn't need interpolation.

##### Whitespace Arrays
When %w syntax uses a variable, such as |foo|, double quoted strings should be used.
__Right:__
```
%w{openssl.cnf pkitool vars Rakefile}.each do |foo|
  template "/etc/openvpn/easy-rsa/#{foo}" do
    source "#{foo}.erb"
    ...
  end
end
```
__Wrong:__
```
%w{openssl.cnf pkitool vars Rakefile}.each do |foo|
  template '/etc/openvpn/easy-rsa/#{foo}' do
    source '#{foo}.erb'
    ...
  end
end
```
##### Shelling Out
Always use mixlib-shellout to shell out. Never use backticks, Process.spawn, popen4, or anything else! The mixlib-shellout module provides a simplified interface to shelling out while still collecting both standard out and standard error and providing full control over environment, working directory, uid, gid, etc. Starting with chef-client version 12.0 you can use the shell_out, shell_out! and shell_out_with_system_locale Recipe DSL methods to interface directly with mixlib-shellout.

##### Constructs to Avoid
Avoid the following patterns:
* node.set / normal_attributes - Avoid using attributes at normal precedence since they are set directly on the node object itself, rather than implied (computed) at runtime.
* node.set_unless - Can lead to weird behavior if the node object had something set. Avoid unless altogether necessary (one example where it's necessary is in:
```
node['postgresql']['server']['password'])
```
* if node.run_list.include?('foo') i.e. branching in recipes based on what's in the node's run-list. Better and more readable to use a feature flag and set its precedence appropriately.
* node['foo']['bar'] i.e. setting normal attributes without specifying precedence. This is deprecated in Chef 11, so either use node.set['foo']['bar'] to replace its precedence in-place or choose the precedence to suit.

##### Patterns to Avoid
This section covers things that should be avoided when authoring cookbooks and recipes.
__node.set__
Use node.default (or maybe node.override) instead of node.set because node.set is an alias for node.normal. Normal data is persisted on the node object. Therefore, using node.set will persist data in the node object. If the code that uses node.set is later removed, if that data has already been set on the node, it will remain. Normal and override attributes are cleared at the start of the chef-client run, and are then  rebuilt as part of the run based on the code in the cookbooks and recipes at that time. node.set (and node.normal) should only be used to do something like generate a password for a  database on the first chef-client run, after which it's remembered (instead of persisted). Even this case should be avoided, as using a data bag is the recommended way to store this type of data.

##### More about Ruby
To learn more about Ruby, see the following:
* http://www.ruby-lang.org/en/documentation/
* http://blog.loftninjas.org/2011/02/16/the-power-of-chef-and-ruby/
* http://www.codecademy.com/tracks/ruby
* http://www.ruby-doc.org/stdlib/


=======================================
Test Driven Chef architecture:
* http://engineering.skybettingandgaming.com/2016/03/01/delivery-pipelines-kitchen-chef-docker/
```
event-service/
.kitchen.yml                <- Test Kitchen configuration. Container setup, Chef run lists
Berksfile                   <- Berkshelf for Chef cookbook version management, pulling in common functionality
workflow.groovy             <- Jenkins Pipeline job definition
+---event-service/              <- Node.js application
+---dockerfiles/                <- Dockerfiles for creating * basic containers from images
+---chef/
    +---cookbooks/
       +---event-service/
          +---recipes/
             lint.rb         <- CI Lint stage definition
             test.rb         <- CI Test stage definition
             build.rb        <- CI Build stage definition
             vendor.rb       <- CI Vendor stage definition
             deploy.rb       <- Application release recipe
```

# CHEF ESSENTIALS

### ABOUT ME
* John Sexton
* Lead DevOps Engineer
* previous job: Verizon SysAdmin -> DataCenter Mgmt -> Enterprise Architect -> DevOps Engineer
* experience with Chef
* text editors: vi/vim on *nix and Atom.io on MacOS, Windows and Linux guis.

### COURSE OBJECTIVES
* use Chef resources to define the state of your system
* write and use Chef recipes and cookbooks
* automate testing of cookbooks (TestKitchen)
* manage multiple nodes with chef server
* create organizations
* bootstrap nodes (bring a node under management of a Chef server)
* assign roles to nodes (apply the same logic to a group of nodes)
* deploy nodes to environments

### DAY 1
* getting a workstation
* using resources
* building cookbooks
* testing with test kitchen
* details about a system
* desired state and data
* local workstation installation

### DAY2
* connecting to a Chef server
* desired state and search
* roles
* environments

### GETTING STARTED
* Chef is a large set of tools used on multiple platforms and numerous configurations
* learning Chef requires practicing Chef
* Centos 6.7 image for starters
* connect your laptop to a VM
* workstation -> Chef server -> nodes

### MY VM
```
* ssh 54.175.129.31 -l chef
* ssh chef/chef@54.175.129.31
```
### VM IMAGE CONTAINS
* Centos 6.7
* ChefDK
* Docker
* kitchen docker gem

### DEFINITIONS
* chef-client command executes recipes on a node
* resource is a statement of configuration policy (vast and vague definition on purpose)
* node = an instance of a "machine"

### FIRST RECIPE
```bash
vi moo.rb
```
```ruby
package 'cowsay' do
        action :install
end
```
":install" is the default action for the package "cowsay"

since install is the default action we could strip it down to:
```ruby
package 'cowsay'
```
nest recipes properly. use 2 spaces NOT a tab
apply the recipe using chef-client

### Command switch to run in local or "Chef Zero" mode
chef-client --local-mode (or -z to denote "chef-zero")
```bash
sudo chef-client -z moo.rb
```
it chose "yum" as the package manager, which is the default package mgr for CentOS
on Ubuntu it would default to apt-get

to test cowsay
```bash
which cowsay
cowsay will moo for food
```
if you rerun chef-client will check the current state against the desired state and will only reinstall if it is missing

### HELLO WORLD EXERCISE
```bash
vi hello.rb
```
```ruby
file '/hello.txt' do
  content 'Hello, World!'
end
```
### execute the recipe
```bash
sudo chef-client -z hello.rb
[2016-12-14T15:32:25+00:00] WARN: No config file found or specified on command line, using command line options.
[2016-12-14T15:32:25+00:00] WARN: No cookbooks directory found at or above current directory.  Assuming /home/chef.
Starting Chef Client, version 12.7.2
resolving cookbooks for run list: []
Synchronizing Cookbooks:
Compiling Cookbooks...
[2016-12-14T15:32:28+00:00] WARN: Node ip-172-31-1-107.ec2.internal has an empty run list.
Converging 1 resources
Recipe: @recipe_files::/home/chef/hello.rb
  * file[/hello.txt] action create
    - create new file /hello.txt
    - update content in file /hello.txt from none to dffd60
    --- /hello.txt      2016-12-14 15:32:28.962125228 +0000
    +++ /.hello.txt20161214-7110-lttdpm 2016-12-14 15:32:28.961125228 +0000
    @@ -1 +1,2 @@
    +Hello, World!
Running handlers:
Running handlers complete
Chef Client finished, 1/1 resources updated in 03 seconds
```
* default action of "file" is create
* if you define mode chef will check it, and if not it won't care
* same goes for anything like version or owner or group or whatever
* add those now:
```ruby
file '/hello.txt' do
  content 'Hello, World!'
  mode '0644'
  owner 'root'
  group 'root'
  action :create
end
```
### WORKSTATION SETUP
```bash
vi setup.rb
```
```ruby
package 'tree'
file '/etc/motd' do
  content 'Property of John Sexton'
end
```
### RESOURCES REFERENCE
https://docs.chef.io/resources.html

### COOKBOOKS
* read the README.md first
* read the metadata.rb second
* the "chef" command can be used to generate a new blank cookbook
* your source code should be stored in source control like Git
* Git = a contemptable person (stupid simple)

Now modify the recipe to add the Git package like
```ruby
package 'git'
package 'tree'
file '/etc/motd' do
  content 'Property of John Sexton'
end
```
Then reapply your recipe
```bash
sudo chef-client -z setup.rb
```
Cookbook definition = defines a scenario such as everything needed to install and configure MySQL and then it contains all components required to support that scenario

### Generate a new default Cookbook
```bash
mkdir cookbooks
chef generate cookbook cookbooks/workstation
```

check the directory structure
```bash
cd cookbooks/workstation
tree
.
├── Berksfile
├── chefignore
├── metadata.rb
├── README.md
├── recipes
│   └── default.rb
├── spec
│   ├── spec_helper.rb
│   └── unit
│       └── recipes
│           └── default_spec.rb
└── test
    └── integration
        ├── default
        │   └── serverspec
        │       └── default_spec.rb
        └── helpers
            └── serverspec
                └── spec_helper.rb

10 directories, 9 files
```
### workstation cookbook

```bash
vi metadata.rb
```
```ruby
TODO: Enter the cookbook description here.
name             'workstation'
maintainer      'John Sexton'
maintainer_email        'john.a.sexton@macys.com'
license         'all rights'
description     'build a workstation'
version         '0.1.0'
```
move your setup.rb to the recipes subdir
```bash
mv setup.rb cookbooks/workstation/recipes/
```
### stage your git Files
```bash
git add .
git status
git commit -m "add workstation cookbook"
git status
```
Generate a new recipe
```bash
chef generate recipe server.rb
```
### Running Multiple recipes
use the run list
sudo chef-client --local-mode -r "recipe[workstation::setup]"
```bash
sudo chef-client -z -r 'recipe[workstation::setup],recipe[apache::server]'
```
### include_recipe and default.rb
```bash
include_recipe within another recipe will be used constantly
```
create a default.rb in workstation and include the apache cookbook. Also create a similar default.rb in the apache cookbook

```ruby
include_recipe 'workstation::setup'
include_recipe 'apache::setup'
```

now with default.rb you can drop the recipe designation after the cookbook
```bash
sudo chef-client -z -r 'recipe[workstation::setup],recipe[apache::server]'
```

### Check Syntax of recipe with ruby
```bash
ruby -c apache/recipes/server.rb
Syntax OK
ruby -c workstation/recipes/setup.rb
Syntax OK
```
### TestKitchen
* Definition "converge" = compile and execute tests
* BDD is outside in testing
* TDD is inside out testing

if your cookbook does not contain a .kitchen.yaml run

```ruby
kitchen init
```

```ruby
driver:
  name: vagrant
driver:
  name: Docker
platforms:
  - name: ubuntu-14.04
  - name: CentOS-6.7
suites:
- name: default
  run_list:
    - recipe[workstation::default]
  attributes:
```

kitchen list will blow up if anything in .kitchen.yml does not exist

change vagrant to docker and change centos-7.1 to centos6.7 like this:

```ruby
driver:
  name: docker

provisioner:
  name: chef_zero

# Uncomment the following verifier to leverage Inspec instead of Busser (the
# default verifier)
# verifier:
#   name: inspec

platforms:
  # - name: ubuntu-14.04
  - name: centos-6.7

suites:
  - name: default
    run_list:
      - recipe[workstation::default]
    attributes:
```
* kitchen create (stand up the cookbook)
* kitchen converge (run all the tests)
* kitchen verify (fire off our user created tests)
* kitchen destroy (should be run whenever you want to destroy the running instance and you want to change the .kitchen.yml when your instance is already running)
* what is tested? syntax errors and whether or not it can be applied to the target system
what is not tested? if our cookbook is meeting our requirements or not. that is why  we write our own tests
* kitchen converge and kitchen verify will run faster but not catch as much as kitchen test
* kitchen test = kitchen destroy, kitchen converge, kitchen verify, kitchen destroy

### ServerSpec
http://serverspec.org/resource_types.html
(resource link shows all testable components)
is an extension of Ruby's rspec that tests your servers' actual state by executing the command locally, via SSH, via WinRM, via Docker API and so on...

EXAMPLE
```ruby
require 'spec_helper'

describe 'workstation::default' do
    it { should be_installed }
  end

end
```
##### Where do tests live?

<cookbook>/test/integration/default/serverspec/default_spec.rb

### OHai
OHai deals with things pertaining to the "node" object

* kitchen test (new intances)
* kitchen verify (existing instances)
* Ohai is close to realtime

ohai with no switches displays all node attributes in JSON format from the node object.
```bash
ohai
```
you can write your own ohai plugins to extend the node attributes it gathers.
You must use double quotes "" in order to use Ruby string interpolation single quotes '' are string literal
NOTE: Ruby does not like dashes. Replace them with underscores word_other_word

### PACKAGE RESOURCES
```ruby
package 'git'
package 'tree'
file '/etc/motd' do
  content "Property of ...

  IPADDRESS: #{node['ipaddress']}
  HOSTNAME: #{node['hostname']}
  MEMORY: #{node['memory']['total']}
  CPU: #{node['cpu']['0']['mhz']}
"
end
```
### TEMPLATES & DESIRED STATE
https://docs.chef.io/resource_template.html
* you must use a template file in the recipes
* template must be defined in the templates folder
* remote_file - resource uses URL to pull remote reference


This goes in your recipe file to reference it
```ruby
template '/var/www/index.html' do
  source 'index.html.erb'
end
```
and your index.html.erb file should look something like this:
```ruby
<h2>ipaddress: <%= node['ipaddress'] %></h2>
<h2>hostname: <%= node['hostname'] %></h2>
<h2>memory: <%= node['memory'] %></h2>
MEMORY: #{node['memory']['total']}
CPU: #{node['cpu']['0']['mhz']}
```

```ruby
Property of ...
ipaddress: <%= node['ipaddress'] %>
hostname: <%= node['hostname'] %>
memory: <%= node['memory']['total'] %>
cpu: <%= node['cpu']['0']['mhz]'] %>

MEMORY: #{node['memory']['total']}
CPU: #{node['cpu']['0']['mhz']}
```

```ruby
template '/etc/motd' do
  source 'motd.erb'
end
```
```bash
sudo chef-client -z -r 'recipe[workstation]'
kitchen verify
```
you cannot put ruby logic into a file resource, but you can in a template object
string interpolation takes longer than string literal on compilation

different ruby tags
* <% (opens ruby logic)
* <%= (opens ruby logic and prints)
* %> (adds a new line)
* -%> (subtracts the empty line)

### THE CHEF SERVER
* provision a node
* install chef tools
* transfer cookbook
* Best practice is setup a load balancer to balance across two web server nodes

##### Types of Chef server
* open source
* on premises chef server with support
* multi tenant chef server with support

##### Creating a Chef Organization
* go to https://manage.chef.io
* login
* create organization (or alternatively Administration | Create)
* download starter kit
* extract to your user home directory /Users/b004829/chef-repo
* clone this repo to your cookbooks directory https://github.com/chef-training/chef-essentials-repo

```bash
b004829@L4317621 MINGW64 ~/chef-repo
$ knife client list
ce_2016_dec_15_jas-validator
```
### Berkshelf
Now change directory to cookbooks/apache and do the berks install
```bash
$ berks install
Resolving cookbook dependencies...
Fetching 'apache' from source at .
Fetching cookbook index from https://supermarket.chef.io...
Using apache (0.2.1) from source at .
```
when you want to upload you use a "berks upload"
once you upload to Chef server your version becomes locked in with a *.lock file to protect your existing version

```bash
$ berks install
Resolving cookbook dependencies...
Fetching 'apache' from source at .
Fetching cookbook index from https://supermarket.chef.io...
Using apache (0.2.1) from source at .
```

##### FLOW
```bash
cd <cookbook-dir>
berks install
berks upload
knife cookbook list
```

### BOOTSTRAP A NODE
FORMAT: knife bootstrap FQDN -x USER -P PWD --sudo -N node1

knife bootstrap 54.175.129.31 -x chef -P chef --sudo -N web1

```bash
$ knife bootstrap 54.175.129.31 -x chef -P chef --sudo -N web1
Creating new client for web1
Creating new node for web1
Connecting to 54.175.129.31
54.175.129.31 -----> Existing Chef installation detected
54.175.129.31 Starting the first Chef Client run...
54.175.129.31 Starting Chef Client, version 12.7.2
54.175.129.31 resolving cookbooks for run list: []
54.175.129.31 Synchronizing Cookbooks:
54.175.129.31 Compiling Cookbooks...
54.175.129.31 [2016-12-15T15:55:22+00:00] WARN: Node web1 has an empty run list.
54.175.129.31 Converging 0 resources
54.175.129.31
54.175.129.31 Running handlers:
54.175.129.31 Running handlers complete
54.175.129.31 Chef Client finished, 0/0 resources updated in 03 seconds
```
show your node info
```bash
knife node show web1
knife node run_list add web1 "recipe[apache]"
```
### CHEF COMMUNITY
* for community cookbooks http://supermarket.chef.io
* create a wrapper cookbook
* create a load balancer
* look for cookbooks that support your OS platform and have lots of followers
* use the haproxy cookbook https://supermarket.chef.io/cookbooks/haproxy
* read over the "Attributes"
```bash
[{
    "hostname" => "localhost",
    "ipaddress" => "127.0.0.1",
    "port" => 4000,
    "ssl_port" => 4000
  }, {
    "hostname" => "localhost",
    "ipaddress" => "127.0.0.1",
    "port" => 4001,
    "ssl_port" => 4001
  }]
```
##### CONVENTIONS
* [array]  
* {block of key value pairs or a "hash"}

##### Check the version
Berkshelf/Librarian
cookbook 'haproxy', '~> 2.0.1'
(a major version number has occurred so it has broken backward compatibility)

### WRAP OR FORK A COOKBOOK
* a fork loses the ongoing updates
* this is why we will wrap the cookbook instead
```bash
cd ~/chef-repo
chef generate cookbook cookbooks/myhaproxy
```

the ip of a cloud node will be wrong since cloud nodes do not know their own public real ip
```bash
knife node show web1 -a ipaddress
web1:
  ipaddress: 172.31.1.107
```
use "knife node NODE -a cloud" instead
```bash
$ knife node show web1 -a cloud
web1:
  cloud:
    local_hostname:  ip-172-31-1-107.ec2.internal
    local_ipv4:      172.31.1.107
    private_ips:     172.31.1.107
    provider:        ec2
    public_hostname: ec2-54-175-129-31.compute-1.amazonaws.com
    public_ips:      54.175.129.31
    public_ipv4:     54.175.129.31
```
 edit the recipes/default.rb

 add the Attributes from the haproxy cookbook
 ```ruby
 [{
     "hostname" => "ec2-54-175-129-31.compute-1.amazonaws.com",
     "ipaddress" => "54.175.129.31",
     "port" => 80,
     "ssl_port" => 80
   }, {
     "hostname" => "localhost",
     "ipaddress" => "127.0.0.1",
     "port" => 80,
     "ssl_port" => 80
   }]
```   
also add the line to wrap the original cookbook
```ruby
include_recipe 'haproxy'
```
add the node.default line and now it should look like this with the populated info from the node query
```ruby
node.default['haproxy']['members']  = [{
    "hostname" => "ec2-54-175-129-31.compute-1.amazonaws.com",
    "ipaddress" => "54.175.129.31",
    "port" => 80,
    "ssl_port" => 80
  }
]

include_recipe 'haproxy'
```
then perform the berkshelf processes
* berks install
* berks upload
* knife cookbook list

##### bootstrap a second node
bootstrap a second node - 54.175.167.32
```bash
knife bootstrap 54.175.167.32 -x chef -P chef --sudo -N web2
Creating new client for web2
Creating new node for web2
Connecting to 54.175.167.32
54.175.167.32 -----> Existing Chef installation detected
54.175.167.32 Starting the first Chef Client run...
54.175.167.32 Starting Chef Client, version 12.7.2
54.175.167.32 resolving cookbooks for run list: []
54.175.167.32 Synchronizing Cookbooks:
54.175.167.32 Compiling Cookbooks...
54.175.167.32 [2016-12-15T16:55:44+00:00] WARN: Node web2 has an empty run list.
54.175.167.32 Converging 0 resources
54.175.167.32
54.175.167.32 Running handlers:
54.175.167.32 Running handlers complete
54.175.167.32 Chef Client finished, 0/0 resources updated in 03 seconds
```
##### show your node info and add "apache" cookbook
```bash
knife node show web2
knife node run_list add web2 "recipe[apache]"
```
##### run chef-client on all nodes
```bash
knife ssh "*:*" -x USERNAME -P PASSWORD "sudo chef-client"
knife ssh "*:*" -x chef -P chef "sudo chef-client"
```
### ARCHITECTURE
* 54.175.129.31 web1
* 54.175.167.32 web2
* 54.207.252.77 lb

##### NODE INFO

$ knife node show web1
Node Name:   web1
Environment: _default
FQDN:        ip-172-31-1-107.ec2.internal
IP:          54.175.129.31
Run List:    recipe[apache]
Roles:
Recipes:     apache, apache::default, apache::server
Platform:    centos 6.7
Tags:

$ knife node show web2
Node Name:   web2
Environment: _default
FQDN:        ip-172-31-3-106.ec2.internal
IP:          54.175.167.32
Run List:
Roles:
Recipes:
Platform:    centos 6.7
Tags:

$ knife node show lb
Node Name:   lb
Environment: _default
FQDN:        ip-172-31-3-95.ec2.internal
IP:          52.207.252.77
Run List:    recipe[myhaproxy]
Roles:
Recipes:
Platform:    centos 6.7
Tags:

### show your node info and add "apache" cookbook
```bash
knife bootstrap 52.207.252.77 -x chef -P chef --sudo -N lb
knife node run_list add lb "recipe[myhaproxy]"
knife node show lb
knife ssh "*:*" -x chef -P chef "sudo chef-client"
```

##### alternatively you can do all 3 commands above in one shot
knife bootstrap FQDN -x USERNAME -P PASSWD --sudo -N lb -r 'recipe[apache]'

### ROLES
* a role describes a run list of recipes that are executed on the node
* a role may also define new defaults or overrides for existing cookbook attributes values
* precedence for roles - default or override

* from within the cookbook directory
* ```bash
cd ~/chef-repo/cookbooks/myhaproxy
mkdir roles
cd roles
cp ~/chef-repo/roles/load_balancer.rb
knife role from file load_balancer.rb
knife role show load_balancer
knife node run_list set NODE 'role[ROLE]'
knife node run_list set lb 'role[load_balancer]'
```

##### create a web.rb
```ruby
name "web"
description "Web"
run_list "recipe[apache]"
```

##### and repeat
```bash
cd ~/chef-repo/cookbooks/apache  
mkdir roles
cd roles
cp ~/chef-repo/roles/web.rb
knife role from file web.rb
knife node run_list set NODE 'role[ROLE]'
knife node run_list set web1 'role[apache]'
knife node run_list set web2 'role[apache]'
knife role show web
```

##### ROLE NESTING
* BASE ROLE
* ntp
* chef-client

##### WEBSERVER ROLE
* contains base role
* apache

### CHEF SERVER & CHEF SEARCH
* Chef Server contains a copy of the node object of every node
* Chef Server contains a copy of the node object of every cookbook
* instead of 'knife ssh "*:*"'

##### Search syntax within a recipe
* all_web_nodes = search('node','role:web')
* knife ssh "*:*" -x chef -P chef "sudo chef-client"
* knife ssh "role:load_balancer" -x chef -P chef "sudo chef-client"
* knife ssh "name:lb" -x chef -P chef "sudo chef-client"

### ENVIRONMENTS
* create a prod and acceptance environment
* deploy a node to an environment
* make load balancer more exact

##### unless specified your default environment is "_default"
```bash
knife environment show _default
knife search node *:*
```
```bash
cd ~/chef-repo
mkdir environments
cd environments

##### create a production environment
vi production.rb (with these contents)
```ruby
name 'production'
description 'where we run production code'

cookbook 'apache', '= 0.2.2'
cookbook 'myhaproxy', '= 1.0.0'
```
##### create an acceptance environment
```ruby
knife node show NODE
knife node environment set

name 'acceptance'
description 'where we run test code'
```
```bash
knife environment from file production.rb acceptance.rb
knife node environment set web1 production
knife node environment set lb production
knife node environment set web2 acceptance
 ```
and as usual converge after everything else is done
```bash
knife ssh "*:*" -x chef -P chef "sudo chef-client"
```
