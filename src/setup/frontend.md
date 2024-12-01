# Frontend setup

For more information on developing the frontend, see the section on [developing the frontend](../developing/frontend.md).

Setting up the frontend is the easiest. The frontend is entirely developed and deployed from the [`DSAV-Dodeka/DSAV-Dodeka.github.io`](https://github.com/DSAV-Dodeka/DSAV-Dodeka.github.io) repository. Look at the instructions for [Git](./git.md) if you haven't already.

## Download the code

The first step is to "clone" (download) the repository to your computer. Because we store all our images inside the repository, the full history contains a lot of large images. In the past, we didn't properly optimize them so sometimes there were multiple versions of very large images. Thankfully, you don't have to download the full history. Instead, when cloning, run the following command (run it in some folder where you want to store it, the result will be a folder called 'dodekafrontend'):

```shell
git clone https://github.com/DSAV-Dodeka/DSAV-Dodeka.github.io.git dodekafrontend --filter=blob:none
```

The `--filter=blob:none` option executes a "partial clone", in which all blobs (so the actual file contents) of old commits are not downloaded. They are only downloaded once you actually switch to a commit. This might cause some occasional issues with GUI clients, so check out a branch first with the command line (`git checkout <branch name>`).

## Node/`npm`

The next step is to install `npm`, which is the standard package manager for JavaScript. We use it to download all our dependencies. `npm` is included when you install NodeJS, which is a JavaScript runtime (a program that runs JavaScript code) based on the same internal engine as Google Chrome. We use that runtime to develop and build our project.

Download and install it from [the NodeJS website](https://nodejs.org/en/download/package-manager). I recommend picking an LTS version (so v22 right now). v20 is also fine if you still have that.

Note, if you don't care about the installation taking a while or are not comfortable with the command line, you can use the [installer](https://nodejs.org/en/download/prebuilt-installer) instead. Otherwise, I highly recommend (if you're on Windows), to use the 'fnm' package manager option. To use the [instructions](https://nodejs.org/en/download/package-manager), you should use a PowerShell terminal (best to use from Windows Terminal, download it from the Microsoft Store). You also need to install WinGet, which you can also get from the Microsoft Store through the App Installer (it's probably already installed, be sure to not download "APK Installer" or other non-Microsoft apps! See [here](https://learn.microsoft.com/en-us/windows/msix/app-installer/install-update-app-installer) for details abou tthe App Installer and [here](https://learn.microsoft.com/en-us/windows/package-manager/winget/) for WinGet).

If you're not on Windows, I recommend using the `nvm` option from the NodeJS website. Otherwise, you could also use [`pnpm`](https://pnpm.io/cli/env) instead.


Next, open the command line in the root folder of the project. This can be easily done by opening it in a IDE (integrated development environment, I recommend using [VS Code](https://code.visualstudio.com/)) and then opening a terminal there. Then, to install all dependencies, run:

`npm install`

## Running the website

Once this is done, we can actually run the website using the command:

`npm run dev`

Under the hood, this will use Vite to bundle and build our project into actual HTML, CSS and JavaScript that our browser can run. The command will start a dev server, which will allow you to access the website in your browser using something like `localhost:3000` (the port, 3000 in this case, might be different).

