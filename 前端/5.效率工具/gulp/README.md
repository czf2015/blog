# gulp


Gulp是基于流的构建系统。Gulp之所以能实现高度重用，主要归功于两项技术：使用插件和自定义构建任务。构建过程是一个流，所以这些任务和插件是可以一个接一个拼在一起的。

用gulp.task()添加任务，这些任务通常都遵循相同的模式:
1. 源文件——收集输入文件；
2. 转译——让它们依次通过一个个对它们进行转换的插件；
3. 合并——把这些文件合到一起，创建一个整体构建文件；
4. 输出——设定文件的目标地址或移动输出文件。


##  Gulp任务的创建与运行

> 代码示例：
```js
const gulp = require('gulp');
const sourcemaps = require('gulp-sourcemaps');   //像加载标准Node模块那样加载Gulp 插件
const babel = require('gulp-babel');
const concat = require('gulp-concat');　

// 用Babel处理ES2015和React的gulpfile 
gulp.task('default', () => {
    return gulp.src('app/*.jsx')   //用Gulp自带的文件聚集工具gulp.src查找所有的React jsx文件
        .pipe(sourcemaps.init())   //开始监视源文件，为调试构建源码映射    
        .pipe(babel({
            presets: ['es2015', 'react']   //使用ES2015和React（JSX）配置gulp-babel    
        }))
        .pipe(concat('all.js'))   //把所有源码文件拼到一个all.js中    
        .pipe(sourcemaps.write('.'))   //单独写入源码映射文件
        .pipe(gulp.dest('dist'));   //将所有文件放到dist/目录下});

// 检测变化
gulp.task('watch', () => {  
    watch('app/**.jsx', () => gulp.start('default'));
}); 
```

首先是用文件聚集找到所有输入文件，然后用gulp-sourcemaps插件为客户端调试采集源码映射指标。注意，源码映射需要两个阶段：一个阶段是声明想要用源码映射，另一个阶段是写源码映射文件。在这个例子里，所有文件转换都是一个插件做的。转换完成后，用gulp-concat插件把文件合到一起。现在所有转译都做完了，可以写源码映射了。最终的构建结果可以放到dist文件夹下。 

> 检测变化：
```js
gulp.task('watch', () => {  
    watch('app/**.jsx', () => gulp.start('default'));
}); 
```


## 在大项目中把任务分散到不同文件中

项目规模变大后，一般会需要更多的Gulp任务。最终会出现一个大到难以理解的长文件，如果把代码分解成不同的模块，就可以解决这个问题。可以按如下步骤来使用分散的文件：
1. 创建一个名为gulp的文件夹以及一个名为tasks的子目录；
2. 在各个文件中用gulp.task()语法定义任务，最好是每个任务放一个文件；
3. 创建一个名为gulp/index.js的文件，在其中加载所有的Gulp任务文件；
4. 在gulpfile.js中引入这个gulp/index.js文件。

> 目录结构：
gulpfile.js
gulp
  |__index.js
  |__tasks
       |__development-build.js
       |__production-build.js

用这个办法组织复杂的构建任务，还可以跟gulp-help模块搭配起来用。gulp-help模块可以生成任务文档，运行gulp help可以显示每个任务的帮助信息。在你需要团队协作时，或者要在很多使用Gulp的项目中切换时，就能体会到这个插件的价值了。 

Gulp是一个通用的项目自动化工具。它适合管理项目里的跨平台清理脚本，比如运行复杂的客户端测试或者为数据库提供固定的测试环境。

> reference
  1. 《Node.js实战》(第2版）——4.3节