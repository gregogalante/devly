---
layout: post
title:  "node-sass python dependencies. How to fix it?"
date:   2024-01-06 00:00:00 +0100
categories: webdev
---

Working on old project I had node-sass installation problem. I tried to install it with `yarn install` but it failed with error:

```bash
gyp info it worked if it ends with ok
gyp verb cli [
gyp verb cli   '/Users/galante/.nvm/versions/node/v16.20.2/bin/node',
gyp verb cli   '/Users/galante/Workspace/tfol-wp/app/public/wp-content/themes/tfol-wp-theme/node_modules/node-gyp/bin/node-gyp.js',
gyp verb cli   'rebuild',
gyp verb cli   '--verbose',
gyp verb cli   '--libsass_ext=',
gyp verb cli   '--libsass_cflags=',
gyp verb cli   '--libsass_ldflags=',
gyp verb cli   '--libsass_library='
gyp verb cli ]
gyp info using node-gyp@3.8.0
gyp info using node@16.20.2 | darwin | x64
gyp verb command rebuild []
gyp verb command clean []
gyp verb clean removing "build" directory
gyp verb command configure []
gyp verb check python checking for Python executable "python2" in the PATH
gyp verb `which` failed Error: not found: python2
gyp verb `which` failed     at getNotFoundError (/Users/galante/Workspace/tfol-wp/app/public/wp-content/themes/tfol-wp-theme/node_modules/which/which.js:13:12)
gyp verb `which` failed     at F (/Users/galante/Workspace/tfol-wp/app/public/wp-content/themes/tfol-wp-theme/node_modules/which/which.js:68:19)
gyp verb `which` failed     at E (/Users/galante/Workspace/tfol-wp/app/public/wp-content/themes/tfol-wp-theme/node_modules/which/which.js:80:29)
gyp verb `which` failed     at /Users/galante/Workspace/tfol-wp/app/public/wp-content/themes/tfol-wp-theme/node_modules/which/which.js:89:16
gyp verb `which` failed     at /Users/galante/Workspace/tfol-wp/app/public/wp-content/themes/tfol-wp-theme/node_modules/isexe/index.js:42:5
gyp verb `which` failed     at /Users/galante/Workspace/tfol-wp/app/public/wp-content/themes/tfol-wp-theme/node_modules/isexe/mode.js:8:5
gyp verb `which` failed     at FSReqCallback.oncomplete (node:fs:202:21)
gyp verb `which` failed  python2 Error: not found: python2
gyp verb `which` failed     at getNotFoundError (/Users/galante/Workspace/tfol-wp/app/public/wp-content/themes/tfol-wp-theme/node_modules/which/which.js:13:12)
gyp verb `which` failed     at F (/Users/galante/Workspace/tfol-wp/app/public/wp-content/themes/tfol-wp-theme/node_modules/which/which.js:68:19)
gyp verb `which` failed     at E (/Users/galante/Workspace/tfol-wp/app/public/wp-content/themes/tfol-wp-theme/node_modules/which/which.js:80:29)
gyp verb `which` failed     at /Users/galante/Workspace/tfol-wp/app/public/wp-content/themes/tfol-wp-theme/node_modules/which/which.js:89:16
gyp verb `which` failed     at /Users/galante/Workspace/tfol-wp/app/public/wp-content/themes/tfol-wp-theme/node_modules/isexe/index.js:42:5
gyp verb `which` failed     at /Users/galante/Workspace/tfol-wp/app/public/wp-content/themes/tfol-wp-theme/node_modules/isexe/mode.js:8:5
gyp verb `which` failed     at FSReqCallback.oncomplete (node:fs:202:21) {
gyp verb `which` failed   code: 'ENOENT'
gyp verb `which` failed }
gyp verb check python checking for Python executable "python" in the PATH
gyp verb `which` succeeded python /Users/galante/miniconda3/bin/python
gyp ERR! configure error 
gyp ERR! stack Error: Command failed: /Users/galante/miniconda3/bin/python -c import sys; print "%s.%s.%s" % sys.version_info[:3];
gyp ERR! stack   File "<string>", line 1
gyp ERR! stack     import sys; print "%s.%s.%s" % sys.version_info[:3];
gyp ERR! stack                 ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
gyp ERR! stack SyntaxError: Missing parentheses in call to 'print'. Did you mean print(...)?
gyp ERR! stack 
gyp ERR! stack     at ChildProcess.exithandler (node:child_process:402:12)
gyp ERR! stack     at ChildProcess.emit (node:events:513:28)
gyp ERR! stack     at maybeClose (node:internal/child_process:1100:16)
gyp ERR! stack     at Process.ChildProcess._handle.onexit (node:internal/child_process:304:5)
gyp ERR! System Darwin 23.2.0
gyp ERR! command "/Users/galante/.nvm/versions/node/v16.20.2/bin/node" "/Users/galante/Workspace/tfol-wp/app/public/wp-content/themes/tfol-wp-theme/node_modules/node-gyp/bin/node-gyp.js" "rebuild" "--verbose" "--libsass_ext=" "--libsass_cflags=" "--libsass_ldflags=" "--libsass_library="
gyp ERR! cwd /Users/galante/Workspace/tfol-wp/app/public/wp-content/themes/tfol-wp-theme/node_modules/node-sass
gyp ERR! node -v v16.20.2
```

The solution is to replace node-sass with sass (which not depends on python):

```bash
yarn remove node-sass
yarn add sass
```