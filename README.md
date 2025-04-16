# LRCBench
A Holistic Survey and Benchmarking of Lossless Reference-free Compressors for Long-read Genomics Sequencing Data

## Algorithms details and commands

### [Cmix](https://github.com/byronknoll/cmix)
Cmix is a neural network based lossless compression algorithm aimed at optimizing compression ratio at the cost of high CPU/memory usage, and it uses thousands of context models followed by an NN-based mixer. We used Cmix V19 to finish the experiments.
```sh
# compression
cmix -c file file.cmix
# decompression
cmix -d file.cmix file.cmix.out
```
### [LSTM-compress](https://github.com/byronknoll/lstm-compress)
LSTM-compressor is an LSTM-based lossless compression algorithm that uses the same LSTM module and preprocessing code as CMIX. LSTM-compress currently only supports compression of a single file. In this manuscript, we used LSTM-compress V3. The detailed commands are as follows.

```sh
# compression
lstm-compress -c file file.lstm
# decompression
lstm-compress -d file.lstm file.lstm.out
```

### [NNCP](https://bellard.org/nncp)
NNCP is a lossless compression algorithm based on LSTM and supports multi-GPU parallel processing. NNCP is an experiment to build a practical lossless data compressor with neural networks. The latest version uses a Transformer model. In this manuscript, we used NNCP V2021-06-01 to finish the experiments. The detailed commands are as follows.

```sh
# compression
nncp c file file.nncp -T 16 --cuda
# decompression
nncp d file.nncp file.nncp.out -T 16 --cuda
```
### [DeepZip](https://github.com/mohit1997/DeepZip)
DeepZip is a general-purpose compression algorithm based on recurrent neural networks. It belongs to the static pre-training method. The detailed commands for using DeepZip are shown below.

```sh
# compression
sh ./compress.sh file file.deepzip bs model
# decompression
sh ./decompress.sh file.deepzip file.deepzip.out bs model
```
### [DZip](https://github.com/mohit1997)
DZip is an upgraded version of DeepZip, with an extra deeper network added to DeepZip to improve compression. DZip includes two compression modes, combined mode and bootstrap mode.  The detailed commands of DZip are as follows.

```sh
# compression
sh ./compress.sh file file.dzip com model
# decompression
sh ./decompress.sh file.dzip file.dzip.out com model
```
### [TRACE](https://github.com/mynotwo/A-Fast-Transformer-based-General-Purpose-LosslessCompressor)
TRACE is a lossless compression algorithm based on Performer (a Transformer variant.) TRACE uses byte grouping and shared FFNs, and therefore has better execution efficiency. Since the original TRACE puts compression and decompression processes into simultaneous execution, in order to test the performance of compression and decompression separately, we have modified the source files to test the performance of compression or decompression separately.

```sh
# compression
python compressor.py --source file --comp file.trace
# decompression
python compressor.py --comp file.trace --decomp file.trace.out
```

### [PAC](https://github.com/mynotwo/Faster-and-Stronger-Lossless-Compression-with-Optimized-Autoregressive-Framework)
PAC is a deep learning based compression algorithm fusing MLP and Ordered Mask. Due to the use of MLP, PAC has a lower computational cost. Again, we separate the compression-decompression process of PAC as shown in the command line below.

```sh
# compression
python compressor.py --source file --comp file.pac
# decompression
python compressor.py --comp file.pac --decomp file.pac.out
```

### [LLMZip](https://github.com/vcskaushik/LLMzip?tab=readme-ov-file)
LLMZip uses LLaMA as probabilistic predictor in combination with entropy coding (zlib, Token-by-Token and arithmetic coding) to achieve general-purpose lossless compression. The compression and decompression commands of LLMZip are as follows.

```sh
# compression
torchrun --nproc_per_node 1 LLMzip_run.py --ckpt_dir llama2/llama-2-7b --tokenizer_path llama2/tokenizer.model --win_len 511 --text_file file --compression_folder file --encode_decode 0
# decompression
torchrun --nproc_per_node 1 LLMzip_run.py --ckpt_dir llama2/llama-2-7b --tokenizer_path llama2/tokenizer.model --win_len 511 --text_file file --compression_folder file --encode_decode 1
```

### [Gzip](https://www.gnu.org/software/gzip/)

Gzip is a popular early general-purpose lossless compression program originally written by Jean-loup Gailly for the GNU project. The commands for Gzip are shown below.

```sh
# compression
gzip -c file > file.gz -9
# decompression
gzip file.gz -9
```
### [PBzip2](https://launchpad.net/pbzip2)
PBzip2 is a parallel implementation of the Bzip2 block-sorting file compression algorithm that uses pthreads and achieves near-linear speedup on SMP devices. PBzip2 utilizes the Burrows-Wheeler block sorting algorithm for compressing files, along with Huffman coding for efficient text compression. This manuscript uses parallel Bzip2 V1.1.13  to compress data.

```sh
# compression
pbzip2 -9 -m2000 -p16 -c file > file.bz2
# decompression
pbzip2 -dc -9 -p16 -m2000 file.bz2
```

### [XZ](https://xz.tukaani.org/xz-utils/)
XZ Utils is free general-purpose data compression software with a high compression ratio. XZ Utils were written for POSIX-like systems, but also work on some not-so-POSIX systems. XZ Utils are the successor to LZMA Utils. In our experiments, we used XZ V5.5.0. The compression and decompression commands are as follows.

```sh
# compression
pbzip2 -9 -m2000 -p16 -c file > file.bz2
# decompression
pbzip2 -dc -9 -p16 -m2000 file.bz2
```

### [BSC](https://github.com/IlyaGrebnov/libbsc)
BSC is a high-performance file compressor based on lossless block-ordered data compression algorithm, block-ordered data compression algorithm, high-performance file compressor. This manuscript uses BSC V3.3.2 to compress and decompress data.

```sh
# compression
bsc e file file.bsc -e2
# decompression
bsc d file.bsc file.bsc.out
```


### [SnZip](https://github.com/kubo/snzip)
SnZip is a traditional general-purpose lossless compression algorithm based on snappy. It supports a wide range of file formats including framing-format, old framing-format and so on. The default is framing-format. The command line to run SnZip is as follows.

```sh
# compression
snzip -k -t snzip file
# decompression
snzip -kd -t snzip file.snz
```

### [LZMA2](https://www.7-zip.org/)
LZMA2 improves the multi-threading capability and performance of the LZMA algorithm and better handles incompressible data, so the compression performance is slightly improved. We also used the built-in LZMA2 algorithm in the 7-Zip application.

```sh
# compression
7zz a -m0=lzma2 -mx9 -mmt16 file.7z file
# decompression
7zz x -y -mx9 -mmt16 file.7z
```

### [PPMD](https://www.7-zip.org/)
PPMD is a context-based compressor, and its core idea is the Partial Matching Prediction (PPM) algorithm proposed by Cleary and Witten. PPM is a statistical modeling technique that uses a set of previous symbols in the input to predict the next symbol to reduce the output data’s entropy. PPM differs from a dictionary because PPM predicts the next symbol instead of trying to find the next symbol in the dictionary to encode. We utilized the PPMD in the 7-Zip to compress data.

```sh
# compression
7zz a -m0=ppmd -mx9 -mmt16 file.7z file
# decompression
7zz x -y -mx9 -mmt16 file.7z
```

### [LZ4-multi](https://github.com/lz4/lz4)
LZ4 is a lossless compression algorithm with compression speeds greater than 500 MB/s per kernel (greater than 0.15 bytes/cycle). Its decoder is extremely fast, up to several GB/s (1 byte/cycle) per kernel. The latest lz4 algorithm supports multi-threaded versions.
```sh
# compression
lz4 -12 -T16 file file.lz4
# decompression
lz4 -12 -T16 -d file.lz4 file.lz4.reads
\end{lstlisting}
```

## Experimental Configuration

All experiments were conducted on a GPU server equipped with 4 * Intel Xeon Silver 4310 CPUs (2.10 GHz, 48 cores in total), 4* NVIDIA GeForce RTX 4090 GPUs (16,384 CUDA cores, 24 GB of GPU memory), and 128 GB of DDR4 RAM. The server runs the operating system Ubuntu 20.04.6 LTS.
