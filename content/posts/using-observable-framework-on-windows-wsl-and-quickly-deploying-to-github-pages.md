---
title: "Using Observable Framework on Windows WSL and quickly deploying to GitHub Pages"
date: "2024-02-16"
categories:
  - "walkthrough"
tags:
  - "coding"
  - "devops"
  - "git"
  - "github"
  - "linux"
  - "observable"
  - "project"
  - "tutorial"
  - "windows"
---

If you've got [the framework](https://github.com/observablehq/framework) installed, and just want to deploy the static pages, [skip to below](#deploying). Note - my first time doing this, but it works. You can always deploy directly to Observable.

Observable Framework is [not supported on Windows](https://github.com/observablehq/framework/issues/90), according to an issue in their GitHub. (Update Mar 2024: Observable Framework is now supported on Windows). To use it, you need to use WSL.

If you want to use it through WSL, then the following still applies. Fortunately, it is very easy to do. Steps I had to take to do this:

Install [WSL from Powershell as described here](https://learn.microsoft.com/en-us/windows/wsl/install), or in the Windows Store, download Windows Subsystem for Linux and a preferred Linux distro. It looks like the latest offering is just named Ubuntu in the Windows store.

![](https://laggingindicators.wordpress.com/wp-content/uploads/2024/02/image.png?w=607)

![](https://laggingindicators.wordpress.com/wp-content/uploads/2024/02/image-1.png?w=614)

Once you get Ubuntu from the App Store, if you don't use the command line version, you need to install the application. Just click on it from your Start Menu, and a terminal will guide you through the install process, setting up a username and password.

Note that you may need to enable WSL in the "Turn Windows Features on or off" menu (search this from the Start menu), and ensure Windows Subsystem for Linux is checked if you don't use the command line install.

<figure>

![](https://laggingindicators.wordpress.com/wp-content/uploads/2024/02/image-3.png?w=336)

<figcaption>

Check this box in the Turn Windows Features on or off menu

</figcaption>

</figure>

In VSCode, you can easily open a "Remote Window" on the bottom left corner of the screen. See the green button below, with the >< signs. Clicking it will open up a menu. Note that you now have a menu at the top; just select Connect to WSL. If a error pops up saying that "no distro is found", then you need to ensure that it is installed, rather than just downloaded on your computer. The Powershell install manages this for you.

![](https://laggingindicators.wordpress.com/wp-content/uploads/2024/02/image-2.png?w=223)

![](https://laggingindicators.wordpress.com/wp-content/uploads/2024/02/image-4.png?w=1024)

Open a new terminal (View > Terminal). It should show up as `username@computername:~$`.

From here, install Node. Here is the [Windows Dev guide](https://github.com/MicrosoftDocs/windows-dev-docs/blob/docs/hub/dev-environment/javascript/nodejs-on-wsl.md). Only one note: a `Could not resolve host: raw.githubusercontent.com` error may appear if you are connected to a VPN.

![](https://laggingindicators.wordpress.com/wp-content/uploads/2024/02/image-5.png?w=658)

Once you can run `nvm ls` and see the above, you can install the [Observable Framework library following the Get Started documentation](https://observablehq.com/framework/getting-started).

You can open the WSL folders in the file explorer of VS Code, as well. If you enabled a Git repository, and want to connect it to your account, use

```
git config --global user.email "you@example.com"git config --global user.name "Your Name"
```

Set up a remote repository (you may need to connect VSCode to git as well).

As instructed in the Observable documentation, just run `npm run dev` and a server should start, and open to a new page in your browser, likely on 127.0.0.1:3000. You're set!

![](https://laggingindicators.wordpress.com/wp-content/uploads/2024/02/image-6.png?w=1024)

Now, deploying the static site to GitHub pages. The steps are very similar for any static site generator process.

Feel free to update your `title`, located in the `observablehq.config.ts` file, to update the website title in the browser.

Run `npm run build` in the working directory, which will create a /dist folder. Remove the /dist folder from the `.gitignore`.

Follow the [instructions on GitHub here](https://docs.github.com/en/pages/getting-started-with-github-pages/creating-a-github-pages-site) to create a new site.

Create a new repository in GitHub, and follow the instructions in the "…or push an existing repository from the command line" code block.

```
git remote add origin https://github.com/user_name/repo_name.gitgit branch -M maingit push -u origin main
```

Verify that your site appears in GitHub. Go to Settings > Pages, and choose Build and deployment > GitHub Actions.

![](https://laggingindicators.wordpress.com/wp-content/uploads/2024/02/image-7.png?w=496)

There is an example workflow called "Deploy static content to Pages." Click that, and a `static.yml` file should be created for you. The only edit to make here is editing the path argument from `.`, which is the whole repository, to `./dist`, which reflects you production files.

```
        with:          # Upload only dist directory, default is '.'          path: './dist'      - name: Deploy to GitHub Pages        id: deployment        uses: actions/deploy-pages@v4
```

This will ensure the base of your site arrives at the `index.htm`l page. Navigate to `https://{username}.github.io/{repositoryname}/`.

It's deployed! Feel free to [add a custom domain based on GitHub's instructions here](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site).
