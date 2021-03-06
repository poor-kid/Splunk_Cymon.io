## Splunk_Cymon.io


#### Description

A python script that can be used by a Splunk custom command to query the [Cymon.io API](https://github.com/eSentire/cymon-python). 
If you have any comments or suggestions please raise an issue and I'll get back to you.

#### Requirements

1. Splunk 6.0+ 
2. Internet connection

#### Installation

1. Add the following to your Splunk apps `$SPLUNK_HOME/etc/apps/<app_name>local/commands.conf`
```python
[cymon]
filename = cymonsplunk.py
```
2. Add cymonsplunk.py to `$SPLUNK_HOME/etc/apps/<app_name>/bin/`

#### Usage
##### To query an IP
From Splunk search run `| cymon __EXECUTE__ 8.8.8.8 | spath input=cy`

##### To query a domain
From Splunk search run `| cymon __EXECUTE__ google.com | spath input=cy`

##### Format JSON resonse as a table
```python
| cymon __EXECUTE__ "8.8.8.8" | spath input=cy | rename results{}.* AS * | eval x = mvzip(title, description) 
| eval x = mvzip(x, details_url, ",")
| eval x = mvzip(x, created, ",") 
| eval x = mvzip(x, updated, ",")
| eval x = mvzip(x, tag, ",")
| mvexpand x | makemv delim="," x 
| eval title=mvindex(x,0) 
| eval description=mvindex(x,1) 
| eval details_url=mvindex(x,2) 
| eval created=mvindex(x,3) 
| eval updated=mvindex(x,4)
| eval tag=mvindex(x,5)
| table title, created, updated, tag, description, details_url
```

#### ToDo

- [x] Add support for API keys.
- [x] Add support for domain queries.

##### Credits
Used a one or two line snippet of another script but can't remember where I found it from. 
Shoutout to Craig B for pointing out an install guide error.
