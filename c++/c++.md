# C++ 语法


## 1. **数组**
- **定义一个数组** 
    - `double abc[3];`
    - `double abc[3]={0,0,0};`
    - `double abc[]={0,0,0};`
    - `double p[3][3];`


-  **静态数组赋值**
  - `double ceres_r0[6]={ 0,1,2,0,0,0 };`
  - `float ceres_r0[] = {2.0F, 5.0F, 100.0F};`


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
    （可以在头文件中先定义l， 在类内构造函数的时候再对l进行初始化)
    - std::vector<int> l;
    - l = std::vector<int>(number, default_idx);
    - vector<int> dst(8, -1); 直接初始化位长度位8，默认值为-1的vector
    - vector<int> src = {0,1,2,3,4} 初始化的时候直接给长度和值


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
        - 要比较的是类的成员变量，可以在类内写一个比较函数，sort的时候直接调这个比较函数
        - `std::sort(selected_obj_his_info_.begin(), selected_obj_his_info_.end(),SortAsc);`
           - `bool LanePreprocess::SortAsc(const Hist_Info& a, const Hist_Info& b) {
  return a.position.x < b.position.x;
} `
    - lamda 表达式
        - ![lambda参数](./imgs/lambdaexpsyntax.png)
          1. [] capture 子句（在 C++ 规范中也称为 Lambda 引导)
          2. () 参数列表（可选）（也称为 Lambda 声明符）
          3. mutable 规范（可选)
          4. exception-specification（可选）
          5. trailing-return-type（可选)
        - 
        ```c++
        sort(recom_spd_list_.begin(), recom_spd_list_.end(),
       [](const ISAOutput& a, const ISAOutput& b) -> bool {
         return a.recom_spd_dist < b.recom_spd_dist;
       });
       ```
        - lambda表达式是：
        `[](const ISAOutput& a, const ISAOutput& b) -> bool {
         return a.recom_spd_dist < b.recom_spd_dist;}`
            - const ISAOutput& a, const ISAOutput& b 函数传入的参数
            - ->bool 指定函数的返回值
            - {} 里是函数的表达式




### 把数组元素复制到vector中
- int arr[] = { 10, 20, 30, 40, 50 };
- vector<int> v1(5);
- copy(arr, arr + 5, v1.begin());
### vector 插入到数组中
- vector<int> src = {0,1,2,3,4}
- int dst[8] = {-1}
- std::copy(src.begin(),src.end(),dst);
### 数组插入到数组中
- int src[5]= {0,1,2,3,4};
- int dst[8]={-1};
- std::copy(src, src + 5, dst);
### vector 复制到 vector
- vector<int> src = {0,1,2,3,4}
- vector<int> dst(8,-1);
- std::copy(src.begin(),src.end(),dst.begin())
### vector 插入到vector末尾
- vector<int> src = {0,1,2,3,4}
- vector<int> dst = {-10,-9};
- std::copy(src.begin(),src.end(),std::back_inserter(dst));

### vector unique 函数 去重
- unique函数功能是去除相邻的重复元素，注意是相邻，所以必须先使用sort函数。还有一个容易忽视的特性是它并不真正把重复的元素删除。之所以说并不真正把重复的元素删除，因为unique实际上并没有删除任何元素，而是将无重复的元素复制到序列的前段，从而覆盖相邻的重复元素
- 先构造一个vector,将vector排序
-  sort(goal.begin(), goal.end());// 1, 2, 3, 3, 4, 4, 6, 7, 8, 9
- 然后使用unique算法，unique返回值是重复元素的开始位置
- auto pos = unique(goal.begin(), goal.end());    // 1, 2, 3, 4, 6, 7, 8, 9, 3, 4 (指针指向3)
- 最后删除后面的那段重复部分
-  goal.erase(pos, goal.end()); // 得到1, 2, 3, 4, 6, 7, 8, 9
## copy 拷贝
- copy只负责复制，不负责申请空间，所以复制前必须有足够的空间
- 将容器的元素从给定的范围从给定的开始位置复制到另一个容器
## 3. array
- 初始化 
  - 初始化的时候指定数据类型和维度：
      - std::array<math::Vector2f, 4> a; 
  - 取值 a.at(0)


## 4. map
  - 正向遍历
    - auto 等价于 std::map<int,int>::iterator
    - for(auto itr = map.begin();itr++;itr!=map.end())
    //正向迭代指针
    - map<int,int>::iterator itr = map.begin();
    - while(itr !=map.end()){itr++}
    <P> //正向遍历 </P>
   
    ```c++
    auto traj = map.begin();
    while(traj!=map.end()){
      ++traj;
    }
    ```
  - 反向遍历
    - auto 等价于 std::map<int,int>::reverse_iterator 
    - for(auto itr= map.rbegin();itr++;itr!=map.end())
    
    - std::map<int,int>::reverse_iterator itr = map.rbegin();
    - while(itr!=map.rend()){itr++}
  - unordered_map
    - 根据key去查找value，需要先判断下是否在容器内
    -   if (flg_obj_out_edge_.find(obj->GetID()) != flg_obj_out_edge_.end()) {
      if (flg_obj_out_edge_.find(obj->GetID())->second){
        continue;
      } 
    }
  - flg_obj_out_edge_[obj->GetID()]; 如果直接用中括号相当于插入一对key
  - 可以通过删除key，去删除整个目标 -> flg_obj_out_edge_.erase(obj->GetID());
  - 

## 5. sturct
   ### struct 初始化
    - CrossMatchCoastInfo cross_tmp{};
    - struct 内也可以定义构造函数
## 6. 类

- 拷贝构造函数


## 符号重载

-   重载函数调用
 bool operator()(const float a,const float b)const{}

 ## STL库

 ### std::move
  1. std::move + std::back_inserter
    - 把x y 两个vector拼接到v这个Vector的末尾
    - `std::move(x.begin(), x.end(), std::back_inserter(v));`
      `std::move(y.begin(), y.end(), std::back_inserter(v));`
  2. 在 C++ 中连接两个Vector
  https://www.techiedelight.com/zh/concatenate-two-vectors-cpp/

