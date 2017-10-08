#速率变换网络中的多参考帧编码 
Dual Frame Motion Compensation With Uneven Quality Assignment
##场景，问题，对象，变量

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



SVC：
解决问题：
波动码率的网络中视频质量提升

* spatially scalable,resolutioin-scalable
    * 更小的图片尺寸
    * 编码不知道终端的显示能力
* temporally scalable： 
    * 低码率
    * 编码器不知道带宽/终端的complexity capacity
* quality scalable，SNR-scalable： 
    * base layer: coding modes, motion vector
    * enhancement layer: DCT residuals 


一般做法：
编码器预编码
终端选择性的下载，减少大小，减少码率版本