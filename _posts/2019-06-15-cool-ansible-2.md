---
published: true
layout: post
title: Cool Ansible 2 - Get User Input During Execution
---

The `pause` module can stop execution for an amount of time or wait for the user to hit Return. One of its return parameters is `user_input` which makes it great for getting variables or confirming actions during task execution. In this example, we ask the user to confirm that they want to delete an important file. The deletion task will be only be run when the user explicitly types `yes` or `true`.

```yaml
- name: Confirm important file deletion
  pause:
    prompt: "Are you sure you want to delete backup.db? (yes/no)"
  register: confirm_delete
  
- name: Delete backup file
  file:
    path: /home/admin/backup.db
    state: absent
  when: confirm_delete.user_input | bool
```

This can only be used when running a playbook interactively from the command line as scripts or Tower/AWX will hang. If the playbook needs to be run both in interactive and non-interactive modes it's best to create a variable and set it to true when running in the latter. The variable can be set in the script or Tower job template.

Supporting both environments can get very complex, however. Multiple checks need to be put in place and variables need to be standardized. Consider this example. It can be run using `ansible-playbook prompt-test.yml`. Since both of the variables are defined, the prompt will be skipped. Now try removing `no_prompt`. The prompt is still skipped because the variable we're looking to get from the user is already defined. If `my_username` is removed but `no_prompt` exists, the prompt is skipped but the variable we want is undefined. Remove both and the prompt is run.

```yaml
---
- hosts: localhost
  vars:
    no_prompt: true
    my_username: example
  tasks:
    - name: Get username from input if not already defined
      pause:
        prompt: "Enter your username"
      register: prompt_username
      when: my_username is not defined and no_prompt is not defined

    - name: Standardize my_username variable
      set_fact:
        my_username: "{{ prompt_username.user_input }}"
      when: prompt_username is not skipped

    - name: Debug username
      debug:
        var: my_username
```  

As you can see, prompts can be very useful but may introduce unneeded complexity.

## Resources

[Pause Module Documentation](https://docs.ansible.com/ansible/latest/modules/pause_module.html)

[Ansible Tower Best Practices](https://docs.ansible.com/ansible-tower/latest/html/userguide/best_practices.html)