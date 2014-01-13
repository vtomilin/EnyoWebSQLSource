Installation
------------
`websql.Source` is installed into your project's lib directory, much like
any other Enyo plugin, for example `layout` or `onyx`. [See this post in Enyo Wiki for details](https://github.com/enyojs/enyo/wiki/Managing-Your-Project).

Generally, you want to add `websql.Source` plugin as a git submodule of your
project, placed under `lib/indexeddb` directory.

One way to do that is to run the following at your project's root directory:

    git submodule add https://github.com/vtomilin/EnyoWebSQLSource.git lib/websql


Then you should add `websql.Source` dependency to your top-level `package.js`
file:

```javascript
enyo.depends(
    '$lib/layout',
    '$lib/onyx',
    '$lib/websql',
    // Other dependencies...
);
```
