#  MLlib基本数据类型

# MLlib基本数据类型

## 一、本地向量（Local Vector）

### 1.基本概念

​	本地向量仅支持单机存储，其元素值类型为浮点型，索引值为整型，且从零开始进行索引。总的来说，本地向量的创建形式可分为密集向量（DenseVector）和系数向量（SparseVector）。

- 密集向量

  用一个人双精度浮点型数组来表示其中每个元素，如向量`(1,0,3)`的密集型向量表示形式为`[1.0,0.0,3.0]`。

- 稀疏向量

  用一个整型索引数组和一个双精度浮点型的值数组来表达每个向量，如向量`(1,0,3)`的稀疏型向量表示形式为`(3,[0,2],[1.0,3.0])`，其中3表示向量长度，[0,2]表示其中非零元素的索引值，[1.0,3.0]则是按索引值排列的向量中非零元素的值。

### 2.创建操作

- 导入基本类

  接下来尝试创建本地向量，可在Zeppelin或Spark-Shell中进行

  ```scala
  import org.apache.spark.mllib.linalg.{Vector, Vectors}
  ```

  所有本地向量都以`org.apache.spark.mllib.linalg.Vector`为基类，`DenseVector`和`SparseVector`分别是它的两个实现类，故推荐使用`Vectors`工具类下定义的工厂方法来创建本地向量。同时需要注意的是，Scala会默认引入`scala.collection.immutable.Vector`，我们要显式地引入`org.apache.spark.mllib.linalg.Vector`来使用MLlib提供的向量类型。

- 创建密集向量

  ```scala
  val dv: Vector = Vectors.dense(2.0, 0.0, 8.0)
  ```

  输入println查看结果

- 创建稀疏向量

  首先采用Array分别两个参数位置中输入非零值索引和非零值元素值

  ```scala
  val sv1: Vector = Vectors.sparse(3, Array(0, 2), Array(2.0, 8.0))
  ```

  然后我们采用另一种创建稀疏向量的方法，将第二个参数设为一个序列，其中每个元素都是非零值的元组(index,elem)

  ```scala
  val sv2: Vector = Vectors.sparse(3, Seq((0, 2.0), (2, 8.0)))
  ```

  输入println查看结果，可查看两种创建方式实际上都对应着一个向量形式

  ![1](G:\屏幕截图\Spark MLlib\1.jpg)

## 二、标注点（Labeled Point）

### 1.基本概念

**标注点**`LabeledPoint`是一种带有**标签（Label/Response）**的本地向量，它可以是稠密或者是稀疏的。在MLlib中，标注点在监督学习算法中被使用。由于标签是用双精度浮点型来存储的，故标注点类型在**回归（Regression）**和**分类（Classification）**问题上均可使用。例如，对于二分类问题，标签选择时，**正样本**的标签为`1`，**负样本**的标签为`0`，而对于多类别的分类问题来说，标签可以是一个以0开始的索引序列:`0, 1, 2 ...`

### 2.创建操作

- 导入基本类

  ```scala
  import org.apache.spark.mllib.linalg.Vectors
  import org.apache.spark.mllib.regression.LabeledPoint
  ```

  标注点的实现类是`org.apache.spark.mllib.regression.LabeledPoint`，请注意它与前面介绍的本地向量不同，并不位于linalg包下

- 创建向量标注点

  稠密向量表现形式时

  ```scala
  val pos = LabeledPoint(1.0, Vectors.dense(2.0, 0.0, 8.0))
  ```

  注，第一个参数为标注点，第二个参数为向量

  稀疏向量表现形式时

  ```scala
  val neg = LabeledPoint(0.0, Vectors.sparse(3, Array(0, 2), Array(2.0, 8.0)))
  ```

  可用println查看结果

- 读取LIBSVM格式数据

  在实际的机器学习问题中，稀疏向量数据是非常常见的，很多时候我们会遇到LIBSVM格式数据，如Spark自带的一个样例。

  ```shell
  cd /usr/local/spark/data/mllib
  cat sample_libsvm_data.txt
  ```

  该数据格式一般形式为`label index1:value1 index2:value2 index3:value3 ...`，其中`label`是该样本点的标签值，一系列`index:value`对则代表了该样本向量中所有非零元素的索引和元素值。这里需要特别注意的是，index是以1开始并递增的。

  首先，导入基本类，MLlib在`org.apache.spark.mllib.util.MLUtils`工具类中提供了读取**LIBSVM**格式的方法`loadLibSVMFile`，其使用非常方便。

  ```scala
  import org.apache.spark.mllib.util.MLUtils
  ```

  然后进行读取

  ```scala
  val examples = MLUtils.loadLibSVMFile(sc, "file:///usr/local/spark/data/mllib/sample_libsvm_data.txt")
  ```

  接下来，我们可以查看下加载进来的标注点的值

  ```scala
  examples.collect().head
  ```

  ![2](G:\屏幕截图\Spark MLlib\2.jpg)

## 三、本地矩阵（Local Matrix）

### 1.基本概念

​	**本地矩阵**具有整型的行、列索引值和双精度浮点型的元素值，它存储在单机上。MLlib支持**密集矩阵**`DenseMatrix`和**稀疏矩阵**`Sparse Matrix`两种本地矩阵，**稠密矩阵**将所有元素的值存储在一个**列优先（Column-major）**的双精度型数组中，而**稀疏矩阵**则将非零元素以列优先的**CSC（Compressed Sparse Column）**模式进行存储。这里所谓的优先，是指在不显示说明的情况下，参数优先表示列，或者非零元素。

### 2.创建操作

- 导入基本类

  ```scala
  import org.apache.spark.mllib.linalg.{Matrix, Matrices}
  ```

  本地矩阵的基类是`org.apache.spark.mllib.linalg.Matrix`，`DenseMatrix`和`SparseMatrix`均是它的实现类，和本地向量类似，MLlib也为本地矩阵提供了相应的工具类`Matrices`，调用工厂方法即可创建实例。

- 创建密集矩阵

  ```scala
  val dm: Matrix = Matrices.dense(3, 2, Array(1.0, 3.0, 5.0, 2.0, 4.0, 6.0))
  ```

  创建了一个三行两列的矩阵，且元素排列方式是按照列进行的排列

  ![3](G:\屏幕截图\Spark MLlib\3.jpg)

- 创建稀疏矩阵

  ```scala
  val sm: Matrix = Matrices.sparse(3, 2, Array(0, 1, 3), Array(0, 2, 1), Array(9, 6, 8))
  ```

  这里创建一个3行2列的稀疏矩阵[ [9.0,0.0], [0.0,8.0], [0.0,6.0]]。Matrices.sparse的参数中，3表示行数，2表示列数。

  ![4](G:\屏幕截图\Spark MLlib\4.jpg)

## 四、分布式矩阵（Distributed Matrix）

### 1.综述

​	分布式矩阵由长整型的行列索引值和双精度浮点型的元素值组成。它可以分布式地存储在一个或多个RDD上，MLlib提供了三种分布式矩阵的存储方案：**行矩阵**`RowMatrix`，**索引行矩阵**`IndexedRowMatrix`、**坐标矩阵**`CoordinateMatrix`和分块矩阵`Block Matrix`。它们都属于`org.apache.spark.mllib.linalg.distributed`包。

### 2.行矩阵（RowMatrix）

#### 2.1 基本概念

​	**行矩阵**`RowMatrix`是最基础的分布式矩阵类型。每行是一个本地向量，行索引无实际意义（即无法直接使用）。数据存储在一个由行组成的RDD中，其中每一行都使用一个本地向量来进行存储。由于行是通过本地向量来实现的，故列数（即行的维度）被限制在普通整型（`integer`）的范围内。在实际使用时，由于单机处理本地向量的存储和通信代价，行维度更是需要被控制在一个更小的范围之内。`RowMatrix`可通过一个`RDD[Vector]`的实例来创建。

#### 2.2 创建操作

- 导入基本类

  ```scala
  import org.apache.spark.rdd.RDD
  import org.apache.spark.mllib.linalg.{Vector,Vectors}
  import org.apache.spark.mllib.linalg.distributed.RowMatrix
  ```

  ​	此处，由于要通过本地向量来创建向量组成的RDD，从而进一步通过RDD来创建行矩阵，因此需要进行上述导入工作。

- 创建本地向量

  ```scala
  val dv1 : Vector = Vectors.dense(1.0,2.0,3.0)
  val dv2 : Vector = Vectors.dense(2.0,3.0,4.0)
  ```

  创建两个本地密集型向量。

- 利用本地向量创建RDD

  ```scala
  val rows : RDD[Vector] = sc.parallelize(Array(dv1,dv2))
  ```

- 通过RDD创建一个行矩阵

  ```scala
  val mat : RowMatrix = new RowMatrix(rows)
  ```

- 获得部分统计指标

  ```scala
  // 获得行数
  mat.numRows()
  // 获得列数
  mat.numCols()
  // 打印每行
  mat.rows.foreach(println)
  ```

  对于`RowMatrix`的实例，我们可以通过其自带的`computeColumnSummaryStatistics()`方法获取该矩阵的一些统计摘要信息，并可以对其进行*QR分解*，*SVD分解*和*PCA分解*，这一部分内容将在特征降维的章节详细解说。

  ```scala
  // 通过computeColumnSummaryStatistics()方法获取统计摘要
  val summary = mat.computeColumnSummaryStatistics()
  // 可以通过summary实例来获取矩阵的相关统计信息，例如行数
  summary.count
  // 最大向量
  summary.max
  // 方差向量
  summary.variance
  // 平均向量
  summary.mean
  // L1范数向量
  summary.normL1
  ```

### 3.索引行矩阵（IndexedRowMatrix）

#### 3.1 基本概念

​	**索引行矩阵**`IndexedRowMatrix`与`RowMatrix`相似，但它的每一行都带有一个有意义的行索引值，这个索引值可以被用来识别不同行，或是进行诸如**join**之类的操作。其数据存储在一个由`IndexedRow`组成的RDD里，即每一行都是一个带长整型索引的本地向量。

#### 3.2 创建操作

- 导入基本类

  ```scala
  import org.apache.spark.mllib.linalg.distributed.{IndexedRow, IndexedRowMatrix}
  ```

  与`RowMatrix`类似，`IndexedRowMatrix`的实例可以通过`RDD[IndexedRow]`实例来创建。下列操作承接行矩阵代码。

- 利用本地向量创建IndexedRow

  ```scala
  val idxr1 = IndexedRow(1,dv1)
  val idxr2 = IndexedRow(2,dv2)
  ```

  此处Index即为行索引。

- 利用IndexedRow创建RDD[IndexedRow]

  ```scala
  val idxrows = sc.parallelize(Array(idxr1,idxr2))
  ```

- 通过RDD[IndexedRow]创建一个索引行矩阵

  ```scala
  val idxmat: IndexedRowMatrix = new IndexedRowMatrix(idxrows)
  idxmat.rows.foreach(println)
  ```

  查看输出结果，矩阵会根据索引顺序生成。

### 4.坐标矩阵（Coordinate Matrix）

#### 4.1 基本概念

​	**坐标矩阵**`CoordinateMatrix`是一个基于**矩阵项**构成的RDD的分布式矩阵。每一个**矩阵项**`MatrixEntry`都是一个三元组：`(i: Long, j: Long, value: Double)`，其中`i`是行索引，`j`是列索引，`value`是该位置的值。坐标矩阵一般在矩阵的两个维度都很大，且矩阵非常稀疏的时候使用。

#### 4.2 创建操作

- 导入基本类

  ```scala
  import org.apache.spark.mllib.linalg.distributed.{CoordinateMatrix, MatrixEntry}
  ```

  `CoordinateMatrix`实例可通过`RDD[MatrixEntry]`实例来创建，其中每一个矩阵项都是一个(rowIndex, colIndex, elem)的三元组。

- 创建矩阵项

  ```scala
  val ent1 = new MatrixEntry(0,1,0.5)
  val ent2 = new MatrixEntry(2,2,1.8)
  ```

- 创建RDD[MatrixEntry]

  ```scala
  val entries : RDD[MatrixEntry] = sc.parallelize(Array(ent1,ent2))
  ```

- 通过RDD[MatrixEntry]创建一个坐标矩阵

  ```scala
  val coordMat: CoordinateMatrix = new CoordinateMatrix(entries)
  coordMat.entries.foreach(println)
  ```

  查看输出结果，矩阵会根据RDD中矩阵项排列顺序生成。

- 其他操作

  ​	坐标矩阵可以通过`transpose()`方法对矩阵进行转置操作，并可以通过自带的`toIndexedRowMatrix()`方法转换成**索引行矩阵**`IndexedRowMatrix`。但目前暂不支持`CoordinateMatrix`的其他计算操作。

  - 转置操作

    ```scala
    val transMat: CoordinateMatrix = coordMat.transpose()
    transMat.entries.foreach(println)
    ```

  - 变成行索引矩阵

    ```scala
    val indexedRowMatrix = transMat.toIndexedRowMatrix()
    indexedRowMatrix.rows.foreach(println)
    ```

### 4.分块矩阵(Block Matrix)

**分块矩阵**是基于**矩阵块**`MatrixBlock`构成的RDD的分布式矩阵，其中每一个矩阵块`MatrixBlock`都是一个元组`((Int, Int), Matrix)`，其中`(Int, Int)`是块的索引，而`Matrix`则是在对应位置的**子矩阵（sub-matrix）**，其尺寸由`rowsPerBlock`和`colsPerBlock`决定，默认值均为*1024*。分块矩阵支持和另一个分块矩阵进行加法操作和乘法操作，并提供了一个支持方法`validate()`来确认分块矩阵是否创建成功。

**分块矩阵**可由**索引行矩阵**`IndexedRowMatrix`或**坐标矩阵**`CoordinateMatrix`调用`toBlockMatrix()`方法来进行转换，该方法将矩阵划分成尺寸默认为*1024×1024*的分块，可以在调用`toBlockMatrix(rowsPerBlock, colsPerBlock)`方法时传入参数来调整分块的尺寸。
下面以矩阵A（如图）为例，先利用矩阵项`MatrixEntry`将其构造成坐标矩阵，再转化成如图所示的4个分块矩阵，最后对矩阵A与其转置进行乘法运算：

![5](G:\屏幕截图\Spark MLlib\5.png)

```scala
import org.apache.spark.mllib.linalg.distributed.{CoordinateMatrix, MatrixEntry}
import org.apache.spark.mllib.linalg.distributed.BlockMatrix
// 创建8个矩阵项，每一个矩阵项都是由索引和值构成的三元组
val ent1 = new MatrixEntry(0,0,1)
val ent2 = new MatrixEntry(1,1,1)
val ent3 = new MatrixEntry(2,0,-1)
val ent4 = new MatrixEntry(2,1,2)
val ent5 = new MatrixEntry(2,2,1)
val ent6 = new MatrixEntry(3,0,1)
val ent7 = new MatrixEntry(3,1,1)
val ent8 = new MatrixEntry(3,3,1)
// 创建RDD[MatrixEntry]
val entries : RDD[MatrixEntry] = sc.parallelize(Array(ent1,ent2,ent3,ent4,ent5,ent6,ent7,ent8))
// 通过RDD[MatrixEntry]创建一个坐标矩阵
val coordMat: CoordinateMatrix = new CoordinateMatrix(entries)
// 将坐标矩阵转换成2x2的分块矩阵并存储，尺寸通过参数传入
val matA: BlockMatrix = coordMat.toBlockMatrix(2,2).cache()
// 可以用validate()方法判断是否分块成功
matA.validate()
```

构建成功后，可通过toLocalMatrix转换成本地矩阵，并查看其分块情况：

```scala
matA.toLocalMatrix
// 查看其分块情况
matA.numColBlocks
matA.numRowBlocks
// 计算矩阵A和其转置矩阵的积矩阵
val ata = matA.transpose.multiply(matA)
ata.toLocalMatrix
```

分块矩阵`BlockMatrix`将矩阵分成一系列矩阵块，底层由矩阵块构成的RDD来进行数据存储。值得指出的是，用于生成分布式矩阵的底层RDD必须是已经确定（Deterministic）的，因为矩阵的尺寸将被存储下来，所以使用未确定的RDD将会导致错误。而且，不同类型的分布式矩阵之间的转换需要进行一个全局的**shuffle**操作，非常耗费资源。所以，根据数据本身的性质和应用需求来选取恰当的分布式矩阵存储类型是非常重要的。