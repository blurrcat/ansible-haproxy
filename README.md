blurrcat.haproxy
================

Install haproxy and configure it.

Requirements
------------

OS: Ubuntu 14.04

Role Variables
--------------

There is a main dictionary that needs to be set named `haproxy`. 
This dictionary has 4 main sections :
- global
- defaults
- frontends
- backends

Each section has the most used parameters that I can think of. This role does not have (yet) full coverage of the main haproxy project.
To add a parameter that's not defined in the templates, pass through "extras".

In addition, you can choose haproxy version by setting `haproxy_version`. The
default is "1.5", but also supports "1.4".

Example Playbook
-------------------------

I *highly* suggest that you use a seperate variable file for this role (using the `vars_files` directive), but you can use it in the main playbook if you insist:

```yaml
- hosts: loadbalances
  roles:
     - role: haproxy
       haproxy_version: 1.5
       haproxy_frontends:
       - name: 'fe-mysupersite'
         ip: '123.123.123.120'
         maxconn: '1000'
         default_backend: 'be-mysupersite'
       haproxy_backends:
       - name: 'be-mysupersite'
         description: 'mysupersite is really cool'
         servers:
           - name: 'be-mysupersite-01'
             ip: '192.168.1.100'
         acl:
           - name: admins
             condition: http_auth(admins)
         extras:
           - |
             userlist admins
               user admin insecure-password admin
           - http-request auth realm mysupersite if !admins
```

See [the full example](https://github.com/blurrcat/ansible-haproxy/blob/master/vars/main.yml) to see all options.


License
-------

Apache v2

Author Information
------------------

Pheromone - Pierre Paul Lefebvre (lefebvre@pheromone.ca)
ModCloth - Rafe Colton (https://github.com/rafecolton)
blurrcat - Harry Liang (blurrcat@gmail.com)
