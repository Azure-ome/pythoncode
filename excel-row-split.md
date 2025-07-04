import pandas as pd

# 读取 Excel 文件
file_path = r'D:\cvc\event\品牌方-标的企业.xlsx'  # 替换为你的文件路径
df = pd.read_excel(file_path, engine='openpyxl')

# 定义关键变量和需要拆分的列
key_columns = ['品牌名']
content_columns = ['标的企业名', '行业', '融资时间', '融资金额', '单位', '轮次', '上市状态', '上市时间']

# 初始化一个空的列表来存储拆分后的数据
result_data = []

# 遍历 DataFrame 的每一行
for index, row in df.iterrows():
    # 获取关键变量的值
    key_values = {col: row[col] for col in key_columns}
    
    # 初始化一个标志，用于检查是否需要跳过当前行
    skip_row = False
    
    # 遍历需要拆分的列
    for content_col in content_columns:
        content = row[content_col]
        
        # 如果内容为空，跳过当前行
        if pd.isna(content):
            skip_row = True
            break
        
        # 按换行符拆分内容
        lines = content.split('\n')
        
        # 如果这是第一个需要拆分的列，直接创建新的数据行
        if content_col == content_columns[0]:
            for line in lines:
                new_row = key_values.copy()  # 复制关键变量的值
                new_row[content_col] = line  # 更新当前列的内容
                result_data.append(new_row)
        else:
            # 如果不是第一个需要拆分的列，需要对已有的结果数据进行进一步拆分
            temp_result_data = []
            for existing_row in result_data:
                for line in lines:
                    new_row = existing_row.copy()
                    new_row[content_col] = line
                    temp_result_data.append(new_row)
            result_data = temp_result_data
    
    # 如果需要跳过当前行，则清空临时结果数据
    if skip_row:
        result_data = []

# 将结果转换为 DataFrame
result_df = pd.DataFrame(result_data)

# 保存处理后的数据到新的 Excel 文件
output_file_path = r"D:\cvc\event\python\品牌方-标的企业.xlsx"  # 输出文件路径
result_df.to_excel(output_file_path, index=False, engine='openpyxl')

print(f"处理完成，结果已保存到 {output_file_path}")
