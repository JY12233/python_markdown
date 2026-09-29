# 总体介绍

SciPy 是一个基于 NumPy 构建的开源 Python 科学计算库，提供数学、科学和工程领域的高级算法与工具集合，通常通过 `pip install scipy` 安装后按需导入子模块使用（如 `from scipy import optimize`），当需要执行优化求根、数值积分、统计分析、信号处理、线性代数、插值、图像处理等超出 NumPy 基础数组运算范畴的复杂科学计算任务时，就应当使用 SciPy。

## 功能作用

SciPy 的核心功能按子模块划分如下：

- 优化与求根（`scipy.optimize`）：函数最小化、非线性方程求根、曲线拟合、约束优化等。
- 数值积分与微分方程（`scipy.integrate`）：单重/多重定积分计算、常微分方程初值问题求解。
- 插值与平滑（`scipy.interpolate`）：一维/二维插值、样条插值、径向基函数插值。
- 线性代数（`scipy.linalg`）：矩阵求逆、行列式、特征值分解、LU/QR/SVD 分解、线性方程组求解。
- 统计分析（`scipy.stats`）：概率分布（PDF/CDF/PPF）、假设检验（t检验、卡方检验等）、描述统计、随机数生成、核密度估计。
- 信号处理（`scipy.signal`）：滤波器设计、信号滤波、卷积、频谱分析。
- 傅里叶变换（`scipy.fft`）：快速傅里叶变换及其逆变换。
- 图像处理（`scipy.ndimage`）：多维图像滤波、形态学操作（腐蚀/膨胀）、几何变换。
- 稀疏矩阵（`scipy.sparse`）：压缩稀疏矩阵存储与运算、稀疏图算法（`scipy.sparse.csgraph`）。
- 特殊函数（`scipy.special`）：贝塞尔函数、伽马函数、误差函数等特殊数学函数。
- 空间算法（`scipy.spatial`）：Kd树、Delaunay三角剖分、距离计算、凸包。
- 聚类算法（`scipy.cluster`）：层次聚类、向量量化。
- 常数与单位（`scipy.constants`）：物理/数学常数、单位换算。
- 文件IO（`scipy.io`）：读写 MATLAB、Fortran 等格式文件。
- 正交距离回归（`scipy.odr`）：处理自变量和因变量均有误差的回归问题。
- 有限差分求导（`scipy.differentiate`）：数值微分工具。

## 子模块与类

SciPy 本身不是一个类，而是一个由多个子包组成的库。

主要子模块包括：`scipy.cluster`、`scipy.constants`、`scipy.differentiate`、`scipy.fft`、`scipy.fftpack`（遗留）、`scipy.integrate`、`scipy.interpolate`、`scipy.io`、`scipy.linalg`、`scipy.ndimage`、`scipy.odr`、`scipy.optimize`、`scipy.signal`、`scipy.sparse`、`scipy.spatial`、`scipy.special`、`scipy.stats`。

各子模块内部包含大量函数和类，例如 `scipy.stats` 中有 `norm`、`t`、`chi2` 等分布类，`scipy.interpolate` 中有 `interp1d`、`CubicSpline` 等插值类，`scipy.optimize` 中有 `minimize`、`curve_fit`、`root` 等函数。

## 常用属性

| 属性                  | 说明                                   |
| --------------------- | -------------------------------------- |
| `scipy.__version__`   | 返回当前安装的 SciPy 版本号            |
| `scipy.__doc__`       | 返回 SciPy 库的文档字符串              |
| `scipy.show_config()` | 显示编译配置信息（BLAS/LAPACK 后端等） |

## 常用函数与方法

| 子模块                | 常用函数/类                                                  | 说明                                 |
| --------------------- | ------------------------------------------------------------ | ------------------------------------ |
| `scipy.optimize`      | `minimize()`、<br />`fsolve()`、<br />`curve_fit()`、<br />`root()`、<br />`leastsq()` | 函数优化、方程求根、曲线拟合         |
| `scipy.integrate`     | `quad()`、<br />`dblquad()`、<br />`solve_ivp()`、<br />`odeint()` | 数值积分、常微分方程求解             |
| `scipy.interpolate`   | `interp1d()`、<br />`interp2d()`、<br />`CubicSpline()`、<br />`RBFInterpolator()` | 一维/二维插值、样条插值              |
| `scipy.linalg`        | `inv()`、<br />`det()`、<br />`eig()`、<br />`solve()`、<br />`lu()`、<br />`svd()` | 矩阵求逆、行列式、特征值、方程求解   |
| `scipy.stats`         | `norm`、<br />`t`、<br />`ttest_ind()`、<br />`ttest_1samp()`、<br />`describe()`、<br />`gaussian_kde()` | 概率分布、假设检验、描述统计         |
| `scipy.signal`        | `butter()`、<br />`filtfilt()`、<br />`lfilter()`、<br />`convolve()`、<br />`find_peaks()` | 滤波器设计、信号滤波、卷积、峰值检测 |
| `scipy.fft`           | `fft()`、<br />`ifft()`、<br />`rfft()`、<br />`irfft()`       | 快速傅里叶变换及逆变换               |
| `scipy.ndimage`       | `gaussian_filter()`、<br />`binary_erosion()`、<br />`binary_dilation()`、<br />`rotate()` | 图像滤波、形态学操作、旋转           |
| `scipy.sparse`        | `csr_matrix()`、<br />`csc_matrix()`、<br />`lil_matrix()`     | 稀疏矩阵创建与转换                   |
| `scipy.spatial`       | `KDTree()`、<br />`Delaunay()`、<br />`ConvexHull()`、<br />`distance.euclidean()` | 空间数据结构、距离计算               |
| `scipy.constants`     | `pi`、<br />`c`、<br />`G`、<br />`convert_temperature()`      | 物理/数学常数、单位换算              |
| `scipy.io`            | `loadmat()`、<br />`savemat()`                                 | 读写 MATLAB `.mat` 文件              |
| `scipy.special`       | `gamma()`、<br />`beta()`、<br />`erf()`、<br />`j0()`         | 特殊数学函数                         |
| `scipy.cluster`       | `hierarchy.linkage()`、<br />`hierarchy.dendrogram()`、<br />`vq.kmeans()` | 层次聚类、K-Means 向量量化           |
| `scipy.odr`           | `Model()`、<br />`Data()`、<br />`ODR()`                       | 正交距离回归                         |
| `scipy.differentiate` | `derivative()`                                                 | 有限差分求导                         |

# stats 模块

`scipy.stats` 是 SciPy 库中专门用于**统计学**计算的子模块，通常通过 `from scipy import stats` 或 `import scipy.stats as stats` 导入使用，当需要进行概率分布建模、描述性统计、假设检验、相关性分析、核密度估计、随机数生成等统计分析任务时，就应当使用该模块。

## 功能作用

`scipy.stats` 的核心功能涵盖以下方面：

1. 概率分布

   提供 100 多个连续分布（如正态分布 `norm`、均匀分布 `uniform`、指数分布 `expon`）和 20 多个离散分布（如二项分布 `binom`、泊松分布 `poisson`、伯努利分布 `bernoulli`），每个分布均支持计算概率密度函数、累积分布函数、生成随机样本等操作。

2. 描述性统计

   计算数据的均值、中位数、众数、方差、标准差、偏度、峰度，以及通过 `describe()` 返回综合统计摘要。

3. 假设检验

   支持单样本 t 检验、独立样本 t 检验、配对样本 t 检验、单因素方差分析、卡方检验、K-S 检验、Mann-Whitney U 检验等。

4. 相关性与回归

   皮尔逊相关系数、斯皮尔曼秩相关、肯德尔秩相关、线性回归（`linregress()`）。

5. 核密度估计

   通过 `gaussian_kde()` 对数据进行非参数概率密度估计。

6. 频率统计

   累积频率直方图、百分位数、四分位数、截尾均值等。

7. 掩码数组统计

   `scipy.stats.mstats` 子模块提供对含缺失值（masked array）数据的统计函数。

8. 准蒙特卡洛采样

   提供低差异序列（如 Sobol 序列）用于准蒙特卡洛积分。

## 子模块与类

`scipy.stats` 内部包含以下主要子模块和基类：

1. 基类

   `rv_continuous`（连续随机变量基类）、`rv_discrete`（离散随机变量基类）、`rv_histogram`（基于直方图构造分布）。

2. 新基础设施类（SciPy 1.15+）

   驼峰命名的分布类，如 `Normal`、`Uniform`、`Exponential`、`Binomial` 等。

3. 子模块

   `scipy.stats.mstats`（掩码数组统计）、`scipy.stats.qmc`（准蒙特卡洛）、`scipy.stats.contingency`（列联表分析）、`scipy.stats.sampling`（自定义随机变量采样）。

4. 预定义分布对象

   `norm`、`t`、`chi2`、`f`、`beta`、`gamma`、`expon`、`uniform`、`binom`、`poisson`、`bernoulli`、`geom`、`hypergeom` 等 100 余个分布。

## 常用属性

| 属性                      | 说明                                       |
| ------------------------- | ------------------------------------------ |
| `scipy.stats.__doc__`     | 返回模块文档字符串，包含可用分布和函数列表 |
| `scipy.stats.__all__`     | 列出模块公开导出的所有名称                 |
| `scipy.stats.__version__` | 返回当前 SciPy 版本号（部分版本可用）      |

## 常用函数与方法


| 类别 | 函数/方法 | 说明 |
| :--- | :--- | :--- |
| 分布通用方法 | `rvs()`、<br />`pdf()`、<br />`pmf()`、<br />`sf()`、<br />`ppf()`、<br />`isf()`、<br />`stats()`、<br />`moment()`、<br />`entropy()`、<br />`fit()`、<br />`support()`、<br />`interval()`、<br />`mean()`、<br />`median()`、<br />`var()`、<br />`std()` | 分布对象的通用方法，连续分布用 `pdf`，离散分布用 `pmf` |
| 描述性统计 | `describe()`、<br />`mean()`、<br />`median()`、<br />`mode()`、<br />`var()`、<br />`std()`、<br />`skew()`、<br />`kurtosis()`、<br />`sem()`、<br />`iqr()`、<br />`gmean()`、<br />`hmean()`、<br />`tmean()`、<br />`tvar()` | 计算数据的各类描述性统计量 |
| 假设检验 | `ttest_1samp()`、<br />`ttest_ind()`、<br />`ttest_rel()`、<br />`f_oneway()`、<br />`chisquare()`、<br />`kstest()`、<br />`mannwhitneyu()`、<br />`wilcoxon()`、<br />`kruskal()`、<br />`friedmanchisquare()`、<br />`shapiro()`、<br />`normaltest()`、<br />`levene()`、<br />`bartlett()` | 各类参数与非参数假设检验 |
| 相关与回归 | `pearsonr()`、<br />`spearmanr()`、<br />`kendalltau()`、<br />`linregress()`、<br />`pointbiserialr()` | 相关系数计算与线性回归 |
| 核密度估计 | `gaussian_kde()` | 高斯核密度估计 |
| 频率统计 | `cumfreq()`、<br />`relfreq()`、<br />`percentileofscore()`、<br />`scoreatpercentile()`、<br />`binned_statistic()`、<br />`binned_statistic_2d()`、<br />`binned_statistic_dd()` | 频率分布与分位数计算 |
| 采样与随机 | `rvs()`（分布方法）、<br />`qmc.Sobol()`、<br />`qmc.LatinHypercube()` | 随机数生成与准蒙特卡洛采样 |
| 列联表 | `contingency.chi2_contingency()`、<br />`contingency.expected_freq()`、<br />`contingency.margins()` | 列联表分析与独立性检验 |
| 掩码统计 | `mstats.describe()`、<br />`mstats.ttest_1samp()`、<br />`mstats.kruskal()` 等 | 针对含缺失值数据的统计函数 |
| 其他工具 | `zscore()`、<br />`rankdata()`、<br />`tiecorrect()`、<br />`combine_pvalues()`、<br />`trim_mean()`、<br />`winsorize()` | 数据标准化、秩转换、p 值合并等辅助函数 |

## 函数具体解释


| 类别 | 函数/方法 | 说明 |
| :--- | :--- | :--- |
| 分布通用方法 | `rvs()`、<br />`pdf()`、<br />`pmf()`、<br />`sf()`、<br />`ppf()`、<br />`isf()`、<br />`stats()`、<br />`moment()`、<br />`entropy()`、<br />`fit()`、<br />`support()`、<br />`interval()`、<br />`mean()`、<br />`median()`、<br />`var()`、<br />`std()` | `rvs()` 生成指定分布的随机样本；<br />`pdf()` 计算连续分布的概率密度函数值；<br />`pmf()` 计算离散分布的概率质量函数值；<br />`sf()` 计算生存函数（即 `1 - cdf`）；<br />`ppf()` 计算百分点函数（`cdf` 的逆函数，用于求分位数）；<br />`isf()` 计算逆生存函数（`sf` 的逆函数）；<br />`stats()` 返回分布的均值、方差、偏度、峰度等矩信息；<br />`moment()` 计算指定阶的非中心矩；<br />`entropy()` 计算分布的（微分）熵；<br />`fit()` 对数据做最大似然估计以拟合分布参数；<br />`support()` 返回分布的支撑域（有效取值范围）上下界；<br />`interval()` 返回以中位数为中心的等尾置信区间；<br />`mean()`、`median()`、`var()`、`std()` 分别返回分布的<br />理论均值、中位数、方差、标准差 |
| 描述性统计 | `describe()`、<br />`mean()`、<br />`median()`、<br />`mode()`、<br />`var()`、<br />`std()`、<br />`skew()`、<br />`kurtosis()`、<br />`sem()`、<br />`iqr()`、<br />`gmean()`、<br />`hmean()`、<br />`tmean()`、<br />`tvar()` | `describe()` 一次性返回样本量、最小最大值、均值、方差、偏度、峰度等综合统计摘要；<br />`mean()`、`median()`、`mode()` 分别计算算术均值、中位数、众数；<br />`var()`、`std()` 计算样本方差与标准差；<br />`skew()` 计算偏度（衡量分布不对称程度）；<br />`kurtosis()` 计算峰度（Fisher 定义，正态分布峰度为 0）；<br />`sem()` 计算均值的标准误；<br />`iqr()` 计算四分位距（第三四分位数减第一四分位数）；<br />`gmean()` 计算几何均值；<br />`hmean()` 计算调和均值；<br />`tmean()`、`tvar()` 计算截尾均值与截尾方差（忽略指定范围外的值） |
| 假设检验 | `ttest_1samp()`、<br />`ttest_ind()`、<br />`ttest_rel()`、<br />`f_oneway()`、<br />`chisquare()`、<br />`kstest()`、<br />`mannwhitneyu()`、<br />`wilcoxon()`、<br />`kruskal()`、<br />`friedmanchisquare()`、<br />`shapiro()`、<br />`normaltest()`、<br />`levene()`、<br />`bartlett()` | `ttest_1samp()` 单样本 t 检验（检验样本均值是否等于给定值）；<br />`ttest_ind()` 独立双样本 t 检验（检验两组独立样本均值是否相等）；<br />`ttest_rel()` 配对样本 t 检验（检验两组配对样本均值差是否为零）；<br />`f_oneway()` 单因素方差分析（检验多组样本均值是否相等）；<br />`chisquare()` 卡方拟合优度检验（检验观测频数与期望频数是否一致）；<br />`kstest()` 单样本 K-S 检验（检验样本是否来自指定分布）；<br />`mannwhitneyu()` Mann-Whitney U 检验（非参数独立双样本位置检验）；<br />`wilcoxon()` Wilcoxon 符号秩检验（非参数配对样本检验）；<br />`kruskal()` Kruskal-Wallis H 检验（非参数多组独立样本检验）；<br />`friedmanchisquare()` Friedman 检验（非参数多组配对样本检验）；<br />`shapiro()` Shapiro-Wilk 正态性检验；<br />`normaltest()` D'Agostino-Pearson 正态性检验（基于偏度和峰度）；<br />`levene()` Levene 方差齐性检验（对非正态数据稳健）；<br />`bartlett()` Bartlett 方差齐性检验（要求数据正态） |
| 相关与回归 | `pearsonr()`、<br />`spearmanr()`、<br />`kendalltau()`、<br />`linregress()`、<br />`pointbiserialr()` | `pearsonr()` 计算皮尔逊相关系数及双侧 p 值（衡量两变量的线性相关程度）；<br />`spearmanr()` 计算斯皮尔曼秩相关系数及 p 值（衡量单调相关性）；<br />`kendalltau()` 计算肯德尔秩相关系数及 p 值（适用于有序数据）；<br />`linregress()` 执行简单线性回归，返回斜率、截距、相关系数、p 值、标准误；<br />`pointbiserialr()` 计算点双列相关系数（一个连续变量与一个二分类变量的相关性） |
| 核密度估计 | `gaussian_kde()` | `gaussian_kde()` 基于高斯核进行核密度估计，<br />可从样本数据估计单变量或多变量概率密度函数，<br />支持自动带宽选择（Scott 规则或 Silverman 规则） |
| 频率统计 | `cumfreq()`、<br />`relfreq()`、<br />`percentileofscore()`、<br />`scoreatpercentile()`、<br />`binned_statistic()`、<br />`binned_statistic_2d()`、<br />`binned_statistic_dd()` | `cumfreq()` 计算累积频率直方图；<br />`relfreq()` 计算相对频率直方图；<br />`percentileofscore()` 计算给定分数在样本中的百分位排名；<br />`scoreatpercentile()` 根据给定百分位计算对应的分数值；<br />`binned_statistic()` 对一维数据分箱并计算每箱的统计量（如均值、总和、计数等）；<br />`binned_statistic_2d()` 对二维数据分箱并计算统计量；<br />`binned_statistic_dd()` 对多维数据分箱并计算统计量 |
| 采样与随机 | `rvs()`（分布方法）、<br />`qmc.Sobol()`、<br />`qmc.LatinHypercube()` | `rvs()` 作为分布对象的方法，从指定分布中抽取随机样本；<br />`qmc.Sobol()` 生成 Sobol 低差异序列（准蒙特卡洛采样）；<br />`qmc.LatinHypercube()` 生成拉丁超立方采样序列 |
| 列联表 | `contingency.chi2_contingency()`、<br />`contingency.expected_freq()`、<br />`contingency.margins()` | `contingency.chi2_contingency()` 对列联表执行卡方独立性检验；<br />`contingency.expected_freq()` 根据行列边际和计算列联表的期望频数；<br />`contingency.margins()` 返回列联表的行和与列和（边际频数） |
| 掩码统计 | `mstats.describe()`、<br />`mstats.ttest_1samp()`、<br />`mstats.kruskal()` 等 | `mstats` 子模块提供与主模块同名的统计函数，<br />但支持 `numpy` 掩码数组（masked array），<br />可自动忽略被标记为缺失的数据，适用于含缺失值的数据集 |
| 其他工具 | `zscore()`、<br />`rankdata()`、<br />`tiecorrect()`、<br />`combine_pvalues()`、<br />`trim_mean()`、<br />`winsorize()` | `zscore()` 计算数据的标准分数（z 分数，即减去均值后除以标准差）；<br />`rankdata()` 对数据排序并返回秩次（支持多种平局处理方法）；<br />`tiecorrect()` 计算秩检验中平局校正因子；<br />`combine_pvalues()` 使用 Fisher 法或 Stouffer 法合并多个独立 p 值；<br />`trim_mean()` 计算截尾均值（去除两端指定比例后求均值）；<br />`winsorize()` 对数据进行缩尾处理（将极端值替换为指定百分位处的值） |

## 代码示例

```py
import numpy as np
from scipy import stats

# 生成正态分布样本并做描述性统计
sample = stats.norm.rvs(loc=5, scale=2, size=100, random_state=42)
desc = stats.describe(sample)
print("样本量:", desc.nobs)
print("均值:", desc.mean)
print("方差:", desc.variance)

# 单样本 t 检验，检验均值是否等于 5
t_stat, p_value = stats.ttest_1samp(sample, popmean=5)
print("t 统计量:", t_stat, "p 值:", p_value)

# 计算皮尔逊相关系数
x = np.random.randn(100)
y = 0.8 * x + np.random.randn(100) * 0.5
corr, p = stats.pearsonr(x, y)
print("皮尔逊相关系数:", corr, "p 值:", p)
```

# optimize 模块

`scipy.optimize` 是 SciPy 中用于数值优化、方程求根、曲线拟合和线性规划的模块，通常通过 `from scipy.optimize import minimize, root, curve_fit` 等方式导入，在需要最小化目标函数、求解方程、拟合模型参数、处理约束优化或线性规划时使用。

它的主要作用包括：单变量和多变量函数最小化、带约束或边界的优化、非线性最小二乘与曲线拟合、单变量和多变量方程求根、线性规划、全局优化，以及提供统一的优化结果对象和约束描述对象。

该模块下包含多个子模块和类，例如用于列联表或特殊优化的子模块（不同 SciPy 版本可能略有差异），以及 `OptimizeResult`、`OptimizeWarning`、`Bounds`、`LinearConstraint`、`NonlinearConstraint`、`BFGS`、`SR1` 等常用类。

## 常用函数

| 函数 | 作用 |
| :--- | :--- |
| `minimize` | 多变量标量函数最小化，支持无约束、有界约束和一般约束优化 |
| `minimize_scalar` | 单变量标量函数最小化 |
| `root` | 求解多变量非线性方程组 |
| `root_scalar` | 求解单变量方程的根 |
| `fsolve` | 求解非线性方程组，是较经典的求根接口 |
| `least_squares` | 求解带变量边界的非线性最小二乘问题 |
| `curve_fit` | 基于非线性最小二乘进行曲线拟合 |
| `linprog` | 求解线性规划问题 |
| `differential_evolution` | 基于差分进化的全局优化 |
| `basinhopping` | 盆地跳跃法全局优化 |
| `brute` | 在给定网格上暴力搜索全局最小值 |
| `shgo` | 单纯形同伦全局优化 |
| `dual_annealing` | 对偶退火全局优化 |
| `nnls` | 非负最小二乘 |
| `lsq_linear` | 带边界的线性最小二乘 |
| `fmin`、`fmin_bfgs`、`fmin_cg`、`fmin_l_bfgs_b`、`fmin_tnc`、`fmin_cobyla`、`fmin_powell` | 较早期的优化接口，通常推荐优先使用 `minimize` |
| `show_options` | 查看各求解器支持的额外选项 |

## 常用类、属性和方法

| 名称 | 类型 | 说明 |
| :--- | :--- | :--- |
| `OptimizeResult` | 类 | 优化或求根结果对象，常用属性包括 <br />`x`、`fun`、`jac`、`hess`、<br />`nfev`、`njev`、`nhev`、`nit`、`status`、`success`、`message` |
| `OptimizeWarning` | 类 | 优化过程中发出的通用警告 |
| `Bounds` | 类 | 描述变量上下界约束，例如 `Bounds(lb, ub)` |
| `LinearConstraint` | 类 | 描述线性约束，形式为 `lb <= A @ x <= ub` |
| `NonlinearConstraint` | 类 | 描述非线性约束，形式为 `lb <= fun(x) <= ub` |
| `BFGS` | 类 | 用于 `trust-constr` 方法的 BFGS Hessian 更新策略 |
| `SR1` | 类 | 用于 `trust-constr` 方法的对称秩一 Hessian 更新策略 |
| `HessianUpdateStrategy` | 接口/基类 | Hessian 更新策略的抽象接口 |
| `OptimizeResult.x` | 属性 | 最优解或方程的根 |
| `OptimizeResult.fun` | 属性 | 最优解处的目标函数值 |
| `OptimizeResult.success` | 属性 | 优化是否成功 |
| `OptimizeResult.message` | 属性 | 终止原因说明 |
| `OptimizeResult.nit` | 属性 | 迭代次数 |
| `OptimizeResult.nfev` | 属性 | 目标函数调用次数 |

## 代码示例

```py
import numpy as np
from scipy.optimize import minimize, root, curve_fit

# 最小化目标函数 f(x) = (x - 3)^2 + 1
def objective(x):
    return (x - 3) ** 2 + 1

res = minimize(objective, x0=0)
print("最优解:", res.x, "最小值:", res.fun)

# 求解方程 x^2 - 4 = 0
def equation(x):
    return x ** 2 - 4

sol = root(equation, x0=1)
print("方程的根:", sol.x)

# 曲线拟合
x_data = np.linspace(0, 5, 50)
y_data = 2.5 * np.exp(-1.3 * x_data) + 0.2 * np.random.normal(size=x_data.size)

def model(x, a, b):
    return a * np.exp(-b * x)

params, _ = curve_fit(model, x_data, y_data, p0=[1, 1])
print("拟合参数 a, b:", params)
```

# integrate

`scipy.integrate` 和 `scipy.optimize` 都是 SciPy 中用于数值计算的子模块。

- `scipy.integrate` 用于数值积分和常微分方程求解，
- `scipy.optimize` 用于函数优化、方程求根、曲线拟合和线性规划。

通常通过 `from scipy import integrate, optimize` 或 `from scipy.integrate import quad, solve_ivp`、`from scipy.optimize import minimize, root, curve_fit` 导入；当需要计算定积分、多重积分、求解微分方程初值问题，或需要最小化目标函数、求方程根、拟合模型参数、处理约束优化和线性规划时使用。

`scipy.integrate` 的作用主要是对函数或离散样本做数值积分，包括一维、二维、三维及多维积分，并提供常微分方程初值问题求解器。 `scipy.optimize` 的作用主要是对目标函数做局部或全局优化，求解非线性方程组，执行最小二乘拟合和曲线拟合，处理带约束或边界的优化问题，以及求解线性规划问题。

`scipy.integrate` 中包含的类和子模块主要有 `IntegrationWarning`、`OdeSolver`、`DenseOutput`、`OdeSolution`，以及用于低层次 ODE 求解器的 `RK23`、`RK45`、`Radau`、`BDF`、`LSODA` 等类。 

## 常用函数

| 函数或类                                | 作用                                                         |
| --------------------------------------- | ------------------------------------------------------------ |
| `quad`                                  | 计算一维定积分，支持无穷限                                   |
| `quad_vec`                              | 计算向量值函数的一维定积分                                   |
| `dblquad`                               | 计算二重积分                                                 |
| `tplquad`                               | 计算三重积分                                                 |
| `nquad`                                 | 计算多维数值积分                                             |
| `fixed_quad`                            | 使用固定阶高斯求积计算定积分                                 |
| `quadrature`                            | 使用固定容差高斯求积计算定积分                               |
| `romberg`                               | 使用 Romberg 方法计算定积分                                  |
| `trapezoid`                             | 基于梯形法则对离散样本求积分                                 |
| `cumulative_trapezoid`                  | 基于梯形法则计算累积积分                                     |
| `simpson`                               | 基于 Simpson 法则对离散样本求积分                            |
| `romb`                                  | 对等距离散样本做 Romberg 积分                                |
| `solve_ivp`                             | 求解常微分方程初值问题                                       |
| `odeint`                                | 求解常微分方程组，新代码推荐改用 `solve_ivp`                 |
| `ode`                                   | 低层次 ODE 求解接口                                          |
| `RK23`、`RK45`、`Radau`、`BDF`、`LSODA` | ODE 求解器类，分别对应显式 Runge-Kutta、隐式 Runge-Kutta、BDF、LSODA 等方法 |
| `IntegrationWarning`                    | 积分过程中出现异常时发出的警告                               |

## 示例

```py
import numpy as np
from scipy import integrate

# 计算一维定积分 ∫_0^1 x^2 dx
result, error = integrate.quad(lambda x: x ** 2, 0, 1)
print("积分结果:", result, "误差估计:", error)

# 求解常微分方程初值问题 dy/dt = -2y, y(0) = 1
def ode(t, y):
    return -2 * y

sol = integrate.solve_ivp(ode, [0, 5], [1], t_eval=np.linspace(0, 5, 100))
print("t 值:", sol.t[:5])
print("y 值:", sol.y[0, :5])
```