# 10️⃣ Ma'lumotlarni saqlash va hisobot tayyorlash

import pandas as pd

# 10.1 Tozalangan DataFrame'ni CSV va Excel formatida saqlash
df.to_csv("education_cleaned.csv", index=False)
df.to_excel("education_cleaned.xlsx", index=False)

print("📂 Fayllar saqlandi: education_cleaned.csv va education_cleaned.xlsx")

# 10.2 Hisobot shaklida qisqacha statistika
report = {}

# Talabalar bo‘yicha umumiy statistikalar
for col in ['Primary_Students','Secondary_Students','Higher_Students']:
    if col in df.columns:
        report[col] = {
            "mean": round(df[col].mean(), 2),
            "min": int(df[col].min()),
            "max": int(df[col].max())
        }

# Ta’lim turi bo‘yicha umumiy son
if 'Type' in df.columns:
    grouped = df.melt(
        id_vars=['Type'],
        value_vars=['Primary_Students','Secondary_Students','Higher_Students'],
        var_name='Category', value_name='Students'
    ).groupby('Type')['Students'].sum().to_dict()
    report["Students_by_Type"] = grouped

# Hisobotni DataFrame qilib chiqarish
report_df = pd.DataFrame(report)
print("\n📊 Qisqacha hisobot:")
print(report_df)

# Hisobotni Excel fayl sifatida saqlash
report_df.to_excel("education_report.xlsx")
print("📑 Hisobot fayli saqlandi: education_report.xlsx")
