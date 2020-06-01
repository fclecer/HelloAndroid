## 笔记初始界面
![](https://img-blog.csdnimg.cn/20200601135114245.png)

1. 可以新建笔记

![](https://img-blog.csdnimg.cn/20200601135349474.png)

2. 此外还可以对笔记进行编辑,以及显示当前所拥有的笔记列表
## 扩展功能
在最初的NotePad中,在每一条笔记信息中只显示了笔记的标题,显示的相关信息比较少,可以扩展一些其他功能,比如增加显示时间戳的功能以及搜索功能
## 扩展功能1:显示日期创建时间戳
1. 首先需要明确的是,要想在笔记列表中显示时间戳就需要对显示笔记列表的xml的布局进行调整,在源码中找到笔记列表的布局文件notelist_item.xml,在该文件中,我创建了一个新的线性布局,在原有标题布局的基础上增加了显示时间戳位置的布局

![](https://img-blog.csdnimg.cn/20200601140147401.png)

2. 设置布局时,也会需要用到一些设置颜色的一些属性,统一放在了values文件夹下面

![](https://img-blog.csdnimg.cn/20200601140322129.png)

3. 下面开始修改代码,找到有关笔记显示列表的代码NoteList.java文件中,可以找到以下代码,找到定义的存储信息的一个变量PROJECTION

![](https://img-blog.csdnimg.cn/20200601140547985.png)

4.在代码中,又定义了一个Cursor进行读取数据

![](https://img-blog.csdnimg.cn/20200601140746137.png)

5. 然后又使用了一个适配器进行数据装配

![](https://img-blog.csdnimg.cn/20200601140844427.png)

6.因此我们可以发现需要在PROJECTION变量中来定义我们要在笔记列表中要显示的时间戳,因此需要修改dataColumns和viewIDs

![](https://img-blog.csdnimg.cn/20200601141020107.png)

7.这个时候运行程序查看效果的话,会发现显示的并不是规范的时间格式而是一串字符串,因此要进行格式转换
8.修改NotePadProvider.java和NoteEditor.java中的代码实现

![](https://img-blog.csdnimg.cn/20200601141224363.png)

![](https://img-blog.csdnimg.cn/20200601141311554.png)

9.修改完瑕疵之后,重新运行查看效果

![](https://img-blog.csdnimg.cn/20200601135137259.png)

## 扩展功能2:增加搜索功能
1. 首先要考虑一下搜索功能的简单实现步骤,参照其他笔记软件来看,一般都是在应用上方存在一个搜索标志按钮,点击后通过用户输入相关信息然后进行搜索
2. 所以这里先在笔记列表页面添加一个搜索的按钮,因此我们需要先找到设置菜单的xml文件list_options_menui.xml

![](https://img-blog.csdnimg.cn/20200601141629728.png)

3. 搜索的图标采用默认安装提供的样子
4.下面开始在笔记列表的代码中,为搜索功能编写代码,首先找到NoteList.java,找到onOptionsItemSelected方法

![](https://img-blog.csdnimg.cn/20200601141925351.png)

5. 我们从该方法前面的方法注释中也可以看到:This method is called when the user selects an option from the menu  当用户从菜单中选择一个选项时调用此方法,而我们恰好增加了一个搜索按钮,因此就是要在该方法中添加搜索功能的逻辑

![](https://img-blog.csdnimg.cn/20200601142132271.png)

6.为了更好地实现搜索效果,我们需要为搜索创建一个activity,同样考虑到搜索一般都是模糊搜索,一次性可以能搜索出很多个笔记,因此在选择搜索功能的布局的时候,完全可以参考笔记列表的布局,因此要新建一个布局文件来作为搜索时使用的

![](https://img-blog.csdnimg.cn/20200601142435886.png)

7.在该布局文件中,选择了一个SearchView控件,他可以达到一个动态显示搜索结果的功能,交互性很好,要动态地显示搜索结果，就要对SearchView文本变化设置监听，搜索类除了要继承ListView外还要实现SearchView.OnQueryTextListener接口

![](https://img-blog.csdnimg.cn/20200601142714642.png)

8.动态搜索的实现最主要的部分在onQueryTextChange方法中，在使用这个方法，要先为SearchView注册监听,而onQueryTextChange方法作用是，当SearchView中文本发生变化时，执行其中代码，搜索还有一个重要的部分就是要做到模糊匹配而不是严格匹配，可以使用数据库查询语句中的LIKE和%结合来实现，newText为输入搜索的内容.

![](https://img-blog.csdnimg.cn/2020060114290037.png)

9.最后在AndroidManifest.xml中注册一下

![](https://img-blog.csdnimg.cn/20200601142954945.png)

10.搜索功能展示,当输入t的时候,在笔记中的两个匹配的笔记可以显示出来

![](https://img-blog.csdnimg.cn/20200601135156725.png)

