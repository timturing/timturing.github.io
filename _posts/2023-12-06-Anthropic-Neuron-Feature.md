[link](https://transformer-circuits.pub/2023/monosemantic-features/index.html)

对每一个MLP内部的一系列的activation值所构成的activation vector进行分解，分解方法是构造了一个新的auto encoder，根据他们的假设，现在的MLP是对真实世界的低位投影，所以他们的auto encoder内部的维数比输入还要高，而且由于现在的维度是较低的，所以会出现知识重叠（即一个神经元代表多个特征）。现在把所有MLP内部的activation vector拿出来，扔进auto encoder训练可以得到一个训练好的auto encoder。

那么什么是分解结果呢？分解结果就是每一个activation vector = auto encoder'decoder matrix * auto encoder activation vector ，也就是每一个activation的值都当作权重，每一组边权就是一个特征（估计是拿这个同维度的特征拿去做了token embedding映射然后得出到底代表什么，这部分没有看到详细说明）

而上述这种方式与字典学习很像，为了保证每一个原始activation vector映射后的结果足够稀疏，所以在loss function加上了auto encoder中间激活值的个数，这也就是为什么叫sparse auto encoder。同样的，他们跑的时候要调reconstruct loss和这个loss的权重超参数，也就有了A/1/1122中间这个不同次数的来源。