# Nmap

# Feroxbuster
```py
200      GET        7l      158w     9028c https://bizness.htb/lib/waypoints/waypoints.min.js
200      GET        1l       44w     2608c https://bizness.htb/lib/lightbox/css/lightbox.min.css
200      GET       11l       56w     2406c https://bizness.htb/lib/counterup/counterup.min.js
200      GET      118l      332w     3375c https://bizness.htb/contactform/contactform.js
200      GET       10l       83w     4474c https://bizness.htb/lib/superfish/superfish.min.js
200      GET        2l      247w     7083c https://bizness.htb/lib/jquery/jquery-migrate.min.js
200      GET        3l      148w     8159c https://bizness.htb/lib/wow/wow.min.js
200      GET      158l      848w     7078c https://bizness.htb/lib/superfish/hoverIntent.js
200      GET        1l       38w     2303c https://bizness.htb/lib/easing/easing.min.js
200      GET        7l       27w     3309c https://bizness.htb/img/apple-touch-icon.png
200      GET       14l      228w    20443c https://bizness.htb/lib/touchSwipe/jquery.touchSwipe.min.js
200      GET       12l      559w    35503c https://bizness.htb/lib/isotope/isotope.pkgd.min.js
200      GET        4l       66w    31000c https://bizness.htb/lib/font-awesome/css/font-awesome.min.css
200      GET     1582l     3107w    26543c https://bizness.htb/css/style.css
200      GET       11l      188w    16964c https://bizness.htb/lib/animate/animate.min.css
200      GET        4l     1298w    86659c https://bizness.htb/lib/jquery/jquery.min.js
200      GET      207l      499w     6663c https://bizness.htb/js/main.js
200      GET        9l       23w      847c https://bizness.htb/img/favicon.png
200      GET       11l       46w    51284c https://bizness.htb/lib/ionicons/css/ionicons.min.css
200      GET       15l      120w     9418c https://bizness.htb/lib/lightbox/js/lightbox.min.js
200      GET        6l       64w     2936c https://bizness.htb/lib/owlcarousel/assets/owl.carousel.min.css
200      GET      160l     1057w    91694c https://bizness.htb/img/about-vision.jpg
404      GET        1l       68w        -c Auto-filtering found 404-like response and created new filter; toggle off with --dont-filter
200      GET      168l      952w    75910c https://bizness.htb/img/about-plan.jpg
200      GET        7l      279w    42766c https://bizness.htb/lib/owlcarousel/owl.carousel.min.js
200      GET        7l      965w    76308c https://bizness.htb/lib/bootstrap/js/bootstrap.bundle.min.js
200      GET        7l     1929w   153182c https://bizness.htb/lib/bootstrap/css/bootstrap.min.css
200      GET      915l     5085w   372733c https://bizness.htb/img/intro-carousel/2.jpg
200      GET     1896l     9607w   743797c https://bizness.htb/img/intro-carousel/4.jpg
200      GET      181l      915w    84161c https://bizness.htb/img/about-mission.jpg
200      GET      922l     4934w   402185c https://bizness.htb/img/intro-carousel/5.jpg
200      GET     1176l     7328w   623279c https://bizness.htb/img/intro-carousel/1.jpg
200      GET      628l     3558w   288020c https://bizness.htb/img/intro-carousel/3.jpg
200      GET      522l     1736w    27200c https://bizness.htb/
500      GET       10l       77w     1443c https://bizness.htb/catalog/images
500      GET        7l       13w      177c https://bizness.htb/common/css/aboutUs
500      GET        7l       13w      177c https://bizness.htb/ebay/59
500      GET        7l       13w      177c https://bizness.htb/marketing/forum_old
200      GET      492l     1596w    34633c https://bizness.htb/content/control
500      GET        7l       13w      177c https://bizness.htb/common/js/plugins/white
200      GET      492l     1596w    34633c https://bizness.htb/catalog/control
200      GET      492l     1596w    34633c https://bizness.htb/marketing/control
200      GET      492l     1596w    34633c https://bizness.htb/ap/control
```

# PoC
https://github.com/abdoghazy2015/ofbiz-CVE-2023-49070-RCE-POC

```
root:x:0:0:root:/root:/bin/bash
ofbiz:x:1001:1001:,,,:/home/ofbiz:/bin/bash
```


```
grep -arin -o -E '(\w+\W+){0,5}password(\W+\w+){0,5}' .
$SHA$d$uP0_QaVBpDWFeo8-dRzDqRwXQ2I
```