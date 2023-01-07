# Clone with a Difference

!!! summary "Clone with a Difference<br>*Difficulty*: :fontawesome-solid-tree:{: style="color: red;"}:fontawesome-solid-tree:{: style="color: grey;"}:fontawesome-solid-tree:{: style="color: grey;"}:fontawesome-solid-tree:{: style="color: grey;"}:fontawesome-solid-tree:{: style="color: grey;"}"
    Clone a code repository.  Get hints for this challenge from Bow Ninecandle in the Elfen Ring.



## Elf Introduction

??? quote "Talk to Bow Ninecandle"
    Well hello! I'm Bow Ninecandle!<br>
    Have you ever used Git before? It's so neat!<br>
    It adds so much convenience to DevOps, like those times when a new person joins the team.<br>
    They can just clone the project, and start helping out right away!<br>
    Speaking of, maybe you could help me out with cloning this repo?<br>
    I've heard there's multiple methods, but I only know how to do one.<br>
    If you need more help, check out the <a href="https://www.youtube.com/watch?v=vIQY_FH1SVk">panel of very senior DevOps experts</a>.<br>


## Hints and Resources

??? hint "Hints from Bow Ninecandle's introduction"
    **HTTPS Git Cloning**<br>
    There's a consistent format for Github repositories cloned <a href="https://github.com/git-guides/git-clone">via HTTPS</a>. Try converting!

## Solution

Open the terminal next to Bow Ninecandle

The terminal tells us that the git command to clone the repository,<br>
`git clone git@haugfactory.com:asnowball/aws_scripts.git`<br>
isn't working. 

Git repositories can typically be accessed either by SSH (if authenticated) or HTTPS (if public).  The format the terminal shows us is for SSH.

| Format | Syntax |
| ------ | ------ |
| SSH | git@<server\>:<user\>/<repo\> |
| HTTPS | https://<server\>/<user\>/<repo\> |

Using the HTTPS format for the repository allows us to clone it, then we just need to examine the README.md file to find the last word in it, then runtoanswer to submit the answer.

```
git clone https://haugfactory.com/asnowball/aws_scripts
cat aws_scripts/README.md
runtoanswer
```

??? success "What's the last word in the README.md file for the aws_scripts repo?"
    maintainers



## Completion

??? quote "Talk to Bow Ninecastle"
    Wow - great work! Thank you!<br>
    Say, if you happen to be testing containers for security, there are some things you should think about.<br>
    Developers love to give ALL TeH PERMz so that things "just work," but it can cause real problems.<br>
    It's always smart to check for excessive user and container permissions.<br>
    You never know! You might be able to interact with host processes or filesystems!


