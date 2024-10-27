# Espnet Installation

> 本文档写于24.10.24 
>
> 修改于24.10.27
>
> 最初版安装的kaldi(2019)不能满足espnet egs2训练的要求，目前的kaldi版本为c06f32ed13e15b71b7d1a84cf06317be80bd889f
>
> 配置环境最大的问题出现在gcc openfst openblas之间的不匹配，gcc9.3能解决这一问题，4.8.5还是太老了😒
>
> 感谢师兄和舍友在调试环境时的帮助

### Version

- os centos6
- Python 3.9.2
- node \*\*\*\*\*
- gcc 9.3.0
- cuda 11.8

### GCC

```shell
conda install gxx_linux-64 gxx_impl_linux-64 gcc_linux-64 gcc_impl_linux-64=9.3.0
cd $(dirname $(which python))
ln -s x86_64-conda-linux-gnu-gcc gcc
ln -s x86_64-conda-linux-gnu-g++ g++
```

### Kaldi

```bash
git clone https://github.com/kaldi-asr/kaldi.git

cd /path/to/your/kaldi/

# Install tools
cd tools
make -j 8
```

修改 /path/to/your/kaldi/tools/extras/install_openblas.sh中

```bash
make PREFIX=$(pwd)/OpenBLAS/install USE_LOCKING=1 USE_THREAD=0 -C OpenBLAS all install
```

改为

```bash
# 在编译OpenBLAS时指定目标架构（TARGET）为HASWELL，即Intel的Haswell微架构
make PREFIX=$(pwd)/OpenBLAS/install USE_LOCKING=1 USE_THREAD=0 TARGET=HASWELL -C OpenBLAS all install
```

```bash
# please fix the install_openblas.sh
./extras/install_openblas.sh

# Compile Kaldi & install
cd /path/to/your/kaldi/src/
./configure --openblas-root=../tools/OpenBLAS/install --use-cuda=no --shared
make -j clean depend; make -j 8
```

**Check yesno dataset process**

```bash
cd /path/to/your/kaldi/egs/yesno/s5/
sh run.sh
```

### Espnet

```bash
git clone https://github.com/espnet/espnet

cd /path/to/your/espnet/tools/

CONDA_ROOT=/path/to/your/anaconda3 
./setup_anaconda.sh ${CONDA_ROOT} espnet 3.9
conda activate espnet
make

ln -s /path/to/your/kaldi/

# check installation
bash -c ". ./activate_python.sh; . ./extra_path.sh; python3 check_install.py"
```

**Check yesno dataset training**

```bash
cd /path/to/your/espnet/egs/yesno/asr1/
sh run.sh
```

### Other

**如何解决cannot find libpthread.so.0**

这里在安装流程中没有用到

```bash
make LDFLAGS="-L/path/to/lib64 -L/path/to/lib -L/path/to/espnet/lib "
```

