LLM On Prem Ansible Role
=========

Sets up Rad-Ninjas' [LLM On Prem](https://github.com/RAD-Ninjas/llm-on-prem)

Requirements
------------

An attempt has been made to only use ansible builtins. However, since it assumes docker is installed and configured (with ample memory and swap), it is best used with geerlingguy/ansible-role-docker

Role Variables
--------------

Group Vars has this (uses 1password for secure token storage):
     ai_huggingface_token: "{{ lookup('community.general.onepassword', 'home-network-huggingface-read-token', field='password', vault='YourVaultName') }}"
     ai_openai_api_key: "{{ lookup('community.general.onepassword', 'home-network-openai-api-key', field='password', vault='YourVaultName') }}"

You can find these used in the J2 scripts at:

     HF_TOKEN="{{ ai_huggingface_token }}"
     OPENAI_API_KEY="{{ ai_openai_api_key }}"

Please keep your secrets, secret.


Dependencies
------------

So far only builtins

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: servers
      roles:
         - { role: username.rolename, x: 42 }

License
-------

BSD

Author Information
------------------

TODO Mention Me, thanks to the RadNinjas and all that good stuff. Hah.
