# page 304-358-412
# Creating and Consuming Modules

nothing to add, you can develop your own ansible modules

# Creating and Consuming Collections

$ mkdir collection-development
$ cd collection-development/
$ ansible-galaxy collection init practicalansible.examples

$ tree
.
└── practicalansible
    └── examples
        ├── README.md
        ├── docs
        ├── galaxy.yml
        ├── meta
        │   └── runtime.yml
        ├── plugins
        │   └── README.md
        └── roles
6 directories, 4 files

$ ansible-galaxy collection build [path]
$ ansible-galaxy collection install [path]

ansible-galaxy and its documentation can be found here:
https://galaxy.ansible.com/docs/

# Creating and Consuming Plugins