TCP与UDP都位于 [[OSI七层网络结构]]中的传输层, 解决的都是传输的问题
TCP是面向连接的, 而UDP是面向报文的
TCP保证可靠  UDP不保证,只做尽力交付 ; 因此UDP适用于实时性要求高且可以容忍差错与丢包的场景 比如直播推流与游戏服务器,  而TCP则因为其面向连接可靠传输的特性, 适用于诸如文件下载等场景

[[TCP报文结构]]

[[TCP拥塞控制与流量控制]]