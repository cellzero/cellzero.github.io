# 编写Unity着色器踩过的坑

## 内置规则

1. 颜色范围的取值为[0.0, 1.0]，故颜色相乘带有乘法衰减的一味。如`_LightColor.rgb * difLight`就表示了光照颜色，在余弦夹角下的衰减。
  _TODO：颜色取值超过1.0，或者小于0.0会怎么样？_
2. 贴图的坐标。贴图的uv值也是[0.0, 1.0]的值，分别标志了x、y方向的百分比，x方向为由左到右，y方向为**由下到上**。
  故(0.1, 0.1)对应贴图左下角，(0.9, 0.9)对应贴图右上角。
  _TODO：纹理的uv超过取值范围会怎样？自动回滚吗？_
3. 输入结构Input，其uv贴图坐标的命名，必须同Properties一致（uv_Properties名）。
  即名为_DiffuseTexture的纹理，其Input坐标变量一定为uv_DiffuseTexture。
4. 编写光照模型时，GI（Global Illumination）类型的光照，方法需声明为`inline fixed4 Lighting<Name>(SurfaceOutput s, UnityGI gi)`。
  同时，需要额外编写GI方法，其声明及一种简单实现为：
  ```HLSL
  inline void Lighting<Name>_GI(SurfaceOutput s, UnityGIInput data, inout UnityGI gi) {
    gi = UnityGlobalIllumination(data, 1.0, s.Normal);
  }
  ```
  _TODO：具体细节不明，有待进一步学习。_
  
## 脑残引发的“血案”

1. 在编写自定义光照模型时，将lightDir类型写错，正确`fixed3`，错误`fixed`。  
  症状：光照表现异常奇怪，物体在直射自身的方向光下，依然漆黑一片。调整光照角度，物体的明暗会发生变化，但与预期不符。  
  _TODO1：添加描述图片。  
  _TODO2：解释错写为`fixed`时，着色器进行了怎样的处理，进而产生了异常的效果。（较麻烦，随时准备弃坑。）  
