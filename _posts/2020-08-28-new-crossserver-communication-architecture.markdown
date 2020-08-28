---
layout: post
title: "New Cross-Server Communication Architecture"
date: "2020-08-28 21:18:45 +0300"
author: "Tareq Kirresh"
---
At OpenMyMed, we have a commitment to openness and access to data. As such, we always believe that patients should have full control of their data, and that they should be free to move it around, and should be the only authority that is allowed to grant/revoke access to it.

So we decided that in order to do that, we needed to create a way for access.md servers to contact each other, and for patients to authorize any number of in-provider and out-of-provider doctors to see or manipulate their data.

## The Initial Concept

Initially, we wanted to rework everything into a federated architecture, with a federation server hosted by OpenMyMed and maintained by it, with all access.md servers phoning home for data exchange, verification, and real time communication.

On re-examining this concept, We found out that it really would not be sustainable, and would create problems in the long run with georouted servers, load balancing, and data security issues(namely, encryption during transport through a 3rd party server). This might have also caused regulatory issues with HIPAA and GDPR, as we are transferring data though our servers, which would require extra care on our part and a more complex protocol.

Instead, we looked towards other self-hosted software, and found some inspiration there in how we can get started with cross-provider communication. Anything we did, we decided, needed to

* Not involve a 3rd party in the communication
* Be Approved directly by the patients
* Be verified via a second factor or random code

## Proposed Solution

The proposed solution is a simple protocol that uses server FQDNs in addition to GUUIDs in order to authorize transfers between servers and allow doctors from other servers to look at patient data from other servers(in the cases of referrals, consultations, etc).

### Patient Transfer
![Patient Data Transfer](/assets/img/posts/new_cross-server_communication_architecture/patient_transfer.png)


### Patient Data Access
![Patient Data Access](/assets/img/posts/new_cross-server_communication_architecture/data_access.png)

In this solution, we rely on the uniqueness of GUUIDs across all access.md servers and uniqueness of the server name. This is similar to the model used by Mastodon, where users across instances can communicate with the instance FQDN added to the username to signify that it is off-server.

The solution also incorporates elements from OAuth2, as tokens are granted based on authorization from the user, and those tokens are time-limited and role-limited in nature.
