# Pandas数据结构

Tags: Python

Learning Date: December 23, 2024

Next Review Day: December 30, 2024

Parent item: Pandas 基础 (https://www.notion.so/Pandas-1666d74ee5e780e39b54d6401a592282?pvs=21)

Status: Review 3

## Series

1. 一维数组对象——由数据标签（索引）+一组数据组成

2. **示例**：索引在左，值在右

   ![image.png](https://raw.githubusercontent.com/RooNat/Myimages/main/2025/03/upgit_20250314_1741936752.png)

3. 通过`ser.values`获取值，`ser.index`获取索引

   ```python
   obj.values
   
   OUT: array([ 4, -7,  5,  3])
   ```

   ```python
   obj.index
   
   OUT: RangeIndex(start=0, stop=4, step=1)
   ```

4. **关于索引（index)**：

   1. 默认自动创建一个0到N-1的整数型索引

   2. **在Series创建时，自己指定索引**

      ```python
      # 指定索引
      obj2 = pd.Series([4, 7, -5, 3], index=['d', 'b', 'a', 'c'])
      ```

      - **索引列表**：通过索引名称获取值

        ```python
        obj2['d']
        # 索引列表
        obj2[['c','a','b']]
        ```

      - **布尔型索引**

        ```python
        obj2[obj2>0]
        ```

   3. **根据索引标签自动对其数据，以进行运算**：如果obj3和obj4有相同的索引，可以直接对其进行运算，两个数组对象会对相同的索引所对应的值进行运算。类似于FULL JOIN，对方列表不存在的索引，将返回NaN值


      ![image.png](https://raw.githubusercontent.com/RooNat/Myimages/main/2025/03/upgit_20250314_1741936833.png)

      ![image.png](https://raw.githubusercontent.com/RooNat/Myimages/main/2025/03/upgit_20250314_1741936838.png)

      ![image.png](https://raw.githubusercontent.com/RooNat/Myimages/main/2025/03/upgit_20250314_1741936843.png)

   4. 给索引列指定名称：`obj4.**index.name** = ‘state’`

   5. **修改索引**：

      1. 可直接通过赋值的方式修改名称：`obj.index =['Bob','Steve','Jeff', 'Ryan']`
      2. 可以使用`ser.rename()`

5. **Series的创建**

   1. 传入列表：`obj = pd.Series([4, -7, 5, 3])`

   2. 将Series看作一个**定长的有序字典**，键为索引，值为数据

      ```python
      sdata = {'Ohio': 35000, 'Texas': 71000, 'Oregon': 16000, 'Utah': 5000}
      obj3 = pd.Series(sdata)
      ```

      1. **可传入自己指定的键以改变数据顺序:**

         1. 传入的键会与原字典的键一一匹配，如果只有键没有对应的值，那么该键将会返回一个NaN值，表示缺失值

            ```python
            states = ['California','Ohio','Oregon','Texas']
            obj4 = pd.Series(sdata, index=states)
            ```

            ![image.png](https://raw.githubusercontent.com/RooNat/Myimages/main/2025/03/upgit_20250314_1741936849.png)

      2. **缺失值的检测**：`pd.isnull(ser)`, `pd.notnull(ser)`, `obj.isnull()`

         1. 返回一组布尔型数据

            ![image.png](https://raw.githubusercontent.com/RooNat/Myimages/main/2025/03/upgit_20250314_1741936862.png)

6. 修改Series对象的名称`obj.name=‘population’`

## DataFrame

1. DataFrame——**表格型数据结构，既有行索引也有列索引，每列可以是不同的值类型**

2. **创建DataFrame**

   1. **传入字典**，DataFrame会自动加上索引：


      ```python
      data = {'state':['Ohio','Ohio','Ohio','Nevada','Nevada','Nevada'],
              'year':[2000,2001,2002,2001,2002,2003],
              'pop':[1.5,1.7,3.6,2.4,2.9,3.2]}
      frame = pd.DataFrame(data)
      ```

      ![image.png](https://raw.githubusercontent.com/RooNat/Myimages/main/2025/03/upgit_20250314_1741936870.png)

      在DataFrame中可以指定列顺序，索引顺序及名称:

      ```python
      pd.DataFrame(data, columns=['year','state','pop'])
      frame2 = pd.DataFrame(data, columns=['year','state','pop','debt'],
                            index=['one','two','three','four','five','six'])
      ```

      1. 指定index名称：`frame2.index.name`
      2. 指定columns名称：`frame2.columns.name`
      3. 使用values获取数据：`frame2.values`

   2. **通过嵌套字典创建DataFrame:**


      ```python
      pop = {'Nevada':{2001:2.4, 2002:2.9},'Ohio':{2000:1.5,2001:1.7, 2002:3.6}}
      frame3 = pd.DataFrame(pop)
      ```

      1. 外层字典的键为列
      2. 内层字典的键为索引，键会被合并

      ![image.png](https://raw.githubusercontent.com/RooNat/Myimages/main/2025/03/upgit_20250314_1741936878.png)

3. **使用`frame.head()`取前五行**

4. `frame.columns`可以获取列名

5. **获取行和列数据**：

   1. `frame[‘列名‘]`可以获取该列：`frame2['state']` 或者使用`frame.year`

      ![image.png](https://raw.githubusercontent.com/RooNat/Myimages/main/2025/03/upgit_20250314_1741936883.png)

   2. `frame.loc[‘索引名’]`可以获取该行：`frame2.loc['three']`

      ![image.png](https://raw.githubusercontent.com/RooNat/Myimages/main/2025/03/upgit_20250314_1741936888.png)

6. **修改列**：通过赋值方式修改——`frame[‘debt’] = 16.5`

   1. 将列表或数组赋值给某个列时。长度必须与DataFrame的长度匹配

   2. **可以将一个Series赋值给某个列**，Series的index会与DataFrame进行匹配，空位会表示为缺失值

      ```python
      val = pd.Series([-1.2,-1.5,-1.7], index=['two','four','five'])
      frame2['debt'] = val
      ```

7. **修改单元格数据**：`data.loc[0, 'name'] = 'New Name'`

8. **del用于删除列**：`del frame[‘eastern’]`

9. 对DataFrame也可以进行转置：`frame.T`

![image.png](https://raw.githubusercontent.com/RooNat/Myimages/main/2025/03/upgit_20250314_1741936895.png)

## 索引对象

1. **获取index对象：**

   ```python
   obj = pd.Series(range(3), index=['a','b','c'])
   index = obj.index
   ```

2. index对象是不可变的，不能对其修改：`index[1]='d'` ❌

3. 创建索引对象：`labels= pd.Index(np.arang(3))`

   1. **对Series指定index: `obj2 = pd.Series([1.5,-2.5,0], index=labels)`**

4. Index可以包含重复的标签：`dup_labels = pd.Index(['foo','foo','bar','bar'])` 

![image.png](https://raw.githubusercontent.com/RooNat/Myimages/main/2025/03/upgit_20250314_1741936900.png)

参考书籍：
[1] 《Python for Data Analysis》
[2] 《利用Python进行数据分析》
