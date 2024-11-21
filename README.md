# freedom
Freedom is a terminal input autofiller created in Python3. Ever wished you could do this?

`echo mypassword | ssh myuser@i_need_no_protection_thanks`

Well, now you can. Enjoy!

## Basic Usage
```
echo P@55w0rd|./freedom ssh user@host
echo P@55w0rd|./freedom scp host:file /tmp
echo P@55w0rd|./freedom sudo -i
echo P@55w0rd|./freedom su -
```

## Advanced Usage
```
echo -e "P@55w0rd\nR00tP@ss"|./freedom ssh -t user@host "su -"
```

## Customization
The default input identification is: `([Pp]assword(?: for .*)?|[Vv]erification code):`
If you want to customize it, make it in the `~/.freedom_regex` file
