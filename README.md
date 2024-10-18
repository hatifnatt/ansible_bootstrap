# Bootstrap host for Ansible via Ansible

Example playbook, let's name it `user_for_ansible.yml`

```yaml
---

- name: Install python if required
  hosts: all
  become: true
  gather_facts: false
  tasks:
    - import_role:
        name: ansible_bootstrap
        tasks_from: python

- name: Prepare host for Ansible
  hosts: all
  become: true
  roles:
    - ansible_bootstrap
```

Call it for single host:

```
ansible-playbook -i %remote.host%, user_for_ansible.yml -u %remoteuser% -kK --become-method=%sumethod%
```

Where:

* `%remote.host%` can be DNS hostname or IP address.
* `%remoteuser%` must have ssh access.
* `%sumethod%` can be `su` or `sudo` ... it depends.

On request enter `%remoteuser%` password and `su` or `sudo` password if required.

To properly set up `authorized_keys` for ansible user it's expected that one of the public key files from `public_keys` variable is present on local system, default value for `public_keys` is:

```yaml
public_keys:
  - ~/.ssh/ansible.pub
  - ~/.ssh/ansible.key.pub
```
