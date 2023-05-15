To use Git with a new repository on Windows:

1.  Install Git on your Windows machine, if it is not already installed. You can download the latest version of Git from the official website: [https://git-scm.com/downloads](https://git-scm.com/downloads)
2.  Open a terminal window (e.g. Git Bash, which is installed along with Git).
3.  Navigate to the local directory where you want to create your new repository using the `cd` command.
4.  Run the following command to initialize a new Git repository in the current directory: `git init`
5.  Add your files to the repository using the `git add` command. For example: `git add .` This will add all files in the current directory to the repository.
6.  Commit your changes using the `git commit` command. For example: `git commit -m "Initial commit"` This will create a new commit with the specified commit message.
7.  Connect your local repository to a remote repository on GitHub.com. To do this, you will need to create a new repository on GitHub.com and then run the following command in your terminal window, replacing `<repo_name>` with the name of your repository: `git remote add origin https://github.com/<username>/<repo_name>.git` This will add the remote repository as an origin for your local repository.
8.  Push your local commits to the remote repository using the `git push` command. For example: `git push -u origin master` This will push the commits from your local `master` branch to the `master` branch of the remote repository.

