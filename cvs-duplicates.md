import pandas as pd

# 读取 CSV 文件
file_path = r'D:\project\financing_events\pyout\merged_output.csv'  # 替换为你的 CSV 文件路径
df = pd.read_csv(file_path)

# 1. 去除重复项
df = df.drop_duplicates()

# 2. 按照某一变量（列）分组
group_column = '企业简称'  # 替换为你的分组列名
# 确保分组列存在于数据中
if group_column not in df.columns:
    raise ValueError(f"列 '{group_column}' 不存在于数据中！")

# 3. 在每个分组中，保留信息最为完整的唯一值
# 定义信息完整性的衡量标准：非空值的数量
df['non_empty_count'] = df.count(axis=1)  # 计算每行的非空值数量

# 按分组列分组，并在每个分组中选择非空值数量最多的行
result_df = df.loc[df.groupby(group_column)['non_empty_count'].idxmax()]

# 删除辅助列（non_empty_count）
result_df = result_df.drop(columns=['non_empty_count'])

# 保存处理后的数据到新的 CSV 文件
output_file = 'D:\project\financing_events\pyout\processed_data.csv'
result_df.to_csv(output_file, index=False, encoding='utf-8-sig')

print(f"处理完成，结果已保存到 {output_file}")
