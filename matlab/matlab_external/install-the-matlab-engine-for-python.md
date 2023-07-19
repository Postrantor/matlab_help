---
tip: translate by openai@2023-07-13 11:08:49
url: https://www.mathworks.com/help/matlab/matlab_external/install-the-matlab-engine-for-python.html
为Python安装MATLAB引擎
title: Install MATLAB Engine API for Python - MATLAB & Simulink
date: 2023-07-13 11:07:07
tag:
summary: To start MATLAB engine within a Python session, install the engine API as a Python package.
> 开始在Python会话中使用MATLAB引擎，请安装引擎API作为Python包。
...

> [!NOTE]
> .`python -m pip install . --ignore-installed matlabengineforpython`

## Install MATLAB Engine API for Python

To start the MATLAB® engine within a Python® session, you first must install the engine API as a Python package. For other requirements, see [System Requirements for MATLAB Engine API for Python](https://www.mathworks.com/help/matlab/matlab_external/system-requirements-for-matlab-engine-for-python.html).

> 要在 Python® 会话中启动 MATLAB® 引擎，您首先必须将引擎 API 安装为 Python 包。有关其他要求，请参阅[MATLAB 引擎 API for Python 的系统要求](https://www.mathworks.com/help/matlab/matlab_external/system-requirements-for-matlab-engine-for-python.html)。

### Verify Your Configuration

Before you install, verify your Python and MATLAB configurations.

- Check that your system has a supported version of Python and MATLAB R2014b or later. For more information, see [Versions of Python Compatible with MATLAB Products by Release](https://www.mathworks.com/support/requirements/python-compatibility.html).

> 请检查您的系统是否有 MATLAB R2014b 或更高版本的受支持版本的 Python。有关更多信息，请参阅[MATLAB 产品兼容的 Python 版本（按发布版本）](https://www.mathworks.com/support/requirements/python-compatibility.html)。

- To check that Python is installed on your system, run Python at the operating system prompt. Make sure that the Python path is included in your system path environment variable.

> 检查 Python 是否已安装在您的系统上，请在操作系统提示符下运行 Python。确保 Python 路径已包含在您的系统路径环境变量中。

- Add the folder that contains the Python interpreter to your path, if it is not already there.

### Install Engine API

You can install MATLAB Engine API for Python using the `pip` command or a Python setup script `setup.py`.

> 你可以使用`pip`命令或 Python 安装脚本`setup.py`来安装 MATLAB Engine API for Python。

#### Install Using `pip`

Starting with MATLAB R2022b, you can use the `pip` command to install the API. Choose one of the following procedures and execute from the system prompt.

> 从 MATLAB R2022b 开始，您可以使用`pip`命令来安装 API。选择以下其中一种程序，从系统提示符下执行。

- To install from the MATLAB folder, on Windows® type:

  ```
  cd "matlabroot\extern\engines\python"
  python -m pip install .

  ```

- Install the engine API from [https://pypi.org/project/matlabengine](https://pypi.org/project/matlabengine) with the command:

> 安装引擎 API，使用以下命令：`pip install matlabengine` 从[https://pypi.org/project/matlabengine](https://pypi.org/project/matlabengine)

```
python -m pip install matlabengine

```

#### Install Using `setup.py`

MATLAB provides a standard Python `setup.py` file for building and installing the engine using Python `setuptools`. For platform-specific commands, see [Python Setup Script to Install MATLAB Engine API](https://www.mathworks.com/help/matlab/matlab_external/python-setup-script-to-install-matlab-engine-api.html).

> MATLAB 提供了一个标准的 Python `setup.py`文件，用于使用 Python `setuptools`构建和安装引擎。有关平台特定命令，请参阅[Python Setup Script to Install MATLAB Engine API](https://www.mathworks.com/help/matlab/matlab_external/python-setup-script-to-install-matlab-engine-api.html)。

### Start MATLAB Engine

Start Python. Type these commands from the Python prompt to import the MATLAB module and start the engine:

> 开始 Python。 从 Python 提示符中输入以下命令来导入 MATLAB 模块并启动引擎：

```
import matlab.engine
eng = matlab.engine.start_matlab()


```

For more information, see [Start and Stop MATLAB Engine for Python](https://www.mathworks.com/help/matlab/matlab_external/start-the-matlab-engine-for-python.html).

> 更多信息，请参阅[为 Python 启动和停止 MATLAB 引擎](https://www.mathworks.com/help/matlab/matlab_external/start-the-matlab-engine-for-python.html)。

### Troubleshooting MATLAB Engine API for Python Installation

- Make sure that your MATLAB release supports your Python version. See [Versions of Python Compatible with MATLAB Products by Release](https://www.mathworks.com/support/requirements/python-compatibility.html).

> 确保您的 MATLAB 版本支持您的 Python 版本。请参阅[MATLAB 产品兼容的 Python 版本](https://www.mathworks.com/support/requirements/python-compatibility.html)。

- Make sure that you have sufficient privileges to execute the install command from the operating system prompt. On Windows, if necessary, open the command prompt with the \***\*Run as administrator\*\*** option.

> 确保您具有从操作系统提示符执行安装命令的足够权限。 在 Windows 上，如有必要，请使用\***\*以管理员身份运行\*\***选项打开命令提示符。

- You must run the Python install command from the specified MATLAB folder. For detailed instructions, choose one of the platform links in [Install Engine API](https://www.mathworks.com/help/matlab/matlab_external/install-the-matlab-engine-for-python.html#mw_dd7c0a95-47a8-4f58-b3e8-e19d4fec1105).

> 您必须从指定的 MATLAB 文件夹运行 Python 安装命令。有关详细说明，请选择[安装引擎 API](https://www.mathworks.com/help/matlab/matlab_external/install-the-matlab-engine-for-python.html#mw_dd7c0a95-47a8-4f58-b3e8-e19d4fec1105)中的一个平台链接。

- The installer installs the engine in the default Python folder. To use a nondefault location, see [Install MATLAB Engine API for Python in Nondefault Locations](https://www.mathworks.com/help/matlab/matlab_external/install-matlab-engine-api-for-python-in-nondefault-locations.html).

> 安装程序将引擎安装到默认的 Python 文件夹中。要使用非默认位置，请参阅[在非默认位置安装 MATLAB 引擎 API for Python](https://www.mathworks.com/help/matlab/matlab_external/install-matlab-engine-api-for-python-in-nondefault-locations.html)。

- If you installed the package in a nondefault folder using `--prefix`, make sure to set the `PYTHONPATH` environment variable. For example, suppose that you used this installation command:

> 如果您使用`--prefix`在非默认文件夹中安装了该软件包，请确保设置`PYTHONPATH`环境变量。例如，假设您使用了以下安装命令：

```
python setup.py install --prefix="matlab22aPy39"


```

In Python, update `PYTHONPATH` with this command:

```
sys.path.append("matlab22aPy39")

```

- For more troubleshooting information, see [Troubleshoot MATLAB Errors in Python](https://www.mathworks.com/help/matlab/matlab_external/troubleshoot-matlab-errors-in-python.html).

> 对于更多故障排除信息，请参阅[Python 中的 MATLAB 错误故障排除](https://www.mathworks.com/help/matlab/matlab_external/troubleshoot-matlab-errors-in-python.html)。

## Related Topics

- [System Requirements for MATLAB Engine API for Python](https://www.mathworks.com/help/matlab/matlab_external/system-requirements-for-matlab-engine-for-python.html)
- [Versions of Python Compatible with MATLAB Products by Release](https://www.mathworks.com/support/requirements/python-compatibility.html)
- [Install Supported Python Implementation](https://www.mathworks.com/help/matlab/matlab_external/install-supported-python-implementation.html#bujjwjn)
- [Install MATLAB Engine API for Python in Nondefault Locations](https://www.mathworks.com/help/matlab/matlab_external/install-matlab-engine-api-for-python-in-nondefault-locations.html)
- [Start and Stop MATLAB Engine for Python](https://www.mathworks.com/help/matlab/matlab_external/start-the-matlab-engine-for-python.html)
---
