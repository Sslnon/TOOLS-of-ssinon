# Espnet Installation

> 本文档写于24.10.24 

### Version

- os centos
- Python 3.9.2
- node \*\*\*\*\*
- gcc 4.8.5
- cuda 11.8

### Kaldi

```bash
git clone https://github.com/kaldi-asr/kaldi.git

# openfst version has collision in newest version
cd /path/to/your/kaldi/
git reset --hard 7b762b1b32140cbf8fbf4c72b713b4bd18c71104

# Install tools
cd tools
make -j 8

# select BLAS library but require to install manually
wget https://github.com/xianyi/OpenBLAS/archive/refs/tags/v0.3.7.tar.gz -O OpenBLAS-0.3.7.tar.gz
tar -xvzf OpenBLAS-0.3.7.tar.gz

cd OpenBLAS-0.3.7
make -j$(nproc)
make PREFIX=path/to/your/OpenBLAS-0.3.7/install install


# Compile Kaldi & install
cd /path/to/your/kaldi/src/
./configure --openblas-root=path/to/your/OpenBLAS-0.3.7/install --use-cuda=no --shared
make -j clean depend; make -j 8
```

### Espnet

```bash
git clone https://github.com/espnet/espnet

cd /path/to/your/espnet/tools/

CONDA_ROOT=/path/to/your/anaconda3 
conda activate espnet
make

ln -s /path/to/your/kaldi/

# check installation
bash -c ". ./activate_python.sh; . ./extra_path.sh; python3 check_install.py"
```

### Check yesno dataset training

```bash
cd /path/to/your/espnet/egs/yesno/asr1/
sh run.sh
```

