
This project contains two special remotes for iRODS: one based on pure
Python, and one using the icommands.  The first is possibly faster,
the second has fewer dependencies.

The data formats of the two should be the same, so they should be
interchangable.

# git-annex special remote for iRODS (python-irodsclient based)

## Caveats

- does not support exporting.

## Usage

You have to set up your iRODS enviroment file using
`~/.irods/irods_environment.json`.  You should run `iinit` yourself.
If you can `ils REMOTE_DIRECTORY`, then you should be able to use
this.  (You can also put your enviroment file location in the
environment variable `IRODS_ENVIRONMENT_FILE`.

This remote does NOT currently handle authentication, but it may in
the future.  You currently have to iinit each time your system
credentials expire.  (In the future, credentials can be managed by
git-annex)

To set up a remote:

```
git-annex initremote $REMOTENAME type=external externaltype=irods directory=REMOTE_DIRECTORY encryption=MODE
```


## Dependencies

- python-irodsclient (`pip install python-irods`) (only tested with latest version)
- annexremote (`pip install annexremote`, https://github.com/Lykos153/AnnexRemote)



# git-annex special remote for iRODS (icommand based

This git-annex specila remote uses iRODS as a backend.  It is a thin
wrapper around the icommands, with minimal additional logic.

iRODS (integrated Rule-based Data management System) is itself a
framework for storing large amount of data by applying different rules
and keeping different metadata and often used for very large amounts
of data.  Thus, it is somewhat similar in concept to git-annex, so
there is a bit of redundancy in combining them.  Still, iRODS
installations are typically large, safe data locations and thus


## Caveats:

- UNTESTED very well.  It passes the internal git-annex test suite,
  and likely won't lose data, but it's possible you might get in an
  inconsistent state and have to recover yourself.

- SLOW for small files: Because it uses icommands, it doesn't keep
  connections alive and spawns very many small programs to do the
  actual work.  For small files, this overhead will become large.
  Currently, the overhead is storage=5 commands, retrieval=1 command
  (overheads are to ensure consistency and safety).  A more efficient
  system could be done with python-irodsclient, and this has roughly
  been started in `git-annex-remote-irods2` in this repo.



## Usage

First, you have to be able to connect to your iRODS server using the
i-commands.  This remote does NOT currently handle authentication,
since it is globally stateful for your system user.  Run `iinit` as
necessary to initialize.  If you can run `ils REMOTE_DIRECTORY`, then
you are ready.  You currently have to iinit every time your system
credentials expire.  (In the future, credentials can be managed by
git-annex)

To set up a remote:

```
git-annex initremote $REMOTENAME type=external externaltype=irods directory=REMOTE_DIRECTORY encryption=MODE
```


## Dependencies

The iRODS icommands (in particular, `ils`, `iget`, `iput`, `imkdir`,
`irm`).

No known limitations on git-annex versions.



# See also

* git-annex: https://git-annex.branchable.com/
* All git-annex special remote options apply here as well: https://git-annex.branchable.com/special_remotes/
* iRODS: https://irods.org/

