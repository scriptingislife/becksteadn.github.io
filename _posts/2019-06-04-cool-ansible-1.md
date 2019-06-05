---
published: true
title: Cool Ansible Things 1 - delegate_to
layout: post
---
I've been working with Ansible a lot recently and consequently also been reading about it. There are some things I've found that are incredibly useful or just interesting. In this series I'll quickly go over something I learned and give examples of where it may be useful.

First up is the `delegate_to` keyword. It can be appended to a task in the same place as `when` and `register`. An IP address or host that Ansible knows about is required (`delegate_to: web1.example.com`). It will run the task on the delegated computer but with all of the facts of the original host.

The examples in the official documentation mostly reference load balancing. When performing updates on a web server, it needs to be taken out of the pool then added back in when it's operational again. With delegation, the removal and addition tasks can be included in the same set of tasks as the updates. It will be cleanly executed on the load balancer while running everything else on the web server.

I find delegation to be most useful when making reports because you're able to write facts and other data on the remote host to a local file.

```yaml
- name:
  lineinfile:
    path: ./operating-systems.txt
    line: "{{ ansible_hostname }} {{ ansible_distribution }}-{{ ansible_distribution_version}}"
  delegate_to: localhost
```

A more complicated example would execute a task on a remote host and register a variable which can then be written to a local report.

**Bonus tidbit.** `local_action` is similar to `delegate_to` but implies delegation to the controller. You can read more about it in the official documentation.

### Resources

[Official Docs on Delegation](https://docs.ansible.com/ansible/latest/user_guide/playbooks_delegation.html#delegation)

[Mastering Ansible](https://www.packtpub.com/virtualization-and-cloud/mastering-ansible-third-edition)
