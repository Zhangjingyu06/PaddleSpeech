# WaveFlow with LJSpeech
## Dataset
### Download the datasaet.
```bash
wget https://data.keithito.com/data/speech/LJSpeech-1.1.tar.bz2
```
### Extract the dataset.
```bash
tar xjvf LJSpeech-1.1.tar.bz2
```
## Get Started
Assume the path to the dataset is `~/datasets/LJSpeech-1.1`.
Assume the path to the Tacotron2 generated mels is `../tts0/output/test`.
Run the command below to
1. **source path**.
2. preprocess the dataset,
3. train the model.
4. synthesize wavs from mels.
```bash
./run.sh
```
### Preprocess the dataset.
```bash
./local/preprocess.sh ${preprocess_path}
```
### Train the model
`./local/train.sh` calls `${BIN_DIR}/train.py`.
```bash
CUDA_VISIBLE_DEVICES=${gpus} ./local/train.sh ${preprocess_path} ${train_output_path}
```
The training script requires 4 command line arguments.
1. `--data` is the path of the training dataset.
2. `--output` is the path of the output directory.
3. `--device` should be "cpu" or "gpu"
4. `--nprocs` is the number of processes to train the model in parallel.

If you want distributed training, set a larger `--nprocs` (e.g. 4). Note that distributed training with cpu is not supported yet.

### Synthesize
`./local/synthesize.sh` calls `${BIN_DIR}/synthesize.py`, which can synthesize waveform from mels.
```bash
CUDA_VISIBLE_DEVICES=${gpus} ./local/synthesize.sh ${input_mel_path} ${train_output_path} ${ckpt_name}
```

Synthesize waveform.
1. We assume the `--input` is a directory containing several mel spectrograms(log magnitude) in `.npy` format.
2. The output would be saved in `--output` directory, containing several `.wav` files, each with the same name as the mel spectrogram does.
3. `--checkpoint_path` should be the path of the parameter file (`.pdparams`) to load. Note that the extention name `.pdparmas` is not included here.
4. `--device` specifies to device to run synthesis on.

## Pretrained Model
Pretrained Model with residual channel equals 128 can be downloaded here. [waveflow_ljspeech_ckpt_0.3.zip](https://paddlespeech.bj.bcebos.com/Parakeet/waveflow_ljspeech_ckpt_0.3.zip).