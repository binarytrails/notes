# Ansible

## ansible-playbook

    # pass env vars
    ANSIBLE_FORCE_COLOR=1 ansible-playbook ...

    # target specific task.yml
    --list-tasks <playbook>
    --start-at-task <task path> <playbook>

    # target specific tag
    --list-tags <playbook>
    -t <tag>
    --skip-tags <tag> <playbook>

# tasks

    - name: ...
      command: ...
      register: status
      failed_when: "'blabla' in status.stdout"
      changed_when: False

    - name:
      debug:
        msg: "var {{ var }}"

    ...
      ignore_errors: yes
