import os
import pandas as pd

# 设置文件夹路径，包含所有需要处理的 Excel 文件 
folder_path = 'path_to_your_excel_files'  # 替换为你的文件夹路径
output_csv = 'merged_output.csv'  # 输出的 CSV 文件名

# 初始化一个空的 DataFrame，用于存储合并后的数据
merged_data = pd.DataFrame()

# 遍历文件夹中的所有文件
for file_name in os.listdir(folder_path):
    if file_name.endswith('.xlsx'):  # 确保只处理 Excel 文件
        file_path = os.path.join(folder_path, file_name)
        # 读取 Excel 文件
        df = pd.read_excel(file_path)
        
        # 提取部分指标（假设需要提取的列名为 'Column1', 'Column2' 等）
        # 如果列名不同，请根据实际情况修改
        selected_columns = ['Column1', 'Column2']  # 替换为实际需要的列名
        df = df[selected_columns]
        
        # 将提取的数据纵向合并到主 DataFrame 中
        merged_data = pd.concat([merged_data, df], ignore_index=True)

# 将合并后的数据输出为 CSV 文件
merged_data.to_csv(output_csv, index=False, encoding='utf-8-sig')

print(f"所有文件已处理完成，结果已保存到 {output_csv}")
