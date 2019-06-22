# Vscode keybindings

Before we begin , Lets get something straight &gt; Vscode is THE BEST. 

I never even had a slight urge to ever try even have look at any other IDE after first having my hands on vscode. It was love at first sight and that's going to stay. But you know what's my second love ? Committing to my remote repositories.  

Now Iam nowhere close to being a git-pro \(yet ;\)  Heck I don't even have a second branch on any of my repositories yet leave alone all the stash/squash/rebase thing. But it's a pain to keep repeating all the steps to push to remote every single time. These steps ⬇ 

{% embed url="https://help.github.com/en/articles/pushing-to-a-remote" %}

So I had to figure out a way to make this more convenient for me. Ofcourse I could go my usual way and try to automate it in some way but I decided to use a workaround instead of that. Here it is.

Referring : [https://code.visualstudio.com/docs/editor/versioncontrol](https://code.visualstudio.com/docs/editor/versioncontrol)

### Staging automatically before committing:

`git add .` and then  `git commit -m "<commit-message>"` . EVERYTIME ! HELL NO! 

![](../../../.gitbook/assets/image%20%2845%29.png)

This is where my changes were visible whenever I made any changes in any file.

![](../../../.gitbook/assets/image%20%286%29.png)

Clicking on the files will open a nice working tree where I am easily able to see what was added or what was deleted from the code.

Now these files have to be staged before you can commit. 

![1. Click stage changes 2. The staged changes will be visible](../../../.gitbook/assets/image%20%2854%29.png)

This is great when you only add stage the changes you want to commit. But not us ;\) I almost always stage everything and commit everything as those are the changes I intended to be version controlled. Maybe the above usecase may apply to me in some project but not now. Hence 

> I enabled the feature to auto stage on every commit :\)

So now when I commit everytime , all the changes get staged and are directly committed. 😋 

### Committing and pushing:

![1. Click the commit button 2.Write the commit message and hit enter](../../../.gitbook/assets/image%20%2827%29.png)

It's that straight forward ! and that's what I love about vscode. No wonder they are the best at what they do. 

**Next pushing to remote:**

![](../../../.gitbook/assets/image%20%2831%29.png)

All you have to do is click the push button and all the commit's will be pushed to the remote master branch. Notice the Alt+p Alt+p to the side of the push option ? That's the workaround !

### WORKAROUND:

Now all the above step's are much better than repeating all the commands in the shell/bash . But still there is a lot of clicking and typing involved. There has to be a    workaround !

**Enter keyboard shortcuts:**

{% embed url="https://code.visualstudio.com/docs/getstarted/keybindings" %}

 We have two main command's which we want to use more efficiently :

1. Git commit
2. Git push

As staging is done automatically when I commit I donot have to think about that :\)

**Lets create a keybinding for git commit:**

![Go To File&amp;gt;Preferences&amp;gt;Keyboard Shortcuts](../../../.gitbook/assets/image%20%2820%29.png)

![1. Type git commit in the filter box 2. Click the add button to assign a key binding](../../../.gitbook/assets/image%20%2810%29.png)

 It will open a dialogbox for the commands. Just click the button combination son the keyboard and you are all set.

![I chose Alt+c two times to commit](../../../.gitbook/assets/image%20%2830%29.png)

![](../../../.gitbook/assets/image%20%2838%29.png)

Did the same thing for git push.

Now everytime I come to a point in coding when I want to stage and commit the changes, I just click 'Alt'+'c'2 times and the commit message window appears. I type the commit message and click 'Alt'+'p' 2 times. BOOM! Change pushed to Remote repo :\)

How cool !! Vscode is the best 😄 

Some interesting things going on in the vscode space:

{% embed url="https://github.com/viatsko/awesome-vscode" %}

{% embed url="https://code.visualstudio.com/blogs/2019/05/02/remote-development" %}









