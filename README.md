# mezzanine

## client

You need [Prism Launcher](https://prismlauncher.org/) or another Modrinth-compatible launcher.

1. Open Prism Launcher
2. Add Instance
3. Import from zip
4. Paste https://mezzanine.bingecraft.net/files/mezzanine-10.mrpack and click OK
5. Edit new instance > Settings
6. Check `Java Installation` and configure to use Java 17
7. Launch!

## server

### install

You need [mrpack-install](https://github.com/nothub/mrpack-install)

```
mkdir mezzanine
cd mezzanine
mrpack-install https://mezzanine.bingecraft.net/files/mezzanine-10.mrpack --server-file server.jar --server-dir .
```

### run

```
java -jar server.jar
```