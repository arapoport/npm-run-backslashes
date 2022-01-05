# npm-run-backslashes

This small project demonstrates a difference between npm 6 and 8, specifically the `npm run` operation and how it passes arguments with backslashes under windows.
npm 8 adds additional backslash for each existing backslash. npm 6 doesn't do this and behaves in the same way as node does.

Steps for reproducing:

```shell
npm install
node print.js c:\abc
npm explore demo -- npm run print-args -- c:\abc
```


Both node and npm run the same file. If using npm 8 the output will be different:

```
C:\Dev\Projects\npm-run-backslashes>node print.js c:\abc
[
  'C:\\Program Files\\nodejs\\node.exe',
  'C:\\Dev\\Projects\\npm-run-backslashes\\print.js',
  'c:\\abc'
]

C:\Dev\Projects\npm-run-backslashes>npm explore demo -- npm run print-args -- c:\abc

> npm-double-slashes@1.0.0 print-args
> node print "c:\\abc"

[
  'C:\\Program Files\\nodejs\\node.exe',
  'C:\\Dev\\Projects\\npm-run-backslashes\\node_modules\\demo\\print',
  'c:\\\\abc'
]


```



More detailed comparison. Running these commands:

```shell
node print.js c:\abc
nvm use 17.3.0
npm -v
npm explore demo -- npm run print-args -- c:\abc
nvm use 14.18.2
npm -v
npm explore demo -- npm run print-args -- c:\abc

```

will output (please not `'c:\\\\abc'`):

```
c:\Dev\Projects\npm-double-slashes>node print.js c:\abc
[
  'C:\\Program Files\\nodejs\\node.exe',
  'c:\\Dev\\Projects\\npm-double-slashes\\print.js',
  'c:\\abc'
]

c:\Dev\Projects\npm-double-slashes>nvm use 17.3.0
Now using node v17.3.0 (64-bit)

c:\Dev\Projects\npm-double-slashes>npm -v
8.3.0

c:\Dev\Projects\npm-double-slashes>npm explore demo -- npm run print-args -- c:\abc

> npm-double-slashes@1.0.0 print-args
> node print "c:\\abc"

[
  'C:\\Program Files\\nodejs\\node.exe',
  'c:\\Dev\\Projects\\npm-double-slashes\\node_modules\\demo\\print',
  'c:\\\\abc'
]

c:\Dev\Projects\npm-double-slashes>nvm use 14.18.2
Now using node v14.18.2 (64-bit)

c:\Dev\Projects\npm-double-slashes>npm -v
6.14.15

c:\Dev\Projects\npm-double-slashes>npm explore demo -- npm run print-args -- c:\abc

> npm-double-slashes@1.0.0 print-args c:\Dev\Projects\npm-double-slashes\node_modules\demo
> node print "c:\abc"

[
  'C:\\Program Files\\nodejs\\node.exe',
  'c:\\Dev\\Projects\\npm-double-slashes\\node_modules\\demo\\print',
  'c:\\abc'
]


```