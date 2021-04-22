# CSE252D Spring 2021 HW1
## 0. Homework instructions

1. Attempt all questions.
2. Please comment all your code adequately.
3. Include all relevant information such as text answers, output images in notebook.
4. **Academic integrity:** The homework must be completed individually.

5. **Submission instructions:**  
 (a) Submit the notebook and its PDF version on Gradescope.  
 (b) Rename your submission files as Lastname_Firstname.ipynb and Lastname_Firstname.pdf.  
 (c) Correctly select pages for each answer on Gradescope to allow proper grading.

6. **Due date:** Assignments are due , May 5, by 4pm PST.

### Steps to access and complete homework
- Clone the homework repository
    - ``git clone https://github.com/ViLab-UCSD/cse252d-sp21-hw1.git``
- The homework is in the Jupyter Notebook ``hw1-CSE252D.ipynb``
- Follow the README (this file) for installation, data and compute instructions.

## 1. Installation instructions
### 1. Set up the environment
#### 1. [Option 1] On your own machine
- (local) SSH into your machine
- Install SWIG
    - On Ubuntu: `sudo apt-get install swig` (sudo required)
    - On MacOS: `brew install swig`
        - You need to install Homebrew first with [HomeBrew](https://brew.sh/)
- Install Python 3.X and Pip
- [Recommended] Create an environment (e.g. with [Anaconda](https://docs.conda.io/en/latest/miniconda.html))
    - ``conda create --name py36 python=3.6 pip``
    - ``conda activate py36``
- Install Jupyter Notebook
    - ``conda install jupyter``
- Install kernels for Jupter Notebook
    - ``conda install nb_conda``
- Launch Jupyter Notebook server in the conda env of the cluster
    - `jupyter notebook`
    - You will be provided with a URL that you can open locally
    - In a opened notebook, change the kernel (on Menu: **Kernel** -> **Change Kernel**) to the name of the conda env you just created (in the case of this documentation it should be `py36`)
    
#### 2. [Option 2] On the ``ieng6.ucsd.edu`` server
- (local) **(IMPORTANT) Connect your [UCSD VPN](https://blink.ucsd.edu/technology/network/connections/off-campus/VPN/index.html)**
- (local) Login with your credentials
    - `ssh {USERNAME}@ieng6.ucsd.edu`
- Launch your pod. 
    - You should enter a node with 1 GPU, 8 CPU, 16 GB RAM, with normal priority (running up to 6 hours)
        - ``launch-scipy-ml.sh -i ucsdets/cse152-252-notebook:latest -g 1 -m 16 -c 8 -p normal``
    - To enable longer runtime k (up to 12) hours with normal priority
        - ``K8S_TIMEOUT_SECONDS=$((3600*k)) launch-scipy-ml.sh -i ucsdets/cse152-252-notebook:latest -g 1 -m 16 -c 8 -p normal``
    - To enable longer runtime k (more than 12) hours with lower priority
        - ``K8S_TIMEOUT_SECONDS=$((3600*k)) launch-scipy-ml.sh -i ucsdets/cse152-252-notebook:latest -g 1 -m 16 -c 8``
    - To run your container in the background up to 12 hours, add ``-b`` to above command. See details [here](https://support.ucsd.edu/its?id=kb_article_view&sys_kb_id=c72a818f1b8e6050df40ed7dee4bcb31).
- You will be provided with a URL that you can open locally:
    ![](demo_jupyter.png)
    - Click on the link. Then natigate to the jupyter notebook for a question which you are going to git clone as follows
- If you cannot launch a pod, set up the environment following these [instructions](https://support.ucsd.edu/its?id=kb_article_view&sys_kb_id=cbb951c31b42a050df40ed7dee4bcb9e)
    
### 2. Pull the repo and install dependencies
- ``git clone https://github.com/ViLab-UCSD/cse252d-sp21-hw1.git``
- ``cd cse252d_hw1``
- Install dependencies (Python 3.X with Pip)
    - ``pip install -r requirements.txt --user``
- Compile and install `pyviso` for the SfM question
    - ``cd pyviso/src/``
    - ``pip install -e . --user``

## 2. Data
On the ``ieng6.ucsd.edu`` server, the datasets are located at
- Q1: SfM
    - `/datasets/cs252-sp21-A00-public/dataset_SfM`
    - Change the dataset path in jupyter notebooks to your paths
- Q5:
    - `/datasets/cs252-sp21-A00-public/sfmlearner_h128w416`
    - `/datasets/cs252-sp21-A00-public/kitti`

## 3. How to run
### Q1-Q4: SfM - Working folder: `./pyviso`
#### Launch Jupyter Notebook
There is a ` hw1-CSE252D.ipynb` jupyter notebook file in the top-level directory `cse252d_hw1`. 

#### Options
One toggle ``if_vis = True/False`` allows you to enable/disable the visualization. Disabling the visualization will make the for loop run significantly faster.

#### Output
The errors are printed and the visualizations are saved at ``vis/``. The images should look like:
![](demo.png)
To fetch the files you can use commands like `scp` to transfer files from the cluster to your local machine:

``scp -r <USERNAME>@dsmlp-login.ucsd.edu:<PATH TO THIS REPO>/pyviso2/vis {LOCAL PATH}``



## 4. [Extra] How to run training sessions without keeping connection for hours
You can run your container in the backgound (See Section 1.1.2) or use TMUX, as explained in the following.

### 1. Set up the environment

#### [Option 1] On the ``ieng6.ucsd.edu`` server

- Login with your credentials
    - `ssh {USERNAME}@ieng6.ucsd.edu`

-  Launch TMUX
    - Reconmended for session management: you can come back anytime after you disconnect your session. Otherwise you have to keep your connection on for hours while training.
    - Just run ``tmux``
    - To detach and come back later, use `ctrl + b` then `d`. 
    - To attach next time, first run `tmux ls` to list current sessions. Then use `tmux attach-session -t 0` to attach (default session name is 0).
    - For more TMUX usages please refer to online tutorials like [https://linuxize.com/post/getting-started-with-tmux/](https://linuxize.com/post/getting-started-with-tmux/)

-  Launch your pod
    - Follow Section 1.1.2

#### [Option 2] On your own server
Just launch TMUX.

### 2. Start training
Now you can create conda env and do your training in there following Section 1.1
