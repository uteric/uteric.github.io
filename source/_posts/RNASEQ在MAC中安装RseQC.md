---
title: RNASEQ分析入门笔记4-MAC安装RseQC
date: 2018-03-23 22:38:56
updated: 2018-03-23 22:38:56
tags: RNASEQ
categories: 编程语言
---

RseQC是对比对结果进行质控的工具，在MAC环境下，可以优先选用conda命令进行安装，不会报错。

``` bash
conda install rseqc #安装RseQC，注意都是小写
```

Solving environment: /

*The following packages will be downloaded:*

package                    |            build
---------------------------|-----------------
wheel-0.30.0               |   py27h677a027_1          66 KB
jedi-0.11.1                |           py27_0         297 KB
idna-2.6                   |   py27hedea723_1         122 KB
python-2.7.14              |      h138c1fe_30        11.5 MB
backports.shutil_get_terminal_size-1.0.0|   py27hc9115de_2           8 KB
pycparser-2.18             |   py27h0d28d88_1         167 KB
send2trash-1.5.0           |           py27_0          16 KB
bx-python-0.7.3            |           py27_0         863 KB  bioconda
ipython-5.4.1              |           py27_2        1014 KB
urllib3-1.22               |   py27hc3787e9_0         153 KB
configparser-3.5.0         |   py27hc7edf1b_0          35 KB
python-dateutil-2.3        |           py27_0         213 KB  bioconda
pygments-2.2.0             |   py27h1a556bb_0         1.3 MB
terminado-0.8.1            |           py27_1          20 KB
backports-1.0              |   py27hb4f9756_1           3 KB
testpath-0.3.1             |   py27h72d81a5_0          89 KB
notebook-5.4.0             |           py27_0         6.6 MB
pyopenssl-17.5.0           |   py27hfda213f_0          77 KB
decorator-4.2.1            |           py27_0          15 KB
webencodings-0.5.1         |   py27h19a9f58_1          19 KB
nbconvert-5.3.1            |   py27h6455e4c_0         394 KB
ipykernel-4.8.2            |           py27_0         143 KB
functools32-3.2.3.2        |   py27h8ceab06_1          22 KB
entrypoints-0.2.3          |   py27hd680fb1_2           9 KB
asn1crypto-0.24.0          |           py27_0         155 KB
pip-9.0.1                  |           py27_5         2.2 MB
pyzmq-17.0.0               |   py27h1de35cc_0         393 KB
backports_abc-0.5          |   py27h6972548_0          12 KB
ruamel_yaml-0.15.35        |   py27h1de35cc_1         228 KB
backports.functools_lru_cache-1.5|           py27_1           9 KB
scandir-1.7                |   py27h1de35cc_0          25 KB
setuptools-38.5.1          |           py27_0         509 KB
pyparsing-2.2.0            |   py27h5bb6aaf_0          94 KB
tornado-5.0                |           py27_0         618 KB
requests-2.18.4            |   py27h9b2b37c_1          91 KB
futures-3.2.0              |   py27h1b80678_0          24 KB
bleach-1.4.2               |           py27_0          31 KB  bioconda
kiwisolver-1.0.1           |   py27h9856860_0          57 KB
html5lib-1.0.1             |   py27h5233db4_0         188 KB
cycler-0.10.0              |   py27hfc73c78_0          13 KB
singledispatch-3.4.0.3     |   py27he22c18d_0          15 KB
jsonschema-2.6.0           |   py27hd9b497e_0          61 KB
numpy-1.14.2               |   py27ha9ae307_0         3.9 MB
nbformat-4.4.0             |   py27hddc86d0_0         136 KB
pandocfilters-1.4.2        |   py27hed78c4e_1          12 KB
pickleshare-0.7.4          |   py27h37e3d41_0          11 KB
jupyter_client-5.2.2       |           py27_0         121 KB
prompt_toolkit-1.0.15      |   py27h4a7b9c2_0         333 KB
jupyter_core-4.4.0         |   py27h5ea6ba4_0          60 KB
cython-0.27.3              |   py27h6429b90_0         2.6 MB
ipython_genutils-0.2.0     |   py27h8b9a179_0          38 KB
pytz-2018.3                |           py27_0         211 KB
markupsafe-1.0             |   py27hd3c86fa_1          23 KB
conda-4.4.11               |           py27_0         931 KB
chardet-3.0.4              |   py27h2842e91_1         180 KB
appnope-0.1.0              |   py27hb466136_0           8 KB
ptyprocess-0.5.2           |   py27h70f6364_0          22 KB
six-1.11.0                 |   py27h7252ba3_1          21 KB
pathlib2-2.3.0             |   py27he09da1e_0          31 KB
pysocks-1.6.8              |           py27_0          22 KB
parso-0.1.1                |   py27he57c4c6_0         115 KB
ipaddress-1.0.19           |           py27_0          32 KB
python.app-2               |   py27hf2d5e94_7          11 KB
wcwidth-0.1.7              |   py27h817c265_0          25 KB
matplotlib-2.2.0           |   py27hfa7797c_0         6.6 MB
pycosat-0.6.3              |   py27h6c51c7e_0         106 KB
subprocess32-3.2.7         |   py27h24b2887_0          38 KB
htseq-0.9.1                |           py27_0         315 KB  bioconda
rseqc-2.6.4                |           py27_0         228 KB  bioconda
certifi-2018.1.18          |           py27_0         143 KB
simplegeneric-0.8.1        |           py27_2           9 KB
mistune-0.8.3              |           py27_0          52 KB
pexpect-4.4.0              |           py27_0          70 KB
pysam-0.14.0               | py27_htslib1.7_2         2.0 MB  bioconda
jinja2-2.10                |   py27h70b8dc5_0         177 KB
hisat2-2.1.0               |   py27pl5.22.0_0         6.9 MB  bioconda
enum34-1.1.6               |   py27hf475452_1          57 KB
cffi-1.11.5                |   py27h342bebf_0         201 KB
traitlets-4.3.2            |   py27hcf08151_0         126 KB
cryptography-2.1.4         |   py27hdbc5e8f_0         548 KB
-------------------------------------------------------Total:        54.1 MB

*The following NEW packages will be INSTALLED:*

backports:                          1.0-py27hb4f9756_1
backports.functools_lru_cache:      1.5-py27_1
backports.shutil_get_terminal_size: 1.0.0-py27hc9115de_2
backports_abc:                      0.5-py27h6972548_0
bx-python:                          0.7.3-py27_0            bioconda
configparser:                       3.5.0-py27hc7edf1b_0
cython:                             0.27.3-py27h6429b90_0
enum34:                             1.1.6-py27hf475452_1
functools32:                        3.2.3.2-py27h8ceab06_1
futures:                            3.2.0-py27h1b80678_0
ipaddress:                          1.0.19-py27_0
pathlib2:                           2.3.0-py27he09da1e_0
rseqc:                              2.6.4-py27_0            bioconda
scandir:                            1.7-py27h1de35cc_0
singledispatch:                     3.4.0.3-py27he22c18d_0
subprocess32:                       3.2.7-py27h24b2887_0

*The following packages will be UPDATED:*

appnope:                            0.1.0-py36hf537a9a_0             --> 0.1.0-py27hb466136_0
asn1crypto:                         0.24.0-py36_0                    --> 0.24.0-py27_0
bleach:                             1.4.2-py36_0            bioconda --> 1.4.2-py27_0            bioconda
certifi:                            2018.1.18-py36_0                 --> 2018.1.18-py27_0
cffi:                               1.11.4-py36h342bebf_0            --> 1.11.5-py27h342bebf_0
chardet:                            3.0.4-py36h96c241c_1             --> 3.0.4-py27h2842e91_1
conda:                              4.4.11-py36_0                    --> 4.4.11-py27_0
cryptography:                       2.1.4-py36h842514c_0             --> 2.1.4-py27hdbc5e8f_0
cycler:                             0.10.0-py36hfc81398_0            --> 0.10.0-py27hfc73c78_0
decorator:                          4.2.1-py36_0                     --> 4.2.1-py27_0
entrypoints:                        0.2.3-py36hd81d71f_2             --> 0.2.3-py27hd680fb1_2
hisat2:                             2.1.0-py36pl5.22.0_0    bioconda --> 2.1.0-py27pl5.22.0_0    bioconda
html5lib:                           1.0.1-py36h2f9c1c0_0             --> 1.0.1-py27h5233db4_0
htseq:                              0.9.1-py36_0            bioconda --> 0.9.1-py27_0            bioconda
idna:                               2.6-py36h8628d0a_1               --> 2.6-py27hedea723_1
ipykernel:                          4.8.2-py36_0                     --> 4.8.2-py27_0
ipython_genutils:                   0.2.0-py36h241746c_0             --> 0.2.0-py27h8b9a179_0
jedi:                               0.11.1-py36_0                    --> 0.11.1-py27_0
jinja2:                             2.10-py36hd36f9c5_0              --> 2.10-py27h70b8dc5_0
jsonschema:                         2.6.0-py36hb385e00_0             --> 2.6.0-py27hd9b497e_0
jupyter_client:                     5.2.2-py36_0                     --> 5.2.2-py27_0
jupyter_core:                       4.4.0-py36h79cf704_0             --> 4.4.0-py27h5ea6ba4_0
kiwisolver:                         1.0.1-py36h792292d_0             --> 1.0.1-py27h9856860_0
markupsafe:                         1.0-py36h3a1e703_1               --> 1.0-py27hd3c86fa_1
matplotlib:                         2.2.0-py36hfa7797c_0             --> 2.2.0-py27hfa7797c_0
mistune:                            0.8.3-py36_0                     --> 0.8.3-py27_0
nbconvert:                          5.3.1-py36h810822e_0             --> 5.3.1-py27h6455e4c_0
nbformat:                           4.4.0-py36h827af21_0             --> 4.4.0-py27hddc86d0_0
notebook:                           5.4.0-py36_0                     --> 5.4.0-py27_0
numpy:                              1.14.2-py36ha9ae307_0            --> 1.14.2-py27ha9ae307_0
pandocfilters:                      1.4.2-py36h3b0b094_1             --> 1.4.2-py27hed78c4e_1
parso:                              0.1.1-py36hc90e01c_0             --> 0.1.1-py27he57c4c6_0
pexpect:                            4.4.0-py36_0                     --> 4.4.0-py27_0
pickleshare:                        0.7.4-py36hf512f8e_0             --> 0.7.4-py27h37e3d41_0
pip:                                9.0.1-py36h1555ced_4             --> 9.0.1-py27_5
prompt_toolkit:                     1.0.15-py36haeda067_0            --> 1.0.15-py27h4a7b9c2_0
ptyprocess:                         0.5.2-py36he6521c3_0             --> 0.5.2-py27h70f6364_0
pycosat:                            0.6.3-py36hee92d8f_0             --> 0.6.3-py27h6c51c7e_0
pycparser:                          2.18-py36h724b2fc_1              --> 2.18-py27h0d28d88_1
pygments:                           2.2.0-py36h240cd3f_0             --> 2.2.0-py27h1a556bb_0
pyopenssl:                          17.5.0-py36h51e4350_0            --> 17.5.0-py27hfda213f_0
pyparsing:                          2.2.0-py36hb281f35_0             --> 2.2.0-py27h5bb6aaf_0
pysam:                              0.14.0-py36_htslib1.7_2 bioconda --> 0.14.0-py27_htslib1.7_2 bioconda
pysocks:                            1.6.7-py36hfa33cec_1             --> 1.6.8-py27_0
python.app:                         2-py36h54569d5_7                 --> 2-py27hf2d5e94_7
pytz:                               2018.3-py36_0                    --> 2018.3-py27_0
pyzmq:                              17.0.0-py36h1de35cc_0            --> 17.0.0-py27h1de35cc_0
requests:                           2.18.4-py36h4516966_1            --> 2.18.4-py27h9b2b37c_1
ruamel_yaml:                        0.15.35-py36h1de35cc_1           --> 0.15.35-py27h1de35cc_1
send2trash:                         1.5.0-py36_0                     --> 1.5.0-py27_0
setuptools:                         38.4.0-py36_0                    --> 38.5.1-py27_0
simplegeneric:                      0.8.1-py36_2                     --> 0.8.1-py27_2
six:                                1.11.0-py36h0e22d5e_1            --> 1.11.0-py27h7252ba3_1
terminado:                          0.8.1-py36_1                     --> 0.8.1-py27_1
testpath:                           0.3.1-py36h625a49b_0             --> 0.3.1-py27h72d81a5_0
tornado:                            5.0-py36_0                       --> 5.0-py27_0
traitlets:                          4.3.2-py36h65bd3ce_0             --> 4.3.2-py27hcf08151_0
urllib3:                            1.22-py36h68b9469_0              --> 1.22-py27hc3787e9_0
wcwidth:                            0.1.7-py36h8c6ec74_0             --> 0.1.7-py27h817c265_0
webencodings:                       0.5.1-py36h3b9701d_1             --> 0.5.1-py27h19a9f58_1
wheel:                              0.30.0-py36h5eb2c71_1            --> 0.30.0-py27h677a027_1

*The following packages will be DOWNGRADED:*

ipython:                            6.2.1-py36h3dda519_1             --> 5.4.1-py27_2
python:                             3.6.4-hc167b69_1                 --> 2.7.14-h138c1fe_30
python-dateutil:                    2.7.0-py36_0                     --> 2.3-py27_0              bioconda

询问是否继续？ Proceed ([y]/n)? y 
输入y以完成安装。

