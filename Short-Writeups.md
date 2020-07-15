### 1. A source code disclosure vulnerability
https://twitter.com/adarshshettyy/status/1276118701717118977

* Scraped the source code of the website and found an internal s3 server used for serving  static content
* Url looked like -> https://domain.com/[bucket-name]/main.js
* Brute-force the bucket-name parameter
* Found  a bucket with name repo which had debian packages
* Downloaded the packages & extracted the files using "ar" command:
  * ar vx package.deb
* Extracted the data.tar.gz inside the extracted package
  * cd package
  * tar xvzf data.tar.gz
* Found a bunch of python files, access tokens and source code with business logic

Dictionary used -> https://github.com/maurosoria/dirsearch/blob/master/db/dicc.txt
