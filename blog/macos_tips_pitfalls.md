title: macOS系统的一些奇技淫巧
description: 不作死不舒服斯基
keywords: Mac
date: 2017-09-16
---
# {{ title }}

Mac从OS X以来发展也有十多年了，此间硬件变化很大，而系统默认设置出于各种原因比较保守，需要做些调整才能更好的发挥效力。

## 加快Samba文件传输速度

    printf "[default]\nsigning_required=no\n" | sudo tee /etc/nsmb.conf >/dev/null

## 优化Mail.app本地数据库

    sqlite3 ~/Library/Mail/V5/MailData/Envelope\ Index vacuum

## 使用noatime挂载文件系统

- 创建 `/Library/LaunchDaemons/com.nullversion.noatime.plist` ，内容：

    ```
    <?xml version="1.0" encoding="UTF-8"?>
    <!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" 
            "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
    <plist version="1.0">
        <dict>
            <key>Label</key>
            <string>com.nullvision.noatime</string>
            <key>ProgramArguments</key>
            <array>
                <string>mount</string>
                <string>-vuwo</string>
                <string>noatime</string>
                <string>/</string>
            </array>
            <key>RunAtLoad</key>
            <true/>
        </dict>
    </plist> /Library/LaunchDaemons/com.nullversion.noatime.plist
    ```

- 设置文件权限

    ```bash
    sudo chown root:wheel /Library/LaunchDaemons/com.noatime.plist
    sudo chmod 644 /Library/LaunchDaemons/com.noatime.plist
    ```

_那像我这样还另外还插了一张JetDrive闪存卡做扩展磁盘的话要怎么办呢？_

再加上一个plist文件 `/Library/LaunchDaemons/com.nullversion.noatime-transcend.plist`

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" 
        "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
    <dict>
        <key>Label</key>
        <string>com.nullvision.noatime-transcend</string>
        <key>ProgramArguments</key>
        <array>
            <string>mount</string>
            <string>-vuwo</string>
            <string>noatime</string>
            <string>/Volumes/Transcend</string>
        </array>
        <key>RunAtLoad</key>
        <false/>
        <key>StartInterval</key>
        <integer>5</integer>
        <key>LaunchOnlyOnce</key>
        <true/>
        <key>KeepAlive</key>
        <false/>
    </dict>
</plist> /Library/LaunchDaemons/com.nullversion.noatime.plist
```

未完待续

参考
----------

- [How to Fix Slow SMB File Transfers on OS X 10.11.5+ and macOS Sierra](https://dpron.com/os-x-10-11-5-slow-smb/)
- <http://disq.us/p/1eedme0>