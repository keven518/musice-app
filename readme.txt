��ʦ��https://github.com/ustbhuangyi?page=1&tab=repositories
3-2·������+�����������
redirect  vue-router ���ض���-redirect
https://www.cnblogs.com/jinsuo/p/8287819.html

4-4 �ֲ����ʵ�֣��ϣ�
moutedִ��ʱ��created�����ݿ���û�л�ȡ��
created ��ʱ����Ȼ�󿴿�mouted��û��ִ��

4-10 SCROLL����ĳ����Ӧ�ã��ϣ�
1��ȷ��ҳ��dom����Ⱦ����
mounted(){
    setTimeout(() => {

    }, 20)
  },
2���ڳ�ʼ��new BScroll() ���this.$refs.wrapper Ϊundefined�Ļᱨ��
3���˽�this.scroll && this.scroll.disable() �﷨
4��<Scroll class="recommend-content"> ��ʱmounted��this.initScroll()�����ݻ�ȡǰ��ִ�У����Թ�����
5��<img @load

�ܽ᣺BScrollʲôʱ������Ҫ���¼��㣨scroll.refresh���ġ�������Ⱦԭ���ǣ����ݳ�ʼ��ʱ�ڣ������ݱ��ʱ
�ڼ��㸸Ԫ�ظ߶Ⱥ���Ԫ�ظ߶�֮�������㣬�������Թ�����λ�á�ʵ���������refreshʱһ��Ҫ��֤dom����
Ⱦ�õ��ܼ���߶ȵġ�


5-3���������ݴ����inger��ķ�װ
1��ͨ��list.forEach ������ͬ��key���ϵ�һ�����������ͬ������
2��for(let key in map) ����ÿ������
3����ĸ����
4��hot.concat(ret) �ϲ���������