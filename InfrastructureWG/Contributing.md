Contributing Code to OpenContrail
=================================

This document outlines the guidelines for submitting code to OpenContrail.

The OpenContrail development process is loosely modeled on the OpenStack
development model so it is worth consulting the OpenStack Developer's Guide
for additional background and details.

The OpenContrail Development Process
------------------------------------

* The code is hosted in a Gerrit controlled Git repository
* The code is mirrored to GitHub after it is merged (accepted) in Gerrit
* There is a mailing list to discuss development, but patches should not
  be emailed
* All patches require a Launchpad bug ID in the commit message
* New feature development should begin with the creation of a Launchpad
  blueprint and Git commit to the contrail-controller/specs directory of a
  Markdown file describing the proposed new feature
* Proposed new features should be announced to the contrail-dev mailing list
  with the URLs of the Launchpad blueprint and the Gerrit spec review
* All development should start based on the master branch. Cherry-picking
  commits to release branches is discussed below

Getting the Source Code
-----------------------

For casual browsing the code is mirrored to GitHub in a large number of
repositories starting with "contrail-" in Juniper's GitHub organization.

In order to make changes you must clone the appropriate repositories from
https://review.opencontrail.org (NOT from GitHub) and install the git-review
tool.

Before Making Your Changes
--------------------------
* Launchpad Bug
* Launchpad Blueprint
* Spec Review
* TSC/ARB Review
* Test Plan Review

Making Your Changes
-----------------

* Style Guide
* Sizing of commits - limit the number of files per commit and 
* MAINTAINERS file?
* Release Notes
* Unit tests
* Other tests
* Documentation
* Automation in contrail-controller-test repository

Commit Messages
---------------

Running Local Tests
-------------------

Running Tempest Tests
---------------------

Uploading Commits to Gerrit
---------------------------

Use the "git review" command to send the commit to Gerrit and take note of the
URL provided in the output.

Continuous Integration Testing
------------------------------

OpenContrail CI

Delivery of Contrail into OpenStack CI in order to allow testing of
networking-opencontrail drivers

The Code Review Process
-----------------------

* Expected timeline
* Cross-repository dependencies

