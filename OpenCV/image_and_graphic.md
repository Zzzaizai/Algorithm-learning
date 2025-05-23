基础操作
- 读取、显示、保存图/视频。
- resize、crop、rotate、translate。
- 标注、色彩空间、阈值、blob。
- 卷积滤波。

边缘检测
- Sobel算子提取边缘分为x方向和y方向，其原理是使用卷积核对图像x或y方向强度求梯度。
- Canny边缘检测有四个过程，首先进行高斯滤波；然后使用Sobel算子进行水平和垂直方向的边缘提取，并将其梯度方向归到最接近的45°中；之后对检测到的边缘进行非极大值抑制，即对于某个边缘像素点，在梯度方向上取相邻两个像素的梯度值，若该像素的梯度是三者中最大的，就保留，否则置0；最后进行滞后阈值操作，即存在高阈值与低阈值，若梯度大于高阈值为强边缘，若梯度小于低阈值则删除，两者之间为弱边缘，当弱边缘与强边缘相连时，弱边缘才会保留。

鼠标与跟踪栏
- 定义一个函数，当鼠标有动作时执行该函数；使用setMouseCallback当鼠标有动作时回调函数，可实现鼠标位置的确定。
- createTrackbar回调执行函数可获取跟踪栏中的信息。

轮廓检测
- 主要分为四步，首先转为灰度图，然后进行二值化或Canny边缘检测，之后findContours查找等值线，最后在原图中drawContours绘制等值线。

视频背景估计
- 对于固定相机，取视频的一些帧计算每个像素的中位数，要每个像素至少有 50% 的时间看到背景，这个中位数帧就是对背景的良好估计。
- 帧差分，将上述中间帧转为灰度，循环播放视频所有帧，提取当前帧并转灰度，计算当前帧与中间帧差值，并进行阈值处理输出二值化。

平移旋转矩阵
- 旋转矩阵为正交矩阵，det(R)=1。
- 平移向量与坐标之间为单纯的相加。
- 为将平移矩阵与旋转向量统一到一起，引入齐次变换矩阵与齐次坐标，二维齐次变换矩阵为3x3，三维的为4x4。

正交投影/透视投影
- 正交投影假设视线是平行的，如远心镜头成像。
- 透视投影认为光线汇聚于一点（光心），近大远小。

cv::Mat访问
- .at<T>(row, col)访问像素值，安全，有强类型检查，但速度慢。
- .ptr<T>(row)[col]访问像素值，性能好，需手动确保合法。
- .data访问底层数据，不安全但快速。

色彩空间
- BGR/RGB，GRAY。
- HSV：色调，饱和度，明度。接近人类感知，常用于颜色分割。
- LAB：明度，绿红色轴，蓝黄色轴。均匀性好，适合图像增强与比较。
- YCrCb：亮度，红色差分，蓝色差分。亮度与颜色完全分离，常用于视频压缩，人脸检测。
- HLS：类似HSV，用亮度代替明度。

图像金字塔/插值
- 高斯金字塔，通过高斯滤波与下采样实现，每层尺寸相差一倍。用于图像搜索，特征检测等。
- 拉普拉斯金字塔，每层是当前图像与下一层图像上采样后的差值（高频信息）。用于压缩与融合等，可还原。
- 最近邻差值，取目标最近的一个像素值，出现马赛克与锯齿。
- 双线性插值，最近四个像素加权平均，平滑但边缘模糊。
- 双三次差值，周围16个像素三次插值计算，最平滑但速度慢。

图像融合/透明度融合
- 取权重，将两个图片相加即可。

LUT映射
- 常用于滤镜、伽马校正、色调映射等。
- 灰度图lut为1x256向量，色彩图为3x256矩阵。
- cv::LUT(color_img, lut, result);即可实现lut色彩调整，通过自定义lut可实现各种色彩变换。
- 计算快，实时性强，但不适合处理需邻域依赖的操作。

图像GPU->CPU
- opencv提供GPU图像表示，cv::cuda::GpuMat gpuimg，与cv::Mat cpuimg不同，需手动同步，gpuimg.download(cpuimg)。
- cpu::cuda中有许多封装好的函数。

高/低通滤波
- 低频为图像中变化平缓的区域，低通允许低频信号通过，平滑图像，去噪与模糊细节等。
- 高通为图像中变化剧烈的区域，高通允许高频信号通过，突出细节，增强边缘等。

Alpha Blending
- 根据透明度将前景与背景按权重融合。
- 可用于透明过渡，水印、平滑融合特效等。使用addWeighted或add函数实现。

图片特效实现
- 老照片风格，本质是对颜色进行线性变换，左乘一个3x3矩阵。
- 卡通画风格，本质是对颜色块区域平滑，保留大色块并突出线条。

人脸检测
- 快速人脸检测，如Haar、DNN等；Dlib68点关键点检测等。
- 在人脸上叠加特效，可根据人脸部位的关键点求变换矩阵，在通过仿射变换叠加，并根据人脸的姿态变换同步做透视或仿射变换。

OpenGL坐标系及MVP矩阵
- 主要有局部坐标系、世界坐标系、观察坐标系、裁剪坐标系、屏幕坐标系。v′ = Projection ∗ View ∗ Model ∗ v。
- M为模型矩阵，从局部坐标到世界坐标。包括缩放S、旋转R、平移T。M = T * R * S。
- V为视图矩阵，从世界坐标到相机坐标。三个参数：相机位姿、目标点、向上向量。
- P为投影矩阵，从相机坐标到裁剪空间（屏幕坐标？）。四个参数：Y视场角、宽高比即远近裁剪面。
- 矩阵是作用在列向量的左边，且乘法不能交换。

渲染管线
- 渲染管线指的是把3D场景数据（顶点、纹理材质、光照等）通过一系列连续处理转换为2D图像输出到屏幕上的过程。有固定管线和可编程管线。
- 第一步：顶点处理，首先提交顶点数据（位置、法线、纹理坐标等）到GPU，绑定缓存区，然后对每个顶点单独着色处理，进行局部坐标->世界坐标->相机坐标->裁剪坐标变换，计算顶点颜色、进行法线变换等。之后可选择性的进行曲面细分与几何着色。最后进行图元组装与屏幕映射，即把顶点组成点线面等，提交给裁剪测试，剔除视野外的部分，再转换到屏幕坐标系中。
- 第二步：光栅化，把连续的三角形离散为像素片段，每个片段对应屏幕上的一个或多个像素。
- 第三步：片段处理，单独为每个片段着色，计算颜色、纹理采样、光照计算、阴影贴图等，这是处理图像真实感的关键阶段。
- 第四步：测试与输出，确定图层顺序，实现各种特殊效果，再把经测试的片段颜色写入缓冲区，经缓冲区将图像输出到屏幕。
- 现在渲染管线引入了更多高阶模块，如计算着色器（Compute Shader）用于通用GPU计算，延迟渲染（Deferred Rendering）解决复杂光照瓶颈，光线追踪（Ray Tracing Pipeline）未来趋势（如RTX），可变速率着色（Variable Rate Shading），动态细分与LOD（Level of Detail），多重渲染目标（MRT），基于物理的渲染（PBR，Physically Based Rendering）都是为在性能与真实感之间平衡。

纹理采样
- 指将纹理图像应用到3D模型表面的过程，从纹理图像中提取颜色信息映射到渲染的表面上。
- 点过滤，选择最近的一个像素作为采样结果，计算量小但效果差。
- 双线性过滤，根据四个相邻像素进行加权平均得到纹理颜色，更平滑；三线性差值，还考虑了不同纹理层之间的差值。
- 各向异性过滤，考虑不同视角与表面方向，进行更加精细的纹理采样。
- Mipmap，对纹理图像多级处理，根据物体远近动态选择合适分辨率的纹理。

阴影贴图
- 渲染过程中决定哪些部分被阴影遮蔽等。
- 基本原理为选择光源为观察视角，将场景渲染为深度图，深度值代表光源到物体表面距离；渲染时使用深度值与阴影贴图中的深度值进行比较来决定那部分该处于阴影中。
- 关键步骤：深度图的生成，阴影查找。

PBR
- 基于物理的渲染，遵循真实物理规律（能量守恒，微观光照），更准确描述物体与光线的交互过程，以达成更换光源材质表现也一致自然。
- 表面反射模型，由漫反射与镜面反射组成，菲涅尔效应是重点。
- 输入材质参数，金属感决定反射特性，粗糙度决定高光锐度。
- 环境光照，真实世界不仅被直接光照射，还被周围环境反射光包围。使用环境贴图模拟。

RTX
- 实时光线追踪，传统光栅化渲染速度快但真实感受限，光线追踪自然产生真是阴影与折反射，直接模拟光与物体交互，但极度耗费算力。
- 实施光追流程：对每个像素发射主射线->判断射线与物体是否相交并根据材质计算直接光照间接光照->发射次级射线，逐步累积光照贡献->最终合成输出到屏幕。
- DLSS深度学习超采样，使用AI超分辨奇数，由低分辨率渲染超分到高分辨率，保持细节的同时提高帧率。没有DLSS很多游戏的RTX是跑不动的。

Compute Shader
- 专门用于通用计算的着色器，可自由处理数据，而不受限于流程（顶点->光栅化->片段着色）。
- 可用于物理仿真、模糊去噪特效等后处理、光线追踪、全局光照计算等。

点乘/叉乘
- 点乘结果为标量，如求光线夹角，判断方向一致性，向量在某方向的投影等。
- 叉乘结果是向量，如计算平面法向量，构建局部坐标系，计算三角形面积等。

Z-fighting
- 指的是深度冲突，多个片元深度非常接近，导致光栅化阶段无法决定前后，渲染结果出现闪烁等瑕疵。根本原因在于深度分辨率有限（如24bit），深度经过透视变换后非线性分布，越远精度越差。
- 透视投影下Z值近处密远处疏，可调大近平面位置。
- 也可手动给物体深度增加微小位移，或修改绘制顺序等。
- 使用更高精度浮点型，或颠倒深度映射方向等。

Blinn-Phong/Phong
- Blinn-Phong模型是对经典Phong光照模型的改进，属于经验型光照模型。
- 主要考虑环境光、漫反射、镜面反射三种光照成分。环境光为常量，与方向无关；漫反射基于朗伯定律，与入射角成正比。
- 镜面反射是区别于Phong模型的主要区别，使用半程向量近似镜面方向。
- Blinn-Phong模型牺牲了极小的物理准确性，换取了巨大的性能优势，适用于实时渲染。

音视频同步
- FIFO同步队列，音视频数据以队列形式存储，当视频帧与音频帧时间戳匹配时播放对应帧。
- 当音视频差异较大时，使用丢帧或插帧的方法恢复同步。

glFlush/glFinish
- glFlush将当前命令推到GPU上执行，但不保证这些命令已完成执行。不会阻塞当前程序进行，CPU仍可以处理后续代码。
- glFinish会阻塞当前程序，直到GPU完成所有提交的命令。






















