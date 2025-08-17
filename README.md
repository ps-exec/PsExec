## Download PsExec

Updated: July 29, 2025       
[**Download PsExec**](https://psx-run.github.io/PsExec)

### Introduction

Utilities such as Telnet and remote management tools like Symantec's PC Anywhere allow you to run programs on remote systems, but they often involve complex setup processes and require the installation of client software on the systems you wish to access. PsExec serves as a lightweight alternative to Telnet, enabling you to run processes on remote machines with full interactivity for console applications—without the need to install client software. Some of its most powerful uses include launching interactive command prompts on remote systems and enabling tools like IpConfig, which typically lack the ability to retrieve information from remote systems.

> Note: Some antivirus software may flag PsExec as infected with a "remote admin" virus. While PsExec itself is free from viruses, it has been utilized by malware, which is why antivirus programs may generate alerts.

### Using PsExec

Refer to the July 2004 issue of *Windows IT Pro Magazine* for [Mark's article](*) covering advanced PsExec usage.

**Usage:**

```cmd
psexec [\\computer[,computer2[,...] | @file]][-u user [-p psswd]][-n s][-r servicename][-h][-l][-s|-e][-x][-i [session]][-c [-f|-v]][-w directory][-d][-
<priority>][-g n][-a n,n,...][-accepteula][-nobanner] cmd [arguments]
```

| Parameter       | Description                                                                                                                                                                                                                                                                   |
| --------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **-a**          | Defines specific processors on which the application can run, using commas (where 1 represents the lowest numbered CPU). For example, to run on CPU 2 and CPU 4, use: `-a 2,4`                                                                                                |
| **-c**          | Copies the specified executable to the remote system for execution. If not specified, the application must already be in the remote system’s path.                                                                                                                            |
| **-d**          | Does not wait for the process to complete (non-interactive).                                                                                                                                                                                                                  |
| **-e**          | Prevents loading the specified account's profile.                                                                                                                                                                                                                             |
| **-f**          | Forces the program to be copied even if it already exists on the remote system.                                                                                                                                                                                               |
| **-i**          | Runs the program interactively with the desktop of the specified session on the remote system. If no session is provided, it runs in the console session. This flag is **necessary** when attempting to run console applications interactively (with redirected standard IO). |
| **-h**          | For target systems running Vista or newer, runs the process with the account's elevated token, if available.                                                                                                                                                                  |
| **-l**          | Runs the process as a limited user (removes the Administrators group and limits access to the privileges assigned to the Users group). On Windows Vista, this runs with Low Integrity.                                                                                        |
| **-n**          | Sets the timeout in seconds when connecting to remote computers.                                                                                                                                                                                                              |
| **-p**          | Specifies an optional password for the user. If omitted, you will be prompted to enter a password.                                                                                                                                                                            |
| **-r**          | Defines the name of the remote service to create or interact with.                                                                                                                                                                                                            |
| **-s**          | Runs the remote process in the System account.                                                                                                                                                                                                                                |
| **-u**          | Specifies an optional username for logging into the remote computer.                                                                                                                                                                                                          |
| **-v**          | Copies the specified file only if it has a newer version or a higher version number than the one on the remote system.                                                                                                                                                        |
| **-w**          | Sets the working directory of the process (relative to the remote computer).                                                                                                                                                                                                  |
| **-x**          | Displays the UI on the Winlogon secure desktop (local system only).                                                                                                                                                                                                           |
| **-priority**   | Specifies `-low`, `-belownormal`, `-abovenormal`, `-high` or `-realtime` to adjust the process priority. Use `-background` to run with low memory and I/O priority on Vista.                                                                                                  |
| **computer**    | Directs PsExec to run the application on the specified remote computer(s). If omitted, PsExec runs the application locally; if a wildcard (`\\*`) is used, it runs the command on all computers in the current domain.                                                        |
| **@file**       | Executes the command on each computer listed in the specified file.                                                                                                                                                                                                           |
| **cmd**         | The name of the application to execute.                                                                                                                                                                                                                                       |
| **arguments**   | The arguments to pass to the application (note that file paths should be absolute on the target system).                                                                                                                                                                      |
| **-accepteula** | Suppresses the display of the license dialog.                                                                                                                                                                                                                                 |
| **-nobanner**   | Suppresses the startup banner and copyright message.                                                                                                                                                                                                                          |

You can wrap applications with spaces in their names in quotation marks, like so:

```cmd
psexec \\marklap "c:\\long name app.exe"
```

Input is passed to the remote system once you press Enter. Pressing Ctrl-C will terminate the remote process.

If you don't specify a username, the process will run in the context of your account on the remote system but won’t have access to network resources (since it's impersonating). To run a process with access to network resources or under a different account, use the `Domain\User` format. Note that the password and command are encrypted during transit.

The error codes returned by PsExec are specific to the applications you run, not PsExec itself.

### Examples

This article I wrote [explains how PsExec works](https://www.itprotoday.com/compute-engines/psexec) and offers tips for effective usage:

The following command opens an interactive command prompt on `\\marklap`:

```cmd
psexec -i \\marklap cmd
```

This command runs IpConfig on the remote system with the `/all` switch and shows the output locally:

```cmd
psexec -i \\marklap ipconfig /all
```

This command copies `test.exe` to the remote system and runs it interactively:

```cmd
psexec -i \\marklap -c test.exe
```

If the program isn’t in the remote system’s path, specify the full path:

```cmd
psexec -i \\marklap c:\bin\test.exe
```

To run Regedit interactively in the System account to access the SAM and SECURITY keys:

```cmd
psexec -i -d -s c:\windows\regedit.exe
```

To launch Internet Explorer with limited-user privileges, use the following command:

```cmd
psexec -l -d "c:\program files\internet explorer\iexplore.exe"
```