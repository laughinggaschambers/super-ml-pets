<div align="center">
<img src="assets/SMLP.svg" width="50%" alt='super-auto-pets'>
<h1 align="center">super-ml-pets</h1>
<h3 align="center">Framework for training and deploying AIs for Super Auto Pets</h3>

[![License](https://img.shields.io/badge/License-MIT-green.svg)](https://opensource.org/licenses/MIT)
![CI](https://github.com/andreped/super-ml-pets/workflows/CI/badge.svg)
![CodeQL](https://github.com/andreped/super-ml-pets/workflows/CodeQL/badge.svg)
[![PEP8](https://img.shields.io/badge/code%20style-pep8-orange.svg)](https://www.python.org/dev/peps/pep-0008/)
[![codecov](https://codecov.io/gh/andreped/super-ml-pets/branch/main/graph/badge.svg?token=9YF7NANQTE)](https://codecov.io/gh/andreped/super-ml-pets)
 
Train AIs for Super Auto Pets through a simulated environment and test the trained model against real opponents in the actual game! AI is trained using reinforcement learning and a machine vision system is used to capture the screen to give information to the AI.

</div>

Framework supports Python `3.7-3.11` and works cross-platform (Ubuntu, Windows, macOS). Deployment is also compatible with the [web app](https://teamwood.itch.io/super-auto-pets).

Training has also been tested with [GitHub Codespaces](https://github.com/features/codespaces) and [Google Colab](https://colab.research.google.com/). A demonstration of model training can be seen in [this gist](https://colab.research.google.com/gist/andreped/cc0789bd711874f792c0991978b2f981/super-ml-pets-test.ipynb).

**Disclaimer:** We recommend using **Windows for deployment** as the UNIX-based systems require root permissions to launch the program out-of-the-box. Also, changing UI style for the web app does not always work.

## [Getting started](https://github.com/andreped/super-ml-pets#getting-started)

1. Clone the repo:
```
git clone https://github.com/andreped/super-ml-pets.git
```

2. Setup virtual environment:
```
cd super-ml-pets/
virtualenv -ppython3 venv --clear
./venv/Scripts/activate
```

To activate the virtual environment on UNIX-based systems, instead of the last line run `source venv/bin/activate`

3. Install requirements:
```
pip install -r requirements.txt
```

<details>
<summary>Additional setup for Ubuntu only</summary>

```
sudo apt install python3-tk
sudo su
source venv/bin/activate
xhost +
export DISPLAY=:0.0
```

Note that the command `sudo su` enables sudo rights. This seems to be required by `keyboard` as mentioned in issue https://github.com/andreped/super-ml-pets/issues/23. The xhost + DISPLAY stuff is needed as the screen might not be found, hence, initializing one solves this issue.

</details>

## [Usage](https://github.com/andreped/super-ml-pets#usage)
This framework currently supports training and deploying RL models for SAP.

<details open>
<summary>Training</summary>

For training in simulated environment, using default arguments, simply run:
```
python main.py --task train
```

Given an existing model, it is also possible to finetune it by (with example):
```
python main.py --task train --finetune /path/to/model_sap_gym_sb3_180822_checkpoint_2175_steps
```

The script supports other arguments. To see what is possible, run:
```
python main.py --help
```

</details>

<details open>
<summary>Testing</summary>

1. To use a trained model in battle, start the game Super Auto Pets.

2. Ensure that the game is in full screen mode, disable all unneccessary prompts, enable auto name picker, and set speed to `200%` (you might also have to enable auto battle which can only be done in the first battle - if this is the first time you are playing this game).

3. Change the UI style to classic for all options in customize including "Food art", "Background art", "Menu background", "Buff style", and "Held food".

4. Enter the arena by clicking `Arena mode`.

5. Go outside the game and download a pretrained model from [here](https://github.com/andreped/super-ml-pets/releases/tag/v0.0.2), or use any pretrained model you might have.

6. Then, simply start the AI by running this command from the terminal (with example path to pretrained model, **without extension .zip**):  
```
python main.py --task deploy --infer_model /path/to/model_sap_gym_sb3_280822_finetuned_641057_steps
```

7. Go back into the game and press the `Space` keyboard button (when you are in the Arena (in team preparation, before battle).

It might take a few seconds, but you should now be able to see the AI start playing. Please, let it play in peace, or else it might get angry and you may have accidentally created [Skynet](https://en.wikipedia.org/wiki/Skynet_(Terminator)). If you accidentally exit the game, or dont have the game in fullscreen, the machine vision system will fail, and you will have to start a completely new game to use the AI (properly).

</details>

<details>
<summary>Training history</summary>

To plot training history, run:
```
python smp/plot_history.py --log /path/to/history/history_rl_model/progress.csv
```

<p align="center">
  <img src="assets/training_history_example.png" width="60%" alt='super-auto-pets'>
</p>

</details>

<details>
<summary>Troubleshoot</summary>

To install virtualenv, run:
```
pip install virtualenv
```

If you do not have virtualenv in the path, you can access it by:
```
python -m virtualenv -ppython3 venv --clear
```

To activate virtual environment on UNIX-based systems (e.g., macOS or Ubuntu), run:
```
source venv/bin/activate
```

If you are using newer versions of Python (e.g., `3.10`), you might have issues with installing and/or using `numpy` with the other dependencies. If that happens, try downgrading numpy by:
```
pip install numpy==1.23.2 --force-reinstall
```
 
On both Ubuntu and macOS, it might require sudo permissions to run deployment. This has to do with keyboard events not being able to be recognized without
sudo rights. On Windows, administrative rights is **not needed**. For more information, see [here](https://pynput.readthedocs.io/en/latest/limitations.html).
 
On macOS, when you are downloading the models (.zip files) from [Releases](https://github.com/andreped/super-ml-pets/releases), they might be unzipped automatically. This is **bad** as the model extension is `.zip`. To fix this, disable the `Open safe files after downloading` in the Safari Preferences (see [here](https://www.lifewire.com/disable-open-safe-files-after-downloading-in-safari-446562) for more information).
 
If deployment fails to start (no mouse movements or events), it may be because your screen resolution differ from the expected resolution. The current machine vision system expects the screen resolution to be `1920x1080`. Please, adjust the resolution to this. This will be fixed in the future.

</details>

## [Acknowledgements](https://github.com/andreped/super-ml-pets#acknowledgements)
This implementation is based on multiple different projects. The core implementation is derived from [GoldExplosion](https://github.com/GoldExplosion/SuperAutoPets-RL-Agent), which further was based upon the super auto pets engine [sapai](https://github.com/manny405/sapai) and RL training through [sapai-gym](https://github.com/alexdriedger/sapai-gym).

All credit to [jpdefrutos](https://github.com/jpdefrutos) for designing the amazing [header figure](https://github.com/andreped/super-ml-pets/blob/main/assets/SMLP.svg).
