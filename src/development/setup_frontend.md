# Frontend setup

Setting up the frontend is the easiest. The frontend is entirely developed and deployed from the [`DSAV-Dodeka/DSAV-Dodeka.github.io`](https://github.com/DSAV-Dodeka/DSAV-Dodeka.github.io) repository. Like in the other pages, we will assume some familiarity with Git and GitHub.

The first step is thus to clone the repository to your computer. Because we store all our images inside the repository, the full history contains countless copies of large images. In the past, we didn't properly optimize them so sometimes there were multiple versions of very large images. Thankfully, you don't have to download the full history. Instead, when cloning, run the following command:

```shell
gh repo clone DSAV-Dodeka/DSAV-Dodeka.github.io dodekafrontend -- --filter=blob:none
```

The `--filter=blob:none` option executes a "partial clone", in which all blobs (so the actual file contents) of old commits are not downloaded. They are only downloaded once you actually switch to a commit 

The next step is to install `npm`, which is the standard package manager for JavaScript. We use it to download all our dependencies. `npm` is included when you install NodeJS, which is a JavaScript runtime (a program that runs JavaScript code) based on the same internal engine as Google Chrome.

Currently, the minimum version for `node` is v18, so be sure to install a version greater than that.

Next, open the command line in the root folder of the project. Then, to install all dependencies, run:

`npm install`

Once this is done, we can actually run the website using the command:

`npm run dev`

Under the hood, this will use Vite to bundle and build our project into actual HTML, CSS and JavaScript that our browser can run. The command will start a dev server, which will allow you to access the website in your browser using something like `localhost:3000` (the port, 3000 in this case, might be different).

