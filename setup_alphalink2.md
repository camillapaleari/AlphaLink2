## Step 1: Install Miniconda

1. **Create a directory for Miniconda**:
    ```bash
    mkdir -p ~/miniconda3
    ```

2. **Download the latest version of Miniconda**:
    ```bash
    wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -O ~/miniconda3/miniconda.sh
    ```

3. **Run the install script**:
    ```bash
    bash ~/miniconda3/miniconda.sh -b -u -p ~/miniconda3
    ```

4. **Delete the install script**:
    ```bash
    rm -rf ~/miniconda3/miniconda.sh
    ```

5. **Add a conda initialize to your bash**:
    ```bash
    ~/miniconda3/bin/conda init bash
    ```

6. **Verify the installation**:
    ```bash
    conda list
    ```


## Create alphalink envirorment
```bash
conda create --name alphalink -c conda-forge python=3.10
conda activate alphalink
```
# Install all the necessary modules
```
pip install nvidia-pyindex
pip install https://github.com/dptech-corp/Uni-Core/releases/download/0.0.3/unicore-0.0.1+cu118torch2.0.0-cp310-cp310-linux_x86_64.whl
conda install -y -c conda-forge openmm==7.7.0 pdbfixer biopython==1.81
conda install -y -c bioconda hmmer hhsuite==3.3.0 kalign2
pip install tensorflow-cpu==2.16.1
pip install -U "jax[cuda12]"
```
# Set up alphafold
```
git clone https://github.com/deepmind/alphafold.git
cd alphafold
python setup.py install

wget --no-check-certificate https://git.scicore.unibas.ch/schwede/openstructure/-/raw/7102c63615b64735c4941278d92b554ec94415f8/modules/mol/alg/src/stereo_chemical_props.txt

cp stereo_chemical_props.txt $CONDA_PREFIX/lib/python3.10/site-packages/`ls $CONDA_PREFIX/lib/python3.10/site-packages/ | grep alphafold`/alphafold/common/
cd ..
```
## Clone aplhalink2 repo
```
git clone https://github.com/Rappsilber-Laboratory/AlphaLink2.git
cd AlphaLink2
python setup.py install
conda install nvidia/label/cuda-12.1.1::cuda-toolkit
conda install pytorch torchvision torchaudio pytorch-cuda=12.1 -c pytorch -c nvidia
```
# Download model weights from here
https://zenodo.org/records/8007238


#### finish of the setting up. Now you have to do this two rows every time you want to predict 

## generate dictionary of crosslinks (check the crosslinks input format from the readme )
```
python generate_crosslink_pickle.py --csv /path/to/the/crosslinks.csv --output /path/to/the/crosslinks/crosslinks.pkl.gz
```
## run prediction
```
sbatch run_alphalink.sh /path/to/file/fasta.fasta /path/to/the/crosslinks/crosslinks.pkl.gz /output/path/ /path/to/weights/AlphaLink-Multimer_SDA_v3.pt  /cluster/project/alphafold/ 2020-05-01
```
