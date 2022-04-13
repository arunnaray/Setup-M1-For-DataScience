## How to setup a TensorFlow environment on M1, M1 Pro, M1 Max using Miniforge (shorter version)

If you're experienced with making environments and using the command line, follow this version. If not, see the longer version below. 

1. Download and install Homebrew from https://brew.sh. Follow the steps it prompts you to go through after installation.
2. [Download Miniforge3](https://github.com/conda-forge/miniforge/releases/latest/download/Miniforge3-MacOSX-arm64.sh) (Conda installer) for macOS arm64 chips (M1, M1 Pro, M1 Max).
3. Install Miniforge3 into home directory.
```bash
chmod +x ~/Downloads/Miniforge3-MacOSX-arm64.sh
sh ~/Downloads/Miniforge3-MacOSX-arm64.sh
source ~/miniforge3/bin/activate
```
4. Restart terminal. 

5. Make and activate Conda environment. **Note:** Python 3.9 is the most stable for using the following setup.
```bash
conda create --name tensorflow python=3.9
conda activate tensorflow
```
7. Install TensorFlow dependencies from Apple Conda channel.
```bash
conda install -c apple tensorflow-deps
```
8. Install base TensorFlow (Apple's fork of TensorFlow is called `tensorflow-macos`).
```bash
python -m pip install tensorflow-macos
```
9. Install Apple's `tensorflow-metal` to leverage Apple Metal (Apple's GPU framework) for M1, M1 Pro, M1 Max GPU acceleration.
```bash
python -m pip install tensorflow-metal
```
10. (Optional) Install TensorFlow Datasets to run benchmarks included in this repo.
```bash
python -m pip install tensorflow-datasets
```
11. Install common data science packages.
```bash
conda install jupyter pandas numpy matplotlib scikit-learn
```
12. Start Jupyter Notebook.
```bash
jupyter notebook
```
13. Import dependencies and check TensorFlow version/GPU access.
```python
import numpy as np
import pandas as pd
import sklearn
import tensorflow as tf
import matplotlib.pyplot as plt

# Check for TensorFlow GPU access
print(f"TensorFlow has access to the following devices:\n{tf.config.list_physical_devices()}")

# See TensorFlow version
print(f"TensorFlow version: {tf.__version__}")
```

If it all worked, you should see something like: 

```bash
TensorFlow has access to the following devices:
[PhysicalDevice(name='/physical_device:CPU:0', device_type='CPU'),
PhysicalDevice(name='/physical_device:GPU:0', device_type='GPU')]
TensorFlow version: 2.8.0
```

## How to setup a TensorFlow environment on M1, M1 Pro, M1 Max using Miniforge (longer version)

If you're new to creating environments, using a new M1, M1 Pro, M1 Max machine and would like to get started running TensorFlow and other data science libraries, follow the below steps.

> **Note:** You're going to see the term "package manager" a lot below. Think of it like this: a **package manager** is a piece of software that helps you install other pieces (packages) of software.

### Installing package managers (Homebrew and Miniforge)

1. Download and install Homebrew from https://brew.sh. Homebrew is a package manager that sets up a lot of useful things on your machine, including Command Line Tools for Xcode, you'll need this to run things like `git`. The command to install Homebrew will look something like:

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

It will explain what it's doing and what you need to do as you go.

2. [Download the most compatible version of Miniforge](https://github.com/conda-forge/miniforge#download) (minimal installer for Conda specific to conda-forge, Conda is another package manager and conda-forge is a Conda channel) from GitHub.

If you're using an M1 variant Mac, it's "[Miniforge3-MacOSX-arm64](https://github.com/conda-forge/miniforge/releases/latest/download/Miniforge3-MacOSX-arm64.sh)" <- click for direct download. 

Clicking the link above will download a shell file called `Miniforge3-MacOSX-arm64.sh` to your `Downloads` folder (unless otherwise specified). 

3. Open Terminal.

4. We've now got a shell file capable of installing Miniforge, but to do so we'll have to modify it's permissions to [make it executable](https://askubuntu.com/tags/chmod/info).

To do so, we'll run the command `chmod -x FILE_NAME` which stands for "change mode of FILE_NAME to -executable".

We'll then execute (run) the program using `sh`.

```bash
chmod +x ~/Downloads/Miniforge3-MacOSX-arm64.sh
sh ~/Downloads/Miniforge3-MacOSX-arm64.sh
```

5. This should install Miniforge3 into your home directory (`~/` stands for "Home" on Mac).

To check this, we can try to activate the `(base)` environment, we can do so using the `source` command.

```bash
source ~/miniforge3/bin/activate
```

If it worked, you should see something like the following in your terminal window.

```bash
(base) daniel@Daniels-MBP ~ %
```

6. We've just installed some new software and for it to fully work, we'll need to **restart terminal**. 

### Creating a TensorFlow environment

Now we've got the package managers we need, it's time to install TensorFlow.

Let's setup a folder called `tensorflow-test` (you can call this anything you want) and install everything in there to make sure it's working.

> **Note:** An **environment** is like a virtual room on your computer. For example, you use the kitchen in your house for cooking because it's got all the tools you need. It would be strange to have an oven in your bedroom. The same thing on your computer. If you're going to be working on specific software, you'll want it all in one place and not scattered everywhere else. 

7. Make a directory called `tensorflow-test`. This is the directory we're going to be storing our environment. And inside the environment will be the software tools we need to run TensorFlow.

We can do so with the `mkdir` command which stands for "make directory".

```bash
mkdir tensorflow-test
```

8. Change into `tensorflow-test`. For the rest of the commands we'll be running them inside the directory `tensorflow-test` so we need to change into it.

We can do this with the `cd` command which stands for "change directory".

```bash
cd tensorflow-test
```

9. Now we're inside the `tensorflow-test` directory, let's create a new Conda environment using the `conda` command (this command was installed when we installed Miniforge above).

We do so using `conda create --prefix ./env` which stands for "conda create an environment with the name `file/path/to/this/folder/env`". The `.` stands for "everything before".

For example, if I didn't use the `./env`, my filepath looks like: `/Users/daniel/tensorflow-test/env`

```bash
conda create --prefix ./env
```

10. Activate the environment. If `conda` created the environment correctly, you should be able to activate it using `conda activate path/to/environment`.

Short version: 

```bash
conda activate ./env
```

Long version:

```bash
conda activate /Users/daniel/tensorflow-test/env
```

> **Note:** It's important to activate your environment every time you'd like to work on projects that use the software you install into that environment. For example, you might have one environment for every different project you work on. And all of the different tools for that specific project are stored in its specific environment.

If activating your environment went correctly, your terminal window prompt should look something like: 

```bash
(/Users/daniel/tensorflow-test/env) daniel@Daniels-MBP tensorflow-test %
```

11. Now we've got a Conda environment setup, it's time to install the software we need.

Let's start by installing various TensorFlow dependencies (TensorFlow is a large piece of software and *depends* on many other pieces of software).

Rather than list these all out, Apple have setup a quick command so you can install almost everything TensorFlow needs in one line.

```bash
conda install -c apple tensorflow-deps
```

The above stands for "hey conda install all of the TensorFlow dependencies from the Apple Conda channel" (`-c` stands for channel).

If it worked, you should see a bunch of stuff being downloaded and installed for you. 

12. Now all of the TensorFlow dependencies have been installed, it's time install base TensorFlow.

Apple have created a fork (copy) of TensorFlow specifically for Apple Macs. It has all the features of TensorFlow with some extra functionality to make it work on Apple hardware.

This Apple fork of TensorFlow is called `tensorflow-macos` and is the version we'll be installing:

```bash
python -m pip install tensorflow-macos
```

Depending on your internet connection the above may take a few minutes since TensorFlow is quite a large piece of software.

13. Now we've got base TensorFlow installed, it's time to install `tensorflow-metal`.

Why?

Machine learning models often benefit from GPU acceleration. And the M1, M1 Pro and M1 Max chips have quite powerful GPUs.

TensorFlow allows for automatic GPU acceleration if the right software is installed.

And Metal is Apple's framework for GPU computing.

So Apple have created a plugin for TensorFlow (also referred to as a TensorFlow PluggableDevice) called `tensorflow-metal` to run TensorFlow on Mac GPUs.

We can install it using:

```bash
python -m pip install tensorflow-metal
```

If the above works, we should now be able to leverage our Mac's GPU cores to speed up model training with TensorFlow.

14. (Optional) Install TensorFlow Datasets. Doing the above is enough to run TensorFlow on your machine. But if you'd like to run the benchmarks included in this repo, you'll need TensorFlow Datasets.

TensorFlow Datasets provides a collection of common machine learning datasets to test out various machine learning code.

```bash
python -m pip install tensorflow-datasets
```

15. Install common data science packages. If you'd like to run the benchmarks above or work on other various data science and machine learning projects, you're likely going to need Jupyter Notebooks, pandas for data manipulation, NumPy for numeric computing, matplotlib for plotting and Scikit-Learn for traditional machine learning algorithms and processing functions.

To install those in the current environment run:

```bash
conda install jupyter pandas numpy matplotlib scikit-learn
```

16. Test it out. To see if everything worked, try starting a Jupyter Notebook and importing the installed packages.

```bash
# Start a Jupyter notebook
jupyter notebook
```

Once the notebook is started, in the first cell:

```python
import numpy as np
import pandas as pd
import sklearn
import tensorflow as tf
import matplotlib.pyplot as plt

# Check for TensorFlow GPU access
print(tf.config.list_physical_devices())

# See TensorFlow version
print(tf.__version__)
```

If it all worked, you should see something like:

```bash
TensorFlow has access to the following devices:
[PhysicalDevice(name='/physical_device:CPU:0', device_type='CPU'),
PhysicalDevice(name='/physical_device:GPU:0', device_type='GPU')]
TensorFlow version: 2.5.0
```

17. To see if it really worked, try running one of the notebooks above end to end!

And then compare your results to the benchmarks above.





