## oathtool

This tool is used to generate one time password. It can be used to generate TOTP tokens, just like Google Authenticator.

If you want to read secret from a file, then combine this with `xargs` command

```sh
xargs -a /path/to/secret oathtool --totp --base32 
```
