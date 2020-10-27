# Ansible

## ansible-playbook

    # sample run with controls using env vars
    PYTHONUNBUFFERED=1 ANSIBLE_FORCE_COLOR=1 ANSIBLE_ROLES_PATH='./roles' \
        ansible-playbook playbooks/play.yml --inventory-file=hosts

    # target specific task.yml
    --list-tasks <playbook>
    --start-at-task <task path> <playbook>

    # target specific tag
    --list-tags <playbook>
    -t <tag>
    --skip-tags <tag> <playbook>

    - name: loop over all the hosts in the current play and track index
      run_once: True
      debug:
        msg: "{{ item }} {{ index }}"
      loop: "{{ ansible_play_batch }}"
      loop_control:
        index_var: index

## ansible-galaxy

    # create offline template
    ansible-galaxy init myrole --offline

## tasks

    # register fail
    - name: ...
      command: ...
      register: status
      failed_when: "'blabla' in status.stdout"
      changed_when: False
    
    # log fail
    - name:
      debug:
        msg: "var {{ var }}"

    # don't fail on errors
    - name:
      ignore_errors: yes

## handlers

    # role/handlers/main.yml
    - name: Restart sshd
      changed_when: true
      service:
        name: sshd
        state: restarted

    # role/tasks/main.yml
    - name: Setup alternate SSH port
      lineinfile:
        dest: "/etc/ssh/sshd_config"
        regexp: "^Port"
        line: "Port 2222"
      notify: "Restart sshd"

## vars

**combining:**

    # vars/main.yml
    __build_packages:
      - same1
      - same2
    __build_packages_specific_os:
      - uncommon

    # somewhere else
    build_packages: "{{ __build_packages + __build_packages_specific_os }}"

## callback plugins

https://serverfault.com/questions/835901/ansible-json-output

    # ./callback_plugins/json_cb.py:

    from __future__ import absolute_import
    from ansible.plugins.callback import CallbackBase
    import json

    class CallbackModule(CallbackBase):

        CALLBACK_VERSION = 2.0
        CALLBACK_TYPE = 'stdout'
        CALLBACK_NAME = 'json_cb'

        def __init__(self):
            self.tasks = {}

        def dump_result(self, result):
            print(json.dumps(dict(name=self.tasks[result._task._uuid],result=result._result)))

        def v2_playbook_on_task_start(self, task, is_conditional):
            self.tasks[task._uuid] = task.name

        v2_runner_on_ok = dump_result
        v2_runner_on_failed = dump_result

    $ ANSIBLE_STDOUT_CALLBACK=json_cb ansible-playbook myplaybook.yml
