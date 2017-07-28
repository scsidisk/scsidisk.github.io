Electron APP 支持应用内下载文件及显示下载进度
===========================================

最近把公司一个 Web APP 项目用 Electron 封装了一个 Mac 客户端，主要是弥补了 Web 浏览器的一些先天不足：

支持原生的通知
支持原生的 icon 未读提醒
支持原生的系统托盘
增强网络状态变更的感知
其中以前下载方式是通过打开系统浏览器进行文件下载的。因为文件需要鉴权，还得携带一些敏感的 cookie、token 过去，感觉不安全，所以希望文件下载能在 APP 内完成

通过 Electron 中 will-download 事件，我们可以很方便的解决这个问题

    mainWindow.webContents.session.on('will-download', (e, item) => {
        //获取文件的总大小
       const totalBytes = item.getTotalBytes();

        //设置文件的保存路径，此时默认弹出的 save dialog 将被覆盖
       const filePath = path.join(app.getPath('downloads'), item.getFilename());
       item.setSavePath(filePath);

        //监听下载过程，计算并设置进度条进度
       item.on('updated', () => {
           mainWindow.setProgressBar(item.getReceivedBytes() / totalBytes);
       });

        //监听下载结束事件
       item.on('done', (e, state) => {
            //如果窗口还在的话，去掉进度条
           if (!mainWindow.isDestroyed()) {
               mainWindow.setProgressBar(-1);
           }

            //下载被取消或中断了
           if (state === 'interrupted') {
               electron.dialog.showErrorBox('下载失败', `文件 ${item.getFilename()} 因为某些原因被中断下载`);
           }

            //下载完成，让 dock 上的下载目录Q弹一下下
           if (state === 'completed') {
               app.dock.downloadFinished(filePath);
           }
       });
    });