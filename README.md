# Imam Digmi
[![Maintenance](https://img.shields.io/badge/Maintained%3F-yes-green.svg)](https://github.com/imamdigmi/imamdigmi.github.io/graphs/commit-activity)
[![Website shields.io](https://img.shields.io/website-up-down-green-red/http/shields.io.svg)](http://shields.io/)
[![Build Status](https://travis-ci.org/imamdigmi/imamdigmi.github.io.svg?branch=source)](https://travis-ci.org/imamdigmi/imamdigmi.github.io)
[![GitHub stars](https://img.shields.io/github/stars/imamdigmi/imamdigmi.github.io.svg?style=social&label=Star&maxAge=2592000)](https://github.com/imamdigmi/imamdigmi.github.io/stargazers/)
[![GitHub followers](https://img.shields.io/github/followers/imamdigmi.svg?style=social&label=Follow&maxAge=2592000)](https://github.com/imamdigmi?tab=followers)

Data Engineer, Independent Researcher, based in Indramayu, live in Yogyakarta.

## Installation
Clone repository
```
git clone git@github.com:imamdigmi/imamdigmi.github.io.git
```

Then checkout to `source` branch
```
git checkout source
```
## Usage
Create new post
```
hugo new post/title.md
```

## Deploy
Build site
```
hugo
```

Checkout to `master` branch
```
git checkout master
```

Copy `docs` directory from `source` branch
```
git checkout source -- docs/
```

Move out all files
```
mv docs/* .
```

Remove `docs` directory
```
rmdir docs
```

Commit them and push!

## LICENSE
<a rel="license" href="http://creativecommons.org/licenses/by-nc-nd/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by-nc-nd/4.0/88x31.png" /></a><br />This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-nc-nd/4.0/">Creative Commons Attribution-NonCommercial-NoDerivatives 4.0 International License</a>.
