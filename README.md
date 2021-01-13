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
    - 20H1-2004.19041.264
    - 20H2-2009.19042.631

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
