# Frontend setup

For more information on developing the frontend, see the section on [developing the frontend](../developing/frontend.md).

Setting up the frontend is the easiest. The frontend is entirely developed and deployed from the [`DSAV-Dodeka/DSAV-Dodeka.github.io`](https://github.com/DSAV-Dodeka/DSAV-Dodeka.github.io) repository. Like in the other pages, we will assume some familiarity with Git and GitHub. We also assume you have Git installed and configured (see ())

Before you start, find some location on your computer where you want to store all the files. Try and put it somewhere you can easily find it again. For example, make a folder called "Git" in your "Documents" folder.

Next, be sure you have access to a terminal. **Windows Terminal**, which you can download from the Microsoft Store, is highly recommended. Once you have that installed, go to the "Git" folder or whatever location you chose, and right-click. It should then have a "Open in Terminal" option.

> Otherwise, you can type Ctrl+L and then copy the file path (using e.g. Ctrl+C). This could be a path like `C:\Users\tip\Documents\Git` (where "tip" is replaced by your username). Then, just open Windows Terminal (just search for it on your computer), and type `cd "<path>"`, where `<path>` is the path you just copied, so in my case that would be `cd C:\Users\tip\Documents\Git`. 

You now have that folder open in your terminal (it should say something like:

`PS C:\Users\tip\Documents\Git > `

## Downloading the code

The next step is to clone the repository to your computer. Because we store all our images inside the repository, the full history contains countless copies of large images. In the past, we didn't properly optimize them so sometimes there were multiple versions of very large images. Thankfully, you don't have to download the full history. Instead, when cloning, run the following command:
>>>>>>> 763e87b (update)

```shell
git clone https://github.com/DSAV-Dodeka/DSAV-Dodeka.github.io.git dodekafrontend --filter=blob:none
```
The `--filter=blob:none` option executes a "partial clone", in which all blobs (so the actual file contents) of old commits are not downloaded. They are only downloaded once you actually switch to a commit. This saves a lot of disk space on your computer and makes the clone much faster.

## Installing NodeJS

The next step is to install `fnm` (https://github.com/Schniz/fnm), which will help us install NodeJS, which is a JavaScript runtime (a program that runs JavaScript code) based on the same internal engine as Google Chrome.

Open Windows Terminal (**install it from the Microsoft Store if you don't have it**) and do this:

```
winget install Schniz.fnm
```

Open a **new** PowerShell terminal and run the following command:

```powershell
if (-not (Test-Path $profile)) { New-Item $profile -Force }
```

This creates a "profile" file that will we then edit with the following command:

```powershell
Invoke-Item $profile
```

Then, at the end of the file, copy-paste the following:
```powershell
# Fast Node Manager to run NodeJS
fnm env --use-on-cd --shell powershell | Out-String | Invoke-Expression
```

Then, open a new PowerShell window and run:

```
fnm use 24
```

If it asks for yes or no, type "y" and press enter to install. This will actually install version 24 of NodeJS. Then, once you open a new terminal with PowerShell and type `node -v`, it should respond something like `v24.x.x` (where x.x can be any number).

Check https://github.com/Schniz/fnm for more instructions if something does not work, or feel free to ask ChatGPT. Otherwise, you can always ask Tip for help.

## Installing dependencies
>>>>>>> 763e87b (update)

Download and install it from [the NodeJS website](https://nodejs.org/en/download/package-manager). I recommend picking an LTS version (so v22 right now). v20 is also fine if you still have that.

Note, if you don't care about the installation taking a while or are not comfortable with the command line, you can use the [installer](https://nodejs.org/en/download/prebuilt-installer) instead. Otherwise, I highly recommend (if you're on Windows), to use the 'fnm' package manager option. To use the [instructions](https://nodejs.org/en/download/package-manager), you should use a PowerShell terminal (best to use from Windows Terminal, download it from the Microsoft Store). You also need to install WinGet, which you can also get from the Microsoft Store through the App Installer (it's probably already installed, be sure to NOT download "APK Installer" or other non-Microsoft apps! See if it exists in the Microsoft Store, otherwise you can ignore the step. See [here](https://learn.microsoft.com/en-us/windows/msix/app-installer/install-update-app-installer) for details about the App Installer and [here](https://learn.microsoft.com/en-us/windows/package-manager/winget/) for WinGet).

If you're not on Windows, I recommend using the `nvm` option from the NodeJS website. Otherwise, you could also use [`pnpm`](https://pnpm.io/cli/env) instead.


Next, open the command line in the root folder of the project. This can be easily done by opening it in a IDE (integrated development environment, I recommend using [VS Code](https://code.visualstudio.com/)) and then opening a terminal there. Then, to install all dependencies, run:

`npm install`

## Running the website locally


Once this is done, we can actually run the website using the command:

`npm run dev`

Under the hood, this will use Vite to bundle and build our project into actual HTML, CSS and JavaScript that our browser can run. The command will start a dev server, which will allow you to access the website in your browser using something like `localhost:3000` (the port, 3000 in this case, might be different).

