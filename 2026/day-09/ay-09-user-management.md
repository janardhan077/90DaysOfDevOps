Today I practiced creating users, groups, and managing permissions in Linux.

I created four users: tokyo, berlin, professor, and nairobi.
I also created three groups: developers, admins, and project-team.

Then I assigned users to groups:

tokyo and berlin → developers

professor → admins

tokyo and nairobi → project-team

After that, I created shared directories inside /opt:

/opt/dev-project (permission 775, group: developers)

/opt/admin-panel (permission 770, group: admins)

/opt/team-workspace (permission 775, group: project-team)

I used commands like:
groupadd, useradd -m, usermod -aG, mkdir, chown, chmod, id, and cat /etc/group.

What I learned:

The difference between user owner and group owner.

How 775 and 770 permissions control access.

Why sudo is required for system directories like /opt.

Today was slightly confusing at first, but after practicing multiple times, I now clearly understand Linux user and group management. 
<img width="1392" height="607" alt="Screenshot from 2026-02-21 13-11-41" src="https://github.com/user-attachments/assets/0245d853-6997-4f2e-9aa7-4d995de180b2" />
