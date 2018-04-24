2018-04-24

stackdump/python/src/stackdump/commands/download_site_info.py:49 specifies a deprecated address for downloading site icons: http://sstatic.net/%s/img/icon-48.png

As of this writing, the correct address format is: https://cdn.sstatic.net/Sites/%s/img/icon-48.png

Line 49 may be replaced with the following:

```        urllib.urlretrieve('https://cdn.sstatic.net/Sites/%s/img/icon-48.png' % site_key, os.path.join(logos_dir_path, '%s.png' % site_key))```
