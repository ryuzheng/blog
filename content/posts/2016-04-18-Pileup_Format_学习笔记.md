---
title: "Pileup Format 学习笔记"
date: 2016-04-18T06:25:00+08:00
published: true
slug: pileup-format-xue-xi-bi-ji
tags: ['SAMtools', '生物信息']
cover_image: 
canonical_url: false
description: "Pileup format is first used by Tony Cox and Zemin Ning at the Sanger Institute. It desribes the base-pair information at each chromosomal position. This format facilitates SNP/indel calling and brief alignment viewing by eyes. "
---



Pileup format is first used by Tony Cox and Zemin Ning at the Sanger Institute. It desribes the base-pair information at each chromosomal position. This format facilitates SNP/indel calling and brief alignment viewing by eyes. 

Pileup 格式是桑格中心（Tony Cox and Zemin Ning）提出，描述可用肉眼观察的某一个区域所有reads匹配的情况。

The pileup format has several variants. The default output by SAMtools looks like this:

```
seq1	272	T	24	,.$.....,,.,.,...,,,.,..^+.	<<<+;<<<<<<<<<<<=<;<;7<&
seq1	273	T	23	,.....,,.,.,...,,,.,..A	<<<;<<<<<<<<<3<=<<<;<<+
seq1	274	T	23	,.$....,,.,.,...,,,.,...	7<7;<;<<<<<<<<<=<;<;<<6
seq1	275	A	23	,$....,,.,.,...,,,.,...^l.	<+;9*<<<<<<<<<=<<:;<<<<
seq1	276	G	22	...T,,.,.,...,,,.,....	33;+<<7=7<<7<&<<1;<<6<
seq1	277	T	22	....,,.,.,.C.,,,.,..G.	+7<;<<<<<<<&<=<<:;<<&<
seq1	278	G	23	....,,.,.,...,,,.,....^k.	%38*<<;<7<<7<=<<<;<<<<<
seq1	279	C	23	A..T,,.,.,...,,,.,.....	;75&<<<<<<<<<=<<<9<<:<<
```

where each line consists of 

1. chromosome,  染色体
2. 1-based coordinate,  染色体上的位置
3. reference base,  该位点参考序列上的碱基
4. the number of reads covering the site,  覆盖度（测得reads的数目）
5. read bases and base qualities.  该位点的每条reads与该位点的匹配方式
6. mapping quality 匹配质量 ([Phred quality score](http://en.wikipedia.org/wiki/Phred_quality_score) from 0 to 93 using [ASCII](http://en.wikipedia.org/wiki/ASCII) 33 to 126 (although in raw read data the Phred quality score rarely exceeds 60, higher scores are possible in assemblies or read maps))

## read bases column

- `.` stands for a match to the reference base on the forward strand 
代表匹配到正链
- `,` for a match on the reverse strand 
代表匹配到负链
- `ACGTN` for a mismatch on the forward strand 
大写的`ACGTN`代表与reference的正向链上不同的实际碱基的5种情况
- `acgtn` for a mismatch on the reverse strand 
小写的`acgtn`代表与reference的反向链上不同的实际碱基的5种情况
- A pattern `\+[0-9]+[ACGTNacgtn]+` indicates there is an insertion between this reference position and the next reference position. The length of the insertion is given by the integer in the pattern, followed by the inserted sequence. 
  - `seq2 156 A 11  .$......+2AG.+2AG.+2AGGG    <975;:<<<<<`中的`+2AG`有3处，代表有3个read上有`AG`的2个bp的插入
- Similarly, a pattern `-[0-9]+[ACGTNacgtn]+' represents a deletion from the reference. 
  - `seq3 200 A 20 ,,,,,..,.-4CACC.-4CACC....,.,,.^~. ==<<<<<<<<<<<::<;2<<`同理，此处的`-4CACC`有2处，代表有2个read上有`CACC`的4个bp的缺失
- a symbol `^` marks the start of a read segment which is a contiguous subsequence on the read separated by `N/S/H` CIGAR operations.  
`^`代表刚好是read的开头
- The ASCII of the character following `^` minus 33 gives the mapping quality. 
`^`后面跟着的符号表示比对的质量（ASCII码减33）
- A symbol `$` marks the end of a read segment. 
`$`代表刚好是read的结尾

## reference

- [Pileup Format | SAMtools](http://samtools.sourceforge.net/pileup.shtml)
- [Pileup format | Wikipedia](https://en.wikipedia.org/wiki/Pileup_format)