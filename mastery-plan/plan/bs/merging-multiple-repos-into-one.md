# Merging multiple repo's into one

So I am creating a gitfolio site which will display all my public repositories in a nice UI. While creating it I realized that there are a lot of single program repositories (mostly written in python ) which can easily be merged into a single repository which I can name "python-scripts". These are the steps I followed:

### Creating the Main repository and cloning it :

![Created a  new repo in github where all the repo's will be stored](<../../../.gitbook/assets/image (59).png>)

![Copy this URL ](<../../../.gitbook/assets/image (60).png>)

![clone the repo in your machine](<../../../.gitbook/assets/image (61).png>)

Now we are going to use a new git concept called as "submodule" . More here :[https://git-scm.com/docs/git-submodule](https://git-scm.com/docs/git-submodule)

### Adding all the repo's as submodule's in this repo:

Use the command

```
git submodule add <repo url>
```

Now you will be able to see all the repo's in seperate folders:

![All the repos as folders in this main repo](<../../../.gitbook/assets/image (63).png>)

### Commit everything and push to Github

If you run `git status` after running the above command's you should be able to see something like this:

![](<../../../.gitbook/assets/image (64).png>)

&#x20;Its time to add all to stage and commit it.

After `git add .` and `git commit -m "added all repos"`

Your master branch must be clean in git status.

![](<../../../.gitbook/assets/image (65).png>)

Now just push it to github. `git push origin master` should do it if git is properly configured in your system.

![All the repo's will be in a single repo. ](<../../../.gitbook/assets/image (66).png>)



