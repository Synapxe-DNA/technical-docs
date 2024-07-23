# Technical Documentation for Data Analytics & AI using MkDocs Material

## Install Dependencies <a name="installation"></a>

### Anaconda (Recommended)

You can download the Anaconda Distribution for your respective operating system [here](https://docs.anaconda.com/anaconda/install/). You may also find out how to get started with Anaconda Distribution [here](https://docs.anaconda.com/anaconda/getting-started/). To verfiy your installation, you can head to the Command Line Interface (CLI) and run the following command:

```bash
conda list
```

You should see a list of packages installed in your active environment and their versions displayed. For more information, refer [here](https://docs.anaconda.com/anaconda/install/verify-install/).

---

Once set up, create a virtual environment using `conda` and install dependencies:

```bash
# Create a virtual environment
conda create -n <VENV_NAME> python=<PYTHON_VERSION> -y
conda activate <VENV_NAME>

# Install dependencies
pip install -r requirements.txt
```

### Poetry

Refer to the documentation [here](https://python-poetry.org/docs/#installing-with-the-official-installer) (recommended) on how to install Poetry based on your operating system.

> [!IMPORTANT]
> For Mac users, if encountering issues with `poetry command not found`, add `export PATH="$HOME/.local/bin:$PATH"` in your `.zshrc` file in your home folder and run `source ~/.zshrc`.

---

First create a virtual environment by running the following commands:

```bash
poetry shell
```

> [!TIP]
> If you see the following error; `The currently activated Python version 3.11.7 is not supported by the project (^3.12). Trying to find and use a compatible version.`, run:
>
> ```bash
> poetry env use 3.12.3  # Python version used in the project
> ```

To install the defined dependencies for your project, just run the `install` command. The `install` command reads the [`pyproject.toml`](pyproject.toml) file from the current project, resolves the dependencies, and installs them.

```bash
poetry install
```

If there is a [`poetry.lock`](poetry.lock) file in the current directory, it will use the exact versions from there instead of resolving them. This ensures that everyone using the library will get the same versions of the dependencies.

If there is no [`poetry.lock`](poetry.lock) file, Poetry will create one after dependency resolution.

> [!TIP]
> It is best practice to commit the `poetry.lock` to version control for more reproducible builds. For more information, refer [here](https://python-poetry.org/docs/basic-usage/#:~:text=changes%20in%20dependencies.-,Committing%20your%20poetry.lock%20file%20to%20version%20control,-As%20an%20application).

---

### venv

You can use Python's native virtual environment `venv` to setup the project

```bash
# Create a virtual environment
python3 -m venv <VENV_NAME>
```

You can then activate the environment and install the dependencies using the following commands -

For UNIX-based systems (macOS / Linux):

```bash
# Activate virtual environment
source <VENV_NAME>/bin/activate

# Install dependencies
pip install -r requirements.txt
```

For Windows:

```powershell
# Activate virtual environment
.\<VENV_NAME>\Scripts\activate

# Install dependencies
pip install -r requirements.txt
```

> [!TIP]
> If you're using Python's native virtual environment `venv`, it is best practice to name your virtual environment `venv`.

---

## Pushing to GitHub

> [!WARNING]
> Refrain from pushing into `main` branch directly — it is bad practice. Always create a new branch and make your changes on your new branch.

Every time you complete a feature or change on a branch and want to push it to GitHub to make a pull request, you need to ensure you lint and format your code.

### Install the Git Hook Scripts

Run pre-commit install to set up the git hook scripts:

```zsh
pre-commit install

> pre-commit installed at .git/hooks/pre-commit
```

This command sets up the git hooks and run them automatically before every commit. For more information, refer to the [pre-commit docs](https://pre-commit.com/). To see what hooks are used, refer to the [`.pre-commit-config.yaml`](.pre-commit-config.yaml) YAML file.

### (Optional) Run `pre-commit run --all-files`

Optionally, you can also run the command `pre-commit run --all-files` to lint and reformat your code. It's usually a good idea to run the hooks against all of the files when adding new hooks (usually pre-commit will only run on the changed files during git hooks).

Alternatively, there is a [`Makefile`](Makefile) that can also lint and reformat your code base when you run the simpler command `make lint`.

You should ensure that all cases are satisfied before you push to GitHub (you should see that all has passed). If not, please debug accordingly or your pull request may be rejected and closed.

The [`run-checks.yml`](.github/workflows/run-checks.yml) is a GitHub workflow that kicks off several GitHub Actions when a pull request is made. These GitHub Actions check that your code have been properly linted and formatted before it is passed for review. Once all actions have passed and the PR approved, your changes will be merged to the `main` branch.

> [!NOTE]
> The `pre-commit` will run regardless if you forget to explicitly call it. Nonetheless, it is recommended to call it explicitly so you can make any necessary changes in advanced.

## Publishing the Site

### GitHub Pages

Since the documentation is already hosted on GitHub, [GitHub Pages](https://pages.github.com/) is
certainly the most convenient way to publish our documentation. It's free of charge and pretty easy
to set up.

#### with GitHub Actions

Using [GitHub Actions](https://github.com/features/actions), we can automate the deployment of our
documentation. At the root of the repository, create a new GitHub Actions workflow, e.g.
[`.github/workflows/ci.yml`](.github/workflows/ci.yml), and copy and paste the following contents at
this
[link](https://squidfunk.github.io/mkdocs-material/publishing-your-site/#:~:text=name%3A%20ci,gh%2Ddeploy%20%2D%2Dforce).
Or you can simply refer to the [`ci.yml`](.github/workflows/ci.yml) which has already been set up.

Now, when a new commit is pushed to either the `master` or `main` branches, the static site is
automatically built and deployed. Push your changes to see the workflow in action.

If the GitHub Page doesn't show up after a few minutes, go to the settings of the repository and
ensure that the
[publishing source branch](https://docs.github.com/en/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site)
for the GitHub Page is set to `gh-pages`.

The documentation should shortly appear at
[wilsven.github.io/technical-docs/](https://wilsven.github.io/technical-docs/).
