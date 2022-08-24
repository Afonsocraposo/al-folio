---
layout: page
title: Remote document access with ransomware protection
description: Allows documents to be shared over a public network in a secure fashion.
img: /assets/img/projects/bag/header.png
category: full stack
---

This project was developed within the scope of the Network and Computer Security 2021 course at Instituto Superior Técnico, being graded with 19 values out of 20.

### Demo

<center>
    <iframe width="560" height="315" src="https://www.youtube.com/embed/gQFS-jBWgyk" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
</center>

<br>

### Description

This solution allows users to create projects and to push corresponding local files to a remote server. This is done through a client program, called bag, running in their computer, with which they interact similarly to Git, using commands such as clone, push, and pull.

When they first login with the client, an asymmetric key pair is generated, which is later used for key sharing. At the login, the client receives and stores a refresh token and an access token with a short life window. Users are required to authenticate themselves when accessing the Resources API (RA) using the access token. The RA verifies the access token with the Auth API (AA) and grants access to the user. The refresh token is used to obtain a new access token once the original one expires.

Each project contains its own symmetric key which is generated locally and used to encrypt files before uploading, ensuring confidentiality. This key is shared with collaborators when a user adds them to their project by encrypting it with their public key. To assure integrity of the files pushed and pulled from the server, each commit has its own signature which is verified before writing the data to memory.

To mitigate ransomware attacks, all data in our Resources Server (RS) is periodically backed up. In case of an attack, the backed up data can be recovered. The machines use a Btrfs filesystem, which has built-in filesystem level snapshot support. A script running on the RS takes a read-only snapshot of the data that is intended to be stored periodically using Cron jobs. The snapshot is compressed into a zip file which is transferred to the Backup Server (BS) via a SCP connection.

To ensure that the data stored in the BS is always valid (that is, none of the files have been modified unauthorizedly, such as by encryption through a ransomware attack), a script to check the integrity of the files in the RS is scheduled to run immediately before creating the snapshot. This compares the hash of each file to the corresponding hash stored in the database.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/projects/bag/architecture.png" title="architecture" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Diagram of the proposed architecture.
</div>

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/projects/bag/protocol.png" title="protocol" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Implemented protocol.
</div>
