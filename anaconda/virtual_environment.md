#### Python Environment Workflow

- Create a project folder in the ~/repos/ directory on my computer.
- Create an environment.yml file in the directory. Typically the environment name will be the same as the folder name. At minimum, it will specify the version of Python I want to use; it will often include anaconda as a dependency.5
- Create the conda environment with $ conda env create.
- Activate the conda environment with $ source activate ENV_NAME.
- Create a .env file containing the line source activate ENV_NAME. Because I have autoenv installed, this file will be run every time I navigate to the project folder in the Terminal. Therefore, my conda environment will be activated as soon as I navigate to the folder.
- Run $ git init to make the folder a Git repository. I then run $ git add environment.yml && git commit -m 'initial commit' to add the YAML file to the repository.
- If I want to push the repository to Github, I use $ git create using Github’s hub commands. I then push the master branch with $ git push -u origin master.
- As I add dependencies to my project, I try to be sure I add them to my environment.yml file.
