Getting Started with OpenContrail Development
==================================
This brief note would serve as a reference for a developer looking to start-up
on the OpenContrail development envrionment.

A new developer can refer the OpenContrail Architecture document from
http://www.opencontrail.org/opencontrail-architecture-documentation.

To get information about latest releases and schedule, please refer to:
https://launchpad.net/juniperopenstack

#1. Registration and User Creation
----------------------------------
a. A new developer should register on OpenContrail development/user forums and
slack on this link: http://www.opencontrail.org/newsletter-and-mailing-lists/

b. In order to submit blueprints for a new feature or to raise bugs on existing features,
create a user-id on Launchpad by clicking `Sign In` in the top right corner of https://review.opencontrail.org/ and completing the Ubuntu One account creation.

c. Create a GitHub account by Signing up on https://github.com/ if not already registered

d. Sign into https://review.opencontrail.org/ with your newly created account and navigate to https://review.opencontrail.org/#/settings/agreements and click `New Contributor Agreement`.  Complete the ICLA (or CCLA) form and submit as per the on-page documentation.

#2. Ubuntu installation and configuration
-------------------
Install Ubuntu version 16.04 trusty for the OpenContrail stable installation.

##2.1 Relevant packages for the OpenContrail installation.
-------------------
    sudo apt-get update
    sudo apt-get install git
    sudo apt-get install git-core
    sudo apt-get install ant
    sudo apt-get install aptitude
    sudo aptitude install build-essential
    sudo apt-get install libev4 libev-dev

##2.2 Generate SSH key and add it to local SSH-agent
-------------------
Ref: https://help.github.com/articles/adding-a-new-ssh-key-to-your-github-account/#platform-linux

    ssh-keygen -t rsa -b 4096 -C "<username@someone.com>"
    eval $(ssh-agent -s)
    ssh-add ~/.ssh/id_rsa
    sudo apt-get install xclip
    xclip -sel clip < ~/.ssh/id_rsa.pub

##2.3 Add SSH key to GitHub account
-------------------
a. Go to profile and select settings

b. Choose SSH and GPG keys -> New SSH key

c. Give a title for the key

d. Paste the key generated above ( xclip command will copy the key from .pub file to your clipboard)

e. Add SSH key.


#3. Opencontrail + Devstack setup
-------------------
Ref: https://github.com/Juniper/contrail-installer/blob/master/README.md

##3.1 Clone Contrail-installer and devstack Repositories
-------------------

    git clone https://github.com/juniper/contrail-installer.git
    git clone https://github.com/openstack-dev/devstack -b stable/newton

##3.2 Setup Opencontrail
-------------------
a. Copy the localrc file from samples to the current directory and edit it as required.

    cd contrail-installer
    cp samples/localrc-all localrc

    change eth interface: default is eth0
    #CONTRAIL_BRANCH=R3.2   //Commented means code will be checked out from Trunk.
                            //Else specify the branch and uncomment it.

b. Execute contrail.sh

    ./contrail.sh

##3.3 Setup DevStack
-------------------
a. Copy the opencontrail plugin to devstack neutron_plugins directory

    cp ../contrail-installer/devstack/lib/neutron_plugins/opencontrail lib/neutron_plugins/

b. Prepare localrc for devstack:

    cp ../contrail-installer/devstack/samples/localrc-all localrc

   Update "PHYSICAL_INTERFACE" as applicable.

   Add following to localrc file:

    NOVNC_BRANCH=v0.6.0

c. Run stack.sh

    ./stack.sh

d. The installation shall fail once. Once it fails, do the following change

   Edit the file "/usr/local/lib/python2.7/dist-packages/openstack/session.py" at line 29
   and replace "openstack.version" with "openstack"

    Line 29: DEFAULT_USER_AGENT = "openstacksdk/%s" % openstack

e. Now do a restack. For this Opencontrail needs to be restarted as well, i.e.

    ./unstack.sh
    cd ../contrail_installer
    ./contrail.sh restart
    cd -
    ./stack.sh

##3.4. Accessing Openstack and Opencontrail GUIs
-------------------
Openstack and contrail GUIs can be accessed as follows:-

a. Openstack GUI: http://localhost/dashboard/

b. Opencontrail GUI: http://localhost:8080/


#4. Setting up Gerrit for committing code-changes for review
-------------------

Ref: https://review.opencontrail.org/Documentation/index.html

##4.1 Setup SSH access
-------------------
a. Login to your account at review.opencontrail.org

b. Goto Top-right corner -> settings

c. Left panel -> SSH public keys -> Add key

d. Follow the instruction under heading "How to generate an SSH key"

e. Test the SSH access

    $ ssh -p 29418 sshusername@review.opencontrail.org

    ****    Welcome to Gerrit Code Review    ****

    Hi John Doe, you have successfully connected over SSH.

    Unfortunately, interactive shells are disabled.
    To clone a hosted Git repository, use:

    git clone ssh://sshusername@review.opencontrail.org:29418/REPOSITORY_NAME.git

    Connection to hostname closed.

##4.2 Pushing code-changes for review
-------------------
a. Install git-review

    sudo apt-get install git-review

b. Configure Gerrit

    git config --global user.email username@someone.com
    gitdir=$(git rev-parse --git-dir);
    scp -p -P 29418 username@review.opencontrail.org:hooks/commit-msg ${gitdir}/hooks/

c. Clone the repo where changes need to be committed

    git clone ssh://sshusername@review.opencontrail.org:29418/REPOSITORY_NAME.git

d. Commit the changes

    git commit -m "<commit-note>"

    Note: please ensure that any change being committed should have a corresponding
    launch-pad bug-id mentioned in the commit message, i.e. "Bug #1679466"

e. Push the locally committed changes up for review

    git push ssh://username@review.opencontrail.org:29418/REPOSITORY_NAME \
    HEAD:refs/for/<branch>%topic=<few-words-describing-the-change>, \
    r=reviewername@someone.com, cc=otherreviewer@someone.com

