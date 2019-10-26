# ENNEoS
VERY ROUGH PROOF OF CONCEPT
Work in progress. 

深度学习的大部分取决于网络的规模和复杂性。随着神经进化，DL体系结构（即网络拓扑、模块和超参数）可以在人类能力之外进行优化。

人工的神经网络是依靠梯度的反向传播来进行优化的，而在生物界中，神经网络中并没有指出优化方向的梯度，感知从下而上正向传播，之后相近的刺激带来共同激活的神经元，再用这些连接来对新事物编码及预测，而一切都依赖于进化机制。所谓神经进化，就是用遗传算法来进行神经网络的结构生成，参数更新及整体的效率优化。其基本的循环是突变->选择->繁衍->再突变。

两种神经进化的策略，一种是不固定网络的结构，通过神经网络间的交叉配对形成下一代的网络，另一组是固定结构，每一代网络中通过引入突变改变连接的强度，最终俩者都通过进化的优胜劣汰来实现神经网络的最优化。

不同于传统的随机梯度下降，是基于对现在错误来源的外推决定下一步进化的方向，即使引入了随机性，也只是在原有方向上引入高斯误差，是一种事后的弥补，而神经进化是通过在下一代中引入在算法空间中性质完全不同的点，之后根据适应度在这些点之间进行内推，虽然速度慢，但是可以更大规模的并行处理，且能够更好的避免陷入局部最优。

神经进化不止在监督学习中应用广泛，在深度强化学习中也有广泛的应用。Uber开发的开源工具Visual Inspector for Neuroevolution（VINE），可以用于神经演化的交互式数据可视化工具。而下文的作者之一也来自Uber的AI实验室。

作者首先指出了神经进化相比神经网络的几个独特的能力，包括通过学习找到合适的网络组成部分（例如激活函数），以及网络的超参数（有几层，每层有多少神经元）以及用于的学习策略本身。不同于AutoML的自动化调参，神经进化始终在搜索答案中保持着一个多样的解法“种群”，而且由于神经进化的研究和传统的神经网络并没有多少交集，因此俩者之间的汇总更容易擦出火花。

最初的神经进化关注小规模网络的拓扑结构的演化，最初的进化算法仅仅是通过（神经元）连接矩阵间的权重加上随机突变来展开，之后受到基因间调控网络的启发，对网络结构展开了间接的编码。随着引入在俩个网络结构中的杂交（crossover），神经进化可以探索更为复杂的网络结构，但需要面对如何避免让新生成的网络结构由于缺少足够的时间进行局部优化而无法发挥出其最优的性能，该方向上最显著的成果是NeuroEvolution of Augmenting Topologies (NEAT)算法，该算法的成果包括模拟机器人行走的控制程序，下图分别是使用遗传算法和进化策略训练模拟机器人走路（来自UberAI实验室Mujoco 人）.

在强化学习领域，natural evolutionary strategy可以在 Atari 游戏机上和Deep Q learning有相近的表现，而且这些算法的并行潜力使得这些算法在有足够计算资源时，可以用更快的时间完成训练，尽管神经进化需要的总的计算资源要多一些。神经进化在强化学习中的成功说明了神经进化方法可以用在现实中的复杂问题上。


Lehman将神经进化和梯度结合了起来。该方法的灵感来自是通过梯度去选择出那些不那么危险的突变。由于强化学习中评估一个策略的适应度需要花费的比评估网络本身要花费更多的资源，前者需要运行游戏或者模拟环境数回合，才能看到收益，而后者只需要去将网络中的错误项前向传播几步即可。神经进化中对策略（policy）加以随机的突变，部分突变不会影响策略的性能，但少部分会让该策略彻底失效。通过对状态和行为归档记录，可以通过梯度信息对变异的大小进行缩放，从而避免突变后的策略对于当前的状态过于激进或保守，从而使得在深度超过100层的网络上可以使用神经进化的策略。

神经进化可以模拟真实进化中对多样性和新奇策略的偏好，在要优化的目标中对全新的策略给予奖励，从而避免陷入局部最优，或者以策略种群的多样性为优化主要目标。在强化学习中，一个策略要想和其他策略不同，需要具有不同的基础能力，从而使策略种群多样性为优化目标好于人为设定的损失函数。

总结：神经进化在meta learning，多任务学习中都可以和现有方法结合。正如卷积操作就是一种编码信息的方式，神经进化还可以找到更好的对信息进行间接编码（Indirect coding）的方法以及通过进化策略重现出类似LSTM的网络结构。强化学习中的自我对弈可以看成是神经进化的一种，而对策略多样性的偏好也鼓励了模型对新策略的探索。最后，在通向通用人工智能的路上，神经进化通过构建开放目地的（open-endedness）的系统，让策略不带有先验目地的探索，模拟自然界的进化，最终得到一个足够普适的智能系统。


Current status: Regularly being broken by me trying to stuff multiple shellcodes into neural networks. 

Evolutionary Neural Network Encoder of Shenanigans

This proof of concept program is to explore the use of
neuroevolution as a method for encoding shellcode.
The encoder uses genetic algorithms
to evolve a neural network that will output the desired
shellcode at runtime.

Lots of code borrowed from:
https://github.com/hoodoer/NEAT_Thesis

Drew Kirkpatrick
@hoodoer
drew.kirkpatrick@gmail.com




The encoder uses genetic algorithms to evolve neural networks that output the shell code on demand. 
The shellcode to be encoded into the neural networks is included in the "encodedOutput.h" file (not a great name). 

You can generate one of these header files from raw shellcode binaries using:
https://github.com/hoodoer/shellcodeEncryptor

Just skip the encryption options, it'll output a header file with your shellcode. 

This is most definitely a work in progress, but the initial demo works,
popping a meterpreter shell in Windows from shellcode output by the neural network


