这是一个资源托管库，和 CDN 搭配加载博客的各种资源。

注意：

*因为 CSS 我用的是 CSS整合，在 publish 处所有 CSS 都被导入到了  index.css （虽然仍有custom文件夹，但此时里面的所有样式已经被整合到 index.css 中了，又因为 inject 处没有引用任何CSS，所以最终页面只会引入被整合好的 index.css），而 index.css 经常要改变，内容不是固定的，不适合CDN引用，不方便调试*。因此 index.css 只能从 github 拿，比较慢，后续利用阿里云CDN加速网站可以提速。

但是我所有的JS（确保JS内容是固定的，不会变化）都先利用gulp压缩后，上传到npm，再利用npm 引用，加快速度（如果是本地引用相当于从github拿，慢！）
