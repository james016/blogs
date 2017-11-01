# matlab eig 矢量化

问题是现在有 N 个 8x8 的矩阵需要对角化，N 很大，希望能通过类似矢量化的方法提速，而无需使用 for 循环求解。

已经找到相关方案是
cn.mathworks.com/matlabcentral/answers/84283-vectorizing-calculation-of-eigen-values-of-a-large-multi-dimensional-array
但这里面只能解决 2x2 和 3x3，思想是解析求解出结果，然后做张量计算得到。

因为在传统 matlab 中实现矢量化有针对标量函数，而像 eig 这类函数都没有矢量化方案。
见 cn.mathworks.com/help/matlab/matlab_prog/vectorization.html

所以，不知道有没有什么方法可以提速上面的对角化过程？

一种想法，可以使用 mex 编程，在底层用 c 实现以提速，不知道过去有没有人做过类似的事？

# 解决方案

找到已有的程序 ndfun

运行 mex ndfun.c -llapack -lblas 编译

然后，就可以运行矢量化的 eig，但实际速度提升并不是太大。虽然 For 可以提升 7倍左右的速度，但是 matlab 自带的 eig 比 lapack （也包括 mwlapack ） 中的 dgeev 要快，当系统矩阵大小增加一倍时，相对速度要比原来快一倍。当系统尺度在 8x8 时，两种速度基本相同。

www.mit.edu/~pwb/matlab/