# Windows Desktop

> History and analysis of Windows images

- Windows 10
    - Threshold 1 - 1507 (10240.16384)
    - Threshold 2 - 1511
        - February 2016 (10586.104)
        - April 2016 (10586.164)
    - Redstone 1 - 1603 (14393.0)
    - Redstone 2 - 1703 (15063.0)
    - Redstone 3 - 1709 (16299.15)
    - Redstone 4 - 1803 (17134.1)
    - Redstone 5 - 1809
        - September 2018 (17763.1)
        - October 2018 (17763.107)
        - March 2019 (17763.379)
    - 19H1 - 1903
        - 18362.30
        - 18362.356
    - 19H2 - 1909 (18363.418)
    - 20H1 - 2004 (19041.264)
    - 20H2 - 2009 (19042.631)

## How to use this repo

### Finding changes between 2 releases

Each commit represents a Windows release.

You can use git diff with filters to highlights new files between the releases.

Example: _"I want to see all the new files inserted in Windows 10 1803"_

Lets git diff, filtering on added files, only by name, between Windows 10 `rs3` `rs4` (`1803`)

~~~
git diff --diff-filter=A --name-only win10-rs3-1709.16299.15..win10-rs4-1803.17134.1 'filesystem/Windows/System32/*.exe'
filesystem/Windows/System32/OpenSSH/sftp.exe
filesystem/Windows/System32/OpenSSH/ssh-add.exe
filesystem/Windows/System32/OpenSSH/ssh-agent.exe
filesystem/Windows/System32/OpenSSH/ssh-keygen.exe
filesystem/Windows/System32/OpenSSH/ssh-keyscan.exe
filesystem/Windows/System32/OpenSSH/ssh.exe
filesystem/Windows/System32/PerceptionSimulationDevice.exe
filesystem/Windows/System32/PerceptionSimulationInput.exe
filesystem/Windows/System32/PinEnrollmentBroker.exe
filesystem/Windows/System32/SgrmBroker.exe
filesystem/Windows/System32/WinBioPlugIns/FaceFodUninstaller.exe
filesystem/Windows/System32/curl.exe
filesystem/Windows/System32/dxgiadaptercache.exe
filesystem/Windows/System32/tar.exe
filesystem/Windows/System32/tcblaunch.exe
filesystem/Windows/System32/upfc.exe
~~~

in this release, we can notice the presence of:
- System Guard Runtime Broker `SgrmBroker.exe`
- `OpenSSH`

Note: [git diff](https://git-scm.com/docs/git-diff) filters:
- `A`: Added
- `D`: Deleted

### Finding which OS commit introduced/removed a given file

Let's the example of `SgrmBroker.exe` again.
To find which commit introduced this file:

~~~
git log --diff-filter=A --name-only  'filesystem/**SgrmBroker.exe'
~~~

~~~diff
commit db5296413381f046aee1302ee5b3bcd9e49a46a9
Author: Mathieu Tarral <mathieu.tarral@protonmail.com>
Date:   Thu Jan 14 02:36:09 2021 +0100

    win10-20H1-2004.19041.264

filesystem/Windows/WinSxS/amd64_security-octagon-broker_31bf3856ad364e35_10.0.19041.84_none_51ae5c25baf813ff/f/SgrmBroker.exe
filesystem/Windows/WinSxS/amd64_security-octagon-broker_31bf3856ad364e35_10.0.19041.84_none_51ae5c25baf813ff/r/SgrmBroker.exe

commit 3c149ac303fc69acd8a5ce0411049ff948dc1f92
Author: Mathieu Tarral <mathieu.tarral@protonmail.com>
Date:   Wed Jan 13 23:02:41 2021 +0100

    win10-rs4-1803.17134.1

filesystem/Windows/System32/SgrmBroker.exe
filesystem/Windows/WinSxS/amd64_security-octagon-broker_31bf3856ad364e35_10.0.17134.1_none_3f8bcb44f6051037/SgrmBroker.exe
~~~

## Plugins

### filesystem

Each filesystem entry is represented by a JSON dict:

~~~JSON
{
  "MIME": "application/x-dosexec",
  "inode_type": "REG",
  "magic_type": "PE32+ executable (native) x86-64, for MS Windows",
  "mode": "-rwxrwxrwx",
  "sha1": "d624b81c5a1a1f24ee720447a56fc2e5e323a5a2"
}
~~~

### checksec

- `stats.json` will give you statistics of binary security properties
- for every executable/dll, a JSON entry describes its security properties

~~~JSON
{
  "dynamic_base": true,
  "seh": true,
  "guard_cf": true,
  "force_integrity": false,
  "nx_compat": true,
  "high_entropy_va": false,
  "manifest_isolation": true,
  "signed": false,
  "cat_filepath": ""
}
~~~

## References

- [OSWatcher](https://github.com/Wenzel/oswatcher)
