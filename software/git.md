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
