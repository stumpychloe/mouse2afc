## About
This guide is a modified version of [PyBpod's Installation Guide](https://pybpod.readthedocs.io/en/v1.8.1/getting-started/install.html) specific to the Poirazi Lab's Mouse2AFC protocol. For more detail on what PyBpod is and how to use other features please see their wiki [here](https://pybpod.readthedocs.io/en/v1.8.1/index.html). 

# Installation

### 1. Install conda
Download & install [Anaconda](https://www.anaconda.com/download/) or [Miniconda](https://conda.io/miniconda.html).

### 2. Download the appropriate environment configuration file and create the virtual environment:
Windows 10 or 11: [environment-windows-10.yml](https://raw.githubusercontent.com/pybpod/pybpod/master/utils/environment-windows-10.yml):

**Only if using Windows 11:** Before creating the environment: open `environment-windows-10.yml` and delete `sqlite=3.19.3=vc14_1`,`jpeg=9b=vc14_2`,`qt=5.6.2=vc14_1`,`matplotlib=2.0.2=np113py36_0` and `tk=8.5.18=vc14_0`

```bash
conda env create -f environment-windows-10.yml
```

Ubuntu 17.10 and up: [environment-ubuntu-17.10.yml](https://raw.githubusercontent.com/pybpod/pybpod/master/utils/environment-ubuntu-17.10.yml):

```bash
conda env create -f environment-ubuntu-17.10.yml
```

Mac OSx: [environment-macOSx.yml](https://raw.githubusercontent.com/pybpod/pybpod/master/utils/environment-macOSx.yml):

```bash
conda env create -f environment-macOSx.yml
```

### 3. Activate the environment you just created
```bash
conda activate pybpod-environment
```

**Only if using Windows 11:** After activating the environment in step 3 of the Installation for Developers:

```bash
conda install jpeg=9b qt=5.6.2 matplotlib=2.0.2 tk --channel conda-forge --channel anaconda --channel defaults
```

### 4. Verify conda environment is activated
```bash
conda env list
```
yields
```bash
# conda environments:
#
base                    /...
pybpod-environment     */...
```
with the asterik `*` next to `pybpod-environment`

### 5. Verify the executable is inside the conda environment path
```bash
pip --version
```
yields
#### Linux: 
```bash
pip 9.0.1 from /home/user/anaconda3/envs/pybpod-environment/lib/python3.6/site-packages/pip (python 3.6)
```
#### Windows:
```bash
pip 18.1 from C:\Users\user\miniconda3\envs\pybpod-environment\lib\site-packages\pip (python 3.6)
```

### 6. Upgrade pip

```bash
pip install --upgrade pip
```

### 7. Clone pybpod github respository

```bash
git clone --recurse-submodules -j8 https://github.com/ckarageorgkaneen/pybpod pybpod
```
The repository will be cloned into a new folder called "pybpod".

### 8. Install pybpod

```bash
cd pybpod
python utils/install.py  # may take a few minutes
```
### 9. Clone mouse2afc github repository
If you haven't cloned the mouse2afc repository already, clone it (outside the pybpod folder):
```bash
cd ../
git clone https://github.com/Poirazi-Lab/mouse2afc.git mouse2afc
```

### 10. Install mouse2afc
```bash
cd mouse2afc
pip install -e .
``` 

# Setting Up the Protocol in PyBpod

### 1. Start PyBpod GUI

```bash
python -c 'import pybpodgui_plugin.__main__ as Main; Main.start()'
```

![image](https://github.com/HenryJFlynn/mouse2afc/assets/130571023/520fb3cd-6d37-4749-9103-3b93f2294cc7)

### 2. Editing user settings
Click `Options` then `Edit user settings`. Paste the following settings:

```python
SETTINGS_PRIORITY = 0

GENERIC_EDITOR_PLUGINS_LIST = ['pybpodgui_plugin', 'pybpod_gui_plugin_emulator']

PYBPOD_SESSION_PATH = '<YOUR_SESSION_PATH>'

TARGET_BPOD_FIRMWARE_VERSION = "22"
EMULATOR_BPOD_MACHINE_TYPE = 1

BPOD_BNC_PORTS_ENABLED = [True,True]
BPOD_WIRED_PORTS_ENABLED = [True, True, True, True]
BPOD_BEHAVIOR_PORTS_ENABLED = [True, True, True, False, False, False, False, False]
```

**AND** replace `<YOUR_SESSION_PATH>` with the path of the folder you want the session to be saved into. (e.g. Linux: `/home/user/Documents/mouse2afc_sessions`, Windows: `"C:\\Users\\mouse2afc\\mouse2afc_sessions"`)

### 3. Save the changes
Click `Save` and close the program.

# Running the protocol in PyBpod

### 1. Start PyBpod GUI

```bash
python -c 'import pybpodgui_plugin.__main__ as Main; Main.start()'
```

### 2. Open project folder 
Click `Open` and select `pybpod-project` located in the `mouse2afc` repository folder downloaded previously. Should have something like this:

![image](https://github.com/HenryJFlynn/mouse2afc/assets/130571023/51a90a6b-64f2-4e51-84c6-63fb1c500427)

### 3. Select user
In the `Projects` window, under `Users`, double-click on `Default-User` to select it

### 4. Select Bpod board information
In the `Projects` window, go to `Bpod boards`, select `Default-Box` then in the `Details` window select the serial port your Bpod is connected to. For Linux the serial port will look like `/dev/tty`, for Windows `COM`

![image](https://github.com/HenryJFlynn/mouse2afc/assets/130571023/c82364bc-c492-4ebf-941a-9869ed7d8467)


and click `Console`:

![image](https://github.com/HenryJFlynn/mouse2afc/assets/130571023/ae4e52e0-aabc-4dc2-acc8-baae1e36198e)

### Note about emulator mode in Pybpod:
If you wish to run the protocol in emulator mode (e.g. if you don't have the physical device):
1. Instead of selecting a serial port, check `Emulator mode`
2. Under `Protocols`, open `Mouse2AFC` and edit line 6 to be `bpod=Bpod(emulator_mode=True)` and click `Save`

### 5. Assign subjects to setup
In the `Projects` window, under `Subjects`, click on `Default-Subject`. If not already selected, click on the dropdown widget next to `Setup` and select the setup you would like this mouse to use:

![image](https://github.com/HenryJFlynn/mouse2afc/assets/130571023/70a85f8f-8348-418d-98b2-f607a3040c9a)

### 6. Assign experiment setup, board, and protocol
Similarly, under `Default Experiment`, click on `Default-Setup` and, if not already selected, select a `Board` and `Procotol` for the experiment:

![image](https://github.com/HenryJFlynn/mouse2afc/assets/130571023/7a90ca3f-5f96-44c5-a845-3f9e8907e121)

### 7. Add subject to setup
In the `Subjects` tab, if not already selected, click `Add Subject` to select the subject(s) for the experiment:

![image](https://github.com/HenryJFlynn/mouse2afc/assets/130571023/42400e4e-8340-413d-8e92-098f3c65d926)

### 8. Open emulator GUI
Click `Test Protocol IO` and the following GUI window should pop up:

![image](https://github.com/HenryJFlynn/mouse2afc/assets/130571023/4a78ee03-a3da-4ac0-9539-2801fa4a6b65)

### 9. Run the protocol using Pybpod
Click `Save`, click `Run Protocol`, select the appropriate parameters in the task parameter GUI, click `Ok` and the protocol should run. Check the boxes of your choice in the `Poke` row to emulate a mouse poking in (checked) and out (unchecked):

![image](https://github.com/HenryJFlynn/mouse2afc/assets/130571023/7e0afd02-d21d-4096-a843-90ff0fd3249b)

## 10. Stop the experiment
When done using, click `Stop` or `Kill`, and then `Save`.
