# GitHub

GitHub is an online host for Git repositories, a very powerful tool for version control that meshes well with buildfiles. Utilizing it will allow you to easily keep a backup of your project's entire history and assist in ease of collaboration with others on your project by allowing multiple people to work on the same project simultaneously.

## Git Basics


## Using Git from Command Line


## GitHub Desktop

[GitHub Desktop](https://desktop.github.com) is a GUI application that trivializes the process of working with git at the level relevant to buildfiles. Rather than running commands from the command line every time you want to do something with git, you instead only need click a few buttons.

### Making a New Repository

To create a new repository, go to `File -> New repository...`, or press `Ctrl+N` to open the `Create a new repository` window. Here, input the name of the folder that contains your buildfile as the repository name and set the local path to the folder containing the one whose name you used. Press `Create repository` and a repository for your buildfile will be created. Note that this is a **local repository**; it won't be available online until you publish it.

### Committing, Pushing, & Pulling

Whenever you make a change to any non-ignored file within your repository, you have to commit the change to save it to the repository. In the GitHub Desktop window, this is done with the left panel. Here, you can see all of the file containing changes that have been made since your last commit. Clicking on a file here will show you what is different about the file on the right. If you want to quickly discard changes made to a specific file, right-click it and select `Discard changes`. Once you're ready, press the `Commit` button at the bottom left of the window to commit your changes.

Once your changes have been committed, the changes are only saved in your local version of the repository. To send them to GitHub, you have to push your changes. To do so, press the `Push origin` button in the top right; if you haven't pushed the repository before, this will instead be a `Publish` button. Once you've pushed your changes, your repository will be updated on GitHub.

If someone other than you, or you in another location, make changes to the repository and push them to GitHub, your local version will be outdated. Don't fret, however: you can continue to make changes to your local version and they will be merged with any changes made simultaneously by others that you do not presently have. To do this, you have to Pull changes from GitHub; this is done with the same button as Pushing. Resolve any conflicts that it tells you exist after pulling, and you can push the combined changes to GitHub.
