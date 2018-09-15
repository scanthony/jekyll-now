---
layout: post
title: R 还是Python，怎么选？
---

[R](https://www.r-project.org/)语言的前身是贝尔实验室推出的S语言，1995年推出。R语言的目标是成为一个好用、用户友好的数据分析，统计和数据可视化的编程语言。[Python](https://www.python.org/)最早1991年推出，强调高效率和代码可读性的。



#### 用途

 R是一门统计分析语言，过去主要在学术界和科研领域使用较多。但是现在，R迅速地向企业应用中扩展。当数据只需要在单独的环境中进行独立分析运算时，R的使用比例往往较高。

 Python在程序员、软件开发者中应用较为广泛，很多从编程开发转行到数据分析的从业者多使用Python。当数据分析需要被实时整合到软件应用中的时候，通常都采用Python实现。

 2014年的统计数据表明，58%的数据分析师使用R，42%使用Python，有大约23%的数据分析师同时使用R和Python。

#### 主要的软件包和库

 R的主要软件包包括：[dplyr](https://dplyr.tidyverse.org/)（数据操纵）、[stringr](https://www.rdocumentation.org/packages/stringr)（字符串处理）、[zoo](https://www.rdocumentation.org/packages/zoo)和[lubridate](https://lubridate.tidyverse.org/)（处理日期和时间数据）、[lattice](https://www.rdocumentation.org/packages/lattice)和[ggplot2](https://ggplot2.tidyverse.org/)（数据可视化）以及[caret](http://caret.r-forge.r-project.org/)（机器学习）。

 Python的主要库包括：[pandas](https://pandas.pydata.org/)（数据操纵）、[SciPy](https://www.scipy.org/)和[NumPy](http://www.numpy.org/)（科学计算）、[scikit-learn](http://scikit-learn.org/)（机器学习）、[matplotlib](https://matplotlib.org/)（数据可视化）、[TensorFlow](https://www.tensorflow.org/)、[PyTorch](https://pytorch.org/)、[CNTK](https://www.microsoft.com/en-us/cognitive-toolkit/)（深度学习）。

#### 优缺点对比

 1. R的数据可视化软件包选项多，功能强大。除了最长见的ggplot2，还有lattice（方便的多元数据可视化）、[rCharts](https://ramnathv.github.io/rCharts/)和[ggvis](https://ggvis.rstudio.com/)（创建基于JavaScript的交互式数据可视化）。尽管如此，越来越多的Python库也可以提供很好的数据可视化了，比如[Seaborn](https://seaborn.pydata.org/)、[Bokeh](https://bokeh.pydata.org/)和[Pygal](http://pygal.org/en/stable/)。

 2. Python的[Anaconda发布](https://www.anaconda.com/)，让用户可以很方便的在[Jupyter Notebook](http://jupyter.org/)中进行数据分析，方便结果的导出和发布，而且比[R Markdown](https://rmarkdown.rstudio.com/)似乎更为直观。

 3. R是传统的统计计算语言。因此在统计分析方面的软件包比Python更为丰富。除了官方的[CRAN](https://cran.r-project.org/)以外，[Bioconductor](https://www.bioconductor.org/)和[GitHub](https://github.com/)上也有丰富的  开源代码和软件包支持。Python的主要库来源为[PyPI](https://pypi.org/)。

 4. Python语言相当直观，就像日常使用的英语一样简单优雅。非常容易学习和上手，让数据分析师把精力花在分析而不是写代码上。

 5. R由统计学家开发，为统计学家开发。没有编程背景的统计学家、工程师和科学家觉得R简单易用。不仅在学术界应用广泛，R在其它行业也开始被广泛应用。

 6. Python用于开发的背景，让许多已经有IT部门的公司企业更容易上手和采用。不需要新招聘使用R的数据分析师，简单的培训就能让已有的员工快速上手。

 7. R的速度不如Pytyon快。R的设计是为了方便统计学家，因此没有过于考虑对电脑的友好性，运行速度也不如Python。可以使用[pqR](www.pqr-project.org/)和[fastR](https://www.rdocumentation.org/packages/fastR)等软件包来提升R的性能。类似地，Python中也可以使用[Cython](http://cython.org/)等C语言的扩展，来进一步提高性能。

 8. R和Python都是开源的数据分析工具。[O'Reilly](https://www.oreilly.com/)在2013年的调查表明，主要使用开源工具的数据分析师，在收入上更高。

#### 结论

没有更好，两个工具各有优势，取长补短才是数据分析师的胜算之道！
