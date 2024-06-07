# Correlation-for-Crypto
This repository provides tools and examples for analyzing the correlation between various cryptocurrencies. The primary focus is on three main techniques: Double Y-Axis Charts, MinMaxScaler, and Spearman Correlation Heatmaps.

คลังนี้ให้เครื่องมือและตัวอย่างสำหรับการวิเคราะห์ความสัมพันธ์ระหว่างคริปโตเคอร์เรนซีหลาย ๆ ตัว โดยเน้นที่เทคนิคหลักสามอย่าง ได้แก่ Double Y-Axis Charts, MinMaxScaler และ Spearman Correlation Heatmaps
## [Correlation for Crypto in Python code.](https://github.com/pongpichaGithub/Correlation-for-Crypto/blob/41a3d040925b6e75a6ee00e3fab84271799f94ac/Spearman_Correlation_for_Crypto.ipynb) 
## Double Y-Axis Chart 
Double Y-Axis Charts (also known as Dual-Axis or Secondary Axis Charts) are used to compare two datasets with different scales on the same graph. This technique is particularly useful when comparing the price movements of two cryptocurrencies that have significantly different values.

Double Y-Axis Charts (หรือที่เรียกว่า Dual-Axis หรือ Secondary Axis Charts) ใช้เพื่อเปรียบเทียบชุดข้อมูลสองชุดที่มีสเกลต่างกันในกราฟเดียว เทคนิคนี้มีประโยชน์อย่างยิ่งเมื่อเปรียบเทียบการเคลื่อนไหวของราคาคริปโตเคอร์เรนซีสองตัวที่มีมูลค่าต่างกันมาก
Example
In this repository, you will find an example that demonstrates how to plot the prices of Bitcoin (BTC) and Ethereum (ETH) using a double y-axis chart. This method allows for a clear comparison of the trends of both cryptocurrencies over time, despite their different price ranges.

```python
import yfinance as yf
import pandas as pd
from scipy.stats import spearmanr
import matplotlib.pyplot as plt

# Download price data from Yahoo Finance
btc = yf.download('BTC-USD', start='2023-01-01', end='2023-12-31')
eth = yf.download('ETH-USD', start='2023-01-01', end='2023-12-31')

# Combine price data
df = pd.DataFrame({
    'BTC': btc['Close'],
    'ETH': eth['Close']
}).dropna()

# Calculate Spearman Correlation
spearman_corr, _ = spearmanr(df['BTC'], df['ETH'])
print(f"Spearman correlation between BTC and ETH: {spearman_corr}")

# Plot the prices of BTC and ETH using two y-axes
fig, ax1 = plt.subplots(figsize=(14, 7))

# Plot BTC price on the first y-axis
color = 'tab:red'
ax1.set_xlabel('Date')
ax1.set_ylabel('BTC Price (USD)', color=color)
ax1.plot(df.index, df['BTC'], color=color)
ax1.tick_params(axis='y', labelcolor=color)

# Create a second y-axis to plot ETH price
ax2 = ax1.twinx()  # instantiate a second axes that shares the same x-axis
color = 'tab:blue'
ax2.set_ylabel('ETH Price (USD)', color=color)  # we already handled the x-label with ax1
ax2.plot(df.index, df['ETH'], color=color)
ax2.tick_params(axis='y', labelcolor=color)

# Adjust layout to prevent overlapping
fig.tight_layout()  # otherwise the right y-label is slightly clipped
plt.title('Price Comparison: BTC vs ETH')
plt.show() 
```

## MinMaxScaler
MinMaxScaler is a preprocessing technique used to normalize the range of independent variables or features of data. In this repository, we use MinMaxScaler to scale cryptocurrency price data to a range of 0 to 1. This makes it easier to visualize and compare different cryptocurrencies on the same scale.

MinMaxScaler เป็นเทคนิคการเตรียมข้อมูลที่ใช้เพื่อทำให้ช่วงของตัวแปรอิสระหรือคุณลักษณะของข้อมูลอยู่ในช่วงเดียวกัน ในคลังนี้ เราใช้ MinMaxScaler เพื่อปรับสเกลข้อมูลราคาของคริปโตเคอร์เรนซีให้อยู่ในช่วง 0 ถึง 1 ทำให้ง่ายต่อการแสดงผลและเปรียบเทียบคริปโตเคอร์เรนซีต่าง ๆ บนสเกลเดียวกัน

Example
The example provided demonstrates how to use MinMaxScaler to scale the prices of Bitcoin and Ethereum, and then plot them on the same graph for comparison.

ตัวอย่าง

ตัวอย่างที่ให้มาแสดงวิธีการใช้ MinMaxScaler เพื่อปรับสเกลราคาของ Bitcoin และ Ethereum จากนั้นพล็อตกราฟเพื่อเปรียบเทียบ
```python
import yfinance as yf
import pandas as pd
from sklearn.preprocessing import MinMaxScaler
import matplotlib.pyplot as plt

# Download price data from Yahoo Finance
btc = yf.download('BTC-USD', start='2020-01-01', end='2023-12-31')
eth = yf.download('ETH-USD', start='2020-01-01', end='2023-12-31')

# Combine price data
df = pd.DataFrame({
    'BTC': btc['Close'],
    'ETH': eth['Close']
}).dropna()

# Use MinMaxScaler to scale the data between 0 and 1
scaler = MinMaxScaler()
df_scaled = pd.DataFrame(scaler.fit_transform(df), columns=df.columns, index=df.index)

# Plot the scaled prices of BTC and ETH
df_scaled.plot(figsize=(14, 7))
plt.title('Price Comparison: BTC vs ETH (Scaled)')
plt.xlabel('Date')
plt.ylabel('Scaled Price')
plt.legend(['BTC', 'ETH'])
plt.show()
```

## Spearman Correlation Heatmap
Spearman Correlation is a non-parametric measure of rank correlation (statistical dependence between the rankings of two variables). This repository includes examples of how to calculate the Spearman Correlation between multiple cryptocurrencies and visualize the results using a heatmap. This heatmap helps to quickly identify the strength and direction of the correlations between different cryptocurrencies.

Spearman Correlation เป็นมาตรการการหาความสัมพันธ์เชิงอันดับแบบไม่อิงพารามิเตอร์ (การพึ่งพาทางสถิติระหว่างอันดับของตัวแปรสองตัว) คลังนี้รวมตัวอย่างการคำนวณ Spearman Correlation ระหว่างคริปโตเคอร์เรนซีหลายตัว และแสดงผลด้วย heatmap ซึ่งช่วยให้สามารถระบุความแข็งแรงและทิศทางของความสัมพันธ์ระหว่างคริปโตเคอร์เรนซีต่าง ๆ ได้อย่างรวดเร็ว

### Example
The example below demonstrates how to calculate the Spearman Correlation between several cryptocurrencies and create a heatmap to visualize the correlations.

 ตัวอย่าง
ตัวอย่างด้านล่างแสดงวิธีการคำนวณ Spearman Correlation ระหว่างคริปโตเคอร์เรนซีหลายตัว และสร้าง heatmap เพื่อแสดงผลความสัมพันธ์
```python
import yfinance as yf
import pandas as pd
from scipy.stats import spearmanr
import seaborn as sns
import matplotlib.pyplot as plt

# List of cryptocurrency symbols to download
cryptos = ['BTC-USD', 'ETH-USD', 'LTC-USD', 'XRP-USD']

# Download price data from Yahoo Finance
data = {}
for crypto in cryptos:
    data[crypto] = yf.download(crypto, start='2023-01-01', end='2023-12-31')['Close']

# Combine price data into a DataFrame
df = pd.DataFrame(data).dropna()

# Calculate Spearman Correlation
spearman_corr_matrix = df.corr(method='spearman')

# Plot heatmap of Spearman Correlation
plt.figure(figsize=(10, 8))
sns.heatmap(spearman_corr_matrix, annot=True, cmap='RdYlGn', center=0, linewidths=0.5, linecolor='black')
plt.title('Spearman Correlation Heatmap of Cryptocurrencies')
plt.show()
```
