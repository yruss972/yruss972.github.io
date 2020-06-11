---
id: 89
title: Listing ZFS Clones using the origin property
date: 2009-10-25T04:36:00-07:00
author: Yonah Russ
layout: revision
guid: http://yonahruss.com/wordpress/2009/10/58-revision.html
permalink: /uncategorized/58-revision.html
---
Recently I created my first ZFS clones but quickly realized that there was no simple way to tell the clones from the regular filesystems. My first instinct was to run &#8216;zfs list -t clone&#8217; similar to &#8216;zfs list -t snapshot&#8217; but this didn&#8217;t work. Maybe it works in newer versions of ZFS.

After some poking around I found the &#8216;origin&#8217; property which sets the clones apart so running something like-

> zfs list -o origin,name,used,avail,refer,mountpoint | grep -v ^- |awk &#8216;{print $2&#8243;\t&#8221;$3&#8243;\t&#8221;$4&#8243;\t&#8221;$5}&#8217;

will get you what you are looking for.

If you haven&#8217;t played with ZFS clones yet, basically they are writable snapshots of a file system.

They are great if you want to copy a lot of data to the side, modify it, and possibly replace the original data, without taking a lot of time or disk space. The ZFS clones take seconds to create, since they don&#8217;t actually copy any data, and they will only store the blocks which have changed since their creation. If you want to replace the original data, you can then transparently promote the clone to be the master filesystem and turn the master into a clone.

The downside of clones is that they are always dependant on the snapshot from which they were created. You can not destroy a snapshot on which a clone is based without destroying the clone.

For the sake of simplicity and since I don&#8217;t usually have disk space issues, I usually prefer to make full copies using ZFS send/recieve but I have definate plans to make more use of ZFS clones in the future.