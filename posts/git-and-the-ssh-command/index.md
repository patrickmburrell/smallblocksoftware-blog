<!--
.. title: Git and the SSH Command
.. slug: git-and-the-ssh-command
.. date: 2021-03-28 13:47:15 UTC-04:00
.. tags: git
.. category: softwaredev
.. link:
.. description:
.. type: text
.. nocomments: True
-->

**TLDR:** I have two GitHub accounts, each with its own key pair. How do I tell `git` which key to use for a given repo?

I use two separate software identities: one for my "work self" and one for my "personal self." I may write a future post about how I came to that practice, but today I'm going to describe how it resulted in a problem and how I solved it.

<!-- TEASER_END -->

As I've recently become more enamored with all things on the command line, I've done more of my work in a terminal ([iTerm2](https://iterm2.com)) by typing commands for SSH connections and GitHub repos. These two areas intersect with the use of public/private key pairs.

I use many of these key pairs - pretty much one for each service or server I use. I wouldn't use one key for all the buildings, rooms, and cars I need to access. I don't see why I'd use only one key pair either. If one of my keys is ever compromised, I will not unintentionally open up all the resources I securely access.

So I use one key pair for the repos in my personal GitHub account and another for the repos at work.

But I was running into strange and unexplained git errors sometimes. I think there is probably an understandable explanation for these errors that seemed to occur randomly. Still, because my access to work repos was affected, I didn't spend much time trying to understand why. I just needed a solution quickly.

The GitHub error messages suggested that I didn't have my keys configured correctly. However, when executed `ssh -T github_personal_pmb` (where `github_personal_pmb` is a host name in my `~/.ssh/config` file), I got this response which confirmed that they were:

```text
Hi patrickmburrell! You’ve successfully authenticated, but GitHub does not provide shell access.
```

After much time googling for this error message:

```text
git@github.com: Permission denied (publickey).
fatal: Could not read from remote repository.

Please make sure you have the correct access rights and the repository exists.
```

I finally just asked Google the basic question ("how do I tell git which key to use") and found [this Super User question](https://superuser.com/questions/232373/hot-to-tell-git-which-private-key-to-use) that led me to a solution. I learned I could specify for each repo exactly the SSH command - _including which private key to use_ - to run for the git actions for that repo. Awesome!

Here is the command I issued to specify the SSH command for a given repo:

```shell
git config core.sshCommand "ssh -i ~/.ssh/[private key filename] -F /dev/null"
```

I issued this command to verify that my configuration change succeeded:

```shell
git config -l
```

I hope this helps someone get to their solution a bit quicker than it took me.
