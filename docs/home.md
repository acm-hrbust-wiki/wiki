## 一：关于本站

!!! check "Info"
    - **博客框架**: MkDocs
    - **主题**: Material for MkDocs
    - **编写文本**：markdown

## 二：编写说明

1.  推荐使用 [Typora](https://www.typora.io/) 来编写markdown文件

2.  博客架构支持 **LaTeX** 数学公式和markdown扩展

    !!! note "例如你所见到的"
        $f_{i,j,k}$, $f(i,j,k)$ <br>
    
3.  支持代码高亮与复制

    **C++**

    ```c++
    #include <bits/stdc++.h>
    using namespace std;
    
    int main() 
    {
    	vector<int> v;
    	for (int i = 1; i <= 10; i++) 
    		v.push_back(i);
    	for (auto x: v) cout << x << " ";
    	printf("Hello World\n");
    	return 0;
    }
    ```

    **python**

    ```python
    def blogs_with_type(request, blog_type_pk):
        context = {}
        blog_type = get_object_or_404(BlogType, pk=blog_type_pk)
        context['blogs'] = Blog.objects.filter(blog_type=blog_type)
        context['blog_type'] = blog_type
        return render(request, 'blogs_with_type.html', context)
    ```

    

## 三：编辑修改

熟悉GitHub的同学可以直接提交PR

不熟悉的将写好的文档发到 QQ: 1486176948， 推荐提交markdown文档。





