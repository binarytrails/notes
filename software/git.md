# Revert file to a past version

    git checkout hash-id path-to-file

# Undo changes on one file

    git checkout -- path/to/file

# Remove commit's from Github
    
    git reset --soft hash-id

or (where 1 is the number of commits to rollback)

    git reset --soft HEAD~1
    
and then do:

    git push --force

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

