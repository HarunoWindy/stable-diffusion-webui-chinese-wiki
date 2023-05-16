您可以运行测试来验证对weubi的修改。

要运行测试，请将`--tests TESTS_DIR`作为命令行参数添加到`launch.py`中，以及其他命令行参数。

有一个`basic test`，我们用它来验证通过API的基本图像创建是否正常工作，您可以为不同的场景创建其他测试。

要运行`basic test`，请将参数`--tests test`传递给`launch.py`：

```sh
python launch.py --tests test
```
for CPU only test:
```sh
python launch.py --tests test --use-cpu all --no-half
```
the test the arguments used for the [automated GitHub actions upon Pull Requests](https://github.com/AUTOMATIC1111/stable-diffusion-webui/blob/master/.github/workflows/run_tests.yaml) is:
```sh
python launch.py --tests test --no-half --disable-opt-split-attention --use-cpu all --skip-torch-cuda-test
```

You'll find outputs of main program in `test/stdout.txt` and `test/stderr.txt`.