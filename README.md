# Reproducible research: version control and R

## Why reproducible research?

Reproducibility is at the core of science.

## Git and GitHub

We will be using the GitHub website, since it simplifies the git workflow and provides a graphical user interface (GUI).

[AlphaFold](https://github.com/google-deepmind/alphafold)

[DIAMOND](https://github.com/bbuchfink/diamond)

[Agents.jl](https://github.com/JuliaDynamics/Agents.jl)

## The Problem

Imagine you are given a test tube with 900 $`\mu`$l of growth media, and an isolate of the bacterium *Escherichia coli* suspended in 100 $`\mu`$l of the same media (total volume of 1 ml).

Surely in the right conditions, the bacteria will start multiplying at a fast rate since there are plenty of resources available to them in the test tube. This may continue for a while but as the population increases, resources will become scarce. The growth rate will start to decrease, and after some time, the population size may remain constant after reaching its maximum capacity in that environment.

You are interested in estimating the initial population size of your bacteria, rate of growth and carrying capacity from experimental data. You also want to make sure that your analysis is reproducible and the data is available for your colleagues all over the world, for them to be able to replicate your findings.

*How can we model population growth in this system and make our analysis reproducible?*

## The Model

We can describe logistic growth using a differential equation:

```math
\begin{equation}
N'(t) = N r (1 - \frac{N}{K})
\end{equation}
```

This expression represents the rate of change of the population size as a function of the current population size times the instantaneous growth rate, scaled by a factor that takes into account the size of the population with respect to the carrying capacity.

&nbsp; 

* **Q1.** *What is the rate of change in the size of the population when N << K? and when N = K? how can we interpret this biologically?*

&nbsp; 

We can find the solution to the differential equation which relates the population size at time t to the initial size of the population ($`N_0`$), the growth rate ($`r`$) and the carrying capacity ($`K`$).

```math
\begin{equation}
N(t) = \frac{K N_0 e^{rt}}{K-N_0+N_0 e^{rt}}
\end{equation}
```

We can plot the solution to get a more intuitive idea of its behaviour (Figure 1). 

&nbsp; 

![Logistic model](https://github.com/josegabrielnb/reproducible_research/blob/main/images/logistic_growth.png)

**Figure 1. Graphical representation of the logistic model.**

(Left panel) At the beginning, the population seems to grows exponentially, but then slows down and reaches a constant size (equilibrium).

(Right panel) If we make a semilog-plot (x-axis linear, y-axis log-transformed), we observe an increasing linear relationship at the early time points (red rectangle), while at later time points, the population size remains constant.

&nbsp; 

* **Q2.** *Looking at the figure, what is the carrying capacity (K) in this example? And the initial population size?*

&nbsp; 

#### Observation #1. When K is much greater than N<sub>0</sub> and *t* is small, the population grows exponentially

If we assume that $` K \gg N_0 `$ and $`t`$ is small, we can approximate the behaviour of the solution at the initial time points.

First, in the denominator, $`K`$ will dominate since $`N_0`$ and $`e^{rt}`$ will be small by comparison. So we can write $`K \thicksim K - N_0+N_0 e^{rt}`$.

```math
\begin{equation}
N(t) = \frac{K N_0 e^{rt}}{K-\cancel{N_0+N_0 e^{rt}}}
\end{equation}
```

The simplified equation looks like this:

```math
\begin{equation}
N(t) = \frac{K N_0 e^{rt}}{K}
\end{equation}
```

We can cancel $`K`$ in the numerator and denominator.

```math
\begin{equation}
N(t) = \frac{ \cancel{K} N_0 e^{rt}}{\cancel{K}}
\end{equation}
```

Therefore, when K is much larger than N_0 and t is small, the population will grow exponentially.

```math
\begin{equation}
N(t) = N_0 e^{rt}
\end{equation}
```

#### Observation #2. In the limit $`t \to \infty`$, $`N(t) \to K`$

When t tends to infinity, the size of the population is equal to a constant number, the carrying capacity.

To show this, we take the limit of N(t) as t -> inf

```math
\begin{equation}
\lim\limits_{t \to \infty} N(t) = \lim\limits_{t \to \infty} \frac{K N_0 e^{rt}}{K-N_0+N_0 e^{rt}} = ind(\frac{\infty}{\infty})
\end{equation}
```

Using L'Hopital's rule, we take the derivate of the numerator and denominator with respect to $`t`$.

```math
\begin{equation}
\lim\limits_{t \to \infty} \frac{f'(t)}{g'(t)} = \lim\limits_{t \to \infty} \frac{K N_0 r e^{rt}}{N_0 r e^{rt}}
\end{equation}
```

We can now cancel the terms in the numerator and denominator,

```math
\begin{equation}
\lim\limits_{t \to \infty} \frac{K \cancel{N_0 r e^{rt}}}{\cancel{N_0 r e^{rt}}} = K
\end{equation}
```

Therefore, as $`t`$ tends to infinity, the size of the population will be equal to K.

```math
\begin{equation}
\lim\limits_{t \to \infty} N(t) = K
\end{equation}
```

### Implications for the data analysis

We can now use these observations to estimate the values of $`N_0`$, $`r`$ and $`K`$ from a given data set, using a linear approximation.

First, remember that the equation of a line is given by:

```math
\begin{equation}
y = b + mx
\end{equation}
```

To estimate $`N_0`$ and $`r`$, we can restrict ourselves to a region that shows exponential growth (remember $` K \gg N_0 `$ and $`t`$ is small).

```math
\begin{equation}
N(t) = N_0 e^{rt}
\end{equation}
```

```math
\begin{equation}
ln(N) = ln(N_0) + rt \cancel{ln(e)}
\end{equation}
```

The linear approximation when $` K \gg N_0 `$ and $`t`$ is small would be:

```math
\begin{equation}
ln(N) = ln(N_0) + rt
\end{equation}
```

On the other hand, when $`t`$ is large and the population size remains constant we can use the following approximation :

```math
\begin{equation}
N(t) = K + 0 \cdot t
\end{equation}
```



### 1. Download the data from OSF

The results from 3 simulated experiments have been uploaded to the Open Science Framework's website (https://osf.io).

OSF is a free online platform that allows researchers to collaborate on projects, deposit their data and make it widely accessible.

- Click on the link to OSF shown above and in the search bar, type **"Logistic growth data"**. Find and enter the project for the practical.

- Read and follow the instructions described on the Wiki.

* **Q3.** *When was the data uploaded to OSF?*

There are a few additional free platforms that you can check out for your own work, including **Zenodo** (https://zenodo.org/) and **figshare** (https://figshare.com/).

### 2. Sign up for a GitHub account

- Sign up for an account if you do not have one already (https://github.com/join). You can get access to some extra features as a student for free if you join using your University email address. 

- Choose a memorable user name!

- Log in to your new account.

### 3. Fork the GitHub repo with the code

First, we will need to find the repository with the code for our analysis.

- Type "**josegabrielnb/logistic_growth**" in the search bar at the top.

- Click on the link to see the repository, it should contain three R scripts for the data analysis.

- Create a fork (your own copy) of the repository by clicking on the top button "**Fork**".

<p align="center">
<img src="https://github.com/josegabrielnb/reproducible_research/blob/main/images/original_repo.png" width="700" height="220">
</p>

- Check the information displayed in the fork window and click on **Create fork**.

<p align="center">
<img src="https://github.com/josegabrielnb/reproducible_research/blob/main/images/create_fork.png" width="500" height="400">
</p>

- Your new copy of the "**logistic_growth**" repository should have been created successfully, congrats!

<p align="center">
<img src="https://github.com/josegabrielnb/reproducible_research/blob/main/images/forked_repo.png" width="800" height="500">
</p>

### 4. Create a dev branch to work on the repo

It is a good idea to work on a development branch when we start making our own changes.

In this way, we will only update the `main` branch once we are confident with our progress on the development branch.

- On the top left side of the repo home page, click on the down arrow next to `main` and type "**dev**" as the name of our development branch.

- Click on "**Create branch**" when prompted.

<p align="center">
<img src="https://github.com/josegabrielnb/reproducible_research/blob/main/images/create_dev.png" width="250" height="300">

- Once you have confirmed, the repository should have changed from `main` to the `dev` branch.

<p align="center">
<img src="https://github.com/josegabrielnb/reproducible_research/blob/main/images/dev_branch.png" width="250" height="300">

Now that we are working on `dev`, we can make changes being confident that these will not affect the `main` branch.

* **Q4.** *Why do you think it would be useful to have separate* `main` *and* `dev` *branches?*

### 5. Add a license and a README.md file

It is important to add a license and README.md files, so that people who find our repository will know how to use it.

Let's start with the license.

- On the top right corner, click on the button "**Add files**" and select "**Create new file**".

<p align="center">
<img src="https://github.com/josegabrielnb/reproducible_research/blob/main/images/create_license.png" width="250" height="100">
</p>

- In the field for the file name, type "**LICENSE**" in all caps.

- An option will appear to "**Choose a license template**", click on it and select a license.

* **Q5.** *Which licenses would be suitable to enable reproducibility of your project?*

<p align="center">
<img src="https://github.com/josegabrielnb/reproducible_research/blob/main/images/add_license.png" width="390" height="120">
</p>

- Once you have selected the license, click on "**Commit changes**".

<p align="center">
<img src="https://github.com/josegabrielnb/reproducible_research/blob/main/images/commit_changes.png" width="250" height="150">
</p>

- Create a README file following the steps outlined above, but instead name the file "**README.md**". 

<p align="center">
<img src="https://github.com/josegabrielnb/reproducible_research/blob/main/images/readme.png" width="500" height="150">
</p>


### 6. Add a collaborator to your project

Often times, you will want to work on a project with your colleagues.

- To add a collaborator, click on the **Settings** tab of the repository upper panel and then select **Collaborators**.

- You can then select **Add people** to invite collaborators to work on your project.

<p align="center">
<img src="https://github.com/josegabrielnb/reproducible_research/blob/main/images/add_collaborator.png" width="750" height="400">
</p>

### 7. Work on the data and code

Now that we are done setting up the `dev` branch of the repo, we can go ahead with the data analysis.

We will be using **Posit cloud** (https://posit.cloud), which is a cloud-based platform that provides an interface similar to RStudio.

- Sign up if you do not have a Posit cloud account, and then sign in.
  
- In your dashboard, click on "**New project**" and select "**New project from GitHub repository**". When asked, paste the link to the repository, this will set up the project with the files in the repo.

<p align="center">
<img src="https://github.com/josegabrielnb/reproducible_research/blob/main/images/new_project.png" width="250" height="180">
</p>

- On the git tab (upper right panel), make sure you change the branch from main to dev.

<p align="center">
<img src="https://github.com/josegabrielnb/reproducible_research/blob/main/images/git_panel_dev.png" width="500" height="150">
</p>

- In the files panel (to the lower right), click on the "**Upload**" and upload the *.csv file you downloaded from OSF.

<p align="center">
<img src="https://github.com/josegabrielnb/reproducible_research/blob/main/images/upload_experiment.png" width="500" height="250">
</p>

**Now that everything has been set up in your Posit cloud project, you can start working on the R scripts provided.**

- The scripts have blank spaces marked with triple question marks **???**

- Work on the code to fill in the blanks as you analyse the data.

- Include annotations in the code to signpost each step.

Start by plotting the data (`plot_data.R`), then fit the linear models (`fit_linear_model.R`) and finally assess the fit of the model to your data graphically (`plot_data_and_model.R`).

### 8. Commit and push the changes to your GitHub repo

Once you have completed the analysis, you are ready to update the GitHub repository!

- Make a list of the packages required and save them in a file called "package-versions.txt".

```
sink(file = "package-versions.txt")
sessionInfo()
sink()
```

- We will need to update the email to make the commits into your repository. Open the **Terminal** tab, and type (replace <YOUR_EMAIL> with the email you signed up with on GitHub):

```
git config --global user.email "<YOUR_EMAIL>"
```

- Now, go to the **git** panel and select the files that you will be updating/adding to the GitHub repo.

<p align="center">
<img src="https://github.com/josegabrielnb/reproducible_research/blob/main/images/commits_ready.png" width="500" height="170">
</p>

- Click on "**Commit**" and follow the instructions (you will be able to add an optional commit message).

- After the commit has been processed you should see the message "Your branch is ahead of origin/dev by 1 commit" in the **git** panel.

<p align="center">
<img src="https://github.com/josegabrielnb/reproducible_research/blob/main/images/commit_ahead.png" width="500" height="120">
</p>

The commit has staged the changes in the current Posit cloud project. To make the changes on GitHub we need to *push* the commit. To do this however, authentication is required.

- Go back to GitHub and click on your profile picture (top right of the window). Then click on **Settings**.

- Scroll to the very end of the left pane in the **Settings** window, and click on the tab "**<> Developer settings**".

-  Click on **Personal access tokens** and then on **Tokens (classic)**.

-  Select **Create a personal access token**, and tick all the boxes to get general permissions.

-  Copy the token (also save it somewhere!). You will use this token to authenticate and push your changes onto the GitHub repo.

<p align="center">
<img src="https://github.com/josegabrielnb/reproducible_research/blob/main/images/personal_token.png" width="600" height="220">
</p>

- Go back to the Posit cloud project, and click "**Push**" on the **git** tab.

- Use your GitHub credentials, and **when asked for the password enter your GitHub token**.

### 9. Merge the final commits to main

The `dev` branch of your GitHub repository should have updated after the push, check that all the files have been updated correctly.

- Go to the **Contribute** tab in the home page of your `dev` branch, and then click on **Open pull request**. 

<p align="center">
<img src="https://github.com/josegabrielnb/reproducible_research/blob/main/images/open_pull_request.png" width="500" height="170">
</p>

- As **base** select the `main` branch of your own repository, and check the message says "**Able to merge**". Click on **Create pull request**.

<p align="center">
<img src="https://github.com/josegabrielnb/reproducible_research/blob/main/images/create_pull_request.png" width="600" height="400">
</p>

- After a successful merge, you should see the following message:

<p align="center">
<img src="https://github.com/josegabrielnb/reproducible_research/blob/main/images/pull_request_success.png" width="520" height="80">
</p>

You can now delete the `dev` branch if you do not wish to continue working on it later.

**Congratulations!** Your repository with a reproducible analysis of logistic growth data is complete. You can add some final comments to the README.md file for any future users who come across your repo.

- To make your repo public, go to the **Settings** tab and scroll down to the **Danger Zone**.

- Click on **Change visibility** and then **Change to public**. Now your repository will be accessible online.

## Questions

1) Q1
   
3) Q2
   
5) Q3
   
7) Q4
   
9) Q5

## Additional materials

### Git and Github Glossary

- **Repository**: a repository (or repo) is essentially a folder that contains all your files and their revision history.
- **Commit**: commits refer to changes in the repository files that have been recorded as part of its history. You can think of it as a snapshot of the repository at a given time.
- **Checkout**: checkouts allow you to switch between different versions of the repo. You can access a previous version of a file, commit or jump to a different branch.
- **Clone**: by cloning a repository with git you create an entire copy of that repository on your machine, including all files, commits and branches.
- **Pull**: you can pull the contents of the remote repository to update your cloned repo to the most recent version.
- **Push**: pushing allows you to update the remote repo with your local commits. However, it will only push commits from the current branch you have checked out.
- **Branch**: branching is a useful option when collaborating on code with others. It lets you work on an isolated copy of the repo, which you can then safely experiment with: any changes will not affect the main branch of the project.
- **Merge**: when you are convinced that the main branch can be updated with changes from a development branch, you can merge those changes into the main branch.
- **Fork**: a fork creates a copy of a repository on your GitHub account, which is independent of the original repo. 
- **Pull request**: you can send pull requests of any commits to the owner of a repo or to collaborators. If they approve the proposed changes, the commits can be merged into the main branch or repository.

### FAIR principles

- **F**indable:
- **A**ccessible:
- **I**nteroperable:
- **R**eusable:

### Best practices for code and data

- Keep it simple
- Annotate your code
- Use informative names for variables
- Use a code block structure
- Use informative names for files
- Use lowercase in file names, avoid using spaces or special characters
- Use conventional file formats

### Google Colab

### Readings / Online materials

Braga PHP, et al. (2023). [Not just for programmers: how GitHub can accelerate collaborative and reproducible research in ecology and evolution](https://besjournals.onlinelibrary.wiley.com/doi/full/10.1111/2041-210X.14108). *Methods in Ecology and Evolution 14*(6): 1364-1380.

Alston JM, & Rick JA. (2021).[ A beginner's guide to conducting reproducible research](https://esajournals.onlinelibrary.wiley.com/doi/epdf/10.1002/bes2.1801). *Bulletin of the Ecological Society of America 102(2)*: e01801.

Cooper N, & Hsing PY. (Eds.). (2017). [A guide to reproducible code in ecology and evolution](https://www.britishecologicalsociety.org/wp-content/uploads/2017/12/guide-to-reproducible-code.pdf). British Ecological Society. London.

Wilkinson MD, et al. (2016). [The FAIR guiding principles for scientific data management and stewardship](https://www.nature.com/articles/sdata201618). *Scientific Data 3*: 160018.

Sandve GK, et al. (2013). [Ten simple rules for reproducible computational research](https://journals.plos.org/ploscompbiol/article?id=10.1371/journal.pcbi.1003285). *PLoS Computational Biology 9(10)*: 1003285.

[GitHub Skills](https://skills.github.com/)

[Software Carpentry](https://software-carpentry.org/lessons/)

## Acknowledgements

I would like to thank Alex Zarebski, Roberto Salguero-Gomez, Lydia France, Juan Antonio Balbuena and Juan Ignacio Lucas for their guidance and feedback while preparing this lesson.
