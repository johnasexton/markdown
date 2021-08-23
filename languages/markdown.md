# Markdown Guide

# H1 heading

## H2 heading

### H3 heading

#### H4 heading

##### H5 heading

### Text formatting

regular text needs no special characters


*italics*

_italics_

**bold**

__bold__


---

### H3 with horizontal rule on both sides
---


- a dash is a bullet

* an asterisk is also a bullet


1. this is a
2. numbered
3. list


### hyperlink

[Atom.io](http://www.atom.io)

### embedded image link:
![](http://i.imgur.com/Gf4gPR2.png)

### block text uses 3 backticks before & after

```
├── LICENSE.md

├── README.md

├── assets

│   ├── css

│   │   └── uno-zen.css # the production css
```



### syntax highlighting for different languages



##### bash

```bash
#!/bin/bash
DAT_DIR=/opt/app/informatica/SrcFiles/portalnonprod/
SFTP_SERVER=moveitsecurelynp.domain.com
SFTP_USER=imatica
SFTP_DIR=/Home/imatica/
PROC_FILE=*
LOG_DIR=${DAT_DIR}
LOG_FILE=portalnonprod.log

exec > ${LOG_DIR}/${LOG_FILE} 2>&1

function sftp_file {
  export SFTP=$(sftp ${SFTP_USER}@${SFTP_SERVER} <<EOF
    cd ${SFTP_DIR}
    lcd ${DAT_DIR}
    get ${PROC_FILE}
    bye
    EOF
  done)
  echo ${SFTP}
}

sftp_file

exec 1>&-
```



##### html

```html
<html>
  <body>
      <head>
  <title>Title Page</title>
      </head>
      <a href="http://nodejs.org/api/">core modules</a>
  </body>
</html>
```

##### javascript

```js
<script>
  var args = process.argv.slice(2);
  console.log('Hello ' + args.join(' ') + '!');
</script>
```




##### Convert RTF to Markdown online
* does not support images which you will have to manage yourself
* http://markitdown.medusis.com/
