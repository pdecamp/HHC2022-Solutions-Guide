# AWS CLI Intro

!!! summary "AWS CLI Intro<br>*Difficulty*: :fontawesome-solid-tree:{: style="color: red;"}:fontawesome-solid-tree:{: style="color: grey;"}:fontawesome-solid-tree:{: style="color: grey;"}:fontawesome-solid-tree:{: style="color: grey;"}:fontawesome-solid-tree:{: style="color: grey;"}"
    Try out some basic AWS command line skills in this terminal. Talk to Jill Underpole in the Cloud Ring for hints.



## Elf Introduction

??? quote "Jill Underpole"
    Umm, can I help you?<br>
    Me? I'm Jill Underpole, thank you very much.<br>
    I'm working on this here smoke terminal.<br>
    Cloud? Sure, whatever you want to call it.<br>
    Anyway, you're welcome to try this out, if you think you know what you're doing.<br>
    You'll have to learn some basics about the AWS command line interface (CLI) to be successful though.

## Solution

Open the terminal next to Jill Underpole to start the AWS CLI tutorial, and answer the questions using either the instructions or the help references.

<br>

**You may not know this, but AWS CLI help messages are very easy to access. First, try typing:<br>`$ aws help`**

??? success "Answer"
    ```
    elf@349ba0da09f9:~$ aws help
    ```

<br>

**Great! When you're done, you can quit with q.<br>Next, please configure the default aws cli credentials with the access key `AKQAAYRKO7A5Q5XUY2IY`, the secret key `qzTscgNdcdwIo/soPKPoJn9sBrl5eMQQL19iO5uf` and the region us-east-1 .<br><a href="https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-quickstart.html#cli-configure-quickstart-config">https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-quickstart.html#cli-configure-quickstart-config</a>**

For this task we just need to enter the `aws configure` command and respond to the prompts.

??? success "Answer"
    ```
    elf@349ba0da09f9:~$ aws configure
    AWS Access Key ID [None]: AKQAAYRKO7A5Q5XUY2IY
    AWS Secret Access Key [None]: qzTscgNdcdwIo/soPKPoJn9sBrl5eMQQL19iO5uf
    Default region name [None]: us-east-1
    Default output format [None]: 
    elf@349ba0da09f9:~$ 
    ```

<br>

**Excellent! To finish, please get your caller identity using the AWS command line. For more details please reference:<br>`$ aws sts help`<br>or reference:<br><a href="https://awscli.amazonaws.com/v2/documentation/api/latest/reference/sts/index.html">https://awscli.amazonaws.com/v2/documentation/api/latest/reference/sts/index.html</a>**

For this task we just need to enter the `aws sts` command to get our caller identity, which can be found in the help reference.

??? success "Answer"
    ```
    elf@349ba0da09f9:~$ aws sts get-caller-identity
    {
        "UserId": "AKQAAYRKO7A5Q5XUY2IY",
        "Account": "602143214321",
        "Arn": "arn:aws:iam::602143214321:user/elf_helpdesk"
    }
    elf@349ba0da09f9:~$
    ```

## Completion

??? quote "Jill Underpole"
    Wait, you got it done, didn't you?<br>
    Ok, consider me impressed. You could probably help Gerty, too.<br>
    The first trick'll be running the Trufflehog tool.<br>
    It's as good at sniffing out secrets as I am at finding mushrooms!<br>
    After that, it's just a matter of getting to the secret the tool found.<br>
    I'd bet a basket of portobellos you'll get this done!
