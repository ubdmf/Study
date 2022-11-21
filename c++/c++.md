# C++ 语法


## 1. 数组
- 定义一个3维数组 
    - `double abc[3];`
    - `double abc[3]={0,0,0};`
    - `double abc[]={0,0,0};`


- 函数中传递数组

```c++
static void ErrorCovUpdateCv(
      const float (&k_mat)[kStateCvDimension][kMeasCvDimension],
      const float (&r_mat)[kMeasCvDimension][kMeasCvDimension],
      const float (&h_mat)[kMeasCvDimension][kStateCvDimension],
      float (&p_mat)[kStateCvDimension][kStateCvDimension]);
```


- 数组初始化
    - `float h_mat_[kMeasDimension][kStateDimension]{};`

## 2. vector

### vector 转为指针
```c++
void Fit::polyfit(const std::vector<double>& x, const std::vector<double>& y,
                  int poly_n, bool isSaveFitYs) {
  polyfit_(&x[0], &y[0], getSeriesLength(x, y), poly_n, isSaveFitYs);
  //
}

void Fit::polyfit_(const T* x, const T* y, size_t length, int poly_n,bool isSaveFitYs) {
    //
  }

```
### resize/reserve 


## 3. array

