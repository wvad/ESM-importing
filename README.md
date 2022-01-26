# ESMのimportの内部動作の雑な説明

<a name="ESMLoader_import"></a>
## async ESMLoader#import(specifiers, parentURL, importAssertions) [loader.js:324](https://github.com/nodejs/node/tree/ccb8aae3932c13f33622203b2ffc5a33120e9d40/lib/internal/modules/esm/loader.js#L324)
- [ESMLoader#getModuleJob](#ESMLoader_getModuleJob) を呼び出す。
- それの戻り値(Promise)のthenに  
```js
(job) => job.run()
```
を渡す。(job.runは、[ModuleJob#run](#ModuleJob_run))

<a name="ESMLoader_getModuleJob"></a>
## async ESMLoader#getModuleJob(specifier, parentURL, importAssertions) [loader.js:242](https://github.com/nodejs/node/tree/ccb8aae3932c13f33622203b2ffc5a33120e9d40/lib/internal/modules/esm/loader.js#L242)
- ESMLoader#moduleMap に 該当する [ModuleJob](#ModuleJob) が無いなら、[ESMLoader#createModuleJob](#ESMLoader_createModuleJob) を呼び出し、ESMLoader#moduleMap にセット。
- あるならば、それを返す。

<a name="ESMLoader_createModuleJob"></a>
## private ESMLoader#createModuleJob(url, importAssertions, parentURL, format) [loader.js:278](https://github.com/nodejs/node/tree/ccb8aae3932c13f33622203b2ffc5a33120e9d40/lib/internal/modules/esm/loader.js#L278)
- [ModuleJob constructor](#ModuleJob) を呼び出す。

<a name="ModuleJob"></a>
## class ModuleJob(loader, url, importAssertions, moduleProvider, isMain, inspectBrk) [module_job.js:53](https://github.com/nodejs/node/tree/ccb8aae3932c13f33622203b2ffc5a33120e9d40/lib/internal/modules/esm/module_job.js#L53)
- [ESMLoader#createModuleJob](#ESMLoader_createModuleJob) から渡された [moduleProvider](#moduleProvider) を呼び出す。

<a name="moduleProvider"></a>
## async function moduleProvider(url, isMain) [loader.js:279](https://github.com/nodejs/node/tree/ccb8aae3932c13f33622203b2ffc5a33120e9d40/lib/internal/modules/esm/loader.js#L279)
- [ESMLoader#load](#ESMLoader_load) を呼び出し、ソースコード等の情報を取得。
- それぞれのファイルにあった関数を呼び出して [ModuleWrap](#ModuleWrap) を取得する。

| 種類 | 使う関数 |
| ---- | ---- |
| ESM | [moduleStrategy](#moduleStrategy) |
| CJS | commonjsStrategy |
| ビルトインモジュール | builtinStrategy |
| JSON | jsonStrategy |

<a name="ESMLoader_load"></a>
## async ESMLoader#load(url, context) [loader.js:353](https://github.com/nodejs/node/tree/ccb8aae3932c13f33622203b2ffc5a33120e9d40/lib/internal/modules/esm/loader.js#L353)
- [defaultLoad](#defaultLoad) を呼び出す。

<a name="defaultLoad"></a>
## async function defaultLoad(url, context) [load.js:13](https://github.com/nodejs/node/tree/ccb8aae3932c13f33622203b2ffc5a33120e9d40/lib/internal/modules/esm/load.js#L13)
- [defaultGetSource](#defaultGetSource) を呼び出す。

<a name="defaultGetSource"></a>
## async function defaultGetSource(url, { format }, defaultGetSource) [get_source.js:25](https://github.com/nodejs/node/tree/ccb8aae3932c13f33622203b2ffc5a33120e9d40/lib/internal/modules/esm/get_source.js#L25)
- [FileHandle#readFile](#FileHandle_readFile) を呼び出す。

<a name="FileHandle_readFile"></a>
## FileHandle#readFile(options) [internal/fs/promises.js:173](https://github.com/nodejs/node/tree/ccb8aae3932c13f33622203b2ffc5a33120e9d40/lib/internal/fs/promises.js#L173)
- ファイルを読み込む。

<a name="moduleStrategy"></a>
## async function moduleStrategy(url, source, isMain) [translators.js:110](https://github.com/nodejs/node/tree/ccb8aae3932c13f33622203b2ffc5a33120e9d40/lib/internal/modules/esm/translators.js#L110)
- [ModuleWrap constructor](#ModuleWrap) を呼び出す。

<a name="ModuleWrap"></a>
## class ModuleWrap(url, context, ...) [module_wrap.cc:110](https://github.com/nodejs/node/blob/ccb8aae3932c13f33622203b2ffc5a33120e9d40/src/module_wrap.cc#L110)
- `constructor(url, context, exportNames, syntheticExecutionFunction)`
- `constructor(url, context, source, lineOffset, columOffset, cachedData)`

<a name="ModuleJob_run"></a>
## async ModuleJob#run() [module_job.js:190](https://github.com/nodejs/node/tree/ccb8aae3932c13f33622203b2ffc5a33120e9d40/lib/internal/modules/esm/module_job.js#L190)
- ModuleWrap#evaluate を呼び出す。
