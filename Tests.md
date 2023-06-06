您可以运行测试来验证您的修改。

发布 [PR #10291](https://github.com/AUTOMATIC1111/stable-diffusion-webui/pull/10291), [py.test](https://docs.pytest.org/en/7.3.x/)用作测试运行器。测试依赖项在 `requirements-test.txt` 中，所以先 `pip install -r requirements-test.txt`。

大多数测试针对 WebUI 的实时实例运行。 您可以使用带有 `--test-server` 参数的合适的baseline configuration启动 WebUI 服务器，但您可能想要添加e.g. `--use-cpu all --no-half`取决于您的系统。

服务器运行后，您只需使用`py.test`即可运行测试。
