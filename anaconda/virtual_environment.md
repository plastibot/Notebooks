```` bash
# To find what version of conda is installed
conda --version

#To update conda to the latest version
conda update conda

# to create a new environment with default python version
conda create --name <your_env_name>

# to create new environment with an specific version of python
conda create --name <your_env_name> python=3.5

# to get a list of the environments created
conda info --envs

# to activate your environment
conda activate <your_env_name>

# to return to the base environment
conda activate

# to install packages within your virtual environment
conda install <package_name>

# to check if a package is installed into the current environment
conda search <package_name>

````



### Python Environment Workflow

- Create a project folder in the ~/repos/ directory on my computer.
- Create an environment.yml file in the directory. Typically the environment name will be the same as the folder name. At minimum, it will specify the version of Python I want to use; it will often include anaconda as a dependency.5
- Create the conda environment with $ conda env create.
- Activate the conda environment with $ source activate ENV_NAME.
- Create a .env file containing the line source activate ENV_NAME. Because I have autoenv installed, this file will be run every time I navigate to the project folder in the Terminal. Therefore, my conda environment will be activated as soon as I navigate to the folder.
- Run $ git init to make the folder a Git repository. I then run $ git add environment.yml && git commit -m 'initial commit' to add the YAML file to the repository.
- If I want to push the repository to Github, I use $ git create using Github’s hub commands. I then push the master branch with $ git push -u origin master.
- As I add dependencies to my project, I try to be sure I add them to my environment.yml file.

### Remove Conda as Default Python

After installing Anaconda or Miniconda, programs that run python switch from invoking the system Python to invoking the Python in the root conda environment. If these programs rely on the system Python to have certain configurations or dependencies that are not in the root conda environment Python, the programs may crash. For example, some users of the Cinnamon desktop environment on Linux Mint have reported these crashes.

#### Solution
Edit your .bash_profile and .bashrc files so that the conda binary directory, such as ~/miniconda3/bin, is no longer added to the PATH environment variable. You can still run conda activate and conda deactivate by using their full path names, such as ~/miniconda3/bin/conda.

You may also create a folder with symbolic links to conda activate and conda deactivate and then edit your .bash_profile or .bashrc file to add this folder to your PATH. If you do this, running python will invoke the system Python, but running conda commands, conda activate MyEnv, conda activate root, or conda deactivate will work normally.

After running conda activate to activate any environment, including after running conda activate root, running python will invoke the Python in the active conda environment.



