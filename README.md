# molecule-orbstack-driver

An implementation of Molecule's delegated driver to leverage an existing Orbstack daemon.

[![Quick Driver demo](https://img.youtube.com/vi/1WWQreGv88o/0.jpg)](https://youtu.be/1WWQreGv88o)


## PreRequisites

- [OrbStack](https://orbstack.dev/)
- [Molecule](https://ansible.readthedocs.io/projects/molecule/)
- [Ansible](https://www.ansible.com/)

## How this works

- Create your ansible role

```
  ansible-galaxy collection init foo.bar
  cd roles
  ansible-galaxy role init my_role
```

- Create a sample task in the role

```
---
- name: Task is running from within the role
  debug:
    msg: "This is a task from my_role."
```

- Create a sample playbook which calls the role main task, inside of the playbooks directory

```
---
- name: Test new role from within this playbook
  hosts: localhost
  gather_facts: false
  tasks:
    - name: Testing role
      include_role:
        name: foo.bar.my_role
        tasks_from: main.yml
```

- Create a new folder called `extensions`, and inside execute this:

```
molecule init scenario
```

- Place the files in the proper folders, should looks like this:

```
molecule
└── default
    ├── converge.yml
    ├── create.yml
    ├── destroy.yml
    ├── driver
    │   ├── create_orb_vm.yml
    │   ├── destroy_orb_vm.yml
    │   └── validate.yml
    ├── molecule.yml
    └── verify.yml
```

## Limitations

The VM customization is pretty poor, the limitation is in the OrbStack backend but eventually will be get better

- You cannot set the Name and the OS Release at the same time using the CLI (E.G):

```
orb create -a arm64 centos:9-stream test01
```

- You cannot customize the VM on creation but the Arch, Release and VM name.