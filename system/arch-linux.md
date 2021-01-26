# AUR (Arch User Repository)

## Creating package repositories

**If you are creating a new package from scratch, establish a local Git repository and an AUR remote by cloning the intended pkgbase. If the package does not yet exist, the following warning is expected:**

    $ git clone ssh://aur@aur.archlinux.org/pkgbase.git
    Cloning into 'pkgbase'...
    warning: You appear to have cloned an empty repository.
    Checking connectivity... done.

Note: The repository will not be empty if pkgbase matches a deleted package.

**If you already have a package, initialize it as a Git repository if it is not one, and add an AUR remote:**

    $ git remote add label ssh://aur@aur.archlinux.org/pkgbase.git

**Publishing new package content**

*Warning: Your commits will be authored with your global Git name and email address. It is very difficult to change commits after pushing them (FS#45425). If you want to push to the AUR under different credentials, you can change them per package with git config user.name "..." and git config user.email "...".*

When releasing a new version of the packaged software, update the pkgver or pkgrel variables to notify all users that an upgrade is needed. Do not update those values if only minor changes to the PKGBUILD such as the correction of a typo are being published.

Be sure to regenerate .SRCINFO whenever PKGBUILD metadata changes, such as pkgver() updates; otherwise the AUR will not show updated version numbers.

To upload or update a package, add at least PKGBUILD and .SRCINFO, then any additional new or modified helper files (such as .install files or local source files such as patches), commit with a meaningful commit message, and finally push the changes to the AUR.

    $ makepkg --printsrcinfo > .SRCINFO
    $ git add PKGBUILD .SRCINFO
    $ git commit -m "useful commit message"
    $ git push
