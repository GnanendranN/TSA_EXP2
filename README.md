# Ex.No: 02 LINEAR AND POLYNOMIAL TREND ESTIMATION
## Date: 17-05-2026
### AIM:
To Implement Linear and Polynomial Trend Estiamtion Using Python.

### ALGORITHM:
1. Import necessary libraries (NumPy, Matplotlib)
2. Load the dataset
3. Calculate the linear trend values using least square method
4. Calculate the polynomial trend values using least square method
5. End the program
## PROGRAM:
```py
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt

data=pd.read_csv('/content/tea_vs_coffee_global_final.csv',parse_dates=['year'],index_col='year')
data.head()

resampled_data = data['cups_per_day'].resample('YE').mean().to_frame()
resampled_data.index = resampled_data.index.year
resampled_data.reset_index(inplace=True)
resampled_data.rename(columns={'year': 'Year', 'cups_per_day': 'cups_per_day_yearly_mean'}, inplace=True)
years = resampled_data['Year'].tolist()
cups_per_day_values = resampled_data['cups_per_day_yearly_mean'].tolist()

X = [i - years[len(years) // 2] for i in years]
x2 = [i ** 2 for i in X]
xy = [i * j for i, j in zip(X, cups_per_day_values)]
n = len(years)

if n < 2:
    b = 0.0
    a = cups_per_day_values[0] if n == 1 else 0.0
    linear_trend = [a + b * x_val for x_val in X]
else:
    b = (n * sum(xy) - sum(cups_per_day_values) * sum(X)) / (n * sum(x2) - (sum(X) ** 2))
    a = (sum(cups_per_day_values) - b * sum(X)) / n
    linear_trend = [a + b * X[i] for i in range(n)]

# Re-calculating X, x2, xy, n to ensure consistency
X = [i - years[len(years) // 2] for i in years]
x2 = [i ** 2 for i in X]
xy = [i * j for i, j in zip(X, cups_per_day_values)]
n = len(years)

x3 = [i ** 3 for i in X]
x4 = [i ** 4 for i in X]
x2y = [i * j for i, j in zip(x2, cups_per_day_values)]

if n < 3:
    a_poly = cups_per_day_values[0] if n > 0 else 0.0
    b_poly = 0.0
    c_poly = 0.0
else:
    coeff = [[len(X), sum(X), sum(x2)],
    [sum(X), sum(x2), sum(x3)],
    [sum(x2), sum(x3), sum(x4)]]
    Y = [sum(cups_per_day_values), sum(xy), sum(x2y)]
    A = np.array(coeff)
    B = np.array(Y)
    solution = np.linalg.solve(A, B)
    a_poly, b_poly, c_poly = solution

poly_trend = [a_poly + b_poly * X[i] + c_poly * (X[i] ** 2) for i in range(n)]

# Linear trend estimation
X = [i - years[len(years) // 2] for i in years]
x2 = [i ** 2 for i in X]
xy = [i * j for i, j in zip(X, cups_per_day_values)]
n = len(years)

if n < 2:
    b = 0.0
    a = cups_per_day_values[0] if n == 1 else 0.0
    linear_trend = [a + b * x_val for x_val in X]
else:
    b = (n * sum(xy) - sum(cups_per_day_values) * sum(X)) / (n * sum(x2) - (sum(X) ** 2))
    a = (sum(cups_per_day_values) - b * sum(X)) / n
    linear_trend = [a + b * X[i] for i in range(n)]

# Polynomial trend estimation
x3 = [i ** 3 for i in X]
x4 = [i ** 4 for i in X]
x2y = [i * j for i, j in zip(x2, cups_per_day_values)]

if n < 3:
    a_poly = cups_per_day_values[0] if n > 0 else 0.0
    b_poly = 0.0
    c_poly = 0.0
else:
    coeff = [[len(X), sum(X), sum(x2)],
    [sum(X), sum(x2), sum(x3)],
    [sum(x2), sum(x3), sum(x4)]]
    Y = [sum(cups_per_day_values), sum(xy), sum(x2y)]
    A = np.array(coeff)
    B = np.array(Y)
    solution = np.linalg.solve(A, B)
    a_poly, b_poly, c_poly = solution

poly_trend = [a_poly + b_poly * X[i] + c_poly * (X[i] ** 2) for i in range(n)]

print(f"Linear Trend: y={a:.2f} + {b:.2f}x")
print(f"\nPolynomial Trend: y={a_poly:.2f} + {b_poly:.2f}x + {c_poly:.2f}x²")
resampled_data['Linear Trend'] = linear_trend
resampled_data['Polynomial Trend'] = poly_trend

plt.figure(figsize=(12, 6))
resampled_data['cups_per_day_yearly_mean'].plot(kind='line',color='blue',marker='o', label='Original Cups per Day (Yearly Mean)')
resampled_data['Linear Trend'].plot(kind='line',color='black',linestyle='--', label='Linear Trend')
resampled_data['Polynomial Trend'].plot(kind='line',color='red',marker='x', label='Polynomial Trend')
plt.title('Cups per Day Yearly Mean with Linear and Polynomial Trends')
plt.xlabel('Year')
plt.ylabel('Cups per Day Yearly Mean')
plt.legend()
plt.grid(True)
plt.show()
```

## OUTPUT
<img width="1039" height="536" alt="image" src="https://github.com/user-attachments/assets/49635699-3746-4979-bb30-00af30142ad4" />

## RESULT:
Thus the python program for linear and Polynomial Trend Estiamtion has been executed successfully.
