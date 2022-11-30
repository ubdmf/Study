# C++ 语法


## 1. **数组**
- **定义一个数组** 
    - `double abc[3];`
    - `double abc[3]={0,0,0};`
    - `double abc[]={0,0,0};`
    - `double p[3][3];`


-  **静态数组赋值**
  - `double ceres_r0[6]={ 0,1,2,0,0,0 };`

  - **数组初始化**
    - `float h_mat_[kMeasDimension][kStateDimension]{};`

-  **数组指针赋值**

1.  double* inPara=new double[4];
    *inPara=fx;
    *(inPara+1)=fy;
    *(inPara+2)=cx;
    *(inPara+3)=cy;
 
2.  double* ppoints=new double[3*vp3d.size()];//vp3d 是3d点的个数，3*vp3d是所占内存空间
    for(size_t i=0;i<vp3d.size();i++)
    {
        double* p=ppoints+3*i;
        *(p)=vp3d[i].x;
        *(p+1)=vp3d[i].y;
        *(p+2)=vp3d[i].z;
    }
3.  double* pcams=new double[6*vCams.size()];
  for(size_t i=0;i<vCams.size();i++)
    {
        double* camPara=pcams+6*i;
       *(camPara)=rvec.at<float>(0,0);
        *(camPara+1)=rvec.at<float>(1,0);
        *(camPara+2)=rvec.at<float>(2,0);
        *(camPara+3)=t.at<float>(0,0);
        *(camPara+4)=t.at<float>(1,0);
        *(camPara+5)=t.at<float>(2,0);
   }



- **函数中传递数组**

```c++
static void ErrorCovUpdateCv(
      const float (&k_mat)[kStateCvDimension][kMeasCvDimension]);
```

 - **数组指针转vector**

double* xPts = a; double* yPts = b;
std::vector<cv::Point2f> corners_;
for (int i = 0; i < CENTERSIZE; i++)
{
  corners_.push_back(cv::Point2f(*(xPts + i), *(yPts + i)));
  if (*(xPts + i) == -1 && *(yPts + i) == -1) {
  continue;//did not find center position
}
else {
       corner_flag.push_back(i);
      corners.push_back(cv::Point2f(*(xPts + i), *(yPts + i)));
    }
}
world_corners_flag.push_back(corners_);



- **静态数组vs动态数组 函数内赋值**
```c++
void transform(cv::Mat& r_vec, cv::Mat& t_vec,double r[9],double t[3]) 
void transform(cv::Mat& r_vec, cv::Mat& t_vec,double* r,double* t)  调用的时候外部new一个空间，方便用完删除
{
	
	cv::Mat R;
	cv::Rodrigues(r_vec, R);
	/**r = R.at<double>(0, 0);
	*(r+1) = R.at<double>(0, 1);
	*(r + 2) = R.at<double>(0, 2);
	*(r + 3) = R.at<double>(1, 0);
	*(r + 4) = R.at<double>(1, 1);
	*(r + 5) = R.at<double>(1, 2);
	*(r + 6) = R.at<double>(2, 0);
	*(r + 7) = R.at<double>(2, 1);
	*(r + 8) = R.at<double>(2, 2);*/
	
	r[0] = R.at<double>(0, 0);
	r[1] = R.at<double>(0, 1);
	r[2] = R.at<double>(0, 2);
	r[3] = R.at<double>(1, 0);
	r[4] = R.at<double>(1, 1);
	r[5] = R.at<double>(1, 2);
	r[6] = R.at<double>(2, 0);
	r[7] = R.at<double>(2, 1);
	r[8] = R.at<double>(2, 2);

	t[0] = t_vec.at<double>(0);
	t[1] = t_vec.at<double>(1);
	t[2] = t_vec.at<double>(2);
}

```

 - **数组指针 转存到容器**
```c++
double* X = new double[count];
double* Y = new double[count];
double* Z = new double[count];
for (int i = 0; i < count; i++)
{
pt3d_vec[level][i][0] = X[i];
pt3d_vec[level][i][1] = Y[i];
pt3d_vec[level][i][2] = Z[i];

}
delete[]X; delete[] Y; delete[]Z;
```
## 2. vector
### vector 初始化

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
### **resize/reserve**

### Sort 排序

- std::sort(vec.begin(),vec.end()); //默认从小到大排序
- std::sort(vec.begin(),vec.end()，Compare()); //写一个
- sort 的比较函数写法
    - 申明比较类
    - 重载类的比较函数
### copy 拷贝
## 3. array

## 4. map

## 5. sturct
   ### struct 初始化
    - CrossMatchCoastInfo cross_tmp{};
    - 
## 6. 类

- 拷贝构造函数


## 符号重载

-   重载函数调用
 bool operator()(const float a,const float b)const{}
