> java�����󲿷�����¾���jvm���������ִ��(��jit���ɻ�����Ͳ���Ӳ���ڽ�֧��),��������ڴ�����ܶ���������,��Ҳ���ǽ���ִ��,��ϵͳ���԰�ȫ��Ҫ����,���C/C++�����������Ӱ�����

> �ο����ϻ���Java SE 7 Edition

# һ. jvm�ڴ�ģ��
> ÿ��java����̾�Ϊһ��jvmʵ��    
* **java����ģ��**  
![�ڴ�ģ��](https://github.com/PeterLee08/resources/blob/master/icon/png/java/JVM.png?raw=true)

1. class loader     ��class�ļ����ص�method area 
2. Execution engine ִ��classָ��,ÿ���߳�һ��engineʵ��
3. Native interface �ͱ��ؿ��ļ�����,����ϵͳ����


* **hotspot ��heap����methodarea��ʵ��**:   
> Ϊ��Сgcɨ������,�����ֳ�����������������,������   
> ��������Ҫ��Ŵ���Ԫ��Ϣ,������Ϣ

![�ִ�ʵ��](https://github.com/PeterLee08/resources/blob/master/icon/png/java/fendai.PNG?raw=true)

> �����������ͼ(����ͼ��������):
![����](https://github.com/PeterLee08/resources/blob/master/icon/png/java/name.PNG?raw=true)

* ������(Young Generation)�����ڴ���´����Ķ��󣬲��ø��ƻ��շ����������s0��s1֮�临��һ��������ת�Ƶ����ϴ��С�������������ս���minor GC;
* ������(Old Generation)����Щ�����������յ�Ƶ�ʽϵͣ����õı����������������������ս��� major GC��
* ������(Permanent Generation)�����Java�����һЩ���ݣ����಻��ʹ��ʱ��Ҳ�ᱻ����

# ��. jvm��������
## 1). ��������
1. java ���ж���������԰��Eden space��������Ҳ����˵�����JAVA ��������ʱ����Ҫ�����µĶ���JVM ���ڸ���
Ϊ�㴴��һ��ָ���Ķ��󹩳���ʹ�á�������������ݼ������ô洢���е�Ԫ��
�ݡ� 
2. �� Eden �ռ䲻��ʱ�������������õĶ����ƶ���s0����������������
3. ��s0�ռ�Ҳ����ʱ���������ö�������ƶ���s1��.������������
4. �������ƶ���S1����,s0��ձ���, eden�ٴβ���ʱ,�ƶ���s1,s1����ʱ�ƶ���s0,��˷���, [S0��S1��С����ȫһ�¿��Ի���] 
4. �������ƶ���һ��������,�������Ķ����ƶ���������,����S0S1�ƶ��ռ䲻��ʱ,�����ö����ƶ���������
5. ���������ռ䲻��ʱ,�����������������ö���,���ɲ���,OOM

> ����ʱ��Ҳ������cpu����ʱ,������jvm���õĶ�ʱ����

## 2). ���ղ���
* �������Ļ�����ΪƵ��,Ҫ���յĶ����Ҫ���Ƶ���,���ø��ƻ��գ���Ϊminor GC��
* �����������������յ�Ƶ�ʽϵͣ�ÿ�λ��ն�ֻ������������, ���õı����������������������ս��� major GC


> Copying����:���������ڴ水��������Ϊ��С��ȵ����飬ÿ��ֻʹ�����е�һ�顣����һ����ڴ������ˣ��ͽ�������ŵĶ����Ƶ�����һ�����棬Ȼ���ٰ���ʹ�õ��ڴ�ռ�һ�������������һ���Ͳ����׳����ڴ���Ƭ������   
> Mark-Sweep�����-�����: ��ǽ׶κ�����׶Ρ���ǽ׶ε������Ǳ�ǳ�������Ҫ�����յĶ�������׶ξ��ǻ��ձ���ǵĶ�����ռ�õĿռ�  
> Mark-Compact�����-����:���֮�󣬽���������һ���ƶ���Ȼ��������˱߽�������ڴ�

## 3). HotSpot���������ռ���   
![gc](https://github.com/PeterLee08/resources/blob/master/icon/png/java/gc.png?raw=true)   
[�������: https://www.jianshu.com/p/b4a03b5de0d9](https://www.jianshu.com/p/b4a03b5de0d9)

## 4). �ִ��ռ�
JVM �����˷ִ��ռ��Ժ�minor gc ֻɨ��������������minor gc ��ô��
���Ƿ����������Ķ����������������Ķ���JVM �����˿�Ƭ��ǵĲ��ԣ���
Ƭ��ǽ��������ֳ���һ��һ��ģ������Ժ��ÿһ����ͽ���һ����Ƭ��JVM
���ÿ���ά����ÿһ�����״̬����JAVA �������е�ʱ�����������������
�����û����ͷ�����������������ã���ô��JVM �ͽ������״̬����Ϊ��״
̬������ÿ��minor gc ��ʱ��ͻ�ֻɨ�豻���Ϊ��״̬�Ŀ�Ƭ��������Ҫɨ��
�����ѡ���������ͼ��  
    
![beyond](https://github.com/PeterLee08/resources/blob/master/icon/png/java/beyond.png?raw=true)


## 5). ��ͬ���û��ղ���
4�в�ͬStrong reference,Soft reference,Weak reference,Phantom reference
* ǿ���� Strong reference: ƽ����������    
* ������ Soft reference, ֻ�����ڴ治���ʱ��JVM�Ż���ոö���,���������õ��ǲ��Ǳ���Ķ���
```java
SoftReference<Web> webRef = new SoftReference<Web>(web); 
if (webRef.get() != null){ 
    webr = webRef.get();
} else { //�����������ʹ��ǰ����һ��
    webRef = new SoftReference<Web>(new Web());
}

```
* ������Weak reference: һ��������ֻ���������õĶ���,������ '������������null����?'
* ������Phantom reference: ��Ӱ������ʱ��,ΨһӰ����ǻ���ʱ�յ�ϵͳ֪ͨ


# ��. jvm���Ų���
## 1. jvm ����ͼ��
![�ִ�ʵ��](https://github.com/PeterLee08/resources/blob/master/icon/png/java/jvm_generation.png?raw=true)

## 2. ����serverģʽ
## 3. ��������

���� | ���� | Ĭ��ֵ | ˵��
---|--- | --- | ---
-Xmn | ��������С |  | Sun�ٷ��Ƽ�����Ϊ�����ѵ�3/8
-XX:NewRatio |	���������������ı�ֵ(��ȥ�־ô�)||	 	-XX:NewRatio=4��ʾ����������ϴ���ռ��ֵΪ1:4,�����ռ������ջ��1/5 (Xms=Xmx����������Xmn������£��ò�������Ҫ��������)
-XX:SurvivorRatio |	Eden����Survivor���Ĵ�С��ֵ | |	 	����Ϊ8,������Survivor����һ��Eden���ı�ֵΪ2:8,һ��Survivor��ռ�����������1/10, �����ʱ�������,���ս��������survivor������
-XX:MaxTenuringThreshold |	�����������||	 	�������Ϊ0�Ļ�,����������󲻾���Survivor��,ֱ�ӽ������ϴ�. �������ϴ��Ƚ϶��Ӧ��,�������Ч��.�������ֵ����Ϊһ���ϴ�ֵ,��������������Survivor�����ж�θ���,�����������Ӷ�����������Ĵ�� ʱ��,������������������յĸ���,�ò���ֻ���ڴ���GCʱ����Ч.
-XX:PretenureSizeThreshold |	���󳬹������ֱ���ھ���������|	0	|��λ�ֽ� ����������Parallel Scavenge GCʱ��Ч ��һ��ֱ���ھ��������������Ǵ���������,�����������ⲿ���ö���.
-XX:+PrintGC |||�����ʽ:[GC 118250K->113543K(130112K), 0.0094143 secs][Full GC 121376K->10414K(130112K), 0.0650971 secs]
XX:+PrintGCDetails	|| |�����ʽ:[GC [DefNew: 8614K->781K(9088K), 0.0123035 secs] 118250K->113543K(130112K), 0.0124633 secs][GC [DefNew: 8614K->8614K(9088K), 0.0000665 secs][Tenured: 112761K->10414K(121024K), 0.0433488 secs] 121376K->10414K(130112K), 0.0436268 secs]
-XX:+PrintGCApplicationStoppedTime|	��ӡ���������ڼ������ͣ��ʱ��.����������ʹ��	 ||	�����ʽ:Total time for which application threads were stopped: 0.0468229 seconds
-XX:+PrintGCApplicationConcurrentTime |	��ӡÿ����������ǰ,����δ�жϵ�ִ��ʱ��.����������ʹ��||	 	�����ʽ:Application time: 0.5291524 seconds
-XX:+PrintHeapAtGC|	��ӡGCǰ�����ϸ��ջ��Ϣ|||	 	 
-Xloggc:filename |	�������־��Ϣ��¼���ļ��Ա����.||�����漸�����ʹ��
-XX:+PrintClassHistogram ||||

	 	 

# ��. jvm��ع���
������
jmap
jcmd
pmap
jps
jhat
top 


>�ܽ�
* �ȼ�����ж��Ӱ���������ռ��ۻ�
* ������������
* ����ʱ��Ϊnull
* �������һ��ֱ������������