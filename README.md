# Nginx L7 DDoS Protection! :boom: :zap:
*(Please Read Whole Page, All Things Are Important Then If You Want You Can Use IT.)*

# To-Do

- [x] Support Ubuntu Trusty.
- [ ] Support Ubuntu Xenial+.
- [ ] Support Debian.
- [ ] Support Centos.
- [x] Support Arch Linux.
- [x] ModSecurity Support.
- [x] Naxsi Support.
- [x] L7 Protection.
- [x] AutoBan System.
- [x] Integrate Fail2Ban > IpTables.
- [ ] GUI ?
- [ ] .....

# Installation

As of now available for use is just Ubuntu version. (Ubuntu 14.04) to try it you need to have a fresh installation of 
Ubuntu 14.04 in you VM/VPS/DEDICATED so 

1. **`git clone https://github.com/theraw/The-World-Is-Yours.git`**

2. **`cd The-World-Is-Yours/; chmod +x *`**

3. **`./install`**


# Informations.
```
=> /nginx/                                = Nginx Path,
=> /nginx/live/                           = Vhosts Config Files Dir,
=> /nginx/logs/                           = Core Logs Files,
=> /nginx/modsecurity/                    = ModSecurity Rules Dir,
=> /hostdata/                             = Place to store your domain folders.
=> /hostdata/yourdomain.com/              = Ex of domain dir (private folder),
=> /hostdata/yourdomain.com/public_html/  = Ex of your domain webroot (public files only),
=> /hostdata/yourdomain.com/logs/         = Place where to store your Domains logs (access.log) (private folder),
=> /hostdata/yourdomain.com/ssl/          = Place where to store domain ssl/key (private folder),
=> /hostdata/yourdomain.com/cache/        = Place where to store site cache (private folder).

// Private Folder - Means this cannot be accessed by public.
// Public Folder  - Means files into this folder can be accessed by public.
```


# Check.

1 . [L7 (Cookie Based Protection)](https://github.com/theraw/The-World-Is-Yours/blob/master/static/nginx.conf#L15-L42) AND [Replace "proxy2.dope.. links with yours click here to find aes](https://github.com/theraw/The-World-Is-Yours/tree/master/static/vhost) which should be stored on a external link or in a place where L7 is disabled because it will not work if you put it in main site dir!.

2 . [Auto Ban System](https://github.com/theraw/The-World-Is-Yours/blob/master/iptables/jail.local#L105-L111) based on [Connection for ip](https://github.com/theraw/The-World-Is-Yours/blob/master/static/nginx.conf#L72-L73)

3 . [Kernel Settings](https://github.com/theraw/The-World-Is-Yours/blob/master/static/sysctl.conf#L1-L34)

4 . [Naxsi Rules Included](https://github.com/theraw/The-World-Is-Yours/blob/master/static/nginx.conf#L118)

5 . [Example of Naxsi](https://github.com/theraw/The-World-Is-Yours/blob/master/static/vhost/default#L12-L19)

6 . [Check Iptables rules](https://github.com/theraw/The-World-Is-Yours/blob/master/iptables/rules) It will not be automatically enabled, because this changes based on providers in ovh it work in azure it doesn't work. so you need to manually activate iptables!

7 . ModSecurity is not loaded. However you need to set it up by yourself. you have a folder `/nginx/modsecurity/`
which ModSecurity rules are stored, open `/nginx/modsecurity/modsecurity.conf` add those

```bash
Include crs-setup.conf
Include rules/*.conf
```
ModSecurity is by default enabled as "detect only" you can turn it on always by doing this

```bash
SecRuleEngine On
```

Using modSecurity for your site
```bash
server { 
     ..... 
        modsecurity on;
        modsecurity_rules_file /nginx/modsecurity/modsecurity.conf; 
        location / { 
     ..... 
        } 
}
```
**Careful** Using modsec rules like
```
   location / { 
       modsecurity_rules_file /nginx/modsecurity/modsecurity.conf; 
   } 
```
it means that's enabled just for your main place `/` not for other dirs in your site ex `/admin/` (:


Test it!
`curl 'http://localhost/?q="><script>wanna hack</script>'`
```html
<html>
<head><title>403 Forbidden</title></head>
<body bgcolor="white">
<center><h1>403 Forbidden</h1></center>
<hr><center>nginx</center>
</body>
</html>
```
