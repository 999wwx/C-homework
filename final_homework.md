# C++期末大作业

学号：18062036

姓名：王伟雄

## 简述

此次作业的主要内容是对github上的开源代码nebula进行单元测试或是根据自己的想法进行修改，并用git提交到github上，以此来锻炼我们的代码阅读水平以及项目工程级代码规范以及操作要求。

## 过程描述

### 第一部分：前期准备

根据nebula开源项目的手册说明，发现该项目可在虚拟机Ubuntu环境中进行构建。于是准备使用git将项目克隆到本地，发现虚拟机里没有安装git。习惯性

```
sudo apt update
sudo apt upgrade
```

发现因为许久没有使用，需要更新的软件过多而且很慢。于是网上寻找国内的镜像软件更新源。而后使用命令

```
sudo gedit /etc/apt/sources.list
```

将文件备份后更改为


> deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic main restricted universe multiverse

>deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-updates main restricted universe multiverse

>deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-backports main restricted universe multiverse

>deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-security main restricted universe multiverse

将软件更新源更改为清华大学的更新源后速度飞快，很快就完成了软件更新，随后使用命令行安装并检查是否安装完毕git。

```
apt install git
git --version
```

### 第二部分：nebula构建

第一步：克隆代码

```
git clone https://github.com/vesoft-inc/nebula.git
```

第二步：安装依赖

```
cd nebula && ./build_dep.sh
```

第三步：应用 ~/.bashrc 修改

```
source ~/.bashrc
```

第四步：构建 Debug 版本

> mkdir build && cd build

> cmake ..

> make -j$(nproc)

> sudo make install -j$(nproc)

ps:根据了解后，得知此处-j$(nproc)是多线程进行make操作

最终得到结果：

> [100%] Built target ....

编译成功！

### 第三部分：艰难的更改代码

第一次更改：
``` cpp
std::cout << "Got " << resp.get_rows()->size()
                      << " rows (Time spent: "
                      << resp.get_latency_in_us() << "/"
                      << dur.elapsedInUSec() << " us)\n";
```
更改为
``` cpp
temp_time = resp.get_latency_in_us();
            if (temp_time < 1000) {
            std::cout << "Empty set (Time spent: "
                      << temp_time << "/"
                      << dur.elapsedInUSec() << " us)\n";
            } else if (temp_time < 1000000) {
            std::cout << "Empty set (Time spent: "
                      << (temp_time-temp_time%100)/1000.0 << "/"
                      << dur.elapsedInUSec()/1000.0 << " ms)\n";
            } else if (resp.get_latency_in_us() >= 1000000) {
            std::cout << "Empty set (Time spent: "
                      << (temp_time-temp_time%100000)/1000000.0 << "/"
                      << dur.elapsedInUSec()/1000000.0 << " s)\n";
            }
```

虽然实现了需要的功能，将us单位成功更改为根据数据量更改为us或ms或s，但是代码可读性太差了。根据老师的建议，将数值宏定义，并且封装函数进行使用。对此，我进行了第二次更改：

``` cpp
#define 1000.0 1k

#define 1000000.0 1m

void time_transformation(std::int temp_time, std::int temp_time1) {
    if (temp_time < 1k) {
            std::cout << "Empty set (Time spent: "
                      << temp_time << "/"
                      << temp_time1 << " us)\n";
            } else if (temp_time < 1m) {
            std::cout << "Empty set (Time spent: "
                      << (temp_time-temp_time%100)/1k << "/"
                      << temp_time1/1k << " ms)\n";
            } else if (resp.get_latency_in_us() >= 1m) {
            std::cout << "Empty set (Time spent: "
                      << (temp_time-temp_time%100000)/1m << "/"
                      << dur.elapsedInUSec()/1m << " s)\n";
            }
}

std::int temp_time = resp.get_latency_in_us();
            std::int temp_time1 = dur.elapsedInUsec();
            time_transformation(temp_time, temp_time1);
```

虽然老师大致认为我可以勉强通过了，但是为了实现保留一位小数的逻辑比较绕，并且提供了函数实现保留一位小数的文档，再次根据教程进行了第三次更改：

``` cpp
#include <iomanip>

#define Kilo 1000.0
#define Million 1000000.0

void time_transformation(std::double temp_time, std::double temp_time1) {
    if (temp_time < Kilo) {
            std::cout << "Empty set (Time spent: "
                      << setiosflags(ios::fixed) << setprecision(1)
                      << temp_time << "/"
                      << temp_time1 << " us)\n";
    } else if (temp_time < Million) {
            std::cout << "Empty set (Time spent: "
                      << setiosflags(ios::fixed) << setprecision(1)
                      << temp_time/Kilo << "/"
                      << temp_time1/Kilo << " ms)\n";
    } else if (resp.get_latency_in_us() >= Million) {
            std::cout << "Empty set (Time spent: "
                      << setiosflags(ios::fixed) << setprecision(1)
                      << temp_time/Million << "/"
                      << temp_time1/Million << " s)\n";
    }
}

std::double temp_time = resp.get_latency_in_us();
std::double temp_time1 = dur.elapsedInUsec();
time_transformation(temp_time, temp_time1);
```

因为以前没有接触过大项目，对于这种"简单的使用库函数"操作很自信，所以直接在本文件里添加了头文件，并且没有重新进行编译，修改为C++谷歌风格后直接上传GitHub。直至第二天看到GitHub发送的邮件，大致意思：你的仓库在所有环境里均发现错误。我蒙了，直到在自己的环境里编译了一遍，发现无数报错。然后只能在无数错误中寻找头绪，最后发现是"简单的包含头文件并使用头文件函数"出错了。

鉴于自己是初次接触大项目，并且由于设备性能限制，更改基础文件base.h需要耗费的时间过久，只能更改为比较差的逻辑修改；此外，在本地编译时，还发现其他错误，比如说函数返回值类型的错误，解决方法：查看函数定义。查看函数定义后发现还可以使用定义好的其他函数，而不必使用逻辑实现。最后对我的代码进行第四次修改：

``` cpp
#define Kilo 1000.0
#define Million 1000000.0

void time_transformation(int32_t temp_time,
                uint64_t temp_time1,
                time::Duration dur,
                cpp2::ExecutionResponse resp) {
    if (temp_time < Kilo || temp_time1 < Kilo) {
        std::cout << "Got " << resp.get_rows()->size()
                  << " rows (Time spent: "
                  << temp_time << "/"
                  << dur.elapsedInUSec() << " us)\n";
    } else if (temp_time/Kilo < Kilo) {
        std::cout << "Got " << resp.get_rows()->size()
                  << " rows (Time spent: "
                  << resp.get_latency_in_us()/Kilo << "/"
                  << dur.elapsedInMSec() << " ms)\n";
    } else {
        std::cout << "Got " << resp.get_rows()->size()
                  << " rows (Time spent: "
                  << resp.get_latency_in_us()/Million << "/"
                  << dur.elapsedInSec() << " s)\n";
    }
}

auto temp_time = resp.get_latency_in_us();
uint64_t temp_time1 = dur.elapsedInUSec();
time_transformation(temp_time, temp_time1, dur, resp);
```

### 第四部分：上传代码至GitHub

第一步：生成密钥

> ssh-keygen -t rsa -C "youremail@mail.com"

第二步：连接GitHub并验证

> 在～/下生成.ssh文件夹，按住ctrl+h可以显示隐藏文件夹，点进去，打开id_rsa.pub，复制里面的key。回到github，进入Account Setting，左边选择SSH Keys，Add SSH，title随便填，粘贴key.

>验证：ssh -T git@github.com

第三步：创建新仓库

第四步：本地仓库初始化

> cd nebula/build

> git init

第五步：增加仓库索引

> git add .

第六步：添加评论

> git commit -m "first commit" 

第七步：清空当前远程oringin

> git remote rm origin

第八步：新建仓库名.git

> git remote add origin https://github.com/999wwx/nebula.git

第九步：上传代码

> git push -u origin +master 

以上为为修改代码前的上传项目步骤

以下为修改代码后，上传修改代码的步骤

第一步：查看修改文件

> git status;

第二步：保存修改(顺便检查代码格式)

> git commit -a

第三步：上传

> git push

## 心得

经过本次作业，我更加了解了git的使用，同时对个github的了解也提升了一个层面，我懂得了如何用git提交代码，以及如何用github的commit与源代码作者进行交流，此外，我对大项目的理解程度更上一层，我还懂得了如何cmake编译代码以及如何直接调用make完的代码。最最重要的是初次接触大项目的经验，虽然接触的不多，但也是一次很珍贵的一次经验。另外，在询问老师您的时候，我也深刻的认识到自己的不足：代码风格不好，没有工业化水准。也知道以后应该如何提升自己，在这里，我深切的感谢老师给我们带来的大项目接触体验和耐心指导。
