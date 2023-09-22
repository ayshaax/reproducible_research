# Reproducible research: version control and R

## Why reproducible research?

Reproducibility is at the core of science. 

### Reproducible papers with live code

## FAIR and FAIREST principles

- **F**indable:
- **A**ccessible:
- **I**nteroperable:
- **R**eusable:

- **E**ngagement:
- **S**ocial connections:
- **T**rust:

## Best practices for code and data

- Keep it simple
- Annotate your code
- Use informative names for variables
- Use a code block structure

- Define a naming convention
- Use informative names for files
- Use lowercase in file names, avoid using spaces or special characters
- Use a versioning system in the file names
- Use conventional file formats

## Git and GitHub

We will be using the GitHub website, since it simplifies the git workflow and provides a graphical user interface (GUI).

## The Problem

Imagine you are given a test tube with 900 ul of growth media, and an isolate of the bacterium *Escherichia coli* suspended in 100 ul of the same media (total volume of 1 ml).

Surely in the right conditions, the bacteria will start multiplying at a fast rate since there are plenty of resources available to them in the test tube. This may continue for a while but as the population increases, resources will become scarce. The growth rate will start to decrease, and after some time, the population size may remain constant after reaching its maximum capacity in that environment.

You are interested in estimating the initial population size of your bacteria, rate of growth and carrying capacity from experimental data. You also want to make sure that your analysis is reproducible and the data is available for your colleagues all over the world, for them to be able to replicate your findings.

How can we go about doing this? 

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

We can find the solution to the differential equation which relates the population size at time t to the initial size of the population (), the growth rate () and the carrying capacity ().

```math
\begin{equation}
N(t) = \frac{K N_0 e^{rt}}{K-N_0+N_0 e^{rt}}
\end{equation}
```

We can plot the solution to get a more intuitive idea of its behaviour (Figure 1). 

&nbsp; 

![Logistic model](https://github.com/josegabrielnb/reproducible_research/blob/main/logistic_growth.png)

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

Click on the link to OSF shown above and in the search bar, type **"Logistic growth data"**. Find and enter the project for the parctical.

Before continuing to the next section, read and follow the instructions described on the Wiki.

* **Q3.** *When was the data uploaded to OSF?*

There are a few additional free platforms that you can check out for your own work, including **Zenodo** (https://zenodo.org/) and **figshare** (https://figshare.com/).

### 2. Fork the GitHub repo with the code

### 3. Create a dev branch to work on the code

### 4. Add a collaborator to your project

### 5. Work on the data and code (Posit cloud)

### 6. Commit the changes to your GitHub repo

### 7. Merge the final commits to main

### 8. Add a license and edit the README.md file



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

## Questions

## Google Colab (extra)

## Acknowledgements

## Additional materials

## References
