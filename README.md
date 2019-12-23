win_veeam_jobs
==============

Ansible role which helps to manage VMware VM association with Veeam jobs.

The configuration of the role is done in such way that it should not be
necessary to change the role for any kind of configuration. All can be done
either by changing role parameters or by declaring completely new configuration
as a variable. That makes this role absolutely universal. See the examples below
for more details.

Please report any issues or send PR.


Examples
--------

```yaml
---

- name Example of how to add VM into a specific job
  hosts: veeam.example.com
  gather_facts: no
  become: no
  vars:
    win_veeam_jobs_job: backups
    win_veeam_jobs_vm: vm01
  roles:
    - win_veeam_jobs

- name Example of how to remove VM from a specific job
  hosts: veeam.example.com
  gather_facts: no
  become: no
  vars:
    win_veeam_jobs_job: backups
    win_veeam_jobs_vm: vm01
    win_veeam_jobs_state: absent
  roles:
    - win_veeam_jobs

- name Example of how to manage multiple VMs at once
  hosts: veeam.example.com
  gather_facts: no
  become: no
  vars:
    win_veeam_jobs_job_list:
      # Add VM
      - job: backups
        vm: vm01
      # Remove VM
      - job: backups
        vm: vm01
        state: absent
  roles:
    - win_veeam_jobs
```

In order to run the playbook, you have to make WinRM working on the machine from
where you run Ansible. It usually requires latest version of
`requests-kerberos`:

```shell
pip install requests-kerberos
```

Then you can run it like this:

```shell
ansible-playbook \
  -i veeam.example.com, \
  -e "{ \
    ansible_connection: winrm, \
    ansible_port: 5986, \
    ansible_user: ansible@EXAMPLE.COM, \
    ansible_winrm_transport: kerberos, \
    ansible_winrm_server_cert_validation: ignore, \
    ansible_password: myt0ps3cr3tp4ssword }" \
  ./site.yaml
```


Role variables
--------------

List of variables used by the role:

```yaml
# Veeam job name (must be provided by the user)
win_veeam_jobs_job: null

# VM name in the Veeam job (must be provided by the user)
win_veeam_jobs_vm: null

# Default state
win_veeam_jobs_state: present

# Final list
win_veeam_jobs_list:
  - job: "{{ win_veeam_jobs_job }}"
    vm: "{{ win_veeam_jobs_vm }}"
    state: "{{ win_veeam_jobs_state }}"
```


License
-------

MIT


Author
------

Jiri Tyr
