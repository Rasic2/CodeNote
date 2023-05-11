# Pytest 测试框架

[Pytest](https://docs.pytest.org/en/7.3.x/)是一个自动化测试框架，可以用来编写小型、可读的测试，并且还可以扩展以支持对应用程序和库的复杂功能测试。

## pytest.ini 文件

`Pytest` 默认会读取 `pytest.ini` 文件进行测试，示例如下：

```ini
[pytest]
addopts=--color=yes  # 输出颜色信息
testpaths=./tests  # 指定测试文件夹
log_cli = true  # 指定在命令行输出 logging
log_cli_level = INFO
log_cli_date_format = %Y-%m-%d %H:%M:%S
log_cli_format = %(asctime)s (%(filename)s:%(lineno)d) [%(name)s][%(levelname)s] %(message)s
```

## 编写测试用例

### 用例规范

使用 `Pytest` 开展测试时，它会自动识别测试目录下的 `test_*` 文件并进行测试。

在该 `test_*` 文件中所有需测试的类及方法都要以 `[Tt]est` 开头命名，测试类只需继承 `object` 即可。

### 初始化及数据清理

`Pytest` 支持通过 `setup/teardown` 开头的函数或方法来分别实现测试前的配置和测试后的数据清理工作，示例如下：

```python
def setup_module():
    os.chdir(f"{Path(RootDir).parent}/tests")

def teardown_module():
    try:
        shutil.rmtree(f"{Path(RootDir).parent / 'tests' / 'continuous' / 'hse'}")
        logger.info(f"Clean the Directory Success!")
    except FileNotFoundError:
        pass
```

### 测试错误

通过使用 `Pytest` 内置的上下文管理器（`pytest.raises`），可以轻松实现错误的测试，示例如下：

```python
def test_new(self):
    with pytest.raises(TypeError):
        MetaFile("XDATCAR")
```

### 测试用户输入

通过使用 `monkeypatch` 的上下文管理器（`pytest.raises`），可以轻松实现用户输入的模拟和测试，示例如下：

```python
def test_submit_neb(self, monkeypatch):
    input_words = ['y', 'n']
    for word in input_words:
        with monkeypatch.context() as m:
            m.setattr('builtins.input', lambda prompt="": word)
```

### 测试时切换工作目录

通过构建如下所示的 `pytest.fixture`，并将给函数作为测试用例的参数进行传递，可以实现在测试时切换工作目录。

```python
@pytest.fixture()
def change_test_dir(request, monkeypatch):
    monkeypatch.chdir(request.fspath.dirname)

class TestSubmitMag(object):

    def test_opt_mag(self, change_test_dir):
        task = OptTask()
        task.generate(vdw=True, sol=True, mag=True, low=True)
```
