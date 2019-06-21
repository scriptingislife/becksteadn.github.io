---
published: true
layout: post
title: Cool Ansible 3 - Advanced Templating
---
{% raw %}
[Jinja](http://jinja.pocoo.org/docs/) is a powerful template engine for Python. It's commonly used to place variables and other content in files. You can see it in action by using Ansible's `template` task. There is much more functionality that can be used to take Ansible playbooks to the next level.

## If Statements

If statements are extremely helpful in Ansible. One of the best use cases I've had is modifying configuration files based on Ansible facts. I use [Goss](https://github.com/aelsabbahy/goss/blob/master/docs/manual.md) to test the roles for my Bytes of Swiss project. One role requires uninstalling the netcat package and installing a different one. Debian systems with apt use `netcat-traditional` and RHEL systems with yum use `nc`. Instead of making a test file for each operating system that only varies by one line, we can use an if statement.

```
---
package:
  {% if ansible_os_family == "Debian" %}
  netcat-traditional:
  {% else %}
  nc:
  {% endif %}
    installed: true

package:
  netcat-openbsd:
    installed: false
```

## Loops

Loops are useful for creating multiple lines in a file from a list in Ansible. If a variable is defined as shown below, the playbook will create a Samba share configuration for each set of variables in the `sambashares` list.

```
---
- hosts: localhost
  gather_facts: no
  vars:
    sambashares:
      - name: public
        comment: Public resources
        path: /samba/public
        read_only: yes
        browsable: yes
      - name: private
        comment: Private resources
        path: /samba/private
        read_only: no
        browsable: no
  tasks:
    - name: Create Samba shares
      blockinfile:
        path: ./samba.conf
        create: yes
        block: |
          {% for share in sambashares %}
            [{{ share.name }}]
            comment = {{ share.comment }}
            path = {{ share.path }}
            read only = {{ share.read_only }}
            browsable = {{ share.browsable }}

          {% endfor %}
```

Another use it to make list objects for Ansible to use. You can find my example playbook at this [GitHub Gist](https://gist.github.com/becksteadn/58754581586791a4082d37f314ca7b1c). It makes a list of strings for `debug` to print which outputs nicer than using `with_items` and actually prints on a new line rather than print the `\n` character.

This use can help make complex Ansible playbooks that require operations or calculations on variables before use.

## Lookups

Ansible extends Jinja templating by introducing lookups. Lookups can be used to read in data from outside of the playbook and its related files. There are a plethora of lookup functions but the popular two are most likely `env` and `file`. These can set a variable to the value of an environment variable and content of a file respectively.

```
vars:
  file_contents: "{{lookup('file', 'path/to/file.txt') }}"
  secret_key: "{{ lookup('env', 'MY_SECRET_KEY') }}"
```

Some other notable lookups are `url` which retrieves the cotent of a URL, and `fileglob` which returns files matching a certain search, like doing a programmatic `ls /my/path/*.txt`.

## Filters

[Filters](http://jinja.pocoo.org/docs/2.10/templates/#filters) can perform operations on variables. They work the same way as functions. There are filters you may recognize from Python like `upper` and `lower` for strings and `join` and `reverse` for lists.

Filters are used by following a variable with a pipe character `|` and then the filter name.

```
{{ [6, 8, 3, 9] | max }}
```

Some are simple like the above example, but other filters can take arguments and do more advanced operations.

```
- name: give me longest combo of three lists , fill with X
  debug:
    msg: "{{ [1,2,3] | zip_longest(['a','b','c','d','e','f'], [21, 22, 23], fillvalue='X') | list }}"
```

Ansible created their own [additional filters](https://docs.ansible.com/ansible/latest/user_guide/playbooks_filters.html). One of the most useful I've found is [ipaddr](https://docs.ansible.com/ansible/latest/user_guide/playbooks_filters_ipaddr.html). It can calculate a subnet mask, broadcast address, etc. given an IP address in CIDR notation which is extremely helpful when working with the [Netbox](https://github.com/digitalocean/netbox) API. It can also return if an address is public or private or how many IP addresses are in a range and even convert IPv4 addresses to IPv6. 

## Conclusion

This is just the tip of the iceberg when it comes to templating. Jinja already has an impressive feature set and Ansible even **extends** that with their own plugins. All of these enable playbooks to have advanced logic and manipulate data in complex ways.  

## Resources

[Jinja Docs](http://jinja.pocoo.org/docs/2.10/templates/)

[Ansible Docs about Jinja2](https://docs.ansible.com/ansible/latest/user_guide/playbooks_templating.html)

{% endraw %}