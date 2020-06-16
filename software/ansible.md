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

