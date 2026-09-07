# Linux For SOC Analysts

Disclaimer : This roadmap provides <u>**comprehensive Linux knowledge for SOC Analysts, covering the practical Linux skills required for L1 and L2-level work, along with substantial L3-relevant Linux knowledge**</u>. As with level L3, roles become more specialised, such as Threat Hunting, DFIR, and other investigation-focused disciplines, and the required skills become increasingly role- and organization-dependent. These capabilities are developed through real-world investigations, experience, and continuous learning.

Scope : This file is strictly about Linux only. It does not cover other disciplines such as networking, Windows, or broader SOC skills such as log correlation, threat hunting, etc. It also excludes specialised Linux tracks such as kernel exploit development.

Expected Outcome : After completing this roadmap, you should be able to sit on an unfamiliar modern Linux host and reason from `kernel/user-space structure` → `files` → `permissions` → `identities` → `processes` → `execution` → `services` → `scheduling` → `authentication` → `persistence` → `security controls` → `package provenance` → `namespaces/cgroups/containers` → `executable/runtime behavior`, and explain what you found using clear, reproducible evidence.


<br><br><br>






---

# CHAPTER 01 — LINUX OPERATING-SYSTEM MENTAL MODEL

## 1.1 Kernel and user space

- [ ] Kernel space vs user space.
- [ ] What the kernel provides to user programs.
- [ ] System calls as the user/kernel boundary.
- [ ] User-space libraries vs kernel interfaces.
- [ ] GNU/Linux terminology.
- [ ] Distribution vs kernel.
- [ ] ABI/API distinction at a practical level.

## 1.2 Program, process, thread, service

- [ ] Program vs executable file.
- [ ] Program vs process.
- [ ] Process vs thread.
- [ ] Service/daemon vs arbitrary process.
- [ ] PID and PPID.
- [ ] Process lifetime.
- [ ] Parent/child relationships.
- [ ] Process groups and sessions.
- [ ] Foreground/background execution.

## 1.3 Boot and PID 1

- [ ] Firmware/bootloader/kernel/initramfs conceptual sequence.
- [ ] Kernel command line.
- [ ] initramfs conceptual purpose.
- [ ] PID 1.
- [ ] systemd as a common PID 1 implementation.
- [ ] How PID 1 supervises/starts services.

## Verification gate — Chapter 01

**Pass condition:** explain a Linux host from bootloader to a user-launched process in 10 minutes; define kernel, syscall, program, process, thread, service, PID and PPID without notes; explain why PID 1 matters; score **90%+** on 30 scenario questions.

<br><br><br>


---

# CHAPTER 02 — FILESYSTEM HIERARCHY AND FILE OBJECTS

## 2.1 Filesystem hierarchy

- [ ] `/`
- [ ] `/boot`
- [ ] `/etc`
- [ ] `/usr`
- [ ] `/usr/bin`
- [ ] `/usr/sbin`
- [ ] `/usr/lib` and `/usr/lib64` where applicable.
- [ ] `/usr/local`
- [ ] `/var`
- [ ] `/var/log` as a location, not a logging course.
- [ ] `/var/lib`
- [ ] `/run`
- [ ] `/tmp`
- [ ] `/var/tmp`
- [ ] `/home`
- [ ] `/root`
- [ ] `/opt`
- [ ] `/dev`
- [ ] `/proc`
- [ ] `/sys`
- [ ] `/mnt`
- [ ] `/media`

## 2.2 File types

- [ ] Regular files.
- [ ] Directories.
- [ ] Symbolic links.
- [ ] Hard links.
- [ ] Character devices.
- [ ] Block devices.
- [ ] Unix sockets.
- [ ] FIFOs/named pipes.
- [ ] inode concept.
- [ ] Device numbers.
- [ ] Link count.

## 2.3 Path semantics

- [ ] Absolute and relative paths.
- [ ] `.` and `..`.
- [ ] `~` expansion.
- [ ] Path traversal.
- [ ] Symbolic-link resolution.
- [ ] Broken links.
- [ ] `PATH` executable lookup.
- [ ] Explicit path execution.

## Core tools

- [ ] `pwd`
- [ ] `ls`
- [ ] `cd`
- [ ] `file`
- [ ] `stat`
- [ ] `readlink`
- [ ] `realpath`
- [ ] `namei`
- [ ] `find`

## Verification gate — CHAPTER 02

**Pass condition:** classify 25 arbitrary filesystem objects; explain 20 Linux paths; resolve five symlink/hard-link scenarios; use `stat`, `readlink`, `namei` and `find` without notes to determine the real object behind a suspicious path.

<br><br><br>


---

# CHAPTER 03 — FILE METADATA, TIMESTAMPS AND LINKS

## 3.1 Metadata

- [ ] inode number.
- [ ] mode/type.
- [ ] UID/GID.
- [ ] size.
- [ ] blocks.
- [ ] atime.
- [ ] mtime.
- [ ] ctime.
- [ ] birth time where supported.
- [ ] link count.

## 3.2 Timestamp semantics

- [ ] atime vs mtime vs ctime.
- [ ] Why ctime is not creation time.
- [ ] Filesystem support differences for birth time.
- [ ] How operations alter timestamps.
- [ ] Why timestamps are evidence, not authorship proof.

## 3.3 Links

- [ ] Symlink points to a pathname.
- [ ] Hard link points to the same inode.
- [ ] Consequences for deletion/rename.
- [ ] Following symlinks safely.
- [ ] Detecting unexpected links.

## Verification gate — CHAPTER 03

**Pass condition:** construct a file and perform 10 controlled operations; predict which timestamps should change before running `stat`; explain every observed change afterward with no unexplained result.

<br><br><br>


---

# CHAPTER 04 — UNIX PERMISSIONS AND AUTHORIZATION

## 4.1 Traditional permissions

- [ ] Owner/group/other.
- [ ] Read/write/execute on regular files.
- [ ] Read/write/execute on directories.
- [ ] Directory traversal semantics.
- [ ] Numeric modes.
- [ ] Symbolic modes.
- [ ] `umask`.
- [ ] Effective access vs displayed mode.

## 4.2 Special bits

- [ ] SUID.
- [ ] SGID on files.
- [ ] SGID on directories.
- [ ] Sticky bit.
- [ ] How special bits appear in `ls -l`.
- [ ] Security implications of unexpected SUID/SGID files.

## 4.3 Ownership and changes

- [ ] `chown`
- [ ] `chgrp`
- [ ] `chmod`
- [ ] Recursive changes and their danger.
- [ ] Ownership of scripts and executables.

## 4.4 Permission reasoning

- [ ] Determine whether a user can read a file.
- [ ] Determine whether a user can create/delete an entry in a directory.
- [ ] Explain why file write and directory write are different.
- [ ] Trace permissions across each component of a path.

## Verification gate — CHAPTER 04

**Pass condition:** create 12 permission scenarios including directory-only execute, SUID, SGID, sticky bit and `umask`; predict access for at least five users; prove the result using `ls`, `stat`, `namei` and `id` with **100% scenario accuracy**.

<br><br><br>


---

# CHAPTER 05 — ACLs, EXTENDED ATTRIBUTES AND FILE SECURITY METADATA

## 5.1 POSIX ACLs

- [ ] Why ACLs exist.
- [ ] Access ACL vs default ACL.
- [ ] Named user entries.
- [ ] Named group entries.
- [ ] ACL mask.
- [ ] Effective rights.
- [ ] How ACLs appear in `ls -l`.
- [ ] `getfacl`.
- [ ] `setfacl`.

## 5.2 Extended attributes

- [ ] xattr concept.
- [ ] `getfattr` / `setfattr`.
- [ ] Security xattrs.
- [ ] `security.capability` concept.
- [ ] Interaction with security frameworks.

## 5.3 File attributes

- [ ] `lsattr`.
- [ ] `chattr`.
- [ ] Immutable.
- [ ] Append-only.
- [ ] Other common filesystem attributes.

## Verification gate — CHAPTER 05

**Pass condition:** create an ACL that grants a named user access not visible in ordinary mode bits; create a default ACL on a directory; create and inspect at least three xattrs; demonstrate an immutable-file failure and restore the state.

<br><br><br>


---

# CHAPTER 06 — USERS, GROUPS, UID/GID AND ACCOUNT DATABASES

## 6.1 Identity model

- [ ] UID.
- [ ] GID.
- [ ] Supplementary groups.
- [ ] Primary group.
- [ ] Root UID 0.
- [ ] Service/system accounts.
- [ ] Login shells.
- [ ] Disabled/locked account concepts.

## 6.2 Account databases

- [ ] `/etc/passwd` structure.
- [ ] `/etc/shadow` purpose.
- [ ] `/etc/group` structure.
- [ ] `/etc/gshadow` purpose.
- [ ] `/etc/shells`.
- [ ] `/etc/login.defs` high-level role.

## 6.3 Commands

- [ ] `id`
- [ ] `whoami`
- [ ] `groups`
- [ ] `getent passwd`
- [ ] `getent group`
- [ ] `passwd`
- [ ] `chage`
- [ ] `useradd`
- [ ] `usermod`
- [ ] `userdel`

## 6.4 Account investigation

- [ ] Determine UID/GID.
- [ ] Determine supplementary groups.
- [ ] Determine login shell.
- [ ] Determine whether an account is privileged.
- [ ] Determine whether the identity is local or supplied by another NSS source.

## Verification gate — CHAPTER 06

**Pass condition:** create three users with different groups and shells; explain every resulting line in `/etc/passwd` and `/etc/group`; prove actual group membership with `id`; demonstrate local-vs-NSS resolution using `getent`.

<br><br><br>


---

# CHAPTER 07 — PAM, NSS, SSSD AND LINUX AUTHENTICATION ARCHITECTURE

## 7.1 PAM

- [ ] PAM as a pluggable authentication framework.
- [ ] PAM service files.
- [ ] `auth` management group.
- [ ] `account` management group.
- [ ] `password` management group.
- [ ] `session` management group.
- [ ] Control flags at a conceptual and practical level.
- [ ] PAM-aware vs non-PAM applications.
- [ ] Risks of changing PAM configuration.

## 7.2 NSS

- [ ] Name Service Switch concept.
- [ ] `/etc/nsswitch.conf`.
- [ ] Local files vs other identity sources.
- [ ] `getent` as the correct “how does this host resolve it?” tool.

## 7.3 SSSD/central identity

- [ ] SSSD purpose.
- [ ] Identity lookup vs authentication vs authorization.
- [ ] CHAPTER-backed identities at a high level.
- [ ] Why an enterprise Linux host can have users absent from `/etc/passwd`.

## Verification gate — CHAPTER 07

**Pass condition:** diagram the authentication/name-resolution path for a local user and a centrally resolved identity; explain where PAM and NSS fit; identify which configuration file or service controls each stage in a test VM.

<br><br><br>


---

# CHAPTER 08 — ROOT, SUDO, SU AND PRIVILEGE TRANSITIONS

## 8.1 Root

- [ ] UID 0.
- [ ] Effective vs real identity concept.
- [ ] Privileged process vs ordinary process.
- [ ] Why “root” is not the same as “unrestricted in every security model”.

## 8.2 sudo

- [ ] `sudo`.
- [ ] `sudo -l`.
- [ ] `/etc/sudoers`.
- [ ] `/etc/sudoers.d/`.
- [ ] `visudo`.
- [ ] Run-as user/group.
- [ ] NOPASSWD concept.
- [ ] Command restrictions.
- [ ] Environment restrictions at a practical level.
- [ ] Timestamp/caching concept.

## 8.3 su and privilege change

- [ ] `su`.
- [ ] Login shell vs non-login shell.
- [ ] Credential checks vs authorization.
- [ ] Session/environment changes.

## 8.4 SOC investigation

- [ ] Identify who is privileged.
- [ ] Determine effective identity of a process.
- [ ] Determine whether a command was executed through `sudo`/`su`.
- [ ] Recognize risky sudoers entries.

## Verification gate — CHAPTER 08

**Pass condition:** create safe sudo scenarios, including a restricted command and a deliberate NOPASSWD test; explain the resulting effective UID, environment and authorization; audit your own configuration for unintended privilege.

<br><br><br>


---

# CHAPTER 09 — PROCESS MODEL AND LIFECYCLE

## 9.1 Process creation

- [ ] `fork()` conceptual semantics.
- [ ] `clone()` conceptual role.
- [ ] `execve()` as replacement of process image.
- [ ] Parent/child relationships.
- [ ] Reparenting/orphans.
- [ ] Zombies.
- [ ] Process exit status.

## 9.2 Process identity

- [ ] PID.
- [ ] PPID.
- [ ] Real/effective/saved credentials.
- [ ] Group credentials.
- [ ] Process start time.
- [ ] Process state.

## 9.3 Signals

- [ ] Signal concept.
- [ ] `SIGTERM`.
- [ ] `SIGKILL`.
- [ ] `SIGHUP`.
- [ ] `SIGINT`.
- [ ] `SIGSTOP` / `SIGCONT`.
- [ ] Signal handling vs default action.
- [ ] Limitations of using signals as evidence.

## Core tools

- [ ] `ps`
- [ ] `pstree`
- [ ] `pgrep`
- [ ] `pidof`
- [ ] `top`
- [ ] `kill`
- [ ] `pkill`

## Verification gate — CHAPTER 09

**Pass condition:** build a parent/child process tree, intentionally create a zombie, explain how it appears, identify process credentials, and trace a program from creation to `execve()` using normal Linux tools.

<br><br><br>


---

# CHAPTER 10 — /PROC: PROCESS AND KERNEL OBSERVABILITY

## 10.1 Essential `/proc/<pid>` interfaces

- [ ] `/proc/<pid>/cmdline`
- [ ] `/proc/<pid>/exe`
- [ ] `/proc/<pid>/cwd`
- [ ] `/proc/<pid>/root`
- [ ] `/proc/<pid>/fd/`
- [ ] `/proc/<pid>/status`
- [ ] `/proc/<pid>/stat`
- [ ] `/proc/<pid>/maps`
- [ ] `/proc/<pid>/environ`
- [ ] `/proc/<pid>/mountinfo`
- [ ] `/proc/<pid>/ns/`
- [ ] `/proc/<pid>/cgroup`

## 10.2 System-wide `/proc`

- [ ] `/proc/cmdline`.
- [ ] `/proc/modules`.
- [ ] `/proc/mounts`.
- [ ] `/proc/net` as a Linux interface only.
- [ ] `/proc/sys`.
- [ ] `/proc/meminfo`.
- [ ] `/proc/cpuinfo`.

## 10.3 Evidence interpretation

- [ ] Distinguish kernel-generated views from files on disk.
- [ ] Understand that `/proc` can change while you inspect it.
- [ ] Understand process command-line mutability.
- [ ] Use `/proc/<pid>/exe` to identify the executable object.
- [ ] Use `/proc/<pid>/fd` to identify open files and sockets.

## Verification gate — CHAPTER 10

**Pass condition:** for five running processes, determine executable, cwd, root, command line, credentials, open file descriptors, memory mappings, namespaces and cgroup from `/proc` without using a GUI; document what each field proves and what it cannot prove.

<br><br><br>


---

# CHAPTER 11 — FILE DESCRIPTORS, OPEN FILES AND PROCESS-RESOURCE RELATIONSHIPS

## 11.1 File descriptor model

- [ ] FD 0/1/2.
- [ ] Per-process descriptor table concept.
- [ ] File description vs descriptor.
- [ ] Descriptor duplication concept.
- [ ] Pipes.
- [ ] Sockets as file descriptors.
- [ ] Deleted-but-open files.

## 11.2 Tools

- [ ] `lsof`.
- [ ] `fuser`.
- [ ] `/proc/<pid>/fd`.
- [ ] `readlink` on file descriptors.

## 11.3 Investigation patterns

- [ ] Find which process has a file open.
- [ ] Find which process uses a device/file.
- [ ] Find deleted-but-open files.
- [ ] Correlate a process with its open object set.

## Verification gate — CHAPTER 11

**Pass condition:** create a deleted-but-open file, identify it using both `/proc` and `lsof`, explain why it remains accessible, then terminate the owning process and observe what changes.

<br><br><br>


---

# CHAPTER 12 — SHELLS AND COMMAND EXECUTION

## 12.1 Bash/sh fundamentals

- [ ] Shell vs terminal.
- [ ] Interactive vs non-interactive shell.
- [ ] Login vs non-login shell.
- [ ] Command parsing.
- [ ] Word splitting.
- [ ] Quoting.
- [ ] Globbing.
- [ ] Variables.
- [ ] Command substitution.
- [ ] Exit status.
- [ ] Pipelines.
- [ ] Redirection.
- [ ] Background jobs.
- [ ] Subshells.

## 12.2 Streams

- [ ] stdin.
- [ ] stdout.
- [ ] stderr.
- [ ] `>`, `>>`, `<`, `2>`, `2>&1`.
- [ ] Pipe behavior.

## 12.3 Environment

- [ ] `env` / `printenv`.
- [ ] `PATH`.
- [ ] `HOME`.
- [ ] `USER`.
- [ ] `LOGNAME`.
- [ ] `SHELL`.
- [ ] `PWD`.
- [ ] Environment inheritance.
- [ ] Per-process environment.

## 12.4 Bash startup files

- [ ] `/etc/profile`.
- [ ] `~/.profile`.
- [ ] `~/.bash_profile`.
- [ ] `~/.bash_login`.
- [ ] `~/.bashrc`.
- [ ] Login/non-login startup differences.
- [ ] Why startup files can be persistence locations.

## Verification gate — CHAPTER 12

**Pass condition:** write one Bash script that uses quoting, variables, conditionals, a pipeline and redirection; demonstrate the difference between interactive/login/non-login startup; explain every process/environment transition involved.

<br><br><br>


---

# CHAPTER 13 — TEXT PROCESSING FOR LOCAL LINUX INVESTIGATION

## 13.1 grep

- [ ] Literal matching.
- [ ] Case-insensitive matching.
- [ ] Inversion.
- [ ] Line numbers.
- [ ] Recursive search.
- [ ] Extended regular expressions.
- [ ] Word boundaries.
- [ ] Character classes.

## 13.2 awk

- [ ] Fields.
- [ ] Field separators.
- [ ] Conditions.
- [ ] Printing selected fields.
- [ ] Arrays/counters.
- [ ] BEGIN/END.
- [ ] Simple aggregation.

## 13.3 sed

- [ ] Print selected ranges.
- [ ] Search/print patterns.
- [ ] Substitution.
- [ ] In-place editing awareness.

## 13.4 Supporting CLI tools

- [ ] `cut`
- [ ] `sort`
- [ ] `uniq`
- [ ] `tr`
- [ ] `wc`
- [ ] `xargs`
- [ ] `tee`
- [ ] `printf`

**Scope note:** these tools are learned for local Linux evidence manipulation, not as a generic text-processing curriculum.

## Verification gate — CHAPTER 13

**Pass condition:** given five unfamiliar local text datasets, extract fields, filter conditions, count categories and produce a reproducible result using only standard CLI tools; complete the tasks within 20 minutes with no copied commands.

<br><br><br>


---

# CHAPTER 14 — SSH AND REMOTE ACCESS ON LINUX

## 14.1 OpenSSH architecture

- [ ] `sshd`.
- [ ] SSH client.
- [ ] Server configuration.
- [ ] Session creation.
- [ ] Authentication methods.

## 14.2 Configuration and key locations

- [ ] `/etc/ssh/`
- [ ] `sshd_config`.
- [ ] `~/.ssh/`.
- [ ] `authorized_keys`.
- [ ] `known_hosts`.
- [ ] File ownership/permissions of SSH material.

## 14.3 SSH keys

- [ ] Public vs private key.
- [ ] Key authorization concept.
- [ ] Key file permissions.
- [ ] Authorized-key options at a practical awareness level.

## 14.4 Investigation

- [ ] Identify SSH server configuration.
- [ ] Identify authorized keys.
- [ ] Determine which users have interactive shells.
- [ ] Trace an SSH-created shell into its child processes.
- [ ] Recognize SSH-based persistence.

## Verification gate — CHAPTER 14

**Pass condition:** configure two test users with different SSH access properties; use `sshd -T` to inspect effective server configuration; trace a controlled SSH login from `sshd` to shell; identify the exact authorized key responsible for access.

<br><br><br>


---

# CHAPTER 15 — SYSTEMD CORE MODEL

## 15.1 Unit types

- [ ] `.service`
- [ ] `.socket`
- [ ] `.timer`
- [ ] `.mount`
- [ ] `.path`
- [ ] `.target`
- [ ] `.slice`
- [ ] `.scope`

## 15.2 Service state

- [ ] loaded.
- [ ] enabled.
- [ ] active.
- [ ] failed.
- [ ] static/masked concepts.

## 15.3 Unit files and search paths

- [ ] `/etc/systemd/system/`.
- [ ] `/run/systemd/system/`.
- [ ] vendor/system unit locations.
- [ ] Drop-in directories.
- [ ] Override precedence concept.

## 15.4 Commands

- [ ] `systemctl status`.
- [ ] `systemctl list-units`.
- [ ] `systemctl list-unit-files`.
- [ ] `systemctl cat`.
- [ ] `systemctl show`.
- [ ] `systemctl is-enabled`.
- [ ] `systemctl is-active`.
- [ ] `systemctl daemon-reload`.

## Verification gate — CHAPTER 15

**Pass condition:** create a harmless custom service, enable/disable it, add a drop-in override, explain effective configuration with `systemctl cat/show`, and remove it cleanly; repeat once on the second distribution.

<br><br><br>


---

# CHAPTER 16 — SYSTEMD ADVANCED SERVICE EXECUTION AND SANDBOXING

## 16.1 Execution directives

- [ ] `ExecStart`.
- [ ] `ExecStartPre`.
- [ ] `ExecStartPost`.
- [ ] `ExecStop`.
- [ ] `User=`.
- [ ] `Group=`.
- [ ] Working directory directives.
- [ ] Environment directives.
- [ ] Restart policies.

## 16.2 Dependencies and activation

- [ ] `Requires=`.
- [ ] `Wants=`.
- [ ] `After=`.
- [ ] `Before=`.
- [ ] `RequiresMountsFor=` concept.
- [ ] Socket activation concept.
- [ ] Path activation concept.

## 16.3 Sandboxing/security properties

- [ ] `NoNewPrivileges=`.
- [ ] `PrivateTmp=`.
- [ ] `ProtectSystem=`.
- [ ] `ProtectHome=`.
- [ ] `ReadOnlyPaths=`.
- [ ] `InaccessiblePaths=`.
- [ ] `CapabilityBoundingSet=`.
- [ ] `RestrictAddressFamilies=` awareness.
- [ ] `SystemCallFilter=` awareness.
- [ ] `DynamicUser=` awareness.
- [ ] `ProtectKernel*` awareness.

## 16.4 Analysis tools

- [ ] `systemd-analyze`.
- [ ] `systemd-analyze security`.
- [ ] `systemd-delta`.
- [ ] `systemd-analyze verify`.

## Verification gate — CHAPTER 16

**Pass condition:** inspect two services and explain at least 15 execution/security directives; deliberately create one vulnerable and one hardened test service; compare their effective systemd security posture and explain why each control changes process capability/access.

<br><br><br>


---

# CHAPTER 17 — CRON, AT AND SYSTEMD TIMERS

## 17.1 Cron model

- [ ] User crontabs.
- [ ] `/etc/crontab`.
- [ ] `/etc/cron.d/`.
- [ ] Periodic directories.
- [ ] Environment for cron jobs.
- [ ] Scheduling syntax.

## 17.2 `at`

- [ ] One-time scheduling concept.
- [ ] Authorization/allow-deny configuration awareness.

## 17.3 systemd timers

- [ ] `.timer` units.
- [ ] `OnCalendar=`.
- [ ] `OnBootSec=`.
- [ ] `OnUnitActiveSec=`.
- [ ] Timer/service pairing.
- [ ] Persistent timers.

## 17.4 Investigation

- [ ] Enumerate scheduled execution.
- [ ] Identify what executes.
- [ ] Identify which account executes it.
- [ ] Trace scheduler → script → process.
- [ ] Identify suspicious scheduled persistence.

## Verification gate — CHAPTER 17

**Pass condition:** create one harmless cron job and one systemd timer, verify both execute as intended, then identify both from the host without using your creation notes.

<br><br><br>


---

# CHAPTER 18 — LINUX LOGGING ARCHITECTURE (HOST-SIDE ONLY)

**Scope restriction:** this module teaches Linux logging components and interfaces, not log-reading methodology, SIEM, correlation, detection engineering, or SOC workflow.

## 18.1 Journald

- [ ] `systemd-journald` role.
- [ ] Journal storage concept.
- [ ] Volatile vs persistent journal.
- [ ] Journal metadata.
- [ ] `journalctl` query mechanics.
- [ ] Service/boot/PID/UID filtering concepts.

## 18.2 Syslog family

- [ ] syslog concept.
- [ ] rsyslog/syslog-ng awareness.
- [ ] Traditional file logging.
- [ ] Distribution differences.

## 18.3 Rotation/retention concepts

- [ ] logrotate role.
- [ ] File rotation.
- [ ] Retention vs persistence.
- [ ] Why local logging may be absent or incomplete.

## Verification gate — CHAPTER 18

**Pass condition:** identify which logging mechanisms are enabled on both lab distributions; determine whether journal storage is persistent; identify the process/service responsible for each tested log path; explain the difference between journald and a syslog daemon.

<br><br><br>


---

# CHAPTER 19 — LINUX AUDIT SYSTEM / AUDITD

## 19.1 Architecture

- [ ] Kernel-side audit processing.
- [ ] `auditd` userspace daemon.
- [ ] `auditctl`.
- [ ] `ausearch`.
- [ ] `aureport`.
- [ ] Rule files.
- [ ] `/var/log/audit/` as a common location.

## 19.2 Event identity fields

- [ ] `auid`.
- [ ] `uid`.
- [ ] `euid`.
- [ ] `gid`.
- [ ] `pid`.
- [ ] `ppid`.
- [ ] `exe`.
- [ ] `comm`.
- [ ] `syscall`.
- [ ] `success`.
- [ ] `exit`.
- [ ] event IDs/serial relationships.

## 19.3 Rule types

- [ ] Control rules.
- [ ] Filesystem/path rules.
- [ ] Syscall rules.
- [ ] Architecture filters.
- [ ] Keys.
- [ ] Persistent rules in `/etc/audit/rules.d/`.
- [ ] `augenrules` awareness.

## 19.4 Practical audit mastery

- [ ] Query by process.
- [ ] Query by executable.
- [ ] Query by user/audit user ID.
- [ ] Query by time.
- [ ] Search by rule key.
- [ ] Understand multi-record events.
- [ ] Distinguish an audit event's initiating user from a process's current/effective UID.
- [ ] Understand audit coverage depends on configured rules.

## Verification gate — CHAPTER 19

**Pass condition:** write three harmless test rules, trigger the intended events, retrieve them with `ausearch`, identify the initiating identity and executable, explain each major field, and remove the rules cleanly. Then create a test case where no rule means no expected event and explain the visibility limitation.

<br><br><br>


---

# CHAPTER 20 — LINUX SECURITY CAPABILITIES

## 20.1 Concept

- [ ] Why capabilities exist.
- [ ] Capability as a split of traditional superuser privilege.
- [ ] Capability sets at a practical level.
- [ ] Per-thread concept.

## 20.2 Important sets

- [ ] Permitted.
- [ ] Effective.
- [ ] Inheritable.
- [ ] Bounding.
- [ ] Ambient.

## 20.3 Important capabilities

Understand the security significance of:

- [ ] `CAP_CHOWN`
- [ ] `CAP_DAC_OVERRIDE`
- [ ] `CAP_DAC_READ_SEARCH`
- [ ] `CAP_FOWNER`
- [ ] `CAP_NET_ADMIN` conceptually.
- [ ] `CAP_NET_RAW` conceptually.
- [ ] `CAP_SETUID`.
- [ ] `CAP_SETGID`.
- [ ] `CAP_SETPCAP`.
- [ ] `CAP_SYS_ADMIN` as a broad/high-impact capability.
- [ ] `CAP_SYS_PTRACE`.
- [ ] `CAP_SYS_MODULE`.
- [ ] `CAP_KILL`.

## 20.4 Tools/interfaces

- [ ] `getcap`.
- [ ] `setcap`.
- [ ] `/proc/<pid>/status` capability fields.
- [ ] `capsh` awareness where installed.

## 20.5 Execution interaction

- [ ] File capabilities.
- [ ] Capability inheritance across `fork`.
- [ ] Capability recalculation on `execve`.
- [ ] Interaction with user namespaces.

## Verification gate — CHAPTER 20

**Pass condition:** create a harmless file-capability test; inspect the capability on disk and in the process; explain what changed across `execve`; demonstrate one capability-restricted action and one permitted action without relying on UID 0.

<br><br><br>


---

# CHAPTER 21 — SUID/SGID AND PRIVILEGE-BOUNDARY ANALYSIS

## 21.1 Enumeration

- [ ] Enumerate SUID files.
- [ ] Enumerate SGID files.
- [ ] Determine package ownership.
- [ ] Determine whether the file is expected.
- [ ] Inspect permissions and path provenance.

## 21.2 Analysis

- [ ] Understand effective UID changes.
- [ ] Understand why environment handling is security-sensitive.
- [ ] Recognize dangerous locations for privileged executables.
- [ ] Distinguish standard system binaries from unusual local binaries.

## 21.3 SOC use

- [ ] Identify newly introduced SUID/SGID executables.
- [ ] Determine owner and package provenance.
- [ ] Determine whether a process can reach/execute the file.

## Verification gate — CHAPTER 21

**Pass condition:** enumerate SUID/SGID files on both distributions, classify a sample into expected/unexpected, prove package ownership where possible, and explain the effective-identity consequence of executing a controlled test binary.

<br><br><br>


---

# CHAPTER 22 — EXECUTABLES, ELF AND BINARY IDENTITY

## 22.1 ELF fundamentals

- [ ] ELF file format.
- [ ] 32-bit vs 64-bit.
- [ ] Architecture.
- [ ] ELF header.
- [ ] Program headers.
- [ ] Section headers.
- [ ] Entry point.
- [ ] Interpreter.
- [ ] Dynamic section.
- [ ] Symbols at a practical level.

## 22.2 File-identification tools

- [ ] `file`.
- [ ] `readelf`.
- [ ] `objdump` awareness.
- [ ] `nm` awareness.
- [ ] `strings`.
- [ ] `sha256sum`.

## 22.3 SOC questions

- [ ] What architecture is this binary?
- [ ] Is it dynamically or statically linked?
- [ ] What interpreter does it request?
- [ ] Which shared libraries are referenced?
- [ ] Is the binary stripped?
- [ ] Does the binary location match normal packaging?

## Verification gate — CHAPTER 22

**Pass condition:** take five ordinary Linux binaries and produce a compact identity profile for each: type, architecture, interpreter, dynamic dependencies, hash and package owner. Explain every field without notes.

<br><br><br>


---

# CHAPTER 23 — DYNAMIC LINKING, THE ELF LOADER AND SHARED LIBRARIES

## 23.1 Loader model

- [ ] Dynamic linker/loader concept.
- [ ] ELF interpreter.
- [ ] Shared objects (`.so`).
- [ ] Library search paths.
- [ ] `/etc/ld.so.conf` and related configuration.
- [ ] `ldconfig` concept.
- [ ] `/etc/ld.so.cache` concept.

## 23.2 Environment influence

- [ ] `LD_LIBRARY_PATH`.
- [ ] `LD_PRELOAD`.
- [ ] Secure-execution behavior awareness.
- [ ] `/etc/ld.so.preload`.

## 23.3 Tools

- [ ] `ldd` with the security caution that it is not universally safe for untrusted binaries.
- [ ] `readelf -d`.
- [ ] `ldconfig -p`.
- [ ] `/proc/<pid>/maps`.
- [ ] `/proc/<pid>/smaps` awareness.

## Verification gate — CHAPTER 23

**Pass condition:** create two harmless shared libraries, demonstrate ordinary dynamic loading and controlled `LD_PRELOAD` behavior in a lab process, identify the loaded library in `/proc/<pid>/maps`, then explain why privileged binaries treat environment-based loading differently.

<br><br><br>


---

# CHAPTER 24 — STRACE, SYSCALL OBSERVATION AND PROCESS BEHAVIOR

## 24.1 Syscall model

- [ ] What a syscall is.
- [ ] User-space call vs kernel syscall.
- [ ] Common file/process syscalls conceptually.
- [ ] `execve`.
- [ ] `openat`/file-opening family awareness.
- [ ] `read`/`write`.
- [ ] `close`.
- [ ] `clone`/`fork` family.
- [ ] `socket` family awareness only as Linux process behavior.

## 24.2 strace

- [ ] Attach to a process where permitted.
- [ ] Trace command execution.
- [ ] Filter by syscall.
- [ ] Follow forks.
- [ ] Show timestamps.
- [ ] Decode paths.
- [ ] Interpret return values and `errno`.

## 24.3 Limitations

- [ ] Permission/security restrictions.
- [ ] Process state changes while tracing.
- [ ] Performance/observer effect.
- [ ] Why absence of a syscall in a short trace is not proof it never happened.

## Verification gate — CHAPTER 24

**Pass condition:** trace a controlled program and identify at least 15 meaningful syscalls; explain how a child process and `execve` appear; use syscall filtering to answer three questions without reading source code.

<br><br><br>


---

# CHAPTER 25 — PROCESS MEMORY MAPS AND RUNTIME OBJECTS

## 25.1 `/proc/<pid>/maps`

- [ ] Executable mappings.
- [ ] Shared-library mappings.
- [ ] Anonymous mappings.
- [ ] Read/write/execute permissions.
- [ ] Private/shared mappings.
- [ ] Pathnames on mappings.

## 25.2 `/proc/<pid>/smaps` awareness

- [ ] Resident vs virtual memory.
- [ ] Mapping-level details.
- [ ] Shared/private accounting.

## 25.3 Runtime investigation

- [ ] Identify unexpected shared libraries.
- [ ] Identify anonymous executable mappings as an indicator requiring investigation.
- [ ] Correlate mapping paths with executable/library identity.

## Verification gate — CHAPTER 25

**Pass condition:** inspect five processes; classify every major mapping category; identify the interpreter and shared libraries; document at least three examples where the mapping view adds information not visible in `ps` alone.

<br><br><br>


---

# CHAPTER 26 — PTR​​ACE AND PROCESS-TO-PROCESS CONTROL

## 26.1 ptrace concept

- [ ] Process tracing/debugging model.
- [ ] Attaching to another process.
- [ ] Reading/writing process state conceptually.
- [ ] Security restrictions around ptrace.
- [ ] `CAP_SYS_PTRACE` significance.

## 26.2 SOC implications

- [ ] Why unexpected process tracing deserves investigation.
- [ ] Why developer/debugger activity can look suspicious.
- [ ] Understand legitimate uses vs malicious abuse.

## 26.3 Supporting interfaces

- [ ] `ptrace` man page.
- [ ] `/proc/sys/kernel/yama/ptrace_scope` awareness where supported.
- [ ] `gdb` awareness, not advanced debugging.

## Verification gate — CHAPTER 26

**Pass condition:** trace a harmless test process, explain which permission/security checks allow or prevent it, and document how `CAP_SYS_PTRACE` and the host's ptrace policy affect the result.

<br><br><br>


---

# CHAPTER 27 — LINUX NAMESPACES

## 27.1 Namespace types

- [ ] Mount namespace.
- [ ] PID namespace.
- [ ] Network namespace concept only as a Linux isolation mechanism.
- [ ] User namespace.
- [ ] UTS namespace.
- [ ] IPC namespace.
- [ ] Cgroup namespace.
- [ ] Time namespace awareness.

## 27.2 Core concepts

- [ ] Namespace isolation.
- [ ] Process membership.
- [ ] Nested namespaces.
- [ ] Namespace-aware `/proc` views.
- [ ] `setns`.
- [ ] `unshare`.
- [ ] `nsenter`.
- [ ] `lsns`.

## 27.3 User namespaces

- [ ] UID mapping.
- [ ] Namespace root vs host root.
- [ ] Capability scope.
- [ ] `uid_map` / `gid_map` awareness.

## 27.4 Investigation

- [ ] Determine a process's namespaces via `/proc/<pid>/ns`.
- [ ] Compare host namespace and process namespace.
- [ ] Recognize that a process can have a different view of PIDs/mounts/users.

## Verification gate — CHAPTER 27

**Pass condition:** create a controlled namespace scenario; prove that two processes can see different PIDs or mounts; identify their namespace inode IDs; explain why a host-level tool can miss context that is visible inside another namespace.

<br><br><br>


---

# CHAPTER 28 — CGROUPS AND RESOURCE/PROCESS HIERARCHY

## 28.1 Concepts

- [ ] cgroup purpose.
- [ ] Hierarchical process grouping.
- [ ] cgroup v1 vs v2 awareness.
- [ ] Controllers.
- [ ] Delegation concept.
- [ ] Resource accounting vs control.

## 28.2 Linux interfaces

- [ ] `/proc/<pid>/cgroup`.
- [ ] cgroup filesystem concept.
- [ ] systemd slices/scopes as cgroup users.
- [ ] Container cgroups at a high level.

## 28.3 SOC use

- [ ] Identify the cgroup associated with a process.
- [ ] Distinguish ordinary service hierarchy from container-style hierarchy.
- [ ] Understand why cgroups help attribute processes to services/containers.

## Verification gate — CHAPTER 28

**Pass condition:** identify the cgroup path for five processes; map at least three processes to their owning systemd slice/scope/service; explain cgroup v2 basics without notes.

<br><br><br>


---

# CHAPTER 29 — SECCOMP

## 29.1 Concepts

- [ ] Seccomp purpose.
- [ ] Strict mode awareness.
- [ ] Filter mode.
- [ ] BPF-based filtering concept.
- [ ] Syscall allow/deny semantics.
- [ ] `No New Privileges` interaction awareness.

## 29.2 Observation

- [ ] `/proc/<pid>/status` `Seccomp` field.
- [ ] `Seccomp_filters` awareness.
- [ ] systemd `SystemCallFilter=` awareness.
- [ ] Container seccomp profiles concept.

## Verification gate — CHAPTER 29

**Pass condition:** inspect at least three processes and identify their seccomp state; deploy a harmless restricted service in a lab and prove that a selected syscall is affected; explain the difference between “not filtered” and “filtered but allowed”.

<br><br><br>


---

# CHAPTER 30 — LINUX SECURITY MODULES / LSM

## 30.1 LSM model

- [ ] Linux Security Modules concept.
- [ ] LSM hooks.
- [ ] DAC vs MAC.
- [ ] Security policy as an additional decision layer.
- [ ] Multiple security components and distro differences.

## 30.2 SELinux

- [ ] SELinux as an LSM.
- [ ] Enforcing/permissive/disabled concepts.
- [ ] Security contexts.
- [ ] Type/CHAPTER concepts.
- [ ] AVC denial concept.
- [ ] `getenforce`.
- [ ] `sestatus`.
- [ ] `ls -Z`.
- [ ] `ps -Z`.
- [ ] `restorecon` concept.
- [ ] `semanage` awareness.

## 30.3 AppArmor

- [ ] AppArmor as an LSM implementation.
- [ ] Profiles.
- [ ] Enforce vs complain.
- [ ] Profile names.
- [ ] `/etc/apparmor.d/`.
- [ ] `aa-status` / `apparmor_status`.
- [ ] Profile enforcement vs audit-only behavior.

## 30.4 Investigation model

- [ ] Determine whether a process is confined.
- [ ] Determine which policy/profile applies.
- [ ] Distinguish DAC denial from MAC denial.
- [ ] Understand that an application may fail because a security policy blocks it.

## Verification gate — CHAPTER 30

**Pass condition:** on SELinux and AppArmor hosts, identify enforcement state and confinement context; create one harmless policy denial; prove the denial mechanism; explain how DAC and MAC can produce different outcomes.

<br><br><br>


---

# CHAPTER 31 — LINUX AUTHENTICATION SESSION MODEL

## 31.1 Sessions

- [ ] Login session.
- [ ] TTY.
- [ ] PTY.
- [ ] Controlling terminal.
- [ ] Process session ID.
- [ ] Process group.
- [ ] SSH session vs local login vs service-started process.

## 31.2 systemd-logind

- [ ] `loginctl`.
- [ ] Session identifiers.
- [ ] User/session state.
- [ ] User runtime directory concept.

## 31.3 Investigation

- [ ] Determine whether a process is associated with an interactive session.
- [ ] Distinguish a service process from a shell-launched process.
- [ ] Relate a process to a user session where possible.

## Verification gate — CHAPTER 31

**Pass condition:** compare a local terminal session, SSH session and systemd service; identify differences in TTY/PTY/session/process-group state; explain how each process got its environment and parentage.

<br><br><br>


---

# CHAPTER 32 — LINUX PACKAGE MANAGEMENT AND SOFTWARE PROVENANCE

## 32.1 Debian family

- [ ] `apt`.
- [ ] `apt-cache`.
- [ ] `dpkg`.
- [ ] `dpkg-query`.
- [ ] `dpkg -S`.
- [ ] `dpkg -L`.
- [ ] `dpkg --verify`/equivalent verification capabilities where available.

## 32.2 RPM family

- [ ] `dnf`.
- [ ] `rpm`.
- [ ] `rpm -q`.
- [ ] `rpm -ql`.
- [ ] `rpm -qf`.
- [ ] `rpm -V`.

## 32.3 Provenance questions

- [ ] Is this executable package-managed?
- [ ] Which package owns it?
- [ ] Which version is installed?
- [ ] Which files belong to the package?
- [ ] Has the package-managed file changed?
- [ ] Is the file outside normal package paths?
- [ ] Where would locally built software usually live?

## Verification gate — CHAPTER 32

**Pass condition:** pick 20 installed binaries across both distributions; identify package ownership/version for ≥18; detect at least one intentionally modified package-owned test file and explain what verification can and cannot establish.

<br><br><br>


---

# CHAPTER 33 — SOFTWARE INSTALLATION, UPDATES AND TRUST BOUNDARIES

## 33.1 Package lifecycle

- [ ] Install.
- [ ] Upgrade.
- [ ] Remove.
- [ ] Repository concept.
- [ ] Package metadata.
- [ ] Package scripts/hooks concept.
- [ ] Configuration-file handling.

## 33.2 Common local software boundaries

- [ ] `/usr/bin` package-managed expectation.
- [ ] `/usr/local/bin` local-software expectation.
- [ ] `/opt` application-specific software.
- [ ] User-local executable paths.
- [ ] Python/Node/Ruby/etc. environment awareness only where it affects Linux executable provenance.

## 33.3 Integrity reasoning

- [ ] Hash is identity evidence, not provenance by itself.
- [ ] Package verification is not the same as malware detection.
- [ ] A legitimate file can be intentionally changed.
- [ ] An unmodified package can still contain vulnerable/abusable software.

## Verification gate — CHAPTER 33

**Pass condition:** create a provenance report for 10 executables covering path, package owner, version, owner, mode, hash and dynamic interpreter; explain any mismatch without guessing.

<br><br><br>


---

# CHAPTER 34 — KERNEL, MODULES AND HOST SECURITY STATE

## 34.1 Kernel identity

- [ ] `uname`.
- [ ] Kernel release vs version concept.
- [ ] Architecture.
- [ ] Boot command line.
- [ ] `/proc/version` awareness.

## 34.2 Modules

- [ ] Kernel module concept.
- [ ] `lsmod`.
- [ ] `modinfo`.
- [ ] `modprobe` awareness.
- [ ] `/proc/modules`.
- [ ] Module parameters concept.
- [ ] Signed-module policy awareness.

## 34.3 Kernel runtime settings

- [ ] `/proc/sys`.
- [ ] `sysctl`.
- [ ] Difference between runtime and persistent configuration.
- [ ] Security-sensitive kernel settings awareness.

## Verification gate — CHAPTER 34

**Pass condition:** produce a host security-state snapshot containing kernel release, command line, loaded modules and selected relevant `sysctl` values; explain which values are kernel runtime state and which are persistent configuration.

<br><br><br>


---

# CHAPTER 35 — BOOT, INITRAMFS, BOOT PARAMETERS AND EARLY PERSISTENCE

## 35.1 Boot artifacts

- [ ] `/boot` contents.
- [ ] Kernel image.
- [ ] initramfs/initrd concept.
- [ ] Bootloader configuration concept.
- [ ] Kernel command line.

## 35.2 Early-boot execution

- [ ] initramfs execution model at a high level.
- [ ] Transition from initramfs to real root filesystem.
- [ ] systemd/PID 1 handoff.

## 35.3 Investigation relevance

- [ ] Recognize unexpected kernel command-line parameters.
- [ ] Recognize changes to boot artifacts as high-impact.
- [ ] Know that boot-time persistence is conceptually different from ordinary user/service persistence.

## Verification gate — CHAPTER 35

**Pass condition:** explain the complete boot sequence on both lab systems; identify kernel command line and initramfs artifacts; state which artifacts are persistent and which are generated.

<br><br><br>


---

# CHAPTER 36 — MOUNTS, MOUNT NAMESPACES AND FILESYSTEM BOUNDARIES

## 36.1 Mount fundamentals

- [ ] Device vs filesystem vs mount point.
- [ ] `mount`.
- [ ] `findmnt`.
- [ ] `lsblk`.
- [ ] `blkid`.
- [ ] `df`.
- [ ] `/proc/mounts`.
- [ ] `/proc/self/mountinfo`.

## 36.2 Mount types

- [ ] ext4/xfs awareness.
- [ ] tmpfs.
- [ ] proc.
- [ ] sysfs.
- [ ] devtmpfs.
- [ ] overlayfs.
- [ ] bind mounts.
- [ ] network filesystem awareness only where necessary to understand Linux mounts.

## 36.3 Security relevance

- [ ] `noexec`.
- [ ] `nosuid`.
- [ ] `nodev`.
- [ ] Bind-mount abuse concept.
- [ ] Namespace-specific mounts.

## Verification gate — CHAPTER 36

**Pass condition:** identify the backing filesystem and mount options for 15 paths; create a bind mount and a private mount namespace; explain why two processes can observe different mount trees.

<br><br><br>


---

# CHAPTER 37 — CONTAINERS FROM THE LINUX OS PERSPECTIVE

**Scope:** Linux internals underneath containers; not a Kubernetes administration course.

## 37.1 Foundations

- [ ] Containers as Linux processes with isolation/resource controls.
- [ ] Namespaces.
- [ ] cgroups.
- [ ] Linux capabilities.
- [ ] seccomp.
- [ ] LSM confinement.
- [ ] Overlay filesystems.

## 37.2 Process identity

- [ ] Host PID vs container PID.
- [ ] Container process namespaces.
- [ ] Container cgroup paths.
- [ ] Mount namespace.
- [ ] User namespace awareness.

## 37.3 Runtime awareness

- [ ] Docker/containerd/CRI-O concept.
- [ ] Container runtime process ancestry.
- [ ] Container runtime sockets/files as Linux artifacts.
- [ ] Container process vs host process reasoning.

## Verification gate — CHAPTER 37

**Pass condition:** run a disposable container, identify the host-side process, PID namespace, mount namespace, cgroup and major capabilities; explain the same process from both host and container perspectives.

<br><br><br>


---

# CHAPTER 38 — ENVIRONMENT, INTERPRETERS AND EXECUTION CONTEXT

## 38.1 Environment handling

- [ ] Inherited environment.
- [ ] Clean environment.
- [ ] `PATH` security.
- [ ] `HOME`.
- [ ] `SHELL`.
- [ ] Locale variables.
- [ ] Proxy environment awareness.
- [ ] Per-service environment.

## 38.2 Interpreters

- [ ] Shebang (`#!`).
- [ ] `/bin/sh` vs Bash and alternatives.
- [ ] Python/Perl/Ruby/Node interpreter concept.
- [ ] How scripts become processes.
- [ ] Interpreter path provenance.

## 38.3 Execution-context reasoning

- [ ] Current working directory.
- [ ] Root directory.
- [ ] Environment.
- [ ] umask.
- [ ] credentials.
- [ ] capabilities.
- [ ] namespace membership.
- [ ] cgroup membership.
- [ ] security context/profile.

## Verification gate — CHAPTER 38

**Pass condition:** reconstruct the complete execution context for five processes from `/proc`, systemd or shell configuration; explain why two identical binaries can behave differently under different environments/security contexts.

<br><br><br>


---

# CHAPTER 39 — UNIX SOCKETS, IPC AND LOCAL PROCESS COMMUNICATION

## 39.1 IPC primitives

- [ ] Pipes.
- [ ] FIFOs.
- [ ] Unix CHAPTER sockets.
- [ ] Signals.
- [ ] Shared memory awareness.
- [ ] Message queues awareness.
- [ ] Semaphores awareness.

## 39.2 Security relevance

- [ ] Unix socket filesystem permissions.
- [ ] Process ownership of local sockets.
- [ ] Service APIs exposed through Unix sockets.
- [ ] Why local IPC can be security-sensitive without network exposure.

## 39.3 Tools/interfaces

- [ ] `ss -x`.
- [ ] `/proc/<pid>/fd`.
- [ ] `lsof`.
- [ ] `find` for socket paths.

## Verification gate — CHAPTER 39

**Pass condition:** build a simple local IPC pair, identify the process and socket endpoint, inspect its filesystem/credential context, and explain the authorization boundary.

<br><br><br>


---

# CHAPTER 40 — LINUX PERSISTENCE MECHANISMS

This is a **Linux mechanism map**, not a generic MITRE course.

## 40.1 Service persistence

- [ ] systemd services.
- [ ] systemd drop-ins.
- [ ] systemd user units.
- [ ] SysV/init-script awareness on legacy hosts.

## 40.2 Scheduled persistence

- [ ] User cron.
- [ ] system cron.
- [ ] `/etc/cron.d`.
- [ ] `at` awareness.
- [ ] systemd timers.

## 40.3 Authentication-related persistence

- [ ] `authorized_keys`.
- [ ] SSH configuration.
- [ ] PAM configuration at a high level.
- [ ] NSS/SSSD configuration changes as a trust-boundary concern.

## 40.4 Shell/environment persistence

- [ ] `/etc/profile`.
- [ ] user shell startup files.
- [ ] environment wrappers.
- [ ] path manipulation.

## 40.5 Dynamic-loader persistence

- [ ] `LD_PRELOAD` concept.
- [ ] `/etc/ld.so.preload`.
- [ ] Loader configuration files.

## 40.6 Device/boot/kernel persistence awareness

- [ ] udev rule concept.
- [ ] kernel module concept.
- [ ] boot configuration concept.
- [ ] initramfs awareness.

## Verification gate — CHAPTER 40

**Pass condition:** build at least eight harmless persistence demonstrations across five mechanism families; later, starting from only the host, find every persistence mechanism you created and explain its trigger, identity and execution context.

<br><br><br>


---

# CHAPTER 41 — UDEV, DEVICE EVENTS AND LOCAL HARDWARE-TRIGGERED EXECUTION

## 41.1 udev model

- [ ] Kernel device event concept.
- [ ] udev daemon/rules concept.
- [ ] Rule matching.
- [ ] Rule actions at a high level.
- [ ] Persistent device naming vs execution-related behavior.

## 41.2 Investigation

- [ ] Locate udev rules.
- [ ] Identify custom/local rules.
- [ ] Recognize executable actions in rules as high-interest.

## Verification gate — CHAPTER 41

**Pass condition:** build a harmless test udev rule or inspect a controlled prebuilt rule; identify its match/action model and explain when the rule executes.

<br><br><br>


---

# CHAPTER 42 — SHELL HISTORY, DOTFILES AND USER-LOCAL EXECUTION

## 42.1 History

- [ ] Bash history file concept.
- [ ] `HISTFILE`.
- [ ] History expansion awareness.
- [ ] History is incomplete by design.
- [ ] Non-interactive execution can bypass shell history.

## 42.2 Dotfiles

- [ ] Hidden files.
- [ ] Shell startup dotfiles.
- [ ] User-local configuration.
- [ ] User-local executable directories.

## 42.3 Investigation

- [ ] Identify user-local executable paths.
- [ ] Identify suspicious startup commands.
- [ ] Distinguish configuration from executable payload.

## Verification gate — CHAPTER 42

**Pass condition:** create three different user-local execution paths, demonstrate which shell/session loads each, and document why shell history cannot be treated as a complete execution record.

<br><br><br>


---

# CHAPTER 43 — CORE FILE/PROCESS UTILITY MASTERY

You must be fluent enough that the command itself does not interrupt reasoning.

## 43.1 File commands

- [ ] `ls`
- [ ] `find`
- [ ] `stat`
- [ ] `file`
- [ ] `readlink`
- [ ] `realpath`
- [ ] `namei`
- [ ] `ln`
- [ ] `cp`
- [ ] `mv`
- [ ] `rm` safely in a disposable lab.

## 43.2 Identity/permission commands

- [ ] `id`
- [ ] `getent`
- [ ] `chmod`
- [ ] `chown`
- [ ] `getfacl`
- [ ] `setfacl`
- [ ] `getcap`
- [ ] `setcap`
- [ ] `lsattr`
- [ ] `chattr`

## 43.3 Process commands

- [ ] `ps`
- [ ] `pstree`
- [ ] `pgrep`
- [ ] `pidof`
- [ ] `top`
- [ ] `kill`
- [ ] `lsof`
- [ ] `fuser`
- [ ] `strace`

## 43.4 Service/scheduler commands

- [ ] `systemctl`
- [ ] `systemd-analyze`
- [ ] `loginctl`
- [ ] `crontab`

## 43.5 Security/kernel commands

- [ ] `ausearch`
- [ ] `aureport`
- [ ] `auditctl`
- [ ] `lsmod`
- [ ] `modinfo`
- [ ] `sysctl`
- [ ] `findmnt`
- [ ] `lsns`
- [ ] `nsenter`
- [ ] `unshare`
- [ ] `ss` only to inspect Linux-owned sockets/process attribution; networking itself is out of scope.

## Verification gate — CHAPTER 43

**Pass condition:** command drill of 80 prompts. For each prompt choose the correct native command/interface, execute it, and interpret the result. Pass at **90%+**, with zero critical errors on permissions, process identity, namespaces or privilege questions.

<br><br><br>


---

# CHAPTER 44 — CROSS-DISTRIBUTION LINUX DIFFERENCES

## 44.1 Ubuntu/Debian family

- [ ] `apt`/`dpkg`.
- [ ] AppArmor.
- [ ] Common account/configuration paths.
- [ ] systemd.
- [ ] Typical SSH configuration layout.

## 44.2 RHEL/Fedora-compatible family

- [ ] `dnf`/`rpm`.
- [ ] SELinux.
- [ ] systemd.
- [ ] Typical SSH configuration layout.
- [ ] Audit configuration conventions.

## 44.3 What must remain conceptual

- [ ] Linux kernel primitives are not the same as distro defaults.
- [ ] Service names may differ.
- [ ] A logging path may differ.
- [ ] Security modules may differ.
- [ ] Package ownership commands differ.

## Verification gate — CHAPTER 44

**Pass condition:** complete 20 “works on Ubuntu, where is the equivalent on RHEL?” questions without notes; repeat the same Linux investigation on both hosts and correctly adapt commands/configuration paths.

<br><br><br>


---

# CHAPTER 45 — LINUX HOST INVESTIGATION PLAYBOOKS

These are **Linux-only investigations**. They are not SOC workflow modules.

## 45.1 Suspicious process

- [ ] Identify PID.
- [ ] Identify PPID.
- [ ] Identify user/group.
- [ ] Identify executable.
- [ ] Identify cwd/root.
- [ ] Inspect environment.
- [ ] Inspect file descriptors.
- [ ] Inspect memory maps.
- [ ] Inspect namespaces/cgroups.
- [ ] Inspect security context/capabilities.

## 45.2 Suspicious executable

- [ ] Determine path.
- [ ] Resolve symlinks.
- [ ] Inspect owner/mode/ACL/xattrs.
- [ ] Determine package ownership.
- [ ] Hash it.
- [ ] Identify ELF/interpreter.
- [ ] Inspect dynamic dependencies.
- [ ] Check SUID/SGID/capabilities.

## 45.3 Suspicious service

- [ ] Identify unit.
- [ ] Inspect effective configuration.
- [ ] Identify executable.
- [ ] Identify user/group.
- [ ] Identify dependencies.
- [ ] Inspect sandboxing controls.
- [ ] Inspect unit file/drop-ins.

## 45.4 Suspicious scheduled job

- [ ] Identify scheduler.
- [ ] Identify triggering account.
- [ ] Identify command/script.
- [ ] Resolve path.
- [ ] Inspect script permissions.
- [ ] Trace resulting process.

## 45.5 Suspicious SSH access mechanism

- [ ] Identify account.
- [ ] Identify authorized key.
- [ ] Inspect key/file ownership and permissions.
- [ ] Inspect effective sshd configuration.
- [ ] Trace session → shell → child process.

## Verification gate — CHAPTER 45

**Pass condition:** complete all five investigations from a clean VM plus an unknown prebuilt scenario. For each, produce a one-page evidence chain containing at least **10 Linux-native facts** and explicit uncertainty statements.

<br><br><br>


---

# CHAPTER 46 — LINUX PRIVILEGE-BOUNDARY REASONING

## 46.1 Boundary layers

Understand that a process's authority may be shaped by:

- [ ] UID/GID.
- [ ] Supplementary groups.
- [ ] File mode bits.
- [ ] ACLs.
- [ ] SUID/SGID.
- [ ] File capabilities.
- [ ] Process capabilities.
- [ ] User namespace.
- [ ] `NoNewPrivileges`.
- [ ] seccomp.
- [ ] SELinux/AppArmor.
- [ ] Mount options.
- [ ] systemd sandboxing.
- [ ] cgroup placement/resources.

## 46.2 Reasoning task

- [ ] Given a process and a target file, identify all authorization layers that may affect access.
- [ ] Given UID 0, explain which controls can still constrain the process.
- [ ] Given a non-root process, identify how it could still possess selected privileged capabilities.

## Verification gate — CHAPTER 46

**Pass condition:** solve 15 access-control scenarios where the answer requires combining at least three Linux authorization mechanisms. Pass at **90%+** with written reasoning, not command-only answers.

<br><br><br>


---

# CHAPTER 47 — LINUX EXECUTION-CHAIN RECONSTRUCTION

## 47.1 Chain elements

- [ ] Trigger.
- [ ] Initial process.
- [ ] Parent/child relationships.
- [ ] `execve` transitions.
- [ ] Interpreter/script transitions.
- [ ] Environment.
- [ ] Identity transitions.
- [ ] Service/scheduler origin.
- [ ] Files accessed.
- [ ] Security context.
- [ ] Namespace/cgroup context.

## 47.2 Reconstruction discipline

- [ ] Do not confuse command line with executable identity.
- [ ] Do not infer authorship from file ownership alone.
- [ ] Distinguish current identity from initiating identity where audit data allows it.
- [ ] Distinguish configured persistence from evidence that it actually executed.
- [ ] Record evidence source for each conclusion.

## Verification gate — CHAPTER 47

**Pass condition:** reconstruct three complete execution chains on a lab host beginning only from a process PID or executable path. Every conclusion must have a cited Linux-native artifact/command output.

<br><br><br>


---

# CHAPTER 48 — LINUX TELEMETRY LIMITATIONS AND ANTI-ASSUMPTION SKILLS

## 48.1 What a Linux interface can and cannot prove

- [ ] `ps` is a view, not an immutable historical record.
- [ ] `/proc` is live and changes.
- [ ] Shell history is incomplete.
- [ ] Journal/audit coverage depends on configuration.
- [ ] File timestamps are not authorship proof.
- [ ] Package verification does not prove absence of malicious activity.
- [ ] A missing file can still be open by a process.
- [ ] A process can be in another namespace.
- [ ] A command line may not equal original intended invocation.
- [ ] A legitimate binary can be abused.

## 48.2 Evidence discipline

- [ ] State what is directly observed.
- [ ] State what is inferred.
- [ ] State what is unknown.
- [ ] State which Linux interface produced the observation.
- [ ] State when an artifact may have changed/disappeared.

## Verification gate — CHAPTER 48

**Pass condition:** given 15 evidence statements, correctly label each as observed/inferred/unknown and explain the limitation. Zero “proof from weak evidence” answers on five critical scenarios.


<br><br><br>


---

# INTEGRATED LINUX LAB SERIES

Complete these in order after the individual modules.

## LAB 01 — Process anatomy

- [ ] Create a test process tree.
- [ ] Identify PID/PPID.
- [ ] Inspect `/proc/<pid>/status`.
- [ ] Inspect executable/cwd/fd.
- [ ] Explain every major field.

**Pass:** reconstruct the tree without `pstree`, using `/proc` and `ps` only.

<br>

## LAB 02 — Permission maze

- [ ] Build owner/group/other scenarios.
- [ ] Add SUID/SGID.
- [ ] Add ACLs.
- [ ] Add an immutable flag.
- [ ] Add a file capability.

**Pass:** predict access for five identities and prove it.

<br>

## LAB 03 — SSH-to-shell chain

- [ ] Create test key access.
- [ ] Login.
- [ ] Trace `sshd → shell → child process`.
- [ ] Inspect session/TTY/PTY state.

**Pass:** explain every transition.

<br>

## LAB 04 — systemd persistence simulation

- [ ] Create a harmless service.
- [ ] Add a drop-in.
- [ ] Enable it.
- [ ] Add a timer.
- [ ] Remove everything.

**Pass:** rediscover all artifacts from the host alone.

<br>

## LAB 05 — Audit visibility

- [ ] Configure a test file rule.
- [ ] Configure a test syscall rule.
- [ ] Trigger events.
- [ ] Query with `ausearch`.
- [ ] Explain identity fields.

**Pass:** identify initiating identity and executable for each event.

<br>

## LAB 06 — Capability boundary

- [ ] Inspect normal process capabilities.
- [ ] Assign a controlled file capability.
- [ ] Execute.
- [ ] Inspect process capability state.
- [ ] Remove capability.

**Pass:** explain capability changes across `execve`.

<br>

## LAB 07 — Namespace visibility

- [ ] Create PID namespace.
- [ ] Create mount namespace.
- [ ] Compare views.
- [ ] Inspect `/proc/<pid>/ns/*`.

**Pass:** prove why host and namespace views differ.

<br>

## LAB 08 — Container anatomy

- [ ] Run a disposable container.
- [ ] Map process host PID/container PID.
- [ ] Identify cgroup.
- [ ] Inspect namespace membership.
- [ ] Identify security profile/capabilities.

**Pass:** produce a one-page container-to-host anatomy diagram.

<br>

## LAB 09 — ELF execution

- [ ] Inspect five binaries.
- [ ] Identify interpreter.
- [ ] Identify shared libraries.
- [ ] Observe mappings.
- [ ] Trace startup with `strace`.

**Pass:** explain execution from ELF file to running process.

<br>

## LAB 10 — Persistence scavenger hunt

Create eight harmless persistence mechanisms across:

- [ ] systemd service.
- [ ] systemd timer.
- [ ] cron.
- [ ] SSH authorized key.
- [ ] shell startup file.
- [ ] user-level executable path.
- [ ] loader configuration in a safe test environment.
- [ ] udev rule or boot-time mechanism as a controlled demonstration.

**Pass:** starting from a clean checklist only, discover all eight and document trigger → process → identity → executable.

<br><br><br>


---

# FINAL LINUX L1+L2 CAPSTONE

## Scenario

You are given an unfamiliar Linux VM. You are told only:

> “A suspicious executable appears to be running and may have persistent execution.”

No SIEM, EDR, network analysis, Windows tools, cloud platforms or external telemetry are provided. Use only Linux host mechanisms covered by this document.

## You must independently determine

- [ ] What executable is running.
- [ ] Where it really resides.
- [ ] Who owns it.
- [ ] Which UID/GID executes it.
- [ ] Which capabilities apply.
- [ ] Which parent chain led to it.
- [ ] Which files it has open.
- [ ] Which environment it inherited.
- [ ] Which libraries it loaded.
- [ ] Which namespaces it occupies.
- [ ] Which cgroup it belongs to.
- [ ] Which security profile/context applies.
- [ ] Whether it is package-managed.
- [ ] Whether its file metadata is unusual.
- [ ] Whether it is SUID/SGID/capability-enabled.
- [ ] Whether systemd, cron, SSH, shell startup, loader configuration or another Linux mechanism provides persistence.
- [ ] Which Linux-native evidence supports each conclusion.
- [ ] Which questions remain unanswered because Linux telemetry is missing or insufficient.

## Capstone pass standard

**Time:** 90 minutes on an unfamiliar but prepared host.

**Required result:** a technically defensible investigation note with:

- [ ] 20+ Linux-native facts.
- [ ] 5+ independent evidence sources/interfaces.
- [ ] Complete process/execution chain.
- [ ] Persistence determination.
- [ ] Privilege/security-boundary determination.
- [ ] Explicit uncertainty section.
- [ ] No unsupported claims.

**Pass:** ≥90% of rubric points, **no critical error** involving UID/GID, executable identity, privilege, namespace, or persistence mechanism.

<br><br><br>


---

# FINAL COMMAND/CONCEPT EXAM

## Part A — Command selection

**100 questions.** For each prompt, select the correct Linux-native interface/command and explain why.

**Pass:** ≥90%.

## Part B — Output interpretation

**50 outputs.** Interpret `ps`, `/proc`, `ls -l`, `stat`, `getfacl`, `getcap`, `systemctl`, `journalctl` metadata fields, audit records, `ss`, `readelf`, `systemd-analyze`, `lsns`, `loginctl`, `sestatus`, `aa-status`, package-query output and related host evidence.

**Pass:** ≥90%.

## Part C — Break/fix

Complete 15 deliberately broken Linux scenarios.

Examples:

- [ ] Wrong file permissions.
- [ ] Broken symlink.
- [ ] Unexpected ACL.
- [ ] SUID misconfiguration.
- [ ] Bad sudo rule.
- [ ] Broken systemd service.
- [ ] Failed timer.
- [ ] SSH key permission problem.
- [ ] PAM configuration issue in a disposable lab.
- [ ] SELinux/AppArmor denial.
- [ ] Package file mismatch.
- [ ] Capability mismatch.
- [ ] Namespace confusion.
- [ ] seccomp restriction.
- [ ] Incorrect shared-library configuration.

**Pass:** ≥13/15, with safe restoration after every exercise.

<br><br><br>

---

# MASTER COMPLETION RULE

You may call the entire syllabus **MASTERED** only when:

- [ ] Every module verification gate is passed.
- [ ] Every integrated lab is passed.
- [ ] Final capstone is passed.
- [ ] Final command exam is ≥90%.
- [ ] Final output-interpretation exam is ≥90%.
- [ ] Break/fix is ≥13/15.
- [ ] You can perform core L1 Linux investigation tasks on both Ubuntu and RHEL-compatible Linux without notes.
- [ ] You can explain the L1/L2 Linux privilege model without reducing it to UID 0.
- [ ] You can explain process execution from service/scheduler/login origin through process, executable, interpreter, libraries, capabilities, namespaces, cgroups and LSM/security policy where applicable.
- [ ] You can state the limitations of every major Linux evidence source you use.
- [ ] You have a written notebook/article series proving the concepts rather than only consuming training.

<br><br><br>

# PRIMARY DOCUMENTATION INDEX

Use primary documentation as the authority when a command, behavior or implementation detail differs across distributions.

## Linux kernel documentation

- [Linux kernel documentation — Filesystems / proc](https://docs.kernel.org/filesystems/proc.html)
- [Linux kernel documentation — LSM](https://docs.kernel.org/security/lsm.html)
- [Linux kernel documentation — AppArmor](https://docs.kernel.org/admin-guide/LSM/apparmor.html)
- [Linux kernel documentation — Cgroups](https://docs.kernel.org/admin-guide/cgroup-v2.html)

## Linux man-pages

- [`proc(5)` / `/proc` interfaces](https://man7.org/linux/man-pages/man5/proc.5.html)
- [`proc_pid_status(5)`](https://man7.org/linux/man-pages/man5/proc_pid_status.5.html)
- [`proc_pid_maps(5)`](https://man7.org/linux/man-pages/man5/proc_pid_maps.5.html)
- [`capabilities(7)`](https://man7.org/linux/man-pages/man7/capabilities.7.html)
- [`user_namespaces(7)`](https://man7.org/linux/man-pages/man7/user_namespaces.7.html)
- [`namespaces(7)`](https://man7.org/linux/man-pages/man7/namespaces.7.html)
- [`cgroups(7)`](https://man7.org/linux/man-pages/man7/cgroups.7.html)
- [`seccomp(2)`](https://man7.org/linux/man-pages/man2/seccomp.2.html)
- [`ptrace(2)`](https://man7.org/linux/man-pages/man2/ptrace.2.html)
- [`fork(2)`](https://man7.org/linux/man-pages/man2/fork.2.html)
- [`execve(2)`](https://man7.org/linux/man-pages/man2/execve.2.html)
- [`systemd.service(5)`](https://man7.org/linux/man-pages/man5/systemd.service.5.html)
- [`systemd.timer(5)`](https://man7.org/linux/man-pages/man5/systemd.timer.5.html)
- [`systemd.unit(5)`](https://man7.org/linux/man-pages/man5/systemd.unit.5.html)
- [`systemd.exec(5)`](https://man7.org/linux/man-pages/man5/systemd.exec.5.html)
- [`systemd-analyze(1)`](https://man7.org/linux/man-pages/man1/systemd-analyze.1.html)
- [`sshd(8)`](https://man7.org/linux/man-pages/man8/sshd.8.html)
- [`sshd_config(5)`](https://man7.org/linux/man-pages/man5/sshd_config.5.html)
- [`authorized_keys(5)`](https://man7.org/linux/man-pages/man5/sshd.8.html)
- [`sudoers(5)`](https://man7.org/linux/man-pages/man5/sudoers.5.html)
- [`pam.conf(5)`](https://man7.org/linux/man-pages/man5/pam.conf.5.html)
- [`nsswitch.conf(5)`](https://man7.org/linux/man-pages/man5/nsswitch.conf.5.html)
- [`loginctl(1)`](https://man7.org/linux/man-pages/man1/loginctl.1.html)
- [`auditd(8)`](https://man7.org/linux/man-pages/man8/auditd.8.html)
- [`audit.rules(7)`](https://man7.org/linux/man-pages/man7/audit.rules.7.html)
- [`journalctl(1)`](https://man7.org/linux/man-pages/man1/journalctl.1.html)
- [`findmnt(8)`](https://man7.org/linux/man-pages/man8/findmnt.8.html)
- [`mount(8)`](https://man7.org/linux/man-pages/man8/mount.8.html)
- [`readelf(1)`](https://man7.org/linux/man-pages/man1/readelf.1.html)
- [`ld-linux(8)`](https://man7.org/linux/man-pages/man8/ld-linux.8.html)
- [`lsof(8)`](https://man7.org/linux/man-pages/man8/lsof.8.html)
- [`strace(1)`](https://man7.org/linux/man-pages/man1/strace.1.html)
- [`getfacl(1)`](https://man7.org/linux/man-pages/man1/getfacl.1.html)
- [`xattr(7)`](https://man7.org/linux/man-pages/man7/xattr.7.html)

## Red Hat documentation

- [RHEL 9 Security Hardening](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/security_hardening/)
- [RHEL 9 SELinux](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/using_selinux/)
- [RHEL 9 Auditing the System](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/security_hardening/auditing-the-system_security-hardening)

## Ubuntu documentation

- [Ubuntu Server Security](https://ubuntu.com/server/docs/how-to/security/)
- [Ubuntu AppArmor](https://ubuntu.com/server/docs/how-to/security/apparmor/)
- [Ubuntu package management](https://ubuntu.com/server/docs/managing-software/)

---

