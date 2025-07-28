# Configure Your Workstation

<hr>

**1. Set Account-Level Environment Variables**

Your system must have account-level environment variables set in order for Python and your development environment to work properly.

1. Go to the Start menu and type in “environment” to locate the **Edit environment variables for your account** panel.

2. Add the following environment variables replacing `username` with your Active Directory username.

3. For the `Path` environment variable, select the existing `Path` value and then the **Edit** button to append the three values to the end of the list.

| Name              | Scope | Mode   | Value                                                                                                                | Application |
| ----------------- | ----- | ------ | -------------------------------------------------------------------------------------------------------------------- | ----------- |
| `Home`            | User  | Add    | `C:\Users\<username>`                                                                                                | Git Bash    |
| `HTTP_PROXY`      | User  | Add    | `http://proxy.Redactedfinancial.com:8080`                                                                            | Many        |
| `HTTPS_PROXY`     | User  | Add    | `http://proxy.Redactedfinancial.com:8080`                                                                            | Many        |
| `Path`            | User  | Append | 1.`%PYTHONPATH%` <br> 2.`%PYTHONPATH%\Scripts` <br> 3.`C:\Users\<username>\AppData\Roaming\Python\Python312\Scripts` | Python      |
| `PIP_CONFIG_FILE` | User  | Add    | `C:\Users\<username>\.config\pip\pip.conf`                                                                           | Python      |
| `PYTHONPATH`      | User  | Add    | `C:\Program Files\Python312`                                                                                         | Python      |
| `WORKON_HOME`     | User  | Add    | `C:\Users\<username>\.virtualenvs`                                                                                   | Python      |

**2. Point `pip` away from PyPI**

The [Python Package Index (PyPI)](https://pypi.org/) located at [pypi.org](https://pypi.org/) is where `pip` normally searches for packages. When you run `pip install`, a web request goes out to [pypi.org](https://pypi.org/) behind the scenes. Since we are in a corporate environment, this is a non-starter due to security concerns.

Instead, we must configure our workstation to pull packages from [Sonatype Nexus](https://nexus2.Redactedfinancial.com/), which serves as a secure internal PyPI proxy.

To point your `pip` tool at [Nexus](https://nexus2.Redactedfinancial.com/),

1. Download [pip.conf](../_static/files/pip.conf).
2. Navigate to `%USERPROFILE%` in a Windows Explorer window.
3. Create a `.config folder` (_with_ the dot!).
4. Create a `pip` folder within `.config`.
5. Save `pip.conf` to `%USERPROFILE%\.config\pip\pip.conf`.

**3. Update `pip`**

The version of `pip` that is installed out-of-the-box will be stale. This will cause warnings to appear when you install packages. These can safely be ignored, but to resolve this issue—

1. Go to the Start menu and type in “powershell” to locate the **Windows PowerShell** application.
2. Launch **Windows PowerShell**.
3. Type in `python -m pip install -U pip` and press Enter.
4. Wait for the update to complete.

**4. Configure Git Bash**

**Git Bash** must be configured to talk to [GitHub Enterprise](https://github.Redactedfinancial.com/). To do this, complete the following steps.

1. Go to the Start menu and type in “git bash” to locate the **Git Bash** application.
2. Launch **Git Bash**.
3. Type in `git config --global user.name "$(id -u -n)"` and press Enter.
4. Type in `git config --global user.email "$(id -u -n | tr -d ' ')@Redacted.com"` and press Enter.
5. Type in `ssh-keygen -t ed25519 -C "$(id -u -n | tr -d ' ')@Redacted.com"` and press Enter.
6. Hit Enter to save the key in the default location (`/c/Users/<username>/.ssh/id_ed25519`).
7. Hit Enter at the `Enter passphrase (empty for no passphrase):` prompt. Optionally, set a passphrase here.
8. Hit Enter at the `Enter same passphrase again:` prompt.
9. Type in `eval "$(ssh-agent -s)"` and press Enter to launch **SSH Agent**.
10. Type in `ssh-add ~/.ssh/id_ed25519` to add the generated key to **SSH Agent**.
11. Type in `clip < ~/.ssh/id_ed25519.pub` and press Enter to copy the public key to your Windows clipboard.
12. Paste your public key in the **Key** field under your [GitHub Profile: SSH and GPG Keys](https://github.Redactedfinancial.com/settings/ssh/new) section.
13. Select the green **Add SSH Key** button.
14. Type in `ssh -Tv git@github.Redactedfinancial.com` and press Enter from from Git Bash to test your connection. You should see something similar to the below screenshot.

![SSH Key Terminal](../_static/images/ssh_key_terminal_screenshot.png)

The line that says `You've successfully authenticated, but GitHub does not provide shell access.` indicates that you've set up Git Bash appropriately.

**5. Configure Python Virtual Enviornments**

A virtual environment in Python is a way of separating _project_ dependencies from _system_ dependencies as well as _other_ project dependencies. For example, you may use `jupyter` frequently for data analysis. If this is the case, `jupyter` should rightfully be installed to your _system_ packages. However, if you are working with `pandas` v1.0.3 on one project and `pandas` v1.5.2 on another, you will need to configure Python virtual environment to avoid conflicts.

These environments work by programmatically prefixing the `PATH` and `PYTHONPATH` with locations under `%USERPROFILE\.virtualenvs\`, so that these locations take precedence over system locations.

1. Go to the Start menu and type in “git bash” to locate the _**Git Bash**_ application.
2. Launch _**Git Bash**_.
3. Type in `pip install virtualenv virtualenvwrapper` and press Enter.
4. Wait for the package installation to complete.
5. Download [.bash_profile](../_static/files/.bash_profile).
6. Navigate to `%USERPROFILE%` in a Windows Explorer window.
7. Save `.bash_profile` to `%USERPROFILE%\.bash_profile`.
8. Go to the Start menu and type in “powershell” to locate the _**Windows PowerShell**_ application.
9. Launch _**Windows PowerShell**_.
10. Type in `pip install virtualenvwrapper-win` and press Enter.
11. Wait for the package installation to complete.

Once installed and configured, a `workon` command will be available within both Windows PowerShell and Git Bash. This will list all of the available virtual environments on your system.

To create a virtual environment, use `mkvirtualenv <venv>`, where `<venv>` is the name of your virtual environment. You will notice that your shell prompt is prefixed with <code>(&lt;name&gt;)</code> to reflect that you are currently in a virtual environment. To exit from the environment, type `deactivate`.

**If you wish to use Python virtual environments with Jupyter**, you must install the `ipykernel` package to your virtual environment and then install a kernelspec.

Additional [context is here](https://towardsdatascience.com/link-your-virtual-environment-to-jupyter-with-kernels-a69bc61728df). To summarize, though&mdash;

1. Type `workon <name>` and press Enter to enter the `<venv>` environment.
2. Type `pip install ipykernel` and press Enter.
3. Wait for the package installation to complete.
4. Type `python -m ipykernel install --name=<venv>` to install a kernelspec.

Other virtual environment packages exist outside of the `virtualenv` package that you just installed, such as `pyenv` and Python's own `venv` module. Feel free to explore these at your own leisure. `virtualenv` has been around for quite some time, however, and it should be a suitable choice for most developers.
