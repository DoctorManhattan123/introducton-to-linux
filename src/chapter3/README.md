# Users and Groups

In Unix-like systems, the concepts of users and groups are fundamental to the system's
security and organization. Users are the entities (people or processes) that interact with the system,
while groups are collections of users that share certain permissions and access rights.

We will first define users and groups and explain them after file permissions.

**Users**

A user is an account representing someone or something that interacts with the system (it will most of the
time be you). Each user has a unique identifier called user id (uid). Every user has its own
home directory (_you should know where the home directory is located by now_), which ensurs that
personal files and settings are kept private.

**Groups**

A group as a collection of users. Each group has a unique identifier called the group id (gid).
Groups simplify permission management. Instead of assigning permissions to each user individually,
you can assign the permission to a group and each user of this group has this permission.
