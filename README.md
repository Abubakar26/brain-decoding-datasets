# Brain decoding datasets

This Python package provides interfaces to datasets published from Kamitani Lab, Kyoto Univ and ATR.

## Installation

``` shellsession
$ pip install git+https://github.com/KamitaniLab/brain-decoding-datasets.git
```

## Usage

### Download all data files

``` shellsession
$ python -m bdds.downloadall <dataset name> [--output <output directory>]
```

Example:

``` shellsession
$ python -m bdds.downloadall handshape --output data/handshape
$ python -m bdds.downloadall god --output data/god
$ python -m bdds.downloadall decodeddnn --output data/decodeddnn
```

### Extract data

``` python
import bdds


# Hand shape decoding dataset
dataset_handshape = bdds.HandShapeDecoding('data/handshape')
data_handshape_s1 = dataset_handshape.get(mode='fmri', subject='S1')  # Return fMRI data as a bdpy dataset

# GOD fMRI dataset
dataset_god = bdds.GenericObjectDecoding('data/god')
data_god_s1 = dataset_god.get(mode='fmri', subject='Subject1')  # Return fMRI data as a bdpy dataset

# Decoded DNN features
dataset_dnn = bdds.DecodedDNN('data/decodeddnn')
decoded_feature = dataset_dnn.get(mode='decoded', subject='S1', net='AlexNet', layer='fc8')  # Return features as (a list of) numpy arrays.
```

If data files are missing in your local filesystem, the dataset instance asks you whether to download the files from online repositories or not.

Example:

``` python
>>> import bdds
>>> dataset_handshape = bdds.HandShapeDecoding('data/handshape')
>>> data_handshape_s1 = dataset_handshape.get(mode='fmri', subject='S1')
Data file is missing. Download? (y/[n])y
Downloading https://ndownloader.figshare.com/files/12227786
Saved data/handshape/S1.h5
>>>
```

See [demo.ipynb](demo.ipynb) for more details.

### Auto-downloading data files

When the dataset instance is initialized with with `auto_download=True` option, it downloads automatically missing files from data repositories.

Example:

``` python
>>> import bdds
>>> dataset_handshape = bdds.HandShapeDecoding('data/handshape', auto_download=True)
>>> data_handshape_s1 = dataset_handshape.get(mode='fmri', subject='S1')
Downloading https://ndownloader.figshare.com/files/12227786
Saved data/handshape/S1.h5
>>>
```

## Supported datasets

- [Generic object decoding](https://github.com/KamitaniLab/GenericObjectDecoding) ('god') #fMRI #DNN
    - [Horikawa & Kamitani (2017) Generic decoding of seen and imagined objects using hierarchical visual features. Nat Commun.](https://www.nature.com/articles/ncomms15037)
- DNN features decoded from human brain activity ('decodeddnn') #fMRI #DNN
    - [Horikawa et al. (2018) Characterization of deep neural network features by decodability from human brain activity. bioRxiv.](https://www.biorxiv.org/content/early/2018/09/23/424168)
- [Hand shape decoding dataset](https://figshare.com/articles/Hand_shape_decoding_rock_paper_scissors_/6698780) ('handshape') #fMRI
