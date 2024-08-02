# 常用命令行操作

## 1. 终端使用基础

打开终端：通常可以使用快捷键 `Ctrl + Alt + T`

## 2. 文件和目录操作

- 列出文件和目录：
  ```bash
  ls
  ls -l  # 详细信息
  ls -a  # 显示隐藏文件
  ```

- 切换目录：
  ```bash
  cd /path/to/directory
  cd ..  # 返回上一级目录
  cd ~   # 返回家目录
  ```

- 创建目录：
  ```bash
  mkdir new_directory
  ```

- 删除文件或目录：
  ```bash
  rm file.txt
  rm -r directory  # 递归删除目录
  ```

- 复制和移动：
  ```bash
  cp source destination
  mv old_name new_name
  ```

## 3. 文本编辑

- 使用nano（适合初学者）：
  ```bash
  nano filename.txt
  ```

- 使用vim（功能强大，学习曲线陡峭）：
  ```bash
  vim filename.txt
  ```
  
  vim基本操作：
  - `i`：进入插入模式
  - `Esc`：返回命令模式
  - `:w`：保存
  - `:q`：退出
  - `:wq`：保存并退出

## 4. 文件查找

- 使用find命令：
  ```bash
  find /path/to/search -name "filename"
  ```

- 使用locate命令（需要更新数据库）：
  ```bash
  sudo updatedb
  locate filename
  ```

## 5. 文本处理

- grep：搜索文本
  ```bash
  grep "search_term" filename
  ```

- sed：流编辑器
  ```bash
  sed 's/old/new/g' filename
  ```

- awk：文本分析工具
  ```bash
  awk '{print $1}' filename  # 打印第一列
  ```