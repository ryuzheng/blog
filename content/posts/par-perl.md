---
title: 解包一个 PAR 打包的 perl 程序源码
date: 2020-10-18T07:00:00Z
published: false
slug: decoded_a_Perl_script
tags:
- 生物信息
- Perl
cover_image: ''
canonical_url: false
description: 最近在流程debug时，和同事发现一个perl脚本有问题；然后发现这个perl脚本是别人打包好的，所以看不到源码。我就想把这个perl脚本给解包（不敢称为反编译，因为实际上也没做反编译）了。

---
最近在流程debug时，和同事发现一个perl脚本有问题；然后发现这个perl脚本是别人打包好的，所以看不到源码。我就想把这个perl脚本给解包（不敢称为反编译，因为实际上也没做反编译）了。因为打包perl脚本这事，我以前也做过，所以我觉得应该可行。

\## 如何用PAR来打包perl程序

首先，一般打包perl程序，现在应该是使用\[PAR::Packer\]([http://search.cpan.org/\~rschupp/PAR-Packer-1.040/lib/PAR/Packer.pm](http://search.cpan.org/\~rschupp/PAR-Packer-1.040/lib/PAR/Packer.pm "http://search.cpan.org/~rschupp/PAR-Packer-1.040/lib/PAR/Packer.pm"))打包perl程序，具体参数参考\[[http://search.cpan.org/\~rschupp/PAR-1.015/lib/PAR/Tutorial.pod](http://search.cpan.org/\~rschupp/PAR-1.015/lib/PAR/Tutorial.pod "http://search.cpan.org/~rschupp/PAR-1.015/lib/PAR/Tutorial.pod")\]([http://search.cpan.org/\~rschupp/PAR-1.015/lib/PAR/Tutorial.pod](http://search.cpan.org/\~rschupp/PAR-1.015/lib/PAR/Tutorial.pod "http://search.cpan.org/~rschupp/PAR-1.015/lib/PAR/Tutorial.pod"))。打包的命令如下：

\`\`\`bash

pp -g -B -o xxx xxx.pl # -g 生成二进制程序, -B 将各种依赖项打包进去, -o 生成的文件名

\`\`\`

\## 解包PAR打包的perl程序

首先运行一次程序，然后在 \`/tmp/\` 目录下，查看是否有 \`par-\` 开头的文件夹，PAR打包的程序有个特点，就是会将程序解压到 \`/tmp\` 目录下，然后再运行。

!\[image.png\](![](https://cdn.nlark.com/yuque/0/2020/png/109224/1595405672421-efbaddb0-f8ed-4324-9834-c559b171b713.png#align=left&display=inline&height=62&margin=%5Bobject%20Object%5D&name=image.png&originHeight=124&originWidth=1172&size=44192&status=done&style=none&width=586))

如果有这样子的目录，应该就是PAR打包的了。然后我们使用 \`unzip -d unarchive xxx\`，xxx是你的程序的名称，这样子我们就能得到解压的目录。（\[xxx.zip\]([https://www.yuque.com/attachments/yuque/0/2020/zip/109224/1595582389314-7fa0e4b2-c9aa-4722-94eb-e75b1cef9f79.zip?_lake_card=%7B%22uid%22%3A%221595582387315-0%22%2C%22src%22%3A%22https%3A%2F%2Fwww.yuque.com%2Fattachments%2Fyuque%2F0%2F2020%2Fzip%2F109224%2F1595582389314-7fa0e4b2-c9aa-4722-94eb-e75b1cef9f79.zip%22%2C%22name%22%3A%22xxx.zip%22%2C%22size%22%3A2757816%2C%22type%22%3A%22application%2Fzip%22%2C%22ext%22%3A%22zip%22%2C%22progress%22%3A%7B%22percent%22%3A99%7D%2C%22status%22%3A%22done%22%2C%22percent%22%3A0%2C%22id%22%3A%22V2kji%22%2C%22card%22%3A%22file%22%7D](https://www.yuque.com/attachments/yuque/0/2020/zip/109224/1595582389314-7fa0e4b2-c9aa-4722-94eb-e75b1cef9f79.zip?_lake_card=%7B%22uid%22%3A%221595582387315-0%22%2C%22src%22%3A%22https%3A%2F%2Fwww.yuque.com%2Fattachments%2Fyuque%2F0%2F2020%2Fzip%2F109224%2F1595582389314-7fa0e4b2-c9aa-4722-94eb-e75b1cef9f79.zip%22%2C%22name%22%3A%22xxx.zip%22%2C%22size%22%3A2757816%2C%22type%22%3A%22application%2Fzip%22%2C%22ext%22%3A%22zip%22%2C%22progress%22%3A%7B%22percent%22%3A99%7D%2C%22status%22%3A%22done%22%2C%22percent%22%3A0%2C%22id%22%3A%22V2kji%22%2C%22card%22%3A%22file%22%7D "https://www.yuque.com/attachments/yuque/0/2020/zip/109224/1595582389314-7fa0e4b2-c9aa-4722-94eb-e75b1cef9f79.zip?_lake_card=%7B%22uid%22%3A%221595582387315-0%22%2C%22src%22%3A%22https%3A%2F%2Fwww.yuque.com%2Fattachments%2Fyuque%2F0%2F2020%2Fzip%2F109224%2F1595582389314-7fa0e4b2-c9aa-4722-94eb-e75b1cef9f79.zip%22%2C%22name%22%3A%22xxx.zip%22%2C%22size%22%3A2757816%2C%22type%22%3A%22application%2Fzip%22%2C%22ext%22%3A%22zip%22%2C%22progress%22%3A%7B%22percent%22%3A99%7D%2C%22status%22%3A%22done%22%2C%22percent%22%3A0%2C%22id%22%3A%22V2kji%22%2C%22card%22%3A%22file%22%7D"))，语雀对上传文件的后缀有限制，大家自行解压，然后执行exe文件测试）

\## 发现是Bleach加密过的源码

一般解压后我们就能得到脚本的原始\`.pl\` 文件，但是这次，我打开 \`xxx.pl\` 文件，却只看到一行很奇怪的代码，而文件的大小却很大。这里上传一个\[xxx.pl\]([https://www.yuque.com/attachments/yuque/0/2020/pl/109224/1595582242754-6491ad57-09cc-47bd-84fc-3049209bbf49.pl?_lake_card=%7B%22uid%22%3A%221595582242585-0%22%2C%22src%22%3A%22https%3A%2F%2Fwww.yuque.com%2Fattachments%2Fyuque%2F0%2F2020%2Fpl%2F109224%2F1595582242754-6491ad57-09cc-47bd-84fc-3049209bbf49.pl%22%2C%22name%22%3A%22xxx.pl%22%2C%22size%22%3A1114%2C%22type%22%3A%22text%2Fx-perl-script%22%2C%22ext%22%3A%22pl%22%2C%22progress%22%3A%7B%22percent%22%3A99%7D%2C%22status%22%3A%22done%22%2C%22percent%22%3A0%2C%22id%22%3A%22kPoyI%22%2C%22card%22%3A%22file%22%7D](https://www.yuque.com/attachments/yuque/0/2020/pl/109224/1595582242754-6491ad57-09cc-47bd-84fc-3049209bbf49.pl?_lake_card=%7B%22uid%22%3A%221595582242585-0%22%2C%22src%22%3A%22https%3A%2F%2Fwww.yuque.com%2Fattachments%2Fyuque%2F0%2F2020%2Fpl%2F109224%2F1595582242754-6491ad57-09cc-47bd-84fc-3049209bbf49.pl%22%2C%22name%22%3A%22xxx.pl%22%2C%22size%22%3A1114%2C%22type%22%3A%22text%2Fx-perl-script%22%2C%22ext%22%3A%22pl%22%2C%22progress%22%3A%7B%22percent%22%3A99%7D%2C%22status%22%3A%22done%22%2C%22percent%22%3A0%2C%22id%22%3A%22kPoyI%22%2C%22card%22%3A%22file%22%7D "https://www.yuque.com/attachments/yuque/0/2020/pl/109224/1595582242754-6491ad57-09cc-47bd-84fc-3049209bbf49.pl?_lake_card=%7B%22uid%22%3A%221595582242585-0%22%2C%22src%22%3A%22https%3A%2F%2Fwww.yuque.com%2Fattachments%2Fyuque%2F0%2F2020%2Fpl%2F109224%2F1595582242754-6491ad57-09cc-47bd-84fc-3049209bbf49.pl%22%2C%22name%22%3A%22xxx.pl%22%2C%22size%22%3A1114%2C%22type%22%3A%22text%2Fx-perl-script%22%2C%22ext%22%3A%22pl%22%2C%22progress%22%3A%7B%22percent%22%3A99%7D%2C%22status%22%3A%22done%22%2C%22percent%22%3A0%2C%22id%22%3A%22kPoyI%22%2C%22card%22%3A%22file%22%7D"))给大家玩一下😄️。

\`\`\`perl

$_=<<'';y;\\r\\n;;d;y; \\t;01;;$_=pack'b*',$_;$_=eval;$@&&die$@;$_

\`\`\`

是的，整个文件，我wc了一下，足足有2000多行，但是打开来看，却只看到一行代码，这时，我就猜想，这里应该是使用了什么方法来加密的代码，将代码替换成不可见的字符。

于是我使用 \`cat -vet xxx.pl| less -SN\` 来查看该文件，发现这个文件只有这种类似与tab的字符，

\`\`\`perl

$_=<<'';y;\\r\\n;;d;y; \\t;01;;$_=pack'b*',$_;$_=eval;$@&&die$@;$_$

^I^I   ^I  ^I$

^I  ^I^I$

^I^I ^I  ^I ^I$

\`\`\`

幸运的是，当我将 \`$_=<<'';y;\\r\\n;;d;y; \\t;01;;$_=pack'b*',$_;$_=eval;$@&&die$@;$_$\` 这串字符串拿去google之后，很快发现了\[A Beginner’s Guide to Compiling Perl Scripts\]([https://www.marcbilodeau.com/compiling-perl/](https://www.marcbilodeau.com/compiling-perl/ "https://www.marcbilodeau.com/compiling-perl/"))这篇文章，原来这种源码是经过Bleach这种方法加密过的。

当然，这里我还在stackoverflow查到这种方法是如何生效的。

\- \`$_ = << '';\` reads the rest of the file into the accumulator.

\- \`y;\\r\\n;;d;\` strips carriage returns and line feeds.

\- \`$_ = pack 'b*', $_;\` converts characters to bits in \`$_\`, LSB first.

\- \`$_ = eval;\` executes \`$_\` as Perl code.

\- \`$@ && die $@; $_\` handles exceptions and the return code gracefully.

\## 解密Bleach加密的源码

明白是怎么加密之后，我就开始找如何解密的方法，一开始我找到一个\[unbleach.pl\]([https://www.perlmonks.org/?node_id=85918](https://www.perlmonks.org/?node_id=85918 "https://www.perlmonks.org/?node_id=85918"))的代码，但是运行了之后，可能是这个代码太老了，并不能解码。

然后我看到\[How does this obfuscated Perl script work?\]([https://stackoverflow.com/questions/24578917/how-does-this-obfuscated-perl-script-work/24591240](https://stackoverflow.com/questions/24578917/how-does-this-obfuscated-perl-script-work/24591240 "https://stackoverflow.com/questions/24578917/how-does-this-obfuscated-perl-script-work/24591240"))和\[bleach question\]([https://www.perlmonks.org/?node_id=1101852](https://www.perlmonks.org/?node_id=1101852 "https://www.perlmonks.org/?node_id=1101852"))，一下子就醒悟过来，上面的原理解释里不是说了吗，用 \`eval\` 来执行 \`$_\`里的perl代码，那将 \`eval\` 修改为 \`print\`，就变成了将代码都打印出来😄️。

原来那么简单！

最后我修改文件头一行为

\`\`\`perl

$_=<<'';y;\\r\\n;;d;y; \\t;01;;$_=pack'b*',$_;$_=print;$@&&die$@;$_

\`\`\`

然后执行 \`perl xxx.pl | less -SN\`，终于看到源码了。

\## 讨论

其实，我在几年前，也做过加密打包perl代码的工作，然后当时就发现，这个打包然并卵。首先会解压到 \`/tmp\` 目录下，其次 \`unzip\` 一下也能解包，当时我也有看到 \`PAR::Filter::Bleach\` 和 \`PAR::Filter::Bytecode\` 等加密方法，以为加上这个参数应该就可以，但现在发现这种方法还是不安全的。

另外，我看了\[A Beginner’s Guide to Compiling Perl Scripts\]([https://www.marcbilodeau.com/compiling-perl/](https://www.marcbilodeau.com/compiling-perl/ "https://www.marcbilodeau.com/compiling-perl/"))里面加上 \`PAR::Filter::Crypto\` 的方法，我看到程序要解密加密后的代码，代码必须将密钥也写在代码里，所以应该也是然并卵的。

\`\`\`bash

dev@virtualbox:\~/Desktop/decompile$ more script/bestProgram.pl

 

use Filter::Crypto::Decrypt;

e6ad69e3dd1e9901ccf6ba701ff66dfb09ba6b2f6c3b872e33e1929e56298f9861777c6a21255012

c658fa873214cd41071f6915ef594ee5447ae02afc1e2d726333fb855148920518e827fbb0990053

73eddabe4e608e15fcc54008c659f218ac32d56ac8d05a78cfd446ae05cf6c19f7e3c1b30fab747f

c0b0017243f764b3d3db0ef081bbf245478bd5a6af4c361b22e488edf203d91d4018d930fe4d2c1b

8d4d103810eb0603

dev@virtualbox:\~/Desktop/decompile$ ../bestProgram_Encrypted.exe 

$VAR1 = {

 'second' => 2,

 'first' => 1,

 'third' => 3

 };

\`\`\`

最好的方法，还是将perl代码转换为c代码，甚至是c代码的模块的形式，增加反编译的难度。

\## 参考

\- \[A Beginner’s Guide to Compiling Perl Scripts\]([https://www.marcbilodeau.com/compiling-perl/](https://www.marcbilodeau.com/compiling-perl/ "https://www.marcbilodeau.com/compiling-perl/"))

\- \[What does this perl line from a “bleached” file do?\]([https://stackoverflow.com/questions/7556782/what-does-this-perl-line-from-a-bleached-file-do](https://stackoverflow.com/questions/7556782/what-does-this-perl-line-from-a-bleached-file-do "https://stackoverflow.com/questions/7556782/what-does-this-perl-line-from-a-bleached-file-do"))

\- \[bleach question\]([https://www.perlmonks.org/?node_id=1101852](https://www.perlmonks.org/?node_id=1101852 "https://www.perlmonks.org/?node_id=1101852"))

\- \[How does this obfuscated Perl script work?\]([https://stackoverflow.com/questions/24578917/how-does-this-obfuscated-perl-script-work/24591240](https://stackoverflow.com/questions/24578917/how-does-this-obfuscated-perl-script-work/24591240 "https://stackoverflow.com/questions/24578917/how-does-this-obfuscated-perl-script-work/24591240"))

\- \[unbleach.pl\]([https://www.perlmonks.org/?node_id=85918](https://www.perlmonks.org/?node_id=85918 "https://www.perlmonks.org/?node_id=85918"))