LLM On Prem Ansible Role
=========

Sets up Rad-Ninjas' [LLM On Prem](https://github.com/RAD-Ninjas/llm-on-prem)

Quick Start
-----------

Add to requirements.yml:

    ---
    - name: llm_on_prem
      src: https://github.com/RAD-Ninjas/ansible-role-llm-on-prem.git
      scm: git

Install the role

    $ ansible-galaxy install -r requirements.yml

Use it:

    - role: llm_on_prem
      vars:
        llm_model_path: lmsys/vicuna-7b-v1.3
        llm_model_name: vicuna-7b-v1.3
        ai_profile: cpu
        # Never store passwords insecurely
        ai_huggingface_token: "{{ lookup('community.general.onepassword', 'huggingface-read-token', field='password', vault='YourVaultName') }}"
        ai_openai_api_key: "{{ lookup('community.general.onepassword', 'penai-api-key', field='password', vault='YourVaultName') }}"
        docker_username: "{{ lookup('community.general.onepassword', 'home-network-docker-username', field='username', vault='HomeAutomation') }}"
        docker_password: "{{ lookup('community.general.onepassword', 'home-network-docker-username', field='password', vault='HomeAutomation') }}"

Requirements
------------

An attempt has been made to only use ansible builtins. However, since it assumes docker is installed and configured (with ample memory and swap), it is best used with geerlingguy/ansible-role-docker

Role Variables
--------------

### Token Storage

By default, the huggingface token and API keys read as follows:

    ai_huggingface_token: "{{ lookup('community.general.onepassword', 'home-network-huggingface-read-token', field='password', vault='YourVaultName') }}"

    ai_openai_api_key: "{{ lookup('community.general.onepassword', 'home-network-openai-api-key', field='password', vault='YourVaultName') }}"

### Model Variables

| Variable | Default | Reason |
|---|---|---|
| llm_model_path | lmsys/vicuna-7b-v1.3 | Huggingface path to the Model.
| llm_model_name | vicuna-7b-v1.3 | Huggingface name of the model
| ai_profile | cpu | Inference build Profile. cpu, gpu, metal, openvino, etc.
|  ai_cpu_threads | 5 |  Number of CPU threads to use for CPU inference
| ai_openvino_ir_path | /home/openvino/models/llama-2/ir_model | The path for OpenVino IR
| ai_openvino_device | GPU.1 | The device name for OpenVino

Dependencies
------------

So far only builtins. The 1password ansible community tasks if you use the 1password lookup on your vars.

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: llm_on_prem_hosts
      roles:
        - role: llm_on_prem
          vars:
            # The Profile as per RAD-Ninjas scripts. CPU, GPU, metal (Mac), etc.
            ai_profile: cpu
            ai_huggingface_token: "{{ lookup('community.general.onepassword', 'huggingface-read-token', field='password', vault='MyVault') }}"
            ai_openai_api_key: "{{ lookup('community.general.onepassword', 'openai-api-key', field='password', vault='MyVault') }}"
            docker_username: "{{ lookup('community.general.onepassword', 'docker', field='username', vault='MyVault') }}"
            docker_password: "{{ lookup('community.general.onepassword', 'docker', field='password', vault='MyVault') }}"

Troubleshooting
---------------

This ansible role should work on any machine that is compatible with docker.

If you use a container-in-container option such as Proxmox or other Hypervisors, the container you use **must** have Nesting (so docker can run in the first place) and NFS support (so docker can mount its own volumes).

License
-------

BSD

Author Information
------------------

This role was written [David Martinez](https://github.com/hackerdude) with help with the amazing AI folks at [RAD-Ninjas](https://github.com/RAD-Ninjas).

