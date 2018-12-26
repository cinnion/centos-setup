# centos-setup

A fork of the CentOS setup RPM source, to tweak the package to standardize a
couple of important user and group IDs so that they are idempotent across
install instances.

Actual code will be on branches with names like c7-ka8zrt (e.g. take the original branch and add the suffix).

## Original README

The master branch has no content
 
Look at the c7 branch if you are working with CentOS-7, or the c4/c5/c6 branch for CentOS-4, 5 or 6
 
If you find this file in a distro specific branch, it means that no content has been checked in yet

## Reasoning

Here are details to help you understand the whichness of the why, and the why of
how...

### Why

When dealing users across multiple machines, certain realms such as NFS prior to
NFS v4 expose a user namespace where inconsistencies between a username and the
user ID, or a group name and group ID become evident. With system accounts for
most packages being created ad hoc, and the potential of this inconsistency
coming to light in a painful way.

Two important accounts are among those being created with ad hoc IDs. These are
the following:

* Users:
  * polkitd: The polkitd user is added by the polkit package, along with the
    group of polkitd.
  * sssd: The sssd user is added by the sssd-common package, as well as the
    secondary packages of sssd-ipa and sssd-krb5-common. Along with this user,
    the group of sssd is added with an ad hoc ID.
* Groups:
  * polkitd
  * sssd

### How

These changes are being done via patches being added to the package. This method
has been chosen due to the fact that the packages involved are if not outright
part of a manditory install requirement (e.g. polkit, via tuned), all but
manditory, and installed during the initial system load process, where no other
control of install ordering is possible, this is pretty much the only way to
solve this problem. For a more complete solution, the utilities such as useradd
and groupadd would also become somehow aware either through a list obtained
during pre-install, through actual communications with an identity provider
service, to allow for handling of other system users and groups. The latter
would be ideal, since this would allow for dynamic registering of other system
users/groups within the domain during installs, rather than having a manual
process involved when a new package is added.

The forked version will also be an RPM, with a "micro-version" change
(e.g. 2.8.71-10 will become 2.8.71.1-10), which will be copied into a separate
repository.

## Initial specific changes

* Make the UID and GID of polkit fixed to 999/999
* Make the UID and GID of sssd fixed 998/998

## License

The changes are open-sourced software licensed under the
[Mozilla Public License, v2.0](https://www.mozilla.org/en-US/MPL/2.0/).

In short... feel free to use them as you will, but please give me a
shout out if this directly helped you and you remember the connection.

## Author Information

This changes for package were created 2018 December 25 by [Douglas Needham](https://www.ka8zrt.com/),
using the sources from the [CentOS git repository version](https://git.centos.org/summary/rpms!setup.git) as the starting point.
