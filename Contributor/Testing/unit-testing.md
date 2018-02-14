Unit Test Information
=====================

This is some basic info to point developers in the right direction on
understanding where unit tests are specified and how the continuous integration
system finds them. Some of the URLs below include line numbers in an effort
to point the reader to exactly where in files the most enlightening information
is, but unfortunately that can become stale if the files change. If you're
reading this sometime long after February 2018, be aware that you might need
to look around the file a bit if the indicated line doesn't seem to be exactly
what is described here.

The main configuration for the CI system is in:
https://github.com/Juniper/contrail-project-config

Some background on Zuul will be helpful and can be found in the OpenStack
documentation: https://docs.openstack.org/infra/zuul/index.html

The main Zuul concepts that you need to know about as a developer are
pipelines, projects, and jobs. These configurations are stored as YAML files in
https://github.com/Juniper/contrail-project-config/tree/master/zuul.d

Jobs basically run Ansible playbooks. For more information on Ansible, see
https://www.ansible.com/resources/get-started

But basically what you need to know is that the unit test job is defined here:
https://github.com/Juniper/contrail-project-config/blob/master/zuul.d/jobs.yaml#L185

and it runs the playbook `playbooks/unittest/contrail` which is defined here:
https://github.com/Juniper/contrail-project-config/blob/master/playbooks/unittest/contrail.yaml

the most important thing this playbook does is make use of the role
`contrail-unittests` which is defined here:
https://github.com/Juniper/contrail-project-config/tree/master/roles/contrail-unittests

and the most important part of this role is the set of tasks it performs which
are defined here:
https://github.com/Juniper/contrail-project-config/blob/master/roles/contrail-unittests/tasks/main.yaml

Many of these tasks are preparatory, so the important part is line line 64
where it runs the shell script `contrail-unittests-job.sh` which is here:
https://github.com/Juniper/contrail-project-config/blob/master/roles/contrail-unittests/files/contrail-unittests-job.sh

and the most important thing that shell script does is gather the unit tests
to be run using the Ruby script:
https://github.com/Juniper/contrail-project-config/blob/master/roles/contrail-unittests/files/contrail-unittests-gather.rb

The Ruby script gathers unit tests by parsing `ci_unittests.json` which is
located here:
https://github.com/Juniper/contrail-controller/blob/master/ci_unittests.json

The JSON file lists each of the Contrail Git repositories and for each one it
lists the scons targets that represent unit tests. For example, in the
vRouter repository we have:
https://github.com/Juniper/contrail-controller/blob/master/ci_unittests.json#L100

which currently defines:
"controller/src/agent:test",
"vrouter:test"

The first one references:
https://github.com/Juniper/contrail-controller/blob/master/src/vnsw/SConscript#L34

and the second one refers to:
https://github.com/Juniper/contrail-vrouter/blob/master/test/SConscript#L24

which, although I'm not very familiar with Scons, I think is referencing a test
command that makes use of the source files that are in the same directory and
contain unit tests.
