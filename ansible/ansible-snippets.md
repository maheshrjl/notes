# 🐬 Ansible Snippets

#### Remove tty requirements with ssh\_pipelining disabled

```yaml
- lineinfile:
    dest: /etc/sudoers
    line: 'Defaults requiretty'
    state: absent
  sudo_user: root
  vars:
      ansible_ssh_pipelining: no
```

