# C++ 语法


## 数组
- 定义一个3维数组 double abc[3]

- 函数中传递数组

```c++
static void ErrorCovUpdateCv(
      const float (&k_mat)[kStateCvDimension][kMeasCvDimension],
      const float (&r_mat)[kMeasCvDimension][kMeasCvDimension],
      const float (&h_mat)[kMeasCvDimension][kStateCvDimension],
      float (&p_mat)[kStateCvDimension][kStateCvDimension]);
```


- 数组初始
    - `float h_mat_[kMeasDimension][kStateDimension]{};`
