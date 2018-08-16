|方法| 描述|
|--|--|
|cat()| 连接字符串|
|split()|在分隔符上分割字符串|
|rsplit()| 从字符串末尾开始分隔字符串|
|get()| 索引到每个元素（检索第i个元素）|
|join()| 使用分隔符在系列的每个元素中加入字符串|
|get_dummies()| 在分隔符上分割字符串，返回虚拟变量的DataFrame|
|contains()| 如果每个字符串都包含pattern / regex，则返回布尔数组|
|replace()| 用其他字符串替换pattern / regex的出现|
|repeat()| 重复值（s.str.repeat(3)等同于x * 3 t2 >） |
|pad()| 将空格添加到字符串的左侧，右侧或两侧|
|center()| 相当于str.center|
|ljust()| 相当于str.ljust|
|rjust()| 相当于str.rjust|
|zfill()| 等同于str.zfill|
|wrap()| 将长长的字符串拆分为长度小于给定宽度的行|
|slice()| 切分Series中的每个字符串|
|slice_replace()| 用传递的值替换每个字符串中的切片|
|count()| 计数模式的发生|
|startswith()| 相当于每个元素的str.startswith(pat)|
|endswith()| 相当于每个元素的str.endswith(pat)|
|findall()| 计算每个字符串的所有模式/正则表达式的列表|
|match()| 在每个元素上调用re.match，返回匹配的组作为列表|
|extract()| 在每个元素上调用re.search，为每个元素返回一行DataFrame，为每个正则表达式捕获组返回一列|
|extractall()| 在每个元素上调用re.findall，为每个匹配返回一行DataFrame，为每个正则表达式捕获组返回一列|
|len()| 计算字符串长度|
|strip()| 相当于str.strip|
|rstrip()| 相当于str.rstrip|
|lstrip()| 相当于str.lstrip|
|partition()| 等同于str.partition|
|rpartition()| 等同于str.rpartition|
|lower()| 相当于str.lower|
|upper()| 相当于str.upper|
|find()| 相当于str.find|
|rfind()| 相当于str.rfind|
|index()| 相当于str.index|
|rindex()| 相当于str.rindex|
|capitalize()| 相当于str.capitalize|
|swapcase()| 相当于str.swapcase|
|normalize()| 返回Unicode标准格式。相当于unicodedata.normalize|
|translate()| 等同于str.translate|
|isalnum()| 等同于str.isalnum|
|isalpha()| 等同于str.isalpha|
|isdigit()| 相当于str.isdigit|
|isspace()| 等同于str.isspace|
|islower()| 相当于str.islower|
|isupper()| 相当于str.isupper|
|istitle()| 相当于str.istitle|
|isnumeric()| 相当于str.isnumeric|
|isdecimal()| 相当于str.isdecimal|