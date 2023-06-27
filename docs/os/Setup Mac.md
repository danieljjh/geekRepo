# Setup Mac

- install homebrew
- install docker
- setup python

## python Setup

### Use pyenv to manage multiple python version

[pyenv doc link](https://realpython.com/intro-to-pyenv/)

- install pyenv
  在 macOS 上使用 zsh shell 和 `pyenv` 管理 Python 版本，需要按照以下步骤进行安装和配置：

1. 打开终端并安装 `pyenv` 工具，可以使用以下命令：

   ```shell
   brew update
   brew install pyenv
   ```

   如果您没有安装 `brew`，请先按照官方文档中的说明安装：<https://brew.sh/index_zh-cn>

2. 将以下内容添加到 `~/.zshrc` 文件的末尾，以启用 `pyenv`：

   ```shell
   export PATH="$HOME/.pyenv/bin:$PATH"
   eval "$(pyenv init -)"
   eval "$(pyenv virtualenv-init -)"
   ```

3. 重新加载 `~/.zshrc` 文件以启用更改：

   ```shell
   source ~/.zshrc
   ```

- use pyenv to install python version

1. 您可以使用以下命令安装 Python 版本：

   ```shell
   pyenv install --list
   pyenv install 3.7.6
   ```

   在此示例中，我们将安装 Python 3.7.6。如果需要安装其他版本，可以将版本号替换为需要安装的版本号。

2. 使用以下命令切换全局 Python 版本：

   ```shell
   pyenv versions
   pyenv global 3.7.6
   ```

   包括 Python 程序在内的所有应用程序现在都将使用全局指定的 Python 版本。

3. 如果您只需要在当前 shell 中使用特定版本的 Python，则可以使用以下命令：

   ```shell
   pyenv shell 3.7.6
   ```

   在此模式下，只有当前 shell 中运行的应用程序将使用指定的 Python 版本。

以上是在 macOS 中使用 zsh 和 `pyenv` 安装和管理 Python 版本的步骤。

- install virtualenvwrapper

以下是在 macOS 系统上安装 virtualenvwrapper 并使用 zsh 作为默认 shell 的步骤：

1. 使用 Homebrew 安装 virtualenvwrapper。在终端中执行以下命令：

   ```shell
   brew install virtualenvwrapper
   ```

2. 打开 `.zshrc` 文件并添加以下行，如果该文件不存在，则需要创建它：

   ```shell
   nano ~/.zshrc
   ```

   添加以下内容：

   ```shell
   export WORKON_HOME=$HOME/.virtualenvs
   export PROJECT_HOME=$HOME/Devel

   source /usr/local/bin/virtualenvwrapper.sh
   ```

   `WORKON_HOME` 变量存储 virtualenv 的位置，`PROJECT_HOME` 指定 projects 的位置。`virtualenvwrapper.sh` 脚本的路径在 macOS 上通常为 `/usr/local/bin/virtualenvwrapper.sh`。

3. 加载 `.zshrc` 文件以使其立即生效：

   ```shell
   source ~/.zshrc
   ```

4. 现在 virtualenvwrapper 可以正常工作。创建一个新的虚拟环境：

   ```shell
   mkvirtualenv myenv
   ```

   这会创建并激活名为 `myenv` 的新虚拟环境。您可以通过以下命令离开虚拟环境：

   ```shell
    deactivate
   ```

- use pyenv to create virtualenv

  **注意** 不要直接用 pip install， 而要在 env 里用 python -m install

  ```shell
  # 确定当前 python 版本
  pyenv local 3.9.10
  python -V

  # create virtualenv
  python -m venv ~/.virtualenvs/py39

  # active virtualenv
  workon py39

  # install packages
  (py39)$ python -m install pandas

  ```

### jupyter lab

- install jupyter lab

- setup jupyter lab server

- create ipythonenv for jupyter
  为 jupyter 创建一个新的虚拟环境

  ```shell
  python -m ipykernel install --user --name=<your-env-name>
  ```

-
