# Trufflehog Search

!!! summary "Trufflehog Search<br>*Difficulty*: :fontawesome-solid-tree:{: style="color: red;"}:fontawesome-solid-tree:{: style="color: red;"}:fontawesome-solid-tree:{: style="color: grey;"}:fontawesome-solid-tree:{: style="color: grey;"}:fontawesome-solid-tree:{: style="color: grey;"}"
    Use Trufflehog to find secrets in a Git repo. Work with Jill Underpole in the Cloud Ring for hints. What's the name of the file that has AWS credentials?



## Elf Introduction

??? quote "Gerty Snowburrow"
    Well now, look who's venturing down into the caves!<br>
    And well, who might you be, exaclty?<br>
    I'm Gerty Snowburrow, if you need to know.<br>
    And, not that I should be telling you, but I'm trying to figure out what Alabaster Snowball's done this time.<br>
    Word is, he committed some secrets to a code repo.<br>
    If you're feeling so inclined, you can try and find them for me.

## Hints and Resources

??? hint "Other resources"
    **Trufflehog**<br>
    <a href="https://github.com/trufflesecurity/trufflehog">https://github.com/trufflesecurity/trufflehog</a>
    

## Solution

Open the terminal next to Sulfrod to start this challenge.  Note that after completing this the terminal will continue on to the Exploitation via AWS CLI objective.

<br>

**Use Trufflehog to find credentials in the Gitlab instance at https://haugfactory.com/asnowball/aws_scripts.git.<br>Configure these credentials for us-east-1 and then run:<br>`$ aws sts get-caller-identity`**


For this objective we first need to run Trufflehog against the Gitlab repository

```
elf@417fd161b3c8:~$ trufflehog git https://haugfactory.com/asnowball/aws_scripts.git
🐷🔑🐷  TruffleHog. Unearth your secrets. 🐷🔑🐷

Found unverified result 🐷🔑❓
Detector Type: AWS
Decoder Type: PLAIN
Raw result: AKIAAIDAYRANYAHGQOHD
File: put_policy.py
Email: asnowball <alabaster@northpolechristmastown.local>
Repository: https://haugfactory.com/asnowball/aws_scripts.git
Timestamp: 2022-09-07 07:53:12 -0700 -0700
Line: 6
Commit: 106d33e1ffd53eea753c1365eafc6588398279b5

Found unverified result 🐷🔑❓
Detector Type: Gitlab
Decoder Type: PLAIN
Raw result: add-a-file-using-the-
Repository: https://haugfactory.com/asnowball/aws_scripts.git
Timestamp: 2022-09-06 19:54:48 +0000 UTC
Line: 14
Commit: 2c77c1e0a98715e32a277859864e8f5918aacc85
File: README.md
Email: alabaster snowball <alabaster@northpolechristmastown.local>

Found unverified result 🐷🔑❓
Detector Type: Gitlab
Decoder Type: BASE64
Raw result: add-a-file-using-the-
Commit: 2c77c1e0a98715e32a277859864e8f5918aacc85
File: README.md
Email: alabaster snowball <alabaster@northpolechristmastown.local>
Repository: https://haugfactory.com/asnowball/aws_scripts.git                                  2
Timestamp: 2022-09-06 19:54:48 +0000 UTC
Line: 14

elf@417fd161b3c8:~$
```

From this output we see that there are two files that might contain secret information

* Line 6 of `put_policy.py`
* Line 14 of `README.md`

The repository is publicly available, so use a browser to access <a href="https://haugfactory.com/asnowball/aws_scripts">https://haugfactory.com/asnowball/aws_scripts</a> in a browser.

Select the put_policy.py file and select the 'History' button.

Here we see that there have been 4 commits to this file, two on 6-Sep-2022, and two on 7-Sep-2022

Browse each of the commits in reverse order using the 'Browse File' button, and we discover the the first commit on 7-Sep-2022 contained AWS keys.

Use these keys to configure and confirm your AWS identity.

??? success "Answer"
    ```
    elf@b6671ab1b76b:~$ aws configure
    AWS Access Key ID [None]: AKIAAIDAYRANYAHGQOHD
    AWS Secret Access Key [None]: e95qToloszIgO9dNBsQMQsc5/foiPdKunPJwc1rL
    Default region name [None]: us-east-1
    Default output format [None]: 
    elf@b6671ab1b76b:~$ aws sts get-caller-identity
    {
        "UserId": "AIDAJNIAAQYHIAAHDDRA",
        "Account": "602123424321",
        "Arn": "arn:aws:iam::602123424321:user/haug"
    }
    elf@b6671ab1b76b:~$
    ```

## Completion

Note that after completing the Trufflehog Search objective the terminal continues on to the Exploitation via AWS CLI objective.

??? quote "Gerty Snowburrow"
    Say, you got it done, didn't you?<br>
    Well now, you might just be able to tackle the other AWS terminal down here.<br>
    It's a bit more involved, but you've got the credentials to get it started now.<br>
    Before you try it, you should know the difference between managed and inline policies.<br>
    Short version: inline policies apply to one identity (user, role, group), and managed policies can be attached to many identities.<br>
    There are different AWS CLI commands to interact with each kind.<br>
    Other than that, the important bit is to know a bit about cloud or IAM privilege escalation.<br>
    Sometimes attackers find access to more resources by just trying things until something works.<br>
    But if they have access to the iam service inside the AWS CLI, they might just be able to ask what access they have!<br>
    You can do it!

