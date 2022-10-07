# qBittorrent 教程

[qBittorrent](https://www.qbittorrent.org/) 是一款开源免费的种子和磁力链接下载工具，支持 Windows、Mac 和 Linux，且功能非常强大。

## qBittorrent 配置

相比迅雷开箱即用，qBittorrent 需要一定的配置才可以有比较好的下载速度，主要配置两个地方一个是`端口映射`，另一个是添加 `trackers`。

端口映射的配置可参照[此处](https://blog.csdn.net/yhblog/article/details/113516378#:~:text=%E4%B8%80%E3%80%81qbittorrent%E4%B8%8B%E8%BD%BD%E9%80%9F%E5%BA%A6%E4%B8%BA0%E7%9A%84%E5%8E%9F%E5%9B%A0%EF%BC%9A%20%E5%A6%82%E6%9E%9C%E6%B2%A1%E6%9C%89%E9%85%8D%E7%BD%AE%E8%B7%AF%E7%94%B1%E5%99%A8%E7%9A%84UPnP%20%2F,NAT-PMP%20%E5%8A%9F%E8%83%BD%EF%BC%8C%E6%96%B0%E7%89%88%E8%B7%AF%E7%94%B1%E5%99%A8%E6%98%AF%E9%80%9A%E8%BF%87webUI%E6%93%8D%E4%BD%9C%E8%B7%AF%E7%94%B1%EF%BC%8C%E5%8F%82%E8%80%83%20https%3A%2F%2Fjingyan.baidu.com%2Farticle%2F7c6fb42821127b80642c90f4.html)，主要内容是在路由器中开启 `UPnP / NAT-PMP` 功能（添加一个端口映射实现）。

`trackers` 列表的添加可查看[此处](https://zhuanlan.zhihu.com/p/89430684)，主要提到可以去这个[项目](https://github.com/ngosang/trackerslist)下面获取最新可用的 `trackers list`。
