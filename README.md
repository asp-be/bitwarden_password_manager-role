Role Name
=========

A simple role to interact with Bitwarden Password Manager

Requirements
------------
/


Role Variables
--------------

Depending on the API call you want to do with BPM, you might need one of all of these:

- operation: Needed to trigger the right API call
  - read_password, read_collection, update, create, delete_entry
- vm_name: The VM that will be the target for password rotation
- login: BPM login
- organization_id: Organization ID from Bitwarden, you can find it in the URL once logged in
- collection_ids: collection id to manage, you can find it in the URL once logged in

Dependencies
------------

/

Example Playbook
----------------
```yaml
- name: Read Bitwarden entries
      ansible.builtin.include_role:
        name: bitwarden_password_manager
      vars:
        operation: read_collection
        organization_id: MyID
        collection_id: MyCollectionID

- name: Update password in Bitwarden
      ansible.builtin.include_role:
        name: bitwarden_password_manager
      vars:
        operation: update
      no_log: true
    
- name: Find matching Bitwarden entry id
      set_fact:
        entry_id: "{{ item.id }}"
        old_password: "{{ item.login.password | password_hash('sha512') }}"
        login: "{{ item.login.username }}"
      when: 
        - item.fields | selectattr('name', 'equalto', 'VM Name') | map(attribute='value') | first == inventory_hostname
      loop: "{{ netbox_linux_root_entries.json.data.data }}"
      delegate_to: localhost
      no_log: true
```
License
-------

BSD

Author Information
------------------

I'm a IT DevOps engineer, working @ ASP. You can contact me on my pro email address 

florian.vanbelle@asp.be
