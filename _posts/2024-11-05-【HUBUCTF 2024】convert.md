# 【HUBUCTF 2024】convert

#### 思路

1.txt  打开只有01文字

考虑是二进制字节，转为文件

```python
def text_to_binary_file(input_file, output_file):
    with open(input_file, 'r') as infile:
        # 读取文本文件中的内容
        binary_text = infile.read().strip()  # 去除首尾空白字符

    # 确保输入是有效的二进制字符串
    if not all(bit in '01' for bit in binary_text):
        raise ValueError("Input file contains non-binary characters.")

    # 将二进制字符串转换为字节数据
    byte_array = bytearray()
    for i in range(0, len(binary_text), 8):
        byte = binary_text[i:i + 8]  # 每8位构成一个字节
        if len(byte) == 8:  # 确保是完整的字节
            byte_array.append(int(byte, 2))  # 将二进制字符串转换为整数并添加到字节数组

    # 写入二进制文件
    with open(output_file, 'wb') as outfile:
        outfile.write(byte_array)

# 使用示例
text_to_binary_file('1.txt', 'output.bin')

```

拖进010 editor 文件头RAR Archive (rar)，文件头： `52617221`​（把1.txt转为16进制也能看出来这个文件头）

是rar ，解压得到图片。

对图片stegsolve无果。

对图片右键属性->主题->base64解密

得到flag
