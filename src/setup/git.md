# Git

To share code with each other, we use a program called Git. We use Git to download the "online" version of the source code (which is hosted on GitHub) so that we can work on it locally on our own machines. We also use Git to again upload it to the server.

For an in-depth guide to Git, checkout the [Pro Git book](https://git-scm.com/book). For an overview of the most important commands, checkout [GitLab's cheat sheet](https://about.gitlab.com/images/press/git-cheat-sheet.pdf) and [this other one](https://github.com/devaaravmishra/git-commands). Want to perform some specific action? Check out [Git Flight Rules](https://github.com/k88hudson/git-flight-rules). Also, ChatGPT is pretty good at Git nowadays.

You can also use a GUI client instead of the command line (although I nowadays recommend against that), such as the one integrated into Visual Studio Code (with an extension such as [GitLens](https://marketplace.visualstudio.com/items?itemName=eamodio.gitlens)), [GitKraken](https://www.gitkraken.com/) or [GitHub Desktop](https://github.com/apps/desktop).

But first, you'll need to install Git itself. On Windows, use the [link at the top here](https://git-scm.com/downloads/win) (the 64-bit standalone installer). On Linux, use your system package manager ([instructions here](https://git-scm.com/downloads/linux)). On macOS, use [HomeBrew](https://git-scm.com/downloads/mac).

For the Windows installer, I recommend against adding the links to your context menu (so disable Windows Explorer integration). Git Bash Profile for Windows Terminal can be useful (if you've installed it). As default editor I recommend something like Notepad++ or Notepad. For the rest the default options should be okay. Be sure to use the recommended option for "Adjusting your PATH environment".

## GUI

If you're using a GUI program (so GitKraken, VS Code or GitHub Desktop), use their documentation to login to GitHub (make sure you have an account). Go to the [frontend](./frontend.md) or [backend](./backend.md) for further instructions.

## Command line (recommended)

Why do I recommend using the command line? Because a lot of developer tools work with it exclusively. Because it's simple to develop one, they usually have the most features and offer you the most control. At the same time, they also usually allow you to make more mistakes and can have a steeper learning curve, although ChatGPT has made things a lot easier nowadays. 

To be able to download and upload repositories, you'll still need to login to GitHub. For that, I recommend using the `gh` CLI. For Windows, I recommend just using the installer. The current version (as of December 2024), can be found [here](https://github.com/cli/cli/releases/download/v2.63.0/gh_2.63.0_windows_amd64.msi). To find newer versions, go to the [releases](https://github.com/cli/cli/releases) page and download what's most similar to "GitHub CLI 2.63.0 windows amd64 installer" (amd64 means x86-64, if you're using an ARM chip you should install using WinGet, see [here](https://github.com/cli/cli?tab=readme-ov-file#windows)). For general installation instructions, see [here](https://github.com/cli/cli?tab=readme-ov-file#installation).

If you haven't already, I recommend installing "Windows Terminal", which is much, much better than the standard Command Prompt. You can find it on the [Microsoft Store](https://apps.microsoft.com/detail/9n0dx20hk701?ocid=webpdpshare) (if the link doesn't work, just search for Windows Terminal). You might also want to [install PowerShell 7](https://aka.ms/PSWindows).

Once you have `gh` installed (you might need to restart your terminal), run `gh auth login` and follow the steps to login to your GitHub account (make sure you have one!).  Finally, go to the [frontend](./frontend.md) or [backend](./backend.md) for further instructions.