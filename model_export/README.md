## 推荐的环境配置
- trace pt / onnx时
    ```sh
    python      3.10 
    torch       1.12.0
    torchvision 0.13.0 
    diffusers   0.24.0
    ```

- 转换bmodel时
    ```sh
    docker pull sophgo/tpuc_dev:latest
    docker run --privileged --name myname -v $PWD:/workspace -it sophgo/tpuc_dev:latest`
    ```
    详见[tpu-mlir文档中的环境配置章节](https://tpumlir.org/docs/quick_start/02_env.html)。

## 常见问题
1. 转换mlir时，报错scale_dot_product_attenton算子不支持。
    
    Solution：trace模型时使用的torch版本较高，trace出的pt模型中包含了该算子；使用较低版本的torch（如1.12.0）可以规避这个问题。
2. 转换bmodel后，出图明显异常。

    Solution：使用较低版本的torch（如1.12.0）重新跑`model_export`中的trace脚本，再重新转bmodel。
3. bmodel出图结果make sense，但与纯CPU上跑出的结果不完全一致。
    
    Solution：检查text encoder输出差异是否过大。C站上SD1.5的不同checkpoint，Unet和text encoder都不同，都需要重新trace和转换bmodel！另外，可比对F32、F16、BF16的输出差异。

todo：export_lcm里加上trace text encoder onnx，convert脚本加上text encoder转bmodel