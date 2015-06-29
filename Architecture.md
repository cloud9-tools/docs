# Architecture
## Overview

Cloud9 is a centralized Version Control System designed for hybrid
online/offline use, suitable for Free Software / Open Source development.  The
system is built from multiple layers; from bottom up, these are: raw storage,
resilient storage, named storage, discovery, Version Control Server, and FUSE
client.

![Architecture diagram][diagram]

## FUSE client

The typical user of Cloud9 interacts with it via a [FUSE mount][fuse].  The
FUSE mount provides a convenient and familiar view of the data in Cloud9, as
well as automatic backups of working copies.  The user can use their favorite
text editor or IDE to edit files in their working copy, and each save to disk
will be copied asynchronously to the Cloud9 servers.  The filesystem also
provides read-only views of branches and other users' working copies, and
includes timeline support that allows seeing what a tree looked like e.g. 5
hours ago.

## Version Control Server

Clients, such as the FUSE client, use a [REST][rest] API to communicate with
the Version Control Server.  The VCS keeps track of the `trunk` (the master
branch), shared branches, private branches, and working copies.  All data is
readable to all authenticated users.

## Named storage

A translation layer converts paths (produced by the Version Control Server)
into UUIDs (functioning as "inodes"), and then converts each UUID into a block
hash.

## Resilient storage

A [Reed-Solomon][rs] encoding/recovery layer.

## Raw storage

A [Content-Addressable Storage][cas] backend.

[fuse]: https://en.wikipedia.org/wiki/Filesystem_in_Userspace
[rest]: https://en.wikipedia.org/wiki/Representational_state_transfer
[rs]: https://en.wikipedia.org/wiki/Reed%E2%80%93Solomon_error_correction
[cas]: https://en.wikipedia.org/wiki/Content-addressable_storage
[diagram]: https://docs.google.com/drawings/d/1YkdDia6Ef83NF2p0CwwvKC--67_Lf86bgB1FrhEsrbo/pub?w=960&h=720
