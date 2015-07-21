
在浏览器中使用seajs2.3.x作为加载器，使用spm-sea打包的js资源时:

* 如果未使用spm_modules中的资源，可以直接使用spm-sea命令打包。
* 如果使用了，则需要在其后加上参数 --include all 

__备注：不添加“--include all”时，require无法找到资源。

#### why ?

spm3使用CommondJS规范，对外直接使用"exports/module.exports"，未做封装： 

    ## init.js
    define(function(require, exports, module) {
        // examples
        // module.exports = function(){};
        exports.init = function(){};
    });
    ## run.js
    define(function(require, exports, module) {
        // examples
        // module.exports = function(){};
        exports.run = require('init').init();
    });
    ## end.js
    define(function(require, exports, module) {
        // examples
        // module.exports = function(){};
        exports.run = require('run').init();
    });

因为不会将其打包进来，所以在浏览器中使用的时候，无法找到资源。


