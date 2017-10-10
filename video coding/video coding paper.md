#速率变换网络中的多参考帧编码 
Dual Frame Motion Compensation With Uneven Quality Assignment
##场景，问题，对象，变量，约束

网络传输过程中带宽速率降低，引起视频质量下降--延迟，帧丢失
* 变化多少，是否已知？此处假设变化已知

**运动补偿，参考帧，帧之间的依赖性！单一依赖，多重依赖，主要依赖于！**

通过多参考帧提高补偿准确性
* 多参考帧：
    * STR, short term reference
    * LTR, long term reference
* 区别对待
    低带宽时周期构建高质量LTR--需要额外bits,带宽，时间
    
实际场景：
* 简单的带宽变化场景
* 不均匀分配时，对问题的解决：
    * 固定带宽分出一部分来传输LTR
    * cognitive radio自动识别未使用部分，短暂借用
    * 直接传输，利用缓存机制减轻时延问题

结论：  
1. 多参考帧遇到rate switching，重新选取选取STR过程一样，但是LTR短时间内质量较高，所以短期内视频质量相比单参考帧上升，长期类似  
2. **不均匀分配的多参考帧**效果好于**常规多参考帧**和**增强质量的单参考帧（plused quality single fram）**  
3. rate switching network中，多参考帧可以平滑场景转换带来的问题 





#264新标准中QP与stepsize关系变化应对及CM,RC,QP鸡蛋问题解决
Rate-distortion analysis for H.264/AVC video coding and its application to rate control

##场景，问题，对象，变量，约束
问题：

1. 264中的quantization scheme使得quantization parameter和true quantization stepsize间保持非线性关系 
2. 鸡蛋问题
	需要已知MV,编码模式来选择量化参数，然而RDO顺序不同（帧，宏块？），并且需要根据量化参数来选择MV,编码模式

RD model，量化参数，量化步长，buffer


##贡献
1. 提出新的RD模型，表示非线性QP和真实步长之间的关系
	基于统计
2. 改进码率控制scheme

	1. frame level one pass, MB level partial two pass但可通过编码参数控制，所以可以实现one pass--完全one pass为一个子集
	2. 融入video buffering，根据H264加入的hypothetical reference decoder（HRD），满足buffer不上溢下溢的条件

	迭代两次，第一次，设定QP，根据MB信息得到最优CM,RC，然后根据CM,RC更新QP，两次的RD cost比较，如果后者高，根据阈值选定部分MB再次更新QP

#SVC中取舍STAR组合问题，rate和perceptual quality模型构建
modeling of rate and perceptual quality of compressed video as functions of frame rate and quantization stepsize and its applications

##场景，问题，对象，变量，约束
具体编码过程，输出码率bit rate受几个因素影响：

	spatial resolution--frame size
	temporal resolution--frame rate
	amplitude resolution--SNR，由QS,QP决定
	STAR

有不同组合，均要满足BR条件，但是编码结果(感知结果)不一样：
	
	noticeable coding artifacts--SR++,TR++,QS,QP++
	high quality--SR--,TR--, QP--

一般经验：控制变量法

	固定TR,SR
	选QP
	不能满足BS，加入skip

	或：
	联合选取QP和skip，以经验规则或MSE控制

考虑接收端，变化多，根据接收端rate，选择scalable stream中特定的组合--提取特定layer   
如何取舍？--video adaption

##贡献
构建两个模型，rate，perceptual quality，变量均为frame rate，QP   
RD model编程QR model

模型构建思路：**控制变量补偿法**

	rate:FR最高时，基于不同QP的范数关系，乘上实际FR的纠正系数(temporal correction)
	quality 同理，基于解码后帧的PSNR,乘上补偿项

参数：场景依赖
利用源视频3个特征，线性组合得到（3+2）

应用场景：
rate：scalable，nonscalable
quality：scalable

扩展：
	
	加入SR因素
	验证quality在nonscalable下的实用性
	对不稳定序列，使用滑动窗口或scene change detection改进内容变化场景下的取舍
