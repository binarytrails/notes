# Update to upstream (across repositories)

    git remote add upstream <git repo url>
    git fetch upstream
    git pull upstream master

# Overwrite by remote

    git fetch --all
    git reset --hard origin/master

# Add modified and deleted to staging

    git add -u

# Forget the staged files

    git restore --staged

# Revert file to a past version

    git checkout hash-id path-to-file

# Checkout a pull-request

    git fetch origin pull/ID/head:BRANCHNAME

# Undo changes on one file

    git checkout -- path/to/file

# Remove commit's from Github

    git reset --soft hash-id

or (where 1 is the number of commits to rollback)

    git reset --soft HEAD~1

and then do:

    git push --force

# Merge & squash

    # To ensure safety go to temporary folder and clone your current master
    git clone git://github.com/<user>/<repo>.git master

    # Create branch for merging and get its code there
    git checkout -b <user>-master master
    git pull git://github.com/<user>/<repo>.git master

    # Go back to master and merge
    git checkout master
    git merge --no-ff <user>-master

    # Squash if needed into one commit (pick for one, squash for others)
    git rebase -i

    # Solve conflicts
    git mergetool --tool vimdiff
    git mergetool --tool meld       # or use something with UI

    # Tag the pull request of GitHub by adding this line to the commit message:
    # 'Merge pull request #<id> from <user>/master'

    git commit --amend              # edit the commit message if needed

    git log                         # verify
    git push origin master          # publish

# Delete

    # local
    git branch -D <name>
    # remote
    git push --delete origin <name>

# Make a patch

    git diff commit1 commit2 > file.patch

# Commit messages

See: [How to Write a Git Commit Message](http://chris.beams.io/posts/git-commit/).
 In short, follow this 7 golden rules:

1. Separate subject from body with a blank line
2. Limit the subject line to 50 characters
3. Capitalize the subject line
4. Do not end the subject line with a period
5. Use the imperative mood in the subject line
6. Wrap the body at 72 characters
7. Use the body to explain what and why vs. how

Imo, 7 is optional depending on the commit size / complexity and project stage.

# Rename submodule

```git mv a b``` - relocate its working tree and adjust the paths in the .gitmodules file

# Different ssh keys for GitHub users

    $ cat ~/.ssh/config

    Host github.com-username
        User git
        IdentityFile /home/user/.ssh/id_rsa
        IdentitiesOnly yes
        Hostname ssh.github.com
        Port 443

    $ cd repository; cat .git/config
    
    ...
    [remote "origin"]
        url = git@github.com-username:username/repository.git
    ...


# Removing sensitive data from commits

        git filter-branch --force --index-filter \
          "git rm --cached --ignore-unmatch PATH-TO-YOUR-FILE-WITH-SENSITIVE-DATA" \
          --prune-empty --tag-name-filter cat -- --all

    https://help.github.com/en/github/authenticating-to-github/removing-sensitive-data-from-a-repository
