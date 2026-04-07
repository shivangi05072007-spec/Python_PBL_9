# Python_PBL_9
Network Intrusion Detection for Cyber Security  
DARASET:
https://www.kaggle.com/datasets/programmer3/nsl-kdd-intrusion-detection-dataset

DATA CLEANING:

import pandas as pd
from sklearn.preprocessing import LabelEncoder, StandardScaler

df = pd.read_csv(r"D:\PBL AI ML\nsl_kdd_dataset.csv")
print("RAw DATASET: \n",df)

df['packet_size'] = df['src_bytes'] + df['dst_bytes']
df['error_rate'] = df['serror_rate'] + df['rerror_rate']

#droping unnecessary columns

df.drop(['src_bytes','dst_bytes','serror_rate','rerror_rate'],axis=1,inplace=True)

#checking and removing rows with missing values
print("Missing values:\n", df.isnull().sum())
df=df.dropna()

#Encode categorical features
categorical_cols = ['protocol_type', 'service', 'flag']

le = LabelEncoder()
for col in categorical_cols:
    df[col] = le.fit_transform(df[col])

#Convert label to binary (normal = 0, attack = 1)
df['label'] = df['label'].apply(lambda x: 0 if x == 'normal' else 1)

#Separate features and target
X = df.drop('label', axis=1)
y = df['label']

#Feature Scaling
scaler = StandardScaler()
X_scaled = scaler.fit_transform(X)

#Convert back to DataFrame
X_scaled = pd.DataFrame(X_scaled, columns=X.columns)

#Save cleaned dataset
X_scaled['label'] = y
X_scaled.to_csv("cleaned_nsl_kdd.csv", index=False)

print("Data cleaning completed")
print("CLEANED DATASET:\n",df)

