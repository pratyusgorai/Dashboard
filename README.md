import pandas as pd

# Example code to load data
stock_data = pd.read_csv('stock_data.csv')
revenue_data = pd.read_csv('revenue_data.csv')
# Handling missing values
stock_data.dropna(inplace=True)
revenue_data.dropna(inplace=True)

# Convert date columns to datetime format
stock_data['Date'] = pd.to_datetime(stock_data['Date'])
revenue_data['Date'] = pd.to_datetime(revenue_data['Date'])

# Merge datasets on date
merged_data = pd.merge(stock_data, revenue_data, on='Date')
import matplotlib.pyplot as plt

# Plotting stock prices
plt.figure(figsize=(12, 6))
plt.plot(merged_data['Date'], merged_data['Stock Price'], label='Stock Price')
plt.title('Historical Stock Prices')
plt.xlabel('Date')
plt.ylabel('Stock Price')
plt.legend()
plt.show()

# Plotting revenue
plt.figure(figsize=(12, 6))
plt.plot(merged_data['Date'], merged_data['Revenue'], label='Revenue')
plt.title('Historical Revenue')
plt.xlabel('Date')
plt.ylabel('Revenue')
plt.legend()
plt.show()
import plotly.express as px

# Create a line chart for stock prices
fig_stock = px.line(merged_data, x='Date', y='Stock Price', title='Stock Prices Over Time')

# Create a line chart for revenue
fig_revenue = px.line(merged_data, x='Date', y='Revenue', title='Revenue Over Time')

# Display the charts
fig_stock.show()
fig_revenue.show()
import dash
from dash import dcc, html
from dash.dependencies import Input, Output

# Initialize the Dash app
app = dash.Dash(__name__)

# Define the layout of the app
app.layout = html.Div([
    html.H1("Company Financial Dashboard"),
    dcc.Graph(id='stock-chart', figure=fig_stock),
    dcc.Graph(id='revenue-chart', figure=fig_revenue)
])

# Run the app
if __name__ == '__main__':
    app.run_server(debug=True)
