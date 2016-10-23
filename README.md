# docker-test-service

## Purpose
This repo provides a shell script that takes the PITA out of following the Udemy course: [Docker for DevOps: From Dev to Prod][1]. The primary focus of the course is docker, not the service contained by docker or the code that comprises the service.

My primary focus is making it easier for others to stand-up the Debian OS (Jessie, for now) in order to get down to the business of the course.

## Prereqs
This process should be entirely OS agnostic but you will need a few things (that you probably already have):

1. An OS with a GUI
    * Mac, Linux, or Windows; it shouldn't matter.
2. A terminal and sudo access.
3. [Install Vagrant][2] on the OS; everything pursued herein should be installed in `/usr/local/bin/`.
3. [Install VirtualBox][3] with the [Extension Pack][4].
4. Install git on the OS; this will vary, but the recommended method is:
  * RHEL (and derivatives): `sudo yum -y install git`
  * Debian (and derivatives): `sudo apt-get -y install git`
  * OS X (and derivatives): `brew install git`
  * Windows: you are on your own.

### POST Install/Configuration Tests
The Wiki has some [Troubleshooting tips][5].

---

## The Process
Move into a directory where you keep your other source code and clone the repo:

`git clone git@github.com:todd-dsm/docker-test-service.git`

Move into the new directory:

`cd docker-test-service`

You should have a directory structure like this:
```bash
$ tree
.
├── MobyDock
...
├── Vagrantfile
├── bootstrap.sh
└── sources
...
```

Open the VirtualBox program. A virtual machine named `docker_debian_jessie` will be displayed after the next step.

Start the Vagrant process and it will do the rest of the work:

`vagrant up --provider virtualbox && vagrant ssh`

**_NOTE: this process takes about 5-10 minutes depending on CPU speed and memory._**

You will know when the app is ready to test. This line will be displayed:

`==> default: postgres_1  | LOG:  autovacuum launcher started`

Keep the terminal in view while you check the app in the browser: [MobyDock][6] (right-click to open in a new tab)

**_NOTE: the logs should be live._**

---

## TEST 1
Feed MobyDock by clicking the button at the bottom; every time the button is clicked:

1. The number of times fed (under the button) will increase.
2. 1 of 3 unique messages should be displayed at the top of the page.

## TEST 2
Open a new terminal tab and ssh into the Vagrant box:
`vagrant ssh`

Change some value in the css file and save. 
`vi MobyDock/mobydock/mobydock/static/main.css`

Click on the original terminal tab to see the logs again.

Then click reload in the browser:
**_NOTE: the css change should be live._**

---

# Tear-down
Close the browser tab that displays the MobyDock app.

In the terminal tab where you changed the css file, exit the terminal and close the tab.

```bash
vagrant@jessie:~$ exit
logout
Connection to 127.0.0.1 closed.
```

In the terminal tab where the live logs are, close the Vagrant process down by doing CTRL+c twice:

```bash
^C==> default: Waiting for cleanup before exiting...
^C==> default: Exiting immediately, without cleanup!
```

Dump the Vagrant box. Make sure you can see VirtualBox while you are using the Terminal.

```bash
$ vagrant halt && vagrant destroy -f 
==> default: Attempting graceful shutdown of VM...
==> default: Destroying VM and associated drives...
```

Now the `docker_debian_jessie` vm has been removed from the host system.

Hopefully the docker test succeded. Your host machine is unscathed with the exception that a few, very useful, programs have been installed. 

The End

[1]: https://www.udemy.com/the-docker-for-devops-course-from-development-to-production/learn/v4/overview
[2]: https://www.vagrantup.com/downloads.html
[3]: https://www.virtualbox.org/wiki/Downloads
[4]: https://www.youtube.com/watch?v=mwKmxxRbvws&feature=youtu.be&t=25s
[5]: https://github.com/todd-dsm/docker-test-service/wiki/Program-Install-Troubleshooting
[6]: http://localhost:8000/seed
