---
layout: default
title:# 以Construct 2制作平台游戏的新手指南
---

# 以Construct 2制作平台游戏的新手指南

## 平台游戏的制作
   首先，在网址https://www.scirra.com/construct2中获取最新版本的Construct 2的副本。
   安装完毕后打开construct 2。在左上角的file中选择New点击。然后会出现如图所示：<img src="https://www.scirra.com/images/articles/newprojdialog65.png" />
   然后，双击布局中的空格或者鼠标右键点击选择insert new object以插入新对象。首先插入平铺背景，如图所示：<img scr="https://www.scirra.com/images/articles/insertobject.png" />然后调整合适的布局，使其平铺整个布局。
   接着管理图层，单击“ 图层”选项卡，该选项卡通常位于“ 项目”栏旁边：<img src="https://www.scirra.com/images/articles/layerstab.png" />单击铅笔图标将第0层重命名为Background。然后添加一个新图层。锁定Background以使添加新对象更加方便。
   接着，按照类似做法选择sprite以分别插入玩家，怪物，子弹和爆炸。通过选择对象，然后更改属性栏中的Name属性来将它们重命名为Player，Monster，Bullet和Explosion。<img src="https://www.scirra.com/images/articles/objectname.png" />
   然后为玩家添加8方向移动，Scroll To等行为。具体操作如下：<img scr="https://www.scirra.com/images/articles/openbehaviors.png" /><img scr="https://www.scirra.com/images/articles/add8dir.png" />
   用同样方法将Bullet移动和Destroy外部布局添加到Bullet对象，将子弹移动添加到Monster对象（因为它也向前移动），将Fade行为添加到Explosion对象（因此它逐渐消失）。将Monster对象速度从400更改为80，Bullet对象速度从400改为600.如图所示<img src="https://www.scirra.com/images/articles/bulletproperties.png" />
   然后使用control + drag，创建7或8个新怪物。
   接下来通过Construct 2的可视化编程方法 - 事件系统来添加我们的自定义功能。
   首先，单击顶部的“ 事件表1”选项卡以切换到“ 事件”表编辑器。事件列表称为事件表，可以为游戏的不同部分或组织设置不同的事件表。
   双击事件表中的空格，出现<img src="https://www.scirra.com/images/articles/newevent_2.png" />然后<img src="https://www.scirra.com/images/articles/everytickcnd.png" />单击事件右侧的“ 添加操作”链接，添加Player。然后选择Set angle toward position.如图所示<img src="https://www.scirra.com/images/articles/addactiondlg.png" />接着将角度设置为鼠标位置，输入Mouse.X为X，和Mouse.Y为ÿ。完成后如图所示<img src="https://www.scirra.com/images/articles/alwayslookatmouse.png" />
   用同样方法得到如下图所示的结果<img src="https://www.scirra.com/images/articles/spawnbullet2.png" />
   让子弹杀死怪物。添加以下事件：条件：子弹 - > 与另一个物体碰撞 - >选择怪物;动作：怪物 - > 摧毁;动作：子弹 - >生成另一个对象 - > 爆炸，第1层;动作：子弹 - > 摧毁；条件：系统 - > 开始布局；动作：怪物 - > 设置角度 - >随机（360）；条件：怪物 - > 外部布局；动作：怪物 - > 设置角度朝向位置 - >对于X，Player.X - 对于Y，Player.Y。
   单击右下角的对象栏中的Explosion对象，或单击项目栏（带有图层栏的选项卡）。其属性显示在左侧的属性栏中。在底部，将其Blend mode属性设置为Additive以消除爆炸黑框。
   然后用实例变量设置在它死前五次射击怪物。点击添加/编辑通过编辑变量。步骤如下图所示<img src="https://www.scirra.com/images/articles/instvars.png" /> <img src="https://www.scirra.com/images/articles/healthinstvar.png" />
   找到事件：Bullet - 与Monster碰撞。用“从健康中减去1”来代替“摧毁怪物”动作。右键单击“destroy monster”操作，然后单击“ 替换”。选择Monster - > Subtract from（在Instance variables category中）- > Instance变量“health”，并为Value输入1。单击完成。
   再次添加事件：条件：怪物 - >比较实例变量 - >健康，更少或相等，0 ；动作：怪物 - >生成另一个对象 - >爆炸，第1层；动作：怪物 - >摧毁。
   最后，右键单击事件表底部的空间，然后选择“ 添加全局变量”。输入Score作为名称。其他字段默认值为OK，它将使其成为从0开始的数字。给玩家一个杀死怪物的点。在“Monster：health less or equal 0”事件中（当怪物死亡时），单击Add action，然后选择System - > Add to（在Global和local variables下） - > Score，value 1。
   为了更好的游戏体验，从文本对象中制作一个非常简单的HUD。回到之前使用的图层栏。添加一个名为HUD的新图层。确保它位于顶部并选中（记住这使它成为活动层）。属性栏现在应该显示其属性。将Parallax属性设置为0,0（在X和Y轴上均为零）。双击空格以插入另一个对象。这次选择Text对象。将其放在布局的左上角。切换回活动表。让用播放器的分数更新文本。在之前添加的Every tick事件中，添加操作Text - > Set text。对于文本，输入"Score: " & Score。
   为使游戏更加丰富，添加事件：条件：系统 - > 每X秒 - > 3；动作：系统 - > 创建对象 - > 怪物，第1层，1400（对于X），随机（1024）（对于Y）。条件：怪物 - > 与另一个物体碰撞 - > 玩家；动作：玩家 - > 摧毁。
   至此，游戏制作完成。